# Meeting notes

The meeting notes (predominantly with co-supervisor and supervisor) for my
Masters project, in partial requirement for a Computer Science MPhil at the
University of Cambridge.

This document is prepend only except where otherwise stated, tracking the
summaries of meetings up to the current date of the project.

<!-- ====================================================================== -->

## Week 6 (6/1/2025 -- 12/1/2025)

### Co-supervisor meeting (9/1/2025)

#### Summary (9/1/2025)

- Roadmap
  1. Lexer -> solving interop/swapping/testing of swapping
  2. Code gen/parsing -> using interop with existing people's work for better gains
- Data structures in core.py / potentially stuff in irdl can be suitable as well

## Week 5 (6/1/2025 -- 12/1/2025)

### Co-supervisor meeting (9/1/2025)

#### Agenda (9/1/2025)

> What needs to be unblocked to win?

- [ ] ASV infra closing up
  - [ ] ASV unhappy with multiple Python versions in CI -- defer?
  - [ ] Could at some point invert submoduling, but best left till more stable?
  - [ ] What was the issue you wanted tracking? Benchmarks being added?

- [ ] Where to optimise/rewrite?
  - [ ] `program_n.mlir` demo shows parser
    - [ ] Had syntax error, so commented out line (might now do dead code eliminate not constant propagation?)
  - [ ] More complex/representative may differ
    - [ ] CIRCT example?

- [ ] Reviewing/picking further benchmarks from thread
  - [ ] Benchmarks so far
    - [ ] End-to-end `xdsl-opt` (more programs for scraped selection?)
    - [ ] Lexer and parser from existing benches (more programs for scraped selection?)
  - [ ] Printer
  - [ ] Loading dialects (is this just generic importing `xdsl.dialects.arith`?)
    - [ ] Should loading go alongside tests for those items?
  - [ ] Rewriting (compare `Builder`, `Rewriter`, and `PatternRewriter`)

#### Summary (9/1/2025)

- Work out what is the research direction, don't get too into the weeds with xDSL software engineering
  - Research questions below are sensible -- need to pick optimising python or backing onto faster language
- Running `mlir-opt` in CI is in wiki
- Lexing big part of dialect loading and easy to split out
- For (very) small files, dialect loading dominates -- this may fall over for CIRCT/ASL
- Switch to new dataclasses (attrs/NamedTuple) for arith (`xdsl/irdl/declarative_assembly_format.py`)
- Fix to `program_n.mlir` by switching to "func.op"
- Try both rust and zig for lexer, choose and how to distribute
- Look at networkx for understanding how backends change

### Supervisor meeting (TBD)

#### Agenda (TBD)

- [ ] Research questions
  1. What are the slow parts of the compiler stack?
  2. How can we make their Python implementation faster?
  3. What applications does this unblock?

- [ ] Plan for chapters in thesis (aligning with future plan)
  1. Introduction
  2. Background (Motivation + MLIR + Python performance)
  3. xDSL implementation (Summary of core features, useful as reference/documentation? Could be merged with background?)
  4. Benchmarking and profiling
  5. Performance optimisation
  6. Performance evaluation
  7. Applications
  8. Conclusion


<!-- ====================================================================== -->

## Weeks 3-4 (23/12/2024 -- 5/1/2025)

No co-supervisor meetings due to Christmas vacation.

<!-- ====================================================================== -->

## Week 2 (16/12/2024 --- 22/12/2024)

### Co-supervisor meeting (19/12/2024)

#### Agenda (19/12/2024)

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

#### Summary (19/12/2024)

- ASV GitHub pages and profiling infrastructure done!
- Both snakeviz and viztracer show different things (aggregate vs timeline of
  call stack)
- Add more micro-benchmarks isolating bits of code
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
    - Asynchronous is better
    - Video recording to Zulip could be a good mechanism?
- End vision is to lower xDSL through MLIR into an optimised executable
  - Python only used as DSL frontend, processing done in lower-level/faster
    languages
  - CPython optimisations less relevant for diminishing returns, since will be
    overwritten later
  - How does this project fit into this bigger vision?
    - Make a flow which lets us define everything in Python, but at runtime
      instead use something else
    - Profiling helps us find which bits of re-writing to focus on, as whole
      rewrite is not viable
    - Developing flow and infrastructure for doing rewriting is the key contribution

<!-- ====================================================================== -->

## Week 1 (9/12/2024 -- 15/12/2i024)

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
  - Pull from paper 1+1+1+1... and LLVM FileCheck examples along with new ones
- Consider Firefox profiler and perf to augment profiling
- Select Zig vs Rust for implementation, informed by which area is most
  bottlenecking (e.g., parser vs re-writes)
- Goal is to have an extra in the pip installation which transparently switches
  to use equivalent and faster backend

<!-- ====================================================================== -->

## Before project start

### Co-supervisor meeting (5/12/2024)

#### Agenda (5/12/2024)

- [x] Understanding the xDSL codebase
  - [x] What do real workloads look like?
    - [x] Do most people use as a command line tool from `xdsl_opt_main`?
    - [x] Or as library components in their own code?
    - [ ] Put example databases example into ASV (+fix ups to get it working)
  - [x] Chat over the codebase
- [x] Profilers
  - [x] SnakeViz/Scalene/…
- [x] Avenues of optimisation
  - [x] Data structure/algorithmic
  - [x] Bindings into other languages
  - [ ] Caching
    - [ ] Python too high-level to capture hardware, so would have to be between
      schedule search runs or similar
  - [x] Hardware-based optimisations (native/Numba/…?)
- [ ] Concerns
  - [x] What if there aren't opportunities for meaningful optimisation?
  - [ ] Directions to pivot?

#### Meeting summary (5/12/2024)

- Representative workloads
  - There are a number of big projects ongoing without large workloads
  - CIRCT is one of the few with big enough workloads you “can feel xDSL being
    slow”
  - xdsl-gui/marimo notebook, runs many times to show exploration, so also feels
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
  - Work log, tracking meeting notes and work done, tracked in git (add Sasha and
    Tobias as commenters to the latex/code repos)

### Supervisor meeting (13/11/2024)

#### Agenda (13/11/2024)

- [x] Admin
  - [x] Forms to fill in
    - [x] Course advisor and project supervisor
  - [x] Ethics/resource approval
    - [x] No ethics approval required since no human interaction, likely some amount of batch CPU time but <30,000hr limit by department
  - [x] How often to meet with you/Sasha
  - [x] Getting involved with research group activities
- [x] Proposal
  - [x] Summary of plan
        1. Performance profile xDSL for representative workloads (traditional + motivating)
        2. Implement performance optimisations along critical path of these workloads
        3. Measure performance of optimised code compared to original/other frameworks
        4. Assess the extent to which these optimisations unblock motivating workloads
  - [x] Aligning with research group motivation
  - [ ] Fine-tuning motivating workloads
    - [ ] Search space exploration for performant execution schedules (proposal)
    - [ ] Explore the space of all possible transformations, either passes or individual rewrites (Sasha's previous work)
    - [ ] Something more hardware co-design-y (SAIL mentioned by Sasha?)

#### Summary (13/11/2024)

- Forms to fill in
  - Send over on Zulip, with proposal draft
- How often to meet with Tobias/Sasha
  - Weekly with Sasha, monthly with Tobias
- Getting involved with research group activities
  - Thursday group meeting
  - Work in lab at free desk
  - Send weekly updates in Zulip channel
- Changes to plan
  - Optimisation in itself is a research effort
  - Can reduce focus on motivating workloads
  - Use micro-benchmarks from (LLVM Dev meeting - Vienna keynote, <https://www.youtube.com/watch?v=7qvVMUSxqz4>)
  - Consider how MLIR maps to xDSL, where do we pay the performance cost?
    - Plan this as research time in, in exchanged for assessing workloads
  - Possibility of forking CPython to drive changes which would help out writing compilers in Python

### Supervisor meeting (11/10/2024)

#### Agenda (11/10/2024)

- [x] Introductions
- [x] Python DSLs for High-Performance Linear Algebra Kernel Scheduling (prompt by Sasha)
- [x] Using information about how hardware can be designed to inform compiler optimisations (self-proposed)

#### Summary (11/10/2024)

- Pitched roofline project likely not a good fit
- Discussed Python DSLs project
  - [xDSL GitHub](https://github.com/xdslproject/xdsl)
  - Could do a project related to making it faster to be competitive with LLVM etc.
  - First stage profiling and comparing, then low-hanging fruit in performance, then stretch goal of PEP to CPython to facilitate specific performance goals
  - [LLVM speed](https://www.youtube.com/watch?v=7qvVMUSxqz4)
  - [PyFaster](https://github.com/xiaodaxia-2008/pyfaster)
- Walked around and met some people in the research group
- Joined the relevant (group + xDSL) Zulip organisations
