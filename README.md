# Performance and Dynamism in User-extensible Compiler Infrastructures

A research project in partial requirement for a Computer Science MPhil at the
University of Cambridge, supervised by [Dr Tobias
Grosser](https://grosser.science/) and co-supervised by [Sasha Lopoukhine](https://www.cst.cam.ac.uk/people/al666).

## Abstract

> MLIR is a modular compiler framework that provides core infrastructure to be leveraged and extended by users implementing their own compilers, an inherently dynamic design as a result of the underlying heterogeneous data structure whose shape is known only at runtime.
> This approach presents an inherent optimization boundary, as the dynamic structures cannot be precisely reasoned about before runtime to guarantee the validity of optimisations, meaning static ahead-of-time compilation provides fewer benefits.
> Previous compiler frameworks accept the limitations of this optimization boundary, leveraging only the remaining optimizations offered by static ahead-of-time compilation, yet still incurring the costs of long build times and reduced flexibility, suggesting dynamic languages might be more suitable.
> We examine performance bottlenecks incurred by dynamic languages for code rewriting tasks in xDSL, a Python-native compiler framework inspired by MLIR.
> We find that both the inherent dynamism of these rewriting tasks over runtime heterogeneous data structures and modern interpreter optimisations narrow the performance gap between static and dynamic languages, using both traditional measurement techniques and a novel tool for performance profiling bytecode instructions.
> Our research challenges the status quo of implementing user-extensible compiler frameworks in static, ahead-of-time compiled languages.
> Instead, we motivate the use of dynamic languages, demonstrating that they balance compilation performance with the flexibility and fast build times.

**Keywords:** xDSL, MLIR, LLVM, Dynamic Programming Languages, Performance, User-extensible Compiler Infrastructure

## Thesis

<p align="center" style="width: 100%;">
<a href="https://github.com/EdmundGoodman/masters-project-report/releases/download/moodle-submission/edjg2-project-1.pdf">
<img src="https://github.com/EdmundGoodman/masters-project-report/blob/main/images/project-cover.png?raw=true" style="width: 50%; border: 1px solid #000;">
</a>
</p>

## Presentation

<p align="center" style="width: 100%;">
<a href="https://github.com/EdmundGoodman/masters-project-presentation/releases/download/tidied/Project_presentation.pdf">
<img src="https://github.com/EdmundGoodman/masters-project-presentation/blob/main/images/title_slide.png?raw=true" style="width: 80%;">
</a>
</p>


## Code

In the course of the thesis, the author contributed a number of PRs to the
[xdsl](https://github.com/xdslproject/xdsl) project, including moving to use
`uv`, along with bug fixes and performance optimisations informed by the
benchmarking and specialisation processes, and development of the PyAST frontend.

In addition to this, the author originated the following repositories:

1. [xdsl-bench](https://github.com/xdslproject/xdsl-bench) -- the infrastructure and artefacts from benchmarking the xDSL compiler framework with air-speed velocity
2. [bytesight](https://github.com/EdmundGoodman/bytesight) -- a Python-native tracing performance profiler operating at the bytecode level, introduced in the fourth chapter of the thesis
3. [llvm-project-benchmarks](https://github.com/EdmundGoodman/llvm-project-benchmarks/tree/benchmarks) -- a fork of source code for the benchmarks implemented by Mehdi Amini
for his "How Slow is MLIR?" talk, as a resource for users trying to benchmark MLIR as the talk provides no instructions nor links to their source code. Additional benchmarks for direct comparison with xDSL are provided in the `dev/` branch
