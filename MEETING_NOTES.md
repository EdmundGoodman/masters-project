# Meeting notes

The meeting notes (predominantly with co-supervisor and supervisor) for my
Masters project, in partial requirement for a Computer Science MPhil at the
University of Cambridge.

This document is prepend only except where otherwise stated, tracking the
summaries of meetings up to the current date of the project.



## Week 13 (3/3/2025 -- 9/3/2025)

### Co-supervisor meeting (6/3/2025)

#### Initial

- Planning for optimisation
  - Worried about getting stuck benchmarking/profiling till very late
    - This is only the first section for two/three in my thesis
  - Already slightly behind proposed timeline which is fine but not ideal
  - Part of our assessed work is "one-minute madness" presentation
    - Would like to have at least a plan more interesting than just benchmarking
      Python and simple engineering improvements to talk about there in two
      weeks time

  - Conversation about rewriting approach
    - I like it not only because it is cool but because has a very clear
      approach and goal
    - Arguable differences with my understanding of Mathieu's work
      - Different language: Mojo != Python
      - Different framework: MLIR != xDSL
      - **Different context: To specifically address problems in dynamic languages**
    - Happy to take Mathieu's work as existing work and the contribution is
      applying it as a mechanism to solve issues in dynamic runtimes?
    - Is it worth having another conversation/message with Tobias suggesting
      the work plan and how it differs to open up conversation with Mathieu?
    - There is an existing (rejected) PEP for it <https://peps.python.org/pep-0511/>
      - Implement this-ish outside the CPython runtime

  - If this is not possible, I have a different crazy idea:
    - ``Bring your own bytecode: low-overhead custom microkernels for performant python''
    - tl;dr define your own custom bytecode instructions, use the copy-patch JIT
      to substitute them as stencils
    - This will probably be a big pain to implement
    - Angle is interpreted languages using copy-patch JITs (rather than dynamic)

- Benchmarking stuff
  - Stared at trace and came up with microbenchmarks of salient bits in
    constant folding
  - Starting to implement these microbenchmarks. May add more fine-grained ones
    once done/looking at bytecode
  - Started writing new `bench_utils` tool to examine bytecode
    - More annoying than it seems since `dis` doesn't follow function calls
    - With magic can get most of the call stack disassembled from a trace
    - Would be nice to get a flat list of bytecode instructions being run?

  - EuroLLVM

#### Trimmed

1. Planning for optimisation approaches
   - Would like to have a concrete plan soon
     - Worried about getting stuck benchmarking till very late in project
     - Part of our assessed work is "one-minute madness" presentation
       - Would like to have something more than just benchmarking Python and simple engineering improvements to talk about for dry run in two weeks time

   - Have spent the past week thinking about this, culminating in the two following possible approaches:

   - Conversation about bytecode rewriting approach
     - I understand if it is just not possible, but want to completely close whether or not it is
     - I like the idea because it has a very clear work plan and goal
     - Differences with my limited (may need correction) understanding of Mathieu's existing work
       - Different language: Mojo != Python
       - Different framework: MLIR != xDSL
       - Different context: To specifically address problems in dynamic languages
     - Happy to take Mathieu's work as existing work rather than overlapping
       - The contribution is then applying it as a mechanism to solve issues in dynamic runtimes (unlike Mojo)
     - Is it worth having another conversation/message with Tobias suggesting the work plan and how it differs to open up conversation with Mathieu?

   - If this is not possible, I have a different possible idea:
     - "Bring your own bytecode: low-overhead custom microkernels for performant python"
     - tl;dr define your own custom bytecode instructions, use the copy-patch JIT to substitute them as stencils
     - Extends from wanting to define the two most helpful new bytecode instructions
     - This might be difficult to implement in CPython?

2. Progress on benchmarking
   - Have identified microbenchmarks from constant folding
   - In progress implementing these microbenchmarks
   - Wrote tool wrapping `dis` to examine bytecode for an entire call stack

3. EuroLLVM?
   - Mentioned in lab meeting, deadline March 12th
   - Is it worth asking about funding to go?

<!-- ====================================================================== -->


## Week 12 (24/2/2025 -- 2/3/2025)

### Supervisor meeting (24/2/2025)

- Summary of xDSL benchmarking
  - End-to-end benchmarks
    - Constant folding traces
    - Python version component comparison graphs (11/13/13jit/pypy)
    - **Parsing/lexing very bad** (example scales very poorly for large dense attr)
    - More complex re-writing workloads might show different results? CIRCT?
  - Microbenchmarks
    - Python ~50x slower than MLIR
  - Import machinery in the order of milliseconds
- Python optimisation opportunities
  - Proposal in abstract -- bytecode rewriting based on domain-specific information
  - ...
- What is the work plan?
  - Would like to get started on optimisation stuff if possible


```
Sasha Lopoukhine: Rough meeting notes:

fixed vs steady state cost of iterating over a block (creating blockops takes time, and block ops iterator)

- maybe see intercept here?
- maybe use slots?
- maybe remove dataclass annotation?
try to replicate MLIR numbers from presentation locally
timeit has function call overhead vs executing the statement in a loop
pinning on a core in macOS
clone is closest to MLIR times, might be worth looking at first

Edmund Goodman:
IsTerminator / NoTerminator instead define new operation and new traits for ubenchmarks
Further synthetic benchmarks (fmadd, dce, ...) preferable than trying to collect many workloads to be representative or just picking few random non-representative workloads
mlir-opt FILENAME --mlir-timing --canonicalizecan be used for direct measurement of rewriting phase (ignoring parsing/lexing as the uninteresting bits of the compiler)
```






<!-- ====================================================================== -->

## Week 11 (17/2/2025 -- 23/2/2025)

### Thoughts

- Value of AST optimisations? Could always be done by developer in code but
  retain expressiveness at cost of understandability?
  - *Unless we have more information from being at runtime...*
- Trivial example of automatic common rewrite like constant folding as decorator
  - Might look like `LOAD_FAST name` -> `LOAD_CONST num`?
  - Would need to run at runtime to have enough information to be interesting
- Generated bytecode can be ingested by PyLIR/PyPy/CPython
- Does the JIT compiler typically run in parallel to program flow? Could it/our
  optimiser?
- Note arguably no longer really a JIT, instead optimising bytecode rewrites
  either on the fly or during build
- If want/need to PR CPython, likely would be looking at adding new bytecode
  instructions to add capabilities
  - Perhaps something like trapdoor-ing out to other binaries?
  - Or some instruction to unblock optimisation like LOAD_CONST for strings etc.

### Background and related work headings

- Background
  - Compilation
    - LLVM and SSA
    - MLIR
    - IRDL
    - xDSL
  - Differences between dynamic and compiled languages
  - JIT compilation
    - Bridging the dynamic/static gap with JITs
    - Modern JIT structure
    - Copy-and-patch compilation
- Related work
  - Python JITs
    - Numba = non-dynamic DSL
    - Pypy = general not accounting for program invariants
    - CPython3.13 = baseline JIT not yet mature
  - Java JITs using code invariants
    - Lancet/surgical precision paper
  - Other python optimisation approaches
    - Specialising adaptive interpreter
    - pyfaster stuff in the pipeline
  - Python + MLIR
    - PyAST and Nelli
      - "Wrong direction" -- Lowering to MLIR and hence losing dynamic properties
    - PyLIR
      - At its core still just a runtime for python, but this time in MLIR
        instructions -- constrained by dynamic semantics

### Abstract drafting session (18/02/2025)

#### Title and abstract

**Bringing domain-specific knowledge to the dynamic language runtime**

//  1. Introduction. In one sentence, what's the topic?
The indirect nature of dynamic languages allows for changes to programs at runtime, providing greater flexibility and faster start-up times, but presents an optimization boundary for the language implementation.
//  2. State the problem you tackle.
API boundaries within dynamic languages may limit their dynamic nature by enforcing invariants and constraints at runtime, which could be leveraged for optimization.
//  3. Summarize (in one sentence) why nobody else has adequately answered the research question yet.
Previous approaches have addressed this by either tracing program execution and discovering these invariants, which incurs a runtime cost, or by using a non-dynamic DSL, which reduces flexibility.
//  4. Explain, in one sentence, how you tackled the research question.
We provide a user-extensible compiler from the Python AST to bytecode, allowing developers to bring their own optimization passes specific to their domain.
//  5. In one sentence, how did you go about doing the research that follows from your big idea.
We evaluate this by applying our performance optimizations to the xDSL compiler framework, demonstrating that it can scale to handle previously infeasible applications.
//  6. As a single sentence, what's the key impact of your research?
Our approach unblocks the use of Python for previously performance-bounded workloads whilst retaining its desirable dynamic properties.

#### Work plan

- [ ] Write trick to only attempt AST subtrees supported by our AST dialect
- [ ] Write AST dialect
- [ ] Write Python frontend to AST dialect (for some subset)
      - https://github.com/Pylir/Pylir/blob/006e47e5694fa93601c5c7c0f06da9e0bbfccdab/src/pylir/Optimizer/PylirPy/IR/PylirPyOps.td
- [ ] Write bytecode dialect
- [ ] Write lowering pass from AST to bytecode dialects
- [ ] Write printer from bytecode dialect to raw bytecode
- [ ] Write optimizations within dialects
- [ ] Apply either at build or runtime
- [ ] Numeric toy example optimiser
- [ ] Write xDSL optimiser

#### Contributions

1. We introduce a novel approach to improve the performance of dynamic languages
   whilst retaining the benefits of flexibility and fast start-up times by
   allowing developers to define custom optimisation passes informed by
   domain-specific knowledge
2. We present a concrete implementation of this approach by providing two new
   xDSL dialects for Python's AST and bytecode, against which developers can use
   the existing xDSL infrastructure to design domain-specific optimisation
   passes
3. We evaluate the effectiveness of this approach by applying it back to the
   xDSL compiler framework itself, a large Python codebase which has
   domain-specific constraints and whose scalability relies on performant
   execution

### xDSL weekly meeting (18/2/2025)

- Benchmarks
  - What have we got so far?
    - Benchmarks at component, end-to-end, and microbenchmark (contrast
      "How slow is MLIR?") levels
    - Utility to profile any benchmark
    - Summary of results
      - Differs by workload and machine heavily
      - Lexing about 1/3rd of parsing (final answer)
      - High cost to import machinery and setting up dialect dataclasses
      - High cost to pattern rewriting and verification
  - What do we need to decide?
    - Where should benchmark code live?
      - `benchmarks/` directory?
    - Where should we install/configure air speed velocity?
      - If `xdsl-bench` may need some other mechanism for developers to run?
        - <https://github.com/tonybaloney/rich-bench>?
    - Where should we install profiling dependencies?
      - As extra group in `xdsl`, since used by developers?
    - Where should infrastructure/artefacts live?
      - `xdsl-bench`.

### Thoughts (17/2/2025)

- Two possible optimisation mechanisms
  1. Extend recent CPython JIT to add further optimisations
     - Key differentiator might be mechanism to bring down invariants
     - Specific to CPython, damaged by performance gains from PyPy in profiling
  2. Bytecode rewriting workload using xDSL
     - Key idea #1: "Best effort" parse AST, only optimise bits we can handle,
       then recombine with rest at end to handle all of Python syntax
     - Key idea #2: As with (1) bring invariants down, but in this case handle
       them in a custom xDSL rewrite pass in addition to other optimisations
     - Workflow: subset of python -(frontend)-> xDSL -(optimisation passes)->
       optimised xDSL -(lowering pass/codegen)-> Python bytecode/machine code
     - This is not a JIT! We would want to do this at library build time.
       Would need to change title?
     - PyAST is really cool but quite limited (although may be enough?), but
       could instead consider optionally binding something like
       <https://github.com/makslevental/mlir-python-extras> into xDSL instead?
- In both cases need to pick invariants for use cases
  - Lexer could be regex, but less desirable to optimise as "hero"
  - PDL / pattern rewriter less clear
- Ready to set up meeting with Tobias? Would like to do one with a bit of a
  gap before mid-project review in March
  - Benchmarks summary of results ready to go
  - Final draft abstract
  - Plan for implementation of in-python optimiser

<!-- ====================================================================== -->

## Week 10 (10/2/2025 -- 16/2/2025)

### Agenda (13/2/2025)

```
python ast -> box[ -> mlir (dialects)] -> bytecode/llvmite/wasm

(read and understand)
(subset of python ast)
(use existing implementation from frontend)
(fix typing from i32 to I32)
(tests for free)
(discard )

pdl is a good target for the JIT since big python loop
 -> other things also possible, need to be benchmark driven
pattern rewriter
```


#### Title and abstract draft v2

```text
# Compiling the compiler: a user-extensible Python JIT for the optimisation of xDSL

//  1. Introduction. In one sentence, what's the topic?
Dynamic programming languages such as Python offer great flexibility, but
commensurately sacrifice performance as this flexibility precludes many common
compiler optimisations such as function inlining.

//  2. State the problem you tackle.
We aim to improve the performance characteristics of Python for
domain-specific applications by providing a mechanism to bring invariants from
the domain to reveal optimisations in the JIT.

//  3. Summarize (in one sentence) why nobody else has adequately answered the research question yet.
CPython's JIT is a recent development, introduced in 2024 by PEP774, and as such
does not have mature techniques for specialisation using domain-specific
information.

//  4. Explain, in one sentence, how you tackled the research question.
~~We approach this by developing new optimisations for CPython's JIT, and examine
the possibility of direct peephole optimisation of bytecode informed by domain
invariants.~~
TODO: We approach this by [either new JIT optimisations or bytecode rewriting]...

//  5. In one sentence, how did you go about doing the research that follows from your big idea.
TODO: Cannot answer until research complete...

//  6. As a single sentence, what's the key impact of your research?
Our approach unblocks the use of Python for performance-bounded workloads, as
demonstrated by scaling the xDSL compiler framework to previously infeasible
applications such as large PDL rewrites and CIRCT.
```

## Week 9 (3/2/2025 -- 9/2/2025)

### Agenda (6/2/2025)

- [ ] Research questions -> goals
  1. What are the slow parts of the compiler stack?
        -> Benchmark xDSL
  2. How can we make their Python implementation faster?
        -> ~~Draw interface and move to low-level language~~
        -> New optimisations in Python JIT and bytecode generation
  3. What applications does this unblock?
        -> Apply to ASL/CIRCT?

- [ ] Plan for chapters in thesis (aligning with future plan)
  1. Introduction
  2. Background (Motivation + MLIR + Python performance)
  3. xDSL implementation (Summary of core features, useful as reference/documentation? Could be merged with background?)
  4. Benchmarking and profiling
  5. Performance optimisation
  6. Performance evaluation
  7. Applications
  8. Conclusion

#### Title and abstract draft v1

```text
# A user-extensible Python JIT

//  1. Introduction. In one sentence, what's the topic?
Dynamic programming languages such as Python offer great flexibility, but
commensurately sacrifice performance as this flexibility precludes many common
compiler optimisations such as function inlining.

//  2. State the problem you tackle.
We aim to improve the performance characteristics of Python for
domain-specific applications by providing a mechanism to bring invariants from
the domain to allow optimisations in the JIT.

//  3. Summarize (in one sentence) why nobody else has adequately answered the research question yet.
CPython's JIT is a recent development, proposed in 2024 by PEP774, and as such
does not have mature techniques for specialisation using domain-specific
information.

//  4. Explain, in one sentence, how you tackled the research question.
We approach this by developing new optimisations for CPython's JIT, and examine
the possibility of direct peephole optimisation of bytecode informed by domain
invariants.

//  5. In one sentence, how did you go about doing the research that follows from your big idea.

//  6. As a single sentence, what's the key impact of your research?
By developing user-extensible optimisations for the Python JIT, we unblock the
use of Python for performance-bounded workloads, as demonstrated by the
optimisation of the xDSL compiler framework.
```

<!-- ====================================================================== -->

## Week 8 (27/1/2025 -- 2/2/2025)

<!-- ====================================================================== -->

## Week 7 (20/1/2025 -- 26/1/2025)

<!-- ====================================================================== -->

## Week 6 (13/1/2025 -- 19/1/2025)

### Co-supervisor meeting (17/1/2025)

#### Summary (17/1/2025)

- Roadmap
  1. Lexer -> solving interop/swapping/testing of swapping
  2. Code gen/parsing -> using interop with existing people's work for better gains
- Data structures in core.py / potentially stuff in irdl can be suitable as well

<!-- ====================================================================== -->

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
