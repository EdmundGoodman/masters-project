# Performance profiling and optimisation of the xDSL compiler framework

A research project in partial requirement for a Computer Science MPhil at the
University of Cambridge, supervised by [Dr Tobias
Grosser](https://grosser.science/) and co-supervised by Sasha Lopoukhine.

## Provisional abstract

> xDSL is a Python-native compiler framework, facilitating benefits such as
> modularity with LLVM MLIR whilst retaining simple scriptability. However,
> being implemented in Python comes with a performance trade-off for these
> benefits. We first leverage performance profiling techniques to identify
> bottlenecks in the xDSL project codebase, then investigate algorithmic, data
> structure, caching, system, and hardware-based optimisations to mitigate them.
> Finally, we assess the degree to which the Python's performance impact on
> compiler workloads can be reduced whilst retaining its benefits, strengthening
> the case for xDSL and its novel approach to compiler design.

**Keywords:** Compilation, LLVM, MLIR, profiling, optimisation, Python

## Download

This is a meta-repository of the written and software deliverables for this
project, and as such contains nested submodules. To clone a copy containing all
nested submodules, consider running the following command:

```bash
git clone --recurse-submodules -j8 https://github.com/EdmundGoodman/masters-project
```

## Directory structure

Written project deliverables are typeset in LaTeX, and both code and written
content can be found in the submodule repositories listed below:

```text
.
├── LICENSE
├── README.md
├── code
│   ├── update-bot
│   └── xdsl-bench
│       └── xdsl
├── proposal
├── report
├── work-log
├── bench -> code/xdsl-bench
└── xdsl -> code/xdsl-bench/xdsl
```

## Further information

See the departmental website for further information:

- General: <https://www.cst.cam.ac.uk/teaching/masters/projects>
- Timetable: <https://www.cst.cam.ac.uk/teaching/masters/projects/acs>
- Guidance: <https://www.cst.cam.ac.uk/teaching/masters/projects/acs/guidelines>
- Archive: <https://www.cst.cam.ac.uk/teaching/masters/projects/archive>
- Typography: <https://www.cl.cam.ac.uk/local/typography/#thesis>
- Submission: <https://www.cst.cam.ac.uk/teaching/masters/projects/submission>
- Assessment: <https://www.cl.cam.ac.uk/teaching/exams/acs_project_marking.pdf>
