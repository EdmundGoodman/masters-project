# Work log

The work log for my Masters project, in partial requirement for a Computer
Science MPhil at the University of Cambridge.

This document is prepend only except where otherwise stated, tracking the work
completed up to the current date of the project.

<!-- ====================================================================== -->

## Week 5 (6/1/2025 -- 12/1/2025)

### Work planned/completed (23/12/2024 -- 12/1/2025)

- [x] Deploy ASV infrastructure to organisation website
- [x] Write more performance benchmarks to cover xDSL stack
- [x] Investigate performance between Python versions
- [x] Continue profiling, now with a focus on identifying/visualising hotspots
- [ ] Start trying to find/fix some simple performance defects
- [ ] Read about CPython internals (deferred due to planned optimisation approach of using bindings to low-level language)

### Plan for next weeks (12/1/2025 -- 19/1/2025)

<!-- ====================================================================== -->

## Weeks 3-4 (23/12/2024 -- 5/1/2025)

Reduced work due to Christmas vacation and other coursework deadlines.

<!-- ====================================================================== -->

## Week 2 (16/12/2024 -- 22/12/2024)

### Work planned/completed (16/12/2024 -- 22/12/2024)

- [x] Finish up ASV infrastructure for benchmarking
- [x] Create meta-repo repository containing deliverables, code, and work log
- [x] Write unit/integration benchmark workloads
- [ ] Explore other profilers

### Plan for next weeks (23/12/2024 -- 12/1/2025)

- Deploy ASV infrastructure to organisation website
- Continue profiling, now with a focus on identifying/visualising hotspots
- Start trying to find/fix some simple performance defects
- Read about CPython internals

<!-- ====================================================================== -->

## Week 1 (9/12/2024 -- 15/12/2024)

### Work planned/completed (9/12/2024 -- 15/12/2024)

- [x] Mostly focussed on literature review on ML compilers
  (Halide/TVM/IREE/tensat+egg/...) for another module, but has been helpful
  background for the project
- [x] `uv` PR for xDSL merged
- [ ]  Started prototyping separate repo for ASV publishing xDSL benchmarks --
  encountered some peculiarities with installation of submodule

### Plan for next week (16/12/2024 -- 22/12/2024)

- Finish up ASV infrastructure for benchmarking
- Write unit/integration benchmark workloads
- Explore other profilers

<!-- ====================================================================== -->

## Before project start

### Work planned/completed (before project start)

- [x] MPhil project approved by admin starting Monday 2nd December
- [x] Kickoff meeting with Sasha (and Chris at beginning)
- [ ] Merge `uv` PR for xDSL - merge conflicts resolved, but that revealed a
  couple more issues which need squashing

### Plan for next week (9/12/2024 -- 15/12/2024)

- Get `uv` PR merged
- Start work on capturing representative workloads in ASV harness
- Continue background reading to understand MLIR/xDSL context
