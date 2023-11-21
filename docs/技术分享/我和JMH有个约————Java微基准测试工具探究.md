---
password: ""
icon: ""
date: "2022-03-30"
type: Post
category: 技术分享
slug: jmh
tags:
  - Java
  - 性能测试
summary: 从 JDK 12开始，JDK 自带 JMH (Java Microbenchmark Harness) ，它是一个工具包，可以帮助我们正确地实现 Java 微基准测试。今天我和它正好有个约。
title: 我和JMH有个约————Java微基准测试工具探究
status: Published
cover: "https://images.unsplash.com/photo-1533709752211-118fcaf03312?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: cab96cc8-d3ee-4627-a63d-6c5ebe6b67db
updated: "2023-08-07 20:24:00"
---

> 😃 从 JDK 12 开始，JDK 就带有 JMH (Java Microbenchmark Harness) ，它是一个工具包，可以帮助您正确地实现 Java 微基准测试。JMH 是由实现 Java 虚拟机(JVM)的同一批人开发的，因此他们了解 Java 的内部原理以及 Java 如何在运行时进行优化。

# 一、What is JMH？

**JMH：** Java Micro Benchmark Harness【代码微基准测试工具集】 的简写。【[github for jmh](https://github.com/openjdk/jmh)】

- JMH 是 OpenJDK 团队开发的一款基准测试工具，一般用于代码的性能调优，精度甚至可以达到纳秒级别，适用于 java 以及其他基于 JVM 的语言。和 Apache JMeter 不同，**JMH 测试的对象可以是任一方法，颗粒度更小**，而不仅限于 rest api。

# 二、Why JMH？

## 2.1 JVM causes！

现在的 JVM 已经越来越为智能，它可以在编译阶段、加载阶段、运行阶段对代码进行优化。在需要进行性能测试时，如果不知道 JVM 优化细节，可能会导致你的测试结果差之毫厘，失之千里。同样的，Java 诞生之初就有一次编译、随处运行的口号，JVM 提供了底层支持，也提供了内存管理机制，这些机制都会对我们的性能测试结果造成不可预测的影响。

也许我们测试一个简单方法，是使用如下方式，亦或者加个循环，然后用总时间除以循环次数。

```java
long start = System.currentTimeMillis();
// ....
long end = System.currentTimeMillis();
System.out.println(end - start);
```

但是，最终测试出来的数据真的准确吗？答案是否定的。

首先，时间戳的获取就有可能存在误差；其次，JVM 可能会对一些代码进行优化，导致运行时不是真实场景下的耗时；再则，在循环中，JVM 同样会有优化，会把循环展开（这里不展开说明）；最后，JVM 会在各个阶段都有可能对代码进行优化，存在不确定性。

## 2.2 Without JMH

- **错误估计代码的性能**

  当基准测试单独执行该组件时，JVM 或底层硬件可能会对您的组件应用许多优化。 当组件作为更大应用程序的一部分运行时，这些优化可能无法应用。 因此，实施不当的微基准测试可能会让你相信你的组件的性能比实际情况要好。

- **无法清晰判断相似方法之间的真实性能差距，从而错误选择方案**

  每当我们遇到问题时，我们倾向于递归地、迭代地或使用我们语言中提供的内置方法来解决它。 编码后，我们可能会倾向于混淆对于给定的数据集的规模，大的，小的，还是固定；有时，是否有更好的优势来使用内置方法或其他的方法 A，B 等等呢？

## 2.3 With JMH

- **性能测试更精确，能够阻止 JVM 和硬件在微基准执行期间应用的优化，从而模拟真实场景的代码运行性能**

  该工具是由 Oracle JVM 开发团队相关成员开发的，借助它，开发者将能足够了解自己所编写的程序代码，以及程序在运行期的精确性能表现。

- **上手简单，只需要一些简单注解修饰，即可对相似的方法集合进行性能测试**

  使用时，我们只需要通过配置告诉 JMH 测试哪些方法以及如何测试，JMH 就可以为我们**自动生成基准测试的代码**。如同编写单元测试一样简单。

  考虑这样一种场景，在实现某个功能时需要某线程安全的类，但是该类却有不同的实现方式，难以取舍之际，即可使用 JMH 进行精准的性能测试，提供一个比较好的参考。

# 三、JMH 快速上手

## 3.1 依赖引入：

```xml
<!--jmh 基准测试 -->
<dependency>
	<groupId>org.openjdk.jmh</groupId>
	<artifactId>jmh-core</artifactId>
	<version>1.34</version>
</dependency>
<dependency>
	<groupId>org.openjdk.jmh</groupId>
	<artifactId>jmh-generator-annprocess</artifactId>
	<version>1.34</version>
</dependency>
```

## 3.2 一个简单 Demo：

我们对一个简单方法进行性能测试

```java
public class JMHExample01 {
    @Benchmark
    public void wellHelloThere() {
        // this method was intentionally left blank.
    }

    public static void main(String[] args) throws RunnerException {
        final Options options = new OptionsBuilder().include(JMHExample01.class.getSimpleName())
                .forks(1)
                .measurementIterations(5)
                .warmupIterations(5)
                .build();
        new Runner(options).run();
    }
}
```

从代码中可以看出，我们对 `wellHelloThere` 函数进行性能测试，这里是故意留空的。

`measurementIterations(5)` `warmupIterations(5)` 分别表示正式运行批次与预热运行批次为 5

运行结果如下：

```text
# JMH version: 1.34
# VM version: JDK 1.8.0_312_fiber, OpenJDK 64-Bit Server VM, 25.312-b1
# VM invoker: D:\Software\TencentKona-8.0.8-312\jre\bin\java.exe
# VM options: -Dfile.encoding=UTF-8 -javaagent:C:\Program Files\JetBrains\IntelliJ IDEA 2021.2\lib\idea_rt.jar=52883:C:\Program Files\JetBrains\IntelliJ IDEA 2021.2\bin -Dfile.encoding=UTF-8
# Blackhole mode: full + dont-inline hint (auto-detected, use -Djmh.blackhole.autoDetect=false to disable)
# Warmup: 5 iterations, 10 s each
# Measurement: 5 iterations, 10 s each
# Timeout: 10 min per iteration
# Threads: 1 thread, will synchronize iterations
# Benchmark mode: Throughput, ops/time
# Benchmark: com.eachen.jmh.samples.JMHSample_01_HelloWorld.wellHelloThere

# Run progress: 0.00% complete, ETA 00:01:40
# Fork: 1 of 1
# Warmup Iteration   1: 4250418666.712 ops/s
# Warmup Iteration   2: 4351990563.955 ops/s
# Warmup Iteration   3: 4294196982.723 ops/s
# Warmup Iteration   4: 4356422901.963 ops/s
# Warmup Iteration   5: 4380265370.149 ops/s
Iteration   1: 4333736833.571 ops/s
Iteration   2: 4357296430.734 ops/s
Iteration   3: 4389356560.825 ops/s
Iteration   4: 4388132443.569 ops/s
Iteration   5: 4383114985.169 ops/s
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8


Result "com.eachen.jmh.samples.JMHSample_01_HelloWorld.wellHelloThere":
  4370327450.774 ±(99.9%) 93359784.885 ops/s [Average]
  (min, avg, max) = (4333736833.571, 4370327450.774, 4389356560.825), stdev = 24245239.658
  CI (99.9%): [4276967665.889, 4463687235.658] (assumes normal distribution)


# Run complete. Total time: 00:01:41

REMEMBER: The numbers below are just data. To gain reusable insights, you need to follow up on
why the numbers are the way they are. Use profilers (see -prof, -lprof), design factorial
experiments, perform baseline and negative tests that provide experimental control, make sure
the benchmarking environment is safe on JVM/OS/HW level, ask for reviews from the domain experts.
Do not assume the numbers tell you what you want them to tell.

Benchmark                                Mode  Cnt           Score          Error  Units
JMHSample_01_HelloWorld.wellHelloThere  thrpt    5  4370327450.774 ± 93359784.885  ops/s
```

得出的结果是，每秒可以运行 4370327450.774 次 【ops/s = operations per second】，误差在 93359784.885

# 四、JMH 基本用法

## 4.1 `@Benchmark`标记基准测试方法

对需要测试的方法使用注解 `@Benchmark`

如果没有检测到被注解，则会抛出异常

```text
Exception in thread "main" org.openjdk.jmh.runner.RunnerException: ERROR: Another JMH instance might be running. Unable to acquire the JMH lock (C:\Users\EACHEN~1\AppData\Local\Temp\/jmh.lock), exiting. Use -Djmh.ignoreLock=true to forcefully continue.
	at org.openjdk.jmh.runner.Runner.run(Runner.java:211)
	at com.eachen.concurrence.jmh.JMHExample02.main(JMHExample02.java:41)
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
```

## 4.2 `Warmup` 和 `Measurement`

### 什么是 Warmup 与 Measurement？

Warmup 与 Measurement 可以设置运行批次，前者表示预热的批次数，后者表示正式运行的批次数。

- Warmup【预热】在 JMH 中，Warmup 所做的就是【在基准测试代码正式度量之前，先对其进行预热，使得代码的执行是经历过了类的早期优化、JVM 运行期编译、JIT 优化之后的最终状态】，从而能够获得代码真实的性能数据。
- Measurement 则是真正的度量操作，在每一轮的度量中，所有的度量数据会被纳入统计之中（预热数据不会纳入统计之中）

### 怎么使用 Warmup 与 Measurement？

1. 设置全局的 Warmup 和 Measurement
   - 既可以通过构造 Options 时设置
   - 也可以在对应的 class 上用相应的注解进行设置。
2. 在基准测试方法上设置 Warmup 和 Measurement

> 注意：runtime 的 options 配置可以覆盖 注解中设置的数值

```java
// 1.1 通过构造Options时设置
public static void main(String[] args) throws RunnerException {
        final Options options = new OptionsBuilder().include(JMHExample01.class.getSimpleName())
                .forks(1)
                .measurementIterations(10)
                .warmupIterations(10)
                .build();
        new Runner(options).run();
}
// 1.2 在对应的class上用相应的注解进行设置
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.MICROSECONDS)
@Measurement(iterations = 10)
@Warmup(iterations = 10)
@State(Scope.Thread)
public class JMHExample02 {
// 2 在基准测试方法上设置Warmup和Measurement
	@Measurement(iterations = 10)
	@Warmup(iterations = 10)
    public void normalMethod() {

    }
```

### Warmup 以及 Measurement 详细说明

事实上，对于 Warmup 以及 Measurement，可以设置四个变量：

- iterations 迭代的批次
- time 对于每个批次的时间
- timeUnit 与 time 对应，是其时间单位
- batchSize 每个批次时 benchmark 方法运行的次数

```java
/**
 * <p>Measurement annotations allows to set the default measurement parameters for
 * the benchmark.</p>
 *
 * <p>This annotation may be put at {@link Benchmark} method to have effect on that
 * method only, or at the enclosing class instance to have the effect over all
 * {@link Benchmark} methods in the class. This annotation may be overridden with
 * the runtime options.</p>
 *
 * @see Warmup
 */
@Inherited
@Target({ElementType.METHOD,ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Measurement {

    int BLANK_ITERATIONS = -1;
    int BLANK_TIME = -1;
    int BLANK_BATCHSIZE = -1;

    /** @return Number of measurement iterations */
    int iterations() default BLANK_ITERATIONS;

    /** @return Time of each measurement iteration */
    int time() default BLANK_TIME;

    /** @return Time unit for measurement iteration duration */
    TimeUnit timeUnit() default TimeUnit.SECONDS;

    /** @return Batch size: number of benchmark method calls per operation */
    int batchSize() default BLANK_BATCHSIZE;

}

/**
 * <p>Warmup annotation allows to set the default warmup parameters for the benchmark.</p>
 *
 * <p>This annotation may be put at {@link Benchmark} method to have effect on that method
 * only, or at the enclosing class instance to have the effect over all {@link Benchmark}
 * methods in the class. This annotation may be overridden with the runtime options.</p>
 *
 * @see Measurement
 */
@Target({ElementType.METHOD,ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
public @interface Warmup {

    int BLANK_ITERATIONS = -1;
    int BLANK_TIME = -1;
    int BLANK_BATCHSIZE = -1;

    /** @return Number of warmup iterations */
    int iterations() default BLANK_ITERATIONS;

    /** @return Time for each warmup iteration */
    int time() default BLANK_TIME;

    /** @return Time unit for warmup iteration duration */
    TimeUnit timeUnit() default TimeUnit.SECONDS;

    /** @return batch size: number of benchmark method calls per operation */
    int batchSize() default BLANK_BATCHSIZE;

}
```

## 4.3 BenchmarkMode

JMH 使用@BenchmarkMode 这个注解来声明使用哪一种模式来运行，JMH 为我们提供了四种运行模式，当然它还允许若干个模式同时存在

1. AverageTimeAverageTime 它主要用于输出基准测试方法每调用一次所耗费的时间，也就是 elapsed time/operation。
2. ThroughputThroughput（方法吞吐量）则刚好与 AverageTime 相反，它的输出信息表明了在单位时间内可以对该方法调用多少次。
3. SampleTimeSampleTime（时间采样）的方式是指采用一种抽样的方式来统计基准测试方法的性能结果，与我们常见的 Histogram 图（直方图）几乎是一样的，它会收集所有的性能数据，并且将其分布在不同的区间中。
4. SingleShotTime 主要可用来进行冷测试，不论是 Warmup 还是 Measurement，在每一个批次中基准测试方法只会被执行一次，一般情况下，我们会将 Warmup 的批次设置为 0。
5. 多 Mode 以及 All 我们除了对某个基准测试方法设置上述四个模式中的一个之外，还可以为其设置多个模式的方式运行基准测试方法，如果你愿意，甚至可以设置全部的 Mode。【可以看到 BenchmarkMode 注解是支持一个 Mode 数组的】

BenchmarkMode 可以作为注解对 Benchmark 方法或者 class 上，也可以通过 Options 进行设置，同样的，它会覆盖注解中的设置。

```java
/**
 * Benchmark mode.
 */
public enum Mode {

    /**
     * <p>Throughput: operations per unit of time.</p>
     *
     * <p>Runs by continuously calling {@link Benchmark} methods,
     * counting the total throughput over all worker threads. This mode is time-based, and it will
     * run until the iteration time expires.</p>
     */
    Throughput("thrpt", "Throughput, ops/time"),

    /**
     * <p>Average time: average time per per operation.</p>
     *
     * <p>Runs by continuously calling {@link Benchmark} methods,
     * counting the average time to call over all worker threads. This is the inverse of {@link Mode#Throughput},
     * but with different aggregation policy. This mode is time-based, and it will run until the iteration time
     * expires.</p>
     */
    AverageTime("avgt", "Average time, time/op"),

    /**
     * <p>Sample time: samples the time for each operation.</p>
     *
     * <p>Runs by continuously calling {@link Benchmark} methods,
     * and randomly samples the time needed for the call. This mode automatically adjusts the sampling
     * frequency, but may omit some pauses which missed the sampling measurement. This mode is time-based, and it will
     * run until the iteration time expires.</p>
     */
    SampleTime("sample", "Sampling time"),

    /**
     * <p>Single shot time: measures the time for a single operation.</p>
     *
     * <p>Runs by calling {@link Benchmark} once and measuring its time.
     * This mode is useful to estimate the "cold" performance when you don't want to hide the warmup invocations, or
     * if you want to see the progress from call to call, or you want to record every single sample. This mode is
     * work-based, and will run only for a single invocation of {@link Benchmark}
     * method.</p>
     *
     * Caveats for this mode include:
     * <ul>
     *  <li>More warmup/measurement iterations are generally required.</li>
     *  <li>Timers overhead might be significant if benchmarks are small; switch to {@link #SampleTime} mode if
     *  that is a problem.</li>
     * </ul>
     */
    SingleShotTime("ss", "Single shot invocation time"),

    /**
     * Meta-mode: all the benchmark modes.
     * This is mostly useful for internal JMH testing.
     */
    All("all", "All benchmark modes"),

    ;

    private final String shortLabel;
    private final String longLabel;

    Mode(String shortLabel, String longLabel) {
        this.shortLabel = shortLabel;
        this.longLabel = longLabel;
    }

    public String shortLabel() {
        return shortLabel;
    }

    public String longLabel() {
        return longLabel;
    }

    public static Mode deepValueOf(String name) {
        try {
            return Mode.valueOf(name);
        } catch (IllegalArgumentException iae) {
            Mode inferred = null;
            for (Mode type : values()) {
                if (type.shortLabel().startsWith(name)) {
                    if (inferred == null) {
                        inferred = type;
                    } else {
                        throw new IllegalStateException("Unable to parse benchmark mode, ambiguous prefix given: \"" + name + "\"\n" +
                                "Known values are " + getKnown());
                    }
                }
            }
            if (inferred != null) {
                return inferred;
            } else {
                throw new IllegalStateException("Unable to parse benchmark mode: \"" + name + "\"\n" +
                        "Known values are " + getKnown());
            }
        }
    }

    public static List<String> getKnown() {
        List<String> res = new ArrayList<>();
        for (Mode type : Mode.values()) {
            res.add(type.name() + "/" + type.shortLabel());
        }
        return res;
    }
}
```

## 4.4 OutputTimeUnit

OutputTimeUnit 提供了统计结果输出时的单位，比如，调用一次该方法将会耗费多少个单位时间，或者在单位时间内对该方法进行了多少次的调用，同样，OutputTimeUnit 既可以设置在 class 上，也可以设置在 method 上，还可以在 Options 中进行设置，它们的覆盖次序与 BenchmarkMode 一致，这里就不再赘述了。

## 4.5 三大 State 的使用

在 JMH 中，有三大 State 分别对应于 Scope 的三个枚举值。

- Benchmark
- Thread
- Group

### Thread 独享的 State

所谓线程独享的 State 是指，每一个运行基准测试方法的线程都会持有一个独立的对象实例，该实例既可能是作为基准测试方法参数传入的，也可能是运行基准方法所在的宿主 class，将 State 设置为 Scope.Thread 一般主要是针对非线程安全的类。

### Thread 共享的 State

有时候，我们需要测试在多线程的情况下某个类被不同线程操作时的性能，比如，多线程访问某个共享数据时，我们需要让多个线程使用同一个实例才可以。因此 JMH 提供了多线程共享的一种状态 Scope.Benchmark。

### 线程组共享的 State

第一，是在多线程情况下的单个实例；第二，允许一个以上的基准测试方法并发并行地运行。比如，在多线程高并发的环境中，多个线程同时对一个 ConcurrentHashMap 进行读写。使用 group 即可实现这种情况，多个基准测试方法可以并发运行。

## 4.6 @Param 的妙用

可以解决代码的冗余，提供类似 Data-Driven-Test 的能力。

使用 param 可以实现 N* N * N 的测试效果。

另外，可以

参考 `JMHSample_27_Params` 例子

```java
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.NANOSECONDS)
@Warmup(iterations = 5, time = 1, timeUnit = TimeUnit.SECONDS)
@Measurement(iterations = 5, time = 1, timeUnit = TimeUnit.SECONDS)
@Fork(1)
@State(Scope.Benchmark)
public class JMHSample_27_Params {

    /**
     * In many cases, the experiments require walking the configuration space
     * for a benchmark. This is needed for additional control, or investigating
     * how the workload performance changes with different settings.
     */

    @Param({"1", "31", "65", "101", "103"})
    public int arg;

    @Param({"0", "1", "2", "4", "8", "16", "32"})
    public int certainty;

    @Benchmark
    public boolean bench() {
        return BigInteger.valueOf(arg).isProbablePrime(certainty);
    }

    public static void main(String[] args) throws RunnerException {
        Options opt = new OptionsBuilder()
                .include(JMHSample_27_Params.class.getSimpleName())
//                .param("arg", "41", "42") // Use this to selectively constrain/override parameters
                .build();

        new Runner(opt).run();
    }
}
```

## 4.7 JMH 的测试套件（Fixture）

### Setup 以及 TearDown

JMH 提供了两个注解@Setup 和@TearDown 用于套件测试，其中@Setup 会在每一个基准测试方法执行前被调用，通常用于资源的初始化，@TearDown 则会在基准测试方法被执行之后被调用，通常可用于资源的回收清理工作

### Level

使用 Setup 和 TearDown 时，在默认情况下，Setup 和 TearDown 会在一个基准方法的所有批次执行前后分别执行，如果需要在每一个批次或者每一次基准方法调用执行的前后执行对应的套件方法，则需要对@Setup 和@TearDown 进行简单的配置。

- Trial：Setup 和 TearDown 默认的配置，该套件方法会在每一个基准测试方法的所有批次执行的前后被执行。【对应下图的位置 1 与位置 2】
- Iteration：由于我们可以设置 Warmup 和 Measurement，因此每一个基准测试方法都会被执行若干个批次，如果想要在每一个基准测试批次执行的前后调用套件方法，则可以将 Level 设置为 Iteration。【对应下图的位置 3 和位置 4】
- Invocation：将 Level 设置为 Invocation 意味着在每一个批次的度量过程中，每一次对基准方法的调用前后都会执行套件方法。【对应下图的位置 5 与位置 6】

![rId47.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/764db17e-0fa6-4bc1-a03f-dac8a4640c28/rId47.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120749Z&X-Amz-Expires=3600&X-Amz-Signature=e5871e72b4e08b1b07443143575b0b193f77fa403d24170817f1cd3937e522b8&X-Amz-SignedHeaders=host&x-id=GetObject)

## 4.8 CompilerControl

JMH 提供了可以控制是否使用内联的注解 @CompilerControl ，它的参数有如下可选：

- CompilerControl.Mode.DONT_INLINE：不使用内联
- CompilerControl.Mode.INLINE：强制使用内联
- CompilerControl.Mode.EXCLUDE：不编译

此外还有其他的参数选项，可以参考：

```java
@Target({ElementType.METHOD, ElementType.CONSTRUCTOR, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface CompilerControl {

    /**
     * The compilation mode.
     * @return mode
     */
    Mode value();

    /**
     * Compilation mode.
     */
    enum Mode {

        /**
         * Insert the breakpoint into the generated compiled code.
         */
        BREAK("break"),

        /**
         * Print the method and it's profile.
         */
        PRINT("print"),

        /**
         * Exclude the method from the compilation.
         */
        EXCLUDE("exclude"),

        /**
         * Force inline.
         */
        INLINE("inline"),

        /**
         * Force skip inline.
         */
        DONT_INLINE("dontinline"),

        /**
         * Compile only this method, and nothing else.
         */
        COMPILE_ONLY("compileonly"),;

        private final String command;

        Mode(String command) {
            this.command = command;
        }

        public String command() {
            return command;
        }
    }
}
```

这里给到 jmh 的一个示例：

```java
@State(Scope.Thread)
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.NANOSECONDS)
public class JMHSample_16_CompilerControl {

    /*
     * We can use HotSpot-specific functionality to tell the compiler what
     * do we want to do with particular methods. To demonstrate the effects,
     * we end up with 3 methods in this sample.
     */

    /**
     * These are our targets:
     *   - first method is prohibited from inlining
     *   - second method is forced to inline
     *   - third method is prohibited from compiling
     *
     * We might even place the annotations directly to the benchmarked
     * methods, but this expresses the intent more clearly.
     */

    public void target_blank() {
        // this method was intentionally left blank
    }

    @CompilerControl(CompilerControl.Mode.DONT_INLINE)
    public void target_dontInline() {
        // this method was intentionally left blank
    }

    @CompilerControl(CompilerControl.Mode.INLINE)
    public void target_inline() {
        // this method was intentionally left blank
    }

    @CompilerControl(CompilerControl.Mode.EXCLUDE)
    public void target_exclude() {
        // this method was intentionally left blank
    }

    /*
     * These method measures the calls performance.
     */

    @Benchmark
    public void baseline() {
        // this method was intentionally left blank
    }

    @Benchmark
    public void blank() {
        target_blank();
    }

    @Benchmark
    public void dontinline() {
        target_dontInline();
    }

    @Benchmark
    public void inline() {
        target_inline();
    }

    @Benchmark
    public void exclude() {
        target_exclude();
    }

    public static void main(String[] args) throws RunnerException {
        Options opt = new OptionsBuilder()
                .include(JMHSample_16_CompilerControl.class.getSimpleName())
                .warmupIterations(0)
                .measurementIterations(3)
                .forks(1)
                .build();

        new Runner(opt).run();
    }
}
```

结果如下：

```text
Benchmark                                Mode  Cnt  Score   Error  Units
JMHSample_16_CompilerControl.baseline    avgt    3  0.231 ± 0.014  ns/op
JMHSample_16_CompilerControl.blank       avgt    3  0.228 ± 0.006  ns/op
JMHSample_16_CompilerControl.dontinline  avgt    3  1.494 ± 3.834  ns/op
JMHSample_16_CompilerControl.exclude     avgt    3  9.419 ± 6.557  ns/op
JMHSample_16_CompilerControl.inline      avgt    3  0.227 ± 0.007  ns/op
```

从执行结果可以看到内联方法和空方法执行速度一样，不编译执行最慢。

# 五、如何正确使用 JMH

> 了解之后，那么应该需要知道——如何编写正确的微基准测试用例：

1. 避免 DCE（Dead Code Elimination）
2. 使用 Blackhole
3. 避免常量折叠（Constant Folding）
4. 避免循环展开（Loop Unwinding）
5. Fork 用于避免 Profile-guided optimizations

> 编写正确的微机准

## 5.1 避免 DCE（Dead Code Elimination）

编译器会对一些冗余的、不被其他地方用到的代码进行删除，如果这些代码在我们的性能测试中，那么会造成结果的不准确。

我们看一个官方例子：

```java
@State(Scope.Thread)
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.NANOSECONDS)
public class JMHSample_08_DeadCode {

    private double x = Math.PI;

    @Benchmark
    public void baseline() {
        // do nothing, this is a baseline
    }

    @Benchmark
    public void measureWrong() {
        // This is wrong: result is not used and the entire computation is optimized away.
        Math.log(x);
    }

    @Benchmark
    public double measureRight() {
        // This is correct: the result is being used.
        return Math.log(x);
    }

    public static void main(String[] args) throws RunnerException {
        Options opt = new OptionsBuilder()
                .include(JMHSample_08_DeadCode.class.getSimpleName())
                .forks(1)
                .build();

        new Runner(opt).run();
    }
}
```

baseline 方法啥都不做，作为基准方法

measureWrong 方法进行了 log 计算，但是结果未被使用，这里会被编译器优化的，实际运行效果与 baseline 一致。

measureRight 方法正好相反，返回了计算的结果，这里会有正常的耗时。

我们直接看结果：

```text
Benchmark                           Mode  Cnt   Score   Error  Units
JMHSample_08_DeadCode.baseline      avgt    5   0.235 ± 0.035  ns/op
JMHSample_08_DeadCode.measureRight  avgt    5  16.278 ± 0.524  ns/op
JMHSample_08_DeadCode.measureWrong  avgt    5   0.231 ± 0.012  ns/op
```

measureWrong 与 baseline 耗时基本持平

measureRight 耗时明显增多

## 5.2 使用 Blackhole

那有什么方法可以避免这种情况呢？上面可以知道，将局部变量返回，就能避免 DCE 了，但是，如果有很多变量，我们不可能去构造一个 List 来保存吧。那构造的时间还得考虑进去，这就太复杂了。

所以，这里就要用到 Blackhole 【黑洞】了。

还是看一个例子：

```java
package com.eachen.jmh.samples;

import org.openjdk.jmh.annotations.*;
import org.openjdk.jmh.infra.Blackhole;
import org.openjdk.jmh.runner.Runner;
import org.openjdk.jmh.runner.RunnerException;
import org.openjdk.jmh.runner.options.Options;
import org.openjdk.jmh.runner.options.OptionsBuilder;

import java.util.concurrent.TimeUnit;

@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.NANOSECONDS)
@State(Scope.Thread)
public class JMHSample_09_Blackholes {
    double x1 = Math.PI;
    double x2 = Math.PI * 2;

    @Benchmark
    public double baseline() {
        return Math.log(x1);
    }

    @Benchmark
    public double measureWrong() {
        Math.log(x1);
        return Math.log(x2);
    }

    @Benchmark
    public double measureRight_1() {
        return Math.log(x1) + Math.log(x2);
    }

    @Benchmark
    public void measureRight_2(Blackhole bh) {
        bh.consume(Math.log(x1));
        bh.consume(Math.log(x2));
    }
    public static void main(String[] args) throws RunnerException {
        Options opt = new OptionsBuilder()
                .include(JMHSample_09_Blackholes.class.getSimpleName())
                .forks(1)
                .build();

        new Runner(opt).run();
    }
}
```

里面有四个基准方法：

- baseline 方法是 通过 return 返回 Log 计算，会执行一次 log，作为我们的 baseline。
- measureWrong 虽然看起来计算了两次，但是只有 return 那个语句会执行，所以结果应该是与 baseline 持平的。
- measureRight_1 通过 加法操作将两次计算求和，时间上大致上是 baseline 的两倍
- measureRight_2 通过将结果保存在 Blackhole 中，让两次计算不会被编译器优化，时间上与 measureRight_1 基本持平，大致上是 baseline 的两倍。

直接看结果：

```text
Benchmark                               Mode  Cnt   Score   Error  Units
JMHSample_09_Blackholes.baseline        avgt    5  16.387 ± 0.799  ns/op
JMHSample_09_Blackholes.measureRight_1  avgt    5  32.154 ± 4.586  ns/op
JMHSample_09_Blackholes.measureRight_2  avgt    5  33.255 ± 0.551  ns/op
JMHSample_09_Blackholes.measureWrong    avgt    5  16.226 ± 0.230  ns/op
```

与我们分析的分毫误差。

measureRight_2 会比 measureRight_1 多一些时间，这是因为 black hole 的 consume 方法也会占用一定的 CPU。这种情况下，对无返回的方法进行性能分析时，针对局部变量的使用都统一使用 black hole，就能够保证同样的基准执行条件了。

## 5.3 避免常量折叠（Constant Folding）

常量折叠是 Java 编译器早起的一种优化——编译优化。

在编译器里进行语法分析的时候，将常量表达式计算求值，并用求得的值来替换表达式，放入常量表，可以算作一种编译优化。这种情况下，也会对我们的真实性能测试起到一些影响。

我们看一个例子：

```java
package com.eachen.jmh.samples;

import org.openjdk.jmh.annotations.*;
import org.openjdk.jmh.runner.Runner;
import org.openjdk.jmh.runner.RunnerException;
import org.openjdk.jmh.runner.options.Options;
import org.openjdk.jmh.runner.options.OptionsBuilder;

import java.util.concurrent.TimeUnit;

@State(Scope.Thread)
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.NANOSECONDS)
public class JMHSample_10_ConstantFold {

	private double x = Math.PI;
    private final double wrongX = Math.PI;

    @Benchmark
    public double baseline() {
        // simply return the value, this is a baseline
        return Math.PI;
    }

    @Benchmark
    public double measureWrong_1() {
        // This is wrong: the source is predictable, and computation is foldable.
        return Math.log(Math.PI);
    }

    @Benchmark
    public double measureWrong_2() {
        // This is wrong: the source is predictable, and computation is foldable.
        return Math.log(wrongX);
    }

    @Benchmark
    public double measureRight() {
        // This is correct: the source is not predictable.
        return Math.log(x);
    }

    public static void main(String[] args) throws RunnerException {
        Options opt = new OptionsBuilder()
                .include(JMHSample_10_ConstantFold.class.getSimpleName())
                .forks(1)
                .build();

        new Runner(opt).run();
    }
}
```

其中有四个基准方法：

- baseline 直接返回 PI 这个常量，用来作为 baseline 对比
- measureWrong_1 中返回的结果 Math.log(Math.PI) 也是可以通过计算得到的一个常量，基本时间与 baseline 一致
- measureWrong_2 中对类中的`final`变量进行计算，final 修饰的数为常量，这里也是可以计算出的一个常量，基本时间与 baseline 一致
- measureRight 中 对类中的成员变量进行 log 计算，这里在类初始化才能拿到值的，所以对于编译器是不可预知的值，所以结果会比前三个方法运行耗时较久。

我们看看运行结果：

```text
Benchmark                                 Mode  Cnt   Score   Error  Units
JMHSample_10_ConstantFold.baseline        avgt    5   1.616 ± 0.052  ns/op
JMHSample_10_ConstantFold.measureRight    avgt    5  16.186 ± 0.283  ns/op
JMHSample_10_ConstantFold.measureWrong_1  avgt    5   1.619 ± 0.075  ns/op
JMHSample_10_ConstantFold.measureWrong_2  avgt    5   1.613 ± 0.018  ns/op
```

结果正如我们分析所料，前三个方法中，在编译器优化阶段的时候就发生了常量折叠，这些方法在运行阶段根本不需要再进行计算，直接将结果返回即可，而 measureRight 则没有进行编译器优化，所以统计数据会较高。

## 5.4 避免循环展开（Loop Unwinding）

**循环展开**，是一种牺牲程序的大小来加快程序执行速度的优化方法。可以由程序员完成，也可由编译器自动优化完成。

对于如下的代码：

```java
for (i = 1; i <= 60; i++)
   a[i] = a[i] * b + c;
```

JVM 可能会优化成这样

```java
for (i = 1; i <= 58; i+=3)
{
  a[i] = a[i] * b + c;
  a[i+1] = a[i+1] * b + c;
  a[i+2] = a[i+2] * b + c;
}
```

我们看一个例子：

```java
package com.eachen.jmh.samples;

import org.openjdk.jmh.annotations.*;
import org.openjdk.jmh.runner.Runner;
import org.openjdk.jmh.runner.RunnerException;
import org.openjdk.jmh.runner.options.Options;
import org.openjdk.jmh.runner.options.OptionsBuilder;

import java.util.concurrent.TimeUnit;

@State(Scope.Thread)
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.NANOSECONDS)
public class JMHSample_11_Loops {

    int x = 1;
    int y = 2;

    /*
     * This is what you do with JMH.
     */

    @Benchmark
    public int measureRight() {
        return (x + y);
    }

    /*
     * The following tests emulate the naive looping.
     * This is the Caliper-style benchmark.
     */
    private int reps(int reps) {
        int s = 0;
        for (int i = 0; i < reps; i++) {
            s += (x + y);
        }
        return s;
    }

    /*
     * We would like to measure this with different repetitions count.
     * Special annotation is used to get the individual operation cost.
     */

    @Benchmark
    @OperationsPerInvocation(1)
    public int measureWrong_1() {
        return reps(1);
    }

    @Benchmark
    @OperationsPerInvocation(10)
    public int measureWrong_10() {
        return reps(10);
    }

    @Benchmark
    @OperationsPerInvocation(100)
    public int measureWrong_100() {
        return reps(100);
    }

    @Benchmark
    @OperationsPerInvocation(1_000)
    public int measureWrong_1000() {
        return reps(1_000);
    }

    @Benchmark
    @OperationsPerInvocation(10_000)
    public int measureWrong_10000() {
        return reps(10_000);
    }

    @Benchmark
    @OperationsPerInvocation(100_000)
    public int measureWrong_100000() {
        return reps(100_000);
    }

    public static void main(String[] args) throws RunnerException {
        Options opt = new OptionsBuilder()
                .include(JMHSample_11_Loops.class.getSimpleName())
                .forks(1)
                .build();

        new Runner(opt).run();
    }
}
```

代码中使用了 `@OperationsPerInvocation` 注解，这里是为了在最终结果中计算单次循环的耗时

@OperationsPerInvocation(1_000) 表示我们将 op 作为 1000，也就是计算耗时的时候，是用总时间除以 1000 次，其他的类似。

```text
Benchmark                               Mode  Cnt  Score    Error  Units
JMHSample_11_Loops.measureRight         avgt    5  1.749 ±  0.039  ns/op
JMHSample_11_Loops.measureWrong_1       avgt    5  1.745 ±  0.040  ns/op
JMHSample_11_Loops.measureWrong_10      avgt    5  0.197 ±  0.010  ns/op
JMHSample_11_Loops.measureWrong_100     avgt    5  0.027 ±  0.001  ns/op
JMHSample_11_Loops.measureWrong_1000    avgt    5  0.019 ±  0.001  ns/op
JMHSample_11_Loops.measureWrong_10000   avgt    5  0.016 ±  0.002  ns/op
JMHSample_11_Loops.measureWrong_100000  avgt    5  0.015 ±  0.001  ns/op
```

我们可以看到，在循环次数越多的情况下，折叠的情况也越多，因此性能会更好，说明 JVM 在运行期对我们的代码进行了优化。

## 5.5 Fork 用于避免 Profile-guided optimizations

**Profile-guided optimization** (**PGO**, sometimes pronounced as _pogo_）是计算机编程中的一种编译器最佳化技术，它使用进程 Profile 来提高程序运行时的性能。

Fork 的引入也是考虑到了这个问题，虽然 Java 支持多线程，但是不支持多进程，这就导致了所有的代码都在一个进程中运行，相同的代码在不同时刻的执行可能会引入前一阶段对进程 profiler 的优化，甚至会混入其他代码 profiler 优化时的参数，这很有可能会导致我们所编写的微基准测试出现不准确的问题。

使用 Fork 能重新开辟一个 JVM，保证环境基础一致，而不会被其他环境影响。

我们还是看一个例子：

```java
package com.eachen.jmh.samples;

import org.openjdk.jmh.annotations.*;
import org.openjdk.jmh.runner.Runner;
import org.openjdk.jmh.runner.RunnerException;
import org.openjdk.jmh.runner.options.Options;
import org.openjdk.jmh.runner.options.OptionsBuilder;

import java.util.concurrent.TimeUnit;

@State(Scope.Thread)
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.NANOSECONDS)
public class JMHSample_12_Forking {

    public interface Counter {
        int inc();
    }

    public static class Counter1 implements Counter {
        private int x;

        @Override
        public int inc() {
            return x++;
        }
    }

    public static class Counter2 implements Counter {
        private int x;

        @Override
        public int inc() {
            return x++;
        }
    }

    public int measure(Counter c) {
        int s = 0;
        for (int i = 0; i < 10; i++) {
            s += c.inc();
        }
        return s;
    }

    Counter c1 = new Counter1();
    Counter c2 = new Counter2();

    @Benchmark
    @Fork(0)
    public int measure_1_c1() {
        return measure(c1);
    }

    @Benchmark
    @Fork(0)
    public int measure_2_c2() {
        return measure(c2);
    }

    @Benchmark
    @Fork(0)
    public int measure_3_c1_again() {
        return measure(c1);
    }

    @Benchmark
    @Fork(1)
    public int measure_4_forked_c1() {
        return measure(c1);
    }

    @Benchmark
    @Fork(1)
    public int measure_5_forked_c2() {
        return measure(c2);
    }

    public static void main(String[] args) throws RunnerException {
        Options opt = new OptionsBuilder()
                .include(JMHSample_12_Forking.class.getSimpleName())
                .build();

        new Runner(opt).run();
    }
}
```

运行结果：

```text
Benchmark                                 Mode  Cnt   Score   Error  Units
JMHSample_12_Forking.measure_1_c1         avgt    5   1.878 ± 0.064  ns/op
JMHSample_12_Forking.measure_2_c2         avgt    5  11.179 ± 0.212  ns/op
JMHSample_12_Forking.measure_3_c1_again   avgt    5  10.537 ± 0.252  ns/op
JMHSample_12_Forking.measure_4_forked_c1  avgt    5   2.843 ± 0.850  ns/op
JMHSample_12_Forking.measure_5_forked_c2  avgt    5   2.721 ± 0.317  ns/op
```

若将 Fork 设置为 0，则会与运行基准测试的类共享同样的进程 Profiler

若设置为 1 则会为每一个基准测试方法开辟新的进程去运行

当然，你可以将 Fork 设置为大于 1 的数值，那么它将多次运行在不同的进程中，不过一般情况下，我们只需要将 Fork 设置为 1 即可。

# 六、如何接入项目？

根据[GitHub - openjdk/jmh](https://github.com/openjdk/jmh)的介绍，可以知道

> The recommended way to run a JMH benchmark is to use Maven to setup a standalone project that depends on the jar files of your application. This approach is preferred to ensure that the benchmarks are correctly initialized and produce reliable results. It is possible to run benchmarks from within an existing project, and even from within an IDE, however setup is more complex and the results are less reliable.

推荐的运行 JMH 基准测试的方法是使用 Maven 建立一个独立的项目，该项目依赖于您的应用程序的 jar 文件。这种方法更适合于确保基准测试程序正确地初始化并产生可靠的结果。在现有的项目中甚至在 IDE 中运行基准测试是可能的，但是设置更复杂，结果更不可靠。

# 参考链接：

[Java 高并发编程详解.汪文君](https://item.jd.com/12707285.html?msclkid=770daf20b6df11ec806b7a0743853a80)

[如何在 Java 中使用 JMH 进行基准测试 - SegmentFault 思否](https://segmentfault.com/a/1190000039902797)

[GitHub idea-jmh-plugin](https://github.com/artyushov/idea-jmh-plugin)

[jmh 官方例子](http://hg.openjdk.java.net/code-tools/jmh/file/tip/jmh-samples/src/main/java/org/openjdk/jmh/samples/)

[GitHub - openjdk/jmh](https://github.com/openjdk/jmh)
