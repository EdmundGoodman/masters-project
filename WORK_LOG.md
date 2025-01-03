# Work log

The work log for my Masters project, in partial requirement for a Computer
Science MPhil at the University of Cambridge.

This document is prepend only except where otherwise stated, tracking the work
completed and summaries of meetings up to the current date of the project.

<!-- ====================================================================== -->

## Week 2 (16/12/2024 - 22/12/2024)

### Co-supervisor meeting (19/12/2024)

#### Agenda

- [x] ASV GitHub pages deployment
  - [x] <https://edmundgoodman.co.uk/xdsl-bench>
- [x] Work log/meta-repo sorted out
  - [x] <https://github.com/EdmundGoodman/masters-project>
- [x] Writing benchmarks
  - [x] Import opt/lexer done
  - [x] End-to-end for empty program and 1+1+1+1+….
    - [x] Running programmatically with opt runner
    - [x] Doesn't seem to actually be folding constants?
      - [x] Answer: `-p canonicalize` vs `-p fold-constant-interop`
- [x] Profiling benchmarks
  - [x] Set up snakeviz and viztracer on ASV benchmarks
- [x] Together constitutes fairly strong movement towards two goals of
  <https://github.com/xdslproject/xdsl/wiki/Performance>
- [x] Found book on CPython internals to read
  - [x] What can we get out of this before we move to total re-write of
      components?
- [x] Plan for next 2-3 weeks?
  - [ ] Deploy ASV infrastructure to organisation website
  - [x] Continue profiling, now with a focus on identifying/visualising hotspots
  - [x] Read about CPython internals
  - [x] Start trying to find/fix some simple performance defects?
- [x] Cancel next week's meeting (26th), decide whether to keep following week's
  meeting (2nd)

#### Summary

- ASV GitHub pages and profiling infrastructure done!
- Both snakeviz and viztracer show different things (aggregate vs timeline of
  call stack)
- Add more microbenchmarks isolating bits of code
  - Sent copy of 1+1+1+1 to us in benchmark
    - Need to use `-p canonicalize` instead of `-p fold-constant-interop`
  - Parser/rewriting/…
- Choose one end-to-end use case as “hero” use case to optimise
  - CIRCT
    - Used by real people
  - SAIL
    - Interesting but not quite ready
  - RISC-V snitch pipeline
    - bottom_up.mlir + test-lower-linalg-to-snitch pipeline from snitch paper
  - CSL stencil
    - Also has a pipeline
  - Compare how optimisations targeted at one workload impact others
  - Chat to Tobias/poll people in research group in new year to select
    - Asychronous is better
    - Video recording to Zulip could be a good mechanism?
- End vision is to lower xDSL through MLIR into an optimised executable
  - Python only used as DSL frontend, processing done in lower-level/faster
    languages
  - CPython optimisations less relevant for diminishing returns, since will be
    overwritten later
  - How does this project fit into this bigger vision?
    - Make a flow which lets us define everything in python, but at runtime
      instead use something else
    - Profiling helps us find which bits of re-writing to focus on, as whole
      rewrite is not viable
    - Developing flow and infrastructure for doing rewriting is the key
      contribution

### Work planned/completed (16/12/2024 - 22/12/2024)

- [x] Finish up ASV infrastructure for benchmarking
- [x] Create meta-repo repository containing deliverables, code, and work log
- [x] Write unit/integration benchmark workloads
- [ ] Explore other profilers

### Plan for next weeks (23/12/2024 - 30/12/2024)

- Deploy ASV infrastructure to organisation website
- Continue profiling, now with a focus on identifying/visualising hotspots
- Start trying to find/fix some simple performance defects
- Read about CPython internals

<!-- ====================================================================== -->

## Week 1 (9/12/2024 - 15/12/2024)

### Co-supervisor meeting (12/12/2024)

#### Agenda (12/12/2024)

- [x] Mostly been doing other literature review (albeit on ML compilers)
- [x] Finished up `uv` MR this week
  - [x] <https://github.com/xdslproject/xdsl/pull/3629#discussion_r1881750775>
  - [x] Dependabot looks to still be working, so we might be able to disable
    update-bot workflow to avoid extra complexity?
- [x] Had a look at ASV as submodule
  - [x] Slightly annoying to set up as opposed to in repo
  - [x] Would inversion to submodule out benchmarks be sufficient?
- [x] Representative workloads
  - [x] `xdsl_opt_main` + files?
  - [x] Tighter motivation with CIRCT/SAIL?
- [x] Idea: Standalone tool for characterising within xDSL where the performance
  bottleneck is
  - [x] Selling point of xDSL in traceability for workloads when developing
    (light wrapper around viztracer which only captures selected functions)
  - [ ] Move towards co-design focus?

#### Summary (12/12/2024)

- Aim to sort out ASV submodule stuff today to unblock benchmarking
- Come up with unit/integration benchmark workloads
  - Pull from paper 1+1+1+1... and LLVM filecheck examples along with new ones
- Consider Firefox profiler and perf to augment profiling
- Select Zig vs Rust for implementation, informed by which area is most
  bottlenecking (e.g. parser vs re-writes)
- Goal is to have an extra in the pip installation which transparently switches
  to use equivalent and faster backend

### Work planned/completed (9/12/2024 - 15/12/2024)

- [x] Mostly focussed on literature review on ML compilers
  (Halide/TVM/IREE/tensat+egg/...) for another module, but has been helpful
  background for the project
- [x] `uv` PR for xDSL merged
- [ ]  Started prototyping separate repo for ASV publishing xDSL benchmarks --
  encountered some peculiarities with installation of submodule

### Plan for next week (16/12/2024 - 22/12/2024)

- Finish up ASV infrastructure for benchmarking
- Write unit/integration benchmark workloads
- Explore other profilers

<!-- ====================================================================== -->

## Before project start

### Co-supervisor meeting (5/12/2024)

#### Agenda (5/12/2024)

- [x] Understanding the xDSL codebase
  - [x] What do real workloads look like?
    - [x] Do most people use as a command line tool from `xdsl_opt_main`?
    - [x] Or as library components in their own code?
    - [ ] Put example databases example into ASV (+fixups to get it working)
  - [x] Chat over the codebase
- [x] Profilers
  - [x] SnakeViz/Scalene/…
- [x] Avenues of optimisation
  - [x] Data structure/algorithmic
  - [x] Bindings into other languages
  - [ ] Caching
    - [ ] Python too high-level to capture hardware, so would have to be between
      schedule search runs or similar
  - [x] Hardware-based optimisations (native/numba/…?)
- [ ] Concerns
  - [x] What if there aren't opportunities for meaningful optimisation?
  - [ ] Directions to pivot?

#### Meeting summary (5/12/2024)

- Representative workloads
  - There are a number of big projects ongoing without large workloads
  - CIRCT is one of the few with big enough workloads you “can feel xDSL being
    slow”
  - Xdsl-gui/marimo notebook, runs many times to show exploration, so also feels
    slow
  - SAIL stuff upcoming, similar to CIRCT and will have large workloads
- Walked through `ir/core.py` code file to
- Next steps:
  - [ASV](https://asv.readthedocs.io/en/latest/) benchmarking harness
    1. Separate repo in xDSL GitHub org
    2. xDSL as submodule then run daily with cron GitHub action
    3. Add published site to xDSL website
  - Find workloads for ASV
  - Traces / statistics to do with workloads
    - Do a number of passes, which proportions are verification/parsing/...
  - Main approach is to find things which are both slow and have fixed contract,
    then swap out with faster implementation
    - Performance matters talks
    - `core.py` and `rewriter.py`likely candidates
    - Make sure all rewrites use the rewriter API, otherwise problems with
      changing types and attributes directly
  - Compare with PyPy / Nuitka naively
  - Ignore parallelism for now
  - Worklog, tracking meeting notes and work done, tracked in git (add Sasha and
    Tobias as commenters to the latex/code repos)

### Work planned/completed (before project start)

- [x] MPhil project approved by admin starting Monday 2nd December
- [x] Kickoff meeting with Sasha (and Chris at beginning)
- [ ] Merge `uv` PR for xDSL - merge conflicts resolved, but that revealed a
  couple more issues which need squashing

### Plan for next week (9/12/2024 - 15/12/2024)

- Get `uv` PR merged
- Start work on capturing representative workloads in ASV harness
- Continue background reading to understand MLIR/xDSL context
