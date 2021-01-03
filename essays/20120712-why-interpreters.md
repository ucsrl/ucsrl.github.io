## Pro-Prologue, 2021-01-03
The following essay has been on my homepage whilst being a postdoctoral scholar at University of California, Irvine. 
Some of the data is outdated, some of the systems mentioned, too. 
The affiliations of the early reviewers who provided valuable comments may be out of date, as well.
I am putting this version online for historical reasons---the general point is, however, still valid.

**Slightly edited version of the original essay**

### Prologue
For the past years I have always wondered, why interpretation has such a bad
reputation.
In most discussions, people are usually quick to point out that interpreters are just
too slow and therefore implementers should not pay any attention to them.
There are, however, clear benefits of having an interpreter---benefits that get lost
when implementing a dynamic compilation subsystem.
There are questions I ask myself when considering system implementation, and this essay
represents my subjective point-of-view of the design space of interpretation
vs. just-in-time compilation and the involved trade-offs.
Initially, I thought that the arguments would hold true for interpreters in general,
however, after getting many insightful comments from some of my colleagues and careful
consideration, I think it is best to limit the scope of this essay to the subdomain of
implementing high level programming languages, such as Python, Ruby, JavaScript, etc.
It would be very interesting to evaluate the design space and trade-offs for lower level
languages, such as Java, and give a complete account of the status-quo in
interpretation---unfortunately, I currently do not have time to undertake such a
resource-intensive endeavor.

# Why interpreters matter (at least for high-level programming languages)

In 1984 Peter Deutsch and Alan Schiffman published their landmark paper on the efficient
implementation of the Smalltalk-80 system.
Reading this paper is highly recommended, since it introduces several ideas for
efficiently implementing systems, dynamic compilation being the key issue.
Substantial research in this area followed, ultimately culminating in today's
high-performance Java virtual machines, JavaScript virtual machines, etc.
For an excellent writeup on the history of just-in-time compilation, please read the
corresponding [paper by John Aycock](http://www.cs.tufts.edu/comp/150CMP/papers/aycock03jit.pdf).

[David Ungar](http://en.wikipedia.org/wiki/David_Ungar) mentions that once he
got a taste of dynamic translation, he never bothered to squeeze smaller performance
improvements out of the interpreters
([source](http://forum.world.st/RoarVM-The-Manycore-SqueakVM-td3025321i20.html#a3030523)).
This is somewhat contrary to Mike Pall's [<a href="#mpall">1</a>] experience, quoting
from ([source]("http://lambda-the-ultimate.org/node/3851#comment-57646")):

>  [...] It's much easier to record what an interpreter is doing. Just patch its
>  dispatch table and intercept every instruction.  The small gains of a simplistic
>  compiler over a carefully handoptimized interpreter are just not worth the trouble
>  (the LJ1 JIT compiler is not much faster than the LJ2 interpreter, sometimes it's
>  worse. [...]

In general, people seem to ignore interpreters and their performance and I think this is a
mistake.
This essay's intent is to provide points arguing in favor of interpreters and improving
their performance---that seems to eclipse the public's opinion and is worthwhile to be
pointed out.
Interpreters inhabit an interesting sweet-spot on the performance-price curve.
This is primarily due to their ease-of-implementation.
The implementation costs for an interpreter pale in comparison to the ones required by
implementing an optimizing native code compiler.
Furthermore, interpreters can be implemented in a highly portable manner
[<a href="#hsinterp">3</a>], so that we are able to port an interpreter to a new
architecture in a matter of days.
This, too, is diametrically opposite to the necessity of building a new backend for a
compiler (i.e., we take the separation of frontend-backend for granted, since this is a
best-practice for compiler implementation.)
Summing up, this gives a "big bang for the buck": with only little effort (my personal
estimate is a matter of weeks) you get a portable execution environment.

Besides drastically reduced implementation costs for an interpreter, there is another secondary
effect: reduced maintenance costs.
In order to have comparable portability, a compiler needs a
backend for each target architecture.
Therefore, there is much more surface available for sometimes hard to find and resolve
bugs. NB: The exact same argument holds for just-in-time compilers.


### The price of using a just-in-time compiler vs. an interpreter.


The previous paragraphs present the rather well-known pro-interpreter arguments.
It turns out, however, that there are several others that seem to not find their way
into public discussions.
Any dynamic compilation subsystem trades off space for time.
They use additional space to hold generated native machine code, and speedup subsequent
execution by handing control over to the optimized representation.
In consequence, a JIT-compiler gives you improved performance at the expense of memory
[<a href="#memory">4</a>].

The following examples illustrate this point:

Dimension  | Unladden Swallow | PyPy 1.8
---------- | -------- | -------------------
*Speedup* |  |
  fastest | 2.86x (`nbody` on a 64 bit)              | 10.6490x `nbody` (arg: [50k, 100k])
  slowest | 0.90x (`spambayes` on a 32 bit)          | 1.6679x `binarytrees` (arg: [10, 12])
*Memory consumption* | 
  highest | 7.92x (~85 MiB) (`spambayes` 64 bit)     | 7.2199x (~49 MiB) on `binarytrees` (arg: 12)
  lowest  | 1.11x (~148 MiB) (`slowspitfire` 64 bit) | 2.8165x (~19 MiB) on `nbody` (arg: 100k)

In addition to this trading off space for time, whenever we look at concrete performance results, we
find that there always are some benchmarks which benefit much more from just-in-time compilation
than others.
For example, if we take a look at the `mandelbrot` benchmark from the computer language benchmarks
game, we see that it consists of simple computations with almost trivial control flow.
This is a kind of program that benefits enormously from JIT compilation, as the JIT compiler is able
to remove all kinds of overheads associated with executing the program in the regular interpreter,
such as removing the overhead of dynamic typing, repeated (un-)boxing and removing all reference
count operations (i.e., all the increment and decrement operations.)
If, for instance, the benchmark program is not that amenable to JIT compilation, e.g.,
by having to use a data type that has no corresponding native machine equivalent (such as
a complex number) and can therefore not be operated on by native machine operations
(e.g., a single addition assembly instruction), the achievable speedup potential is much
more limited. [<a href="#libs">7</a>]
Here, for concrete examples we can take a look at the `binarytrees`
and `spectralnorm` benchmarks of the computer language benchmarks game:

Dimension | PyPy 1.8 | Optimized Interpreter
--------- | -------- | ----------------------
*Binarytrees* | | 
Speedup   | 1.6679x | 1.8050x
Memory    | ~49 MiB | ~7 MiB 
*Spectralnorm* | | 
Speedup   | 2.9707x | 2.2528x
Memory    | ~24 MiB | ~5 MiB

To put these numbers into context, an optimized interpreter using advanced purely
interpretative optimization techniques can have better performance (for
`binarytrees`), or cover a lot of the distance of the compiled code, i.e.,
relative to an optimized interpreter for `spectralnorm`, the JIT compiler is
"only" 32% faster (speedup by a factor of 1.3187.)
At the same time, we see the impact JIT compilation has on memory consumption, which is
roughly a factor of 7x for `binarytrees` and almost 5x for
`spectralnorm`. [<a href="#java">8</a>]


This essay's intent is not to talk down the importance of just-in-time compilers or the highly
relevant insights gained from the substantial research efforts going into just-in-time
compilers.
This could not be further from the truth, as I believe that if performance is the
ultimate goal for a virtual machine, there is only so much an interpreter can do for you.
There are, however, other scenarios where the trade-offs involved might have adversarial
effects.

Let's take a look at the current state of affairs for web applications.
If, for a given set of available hardware we can have N concurrent connections, then
additional memory requirements might reduce the amount of concurrent connections by the
corresponding overhead; for the sake of argument let's just use the 2.8x minimum
additional memory requirements from PyPy 1.8.
Switching from CPython 3.3a0 to PyPy 1.8 reduces the concurrent connections from N to
N/2.8, or if we wanted to keep concurrent connections constant, we would have to add 2.8
times more memory to these machines.
That is, however, only one side of the medal.
If your Python application is amenable to JIT compilation, i.e., achieves performance
speedups in the 11x area, the additional memory requirements might be offset by the
additional gains in throughput (not factoring in network or IO latencies.)
[<a href="#webcmp">9</a>, <a href="#llwa">10</a>]

Finally, in a mixed-mode execution environment, where a virtual machine starts interpreting programs
and only performs JIT compilation once it detects that pieces of code are hot, i.e., much more
frequently executed.
For such a virtual machine, interpreter performance is still interesting, as compilation is driven
by heuristics, such as function-size.
If a function is too long, it might never be compiled and always interpreted instead.
The same holds true for deoptimization purposes: if speculatively compiled code is
incorrect, the dynamic compilation subsystem restores interpreter state and hands
control over to the interpreter.
A similar situation exists for trace-based JIT compilation, where one would use an
interpreter to record traces before actually compiling them.

An alternative to mixed-mode execution environments are actual just-in-time compilation
virtual machines, such as the Google's V8 or Oracle Labs' Maxine virtual machine.
In those systems, a so-called template JIT generates native machine code for each
function/method it encounters, i.e., the code is never interpreted.
Consequently, this strategy requires more memory, as it generates native machine code
for all functions directly.
It would be interesting to give figures; unfortunately, I do not have any.


Summing up, I hope this essay dispenses the usual argument that interpreters are just
slow and using a dynamic compilation subsystem is *always* better (i.e., unconditionally).
There is indeed a sweet-spot for interpreters on the performance/price curve and
interpretation frequently offers the following benefits:
- ease of implementation, proportional maintenance costs,
- portability,
- low latency,
- space efficiency,
- security aspects (for example:
  - not affected by security implications of writable code pages (cf. W-XOR-X,
    JIT spraying),
  - easier to verify, e.g., using abstract-state machines),
- an important component in a mixed-mode execution environment (for example for
              trace recording, or in deoptimization scenarios)


If, however, performance is your ultimate goal, you usually have to trade off either one or some
combination of these characteristics when deciding to implement a just-in-time compiler.
In closing, I want to say that I think combining fast interpreters with just-in-time
compilers is mutually beneficial, e.g., if you have a fast interpreter, you might end up
delaying code generation or leverage additional information gathered by the interpreter
to generate more aggressive---therefore faster---machine code directly.
The design space in construction of dynamic compilation subsystems is rather large and
there is no systematic discussion on pros/cons for either approach.
Which approach works best usually depends on concrete system configuration and the
program/application at hand.
Therefore, it seems that the design space for virtual machines is too large to be able
to have a one-size-fits-all solution for all programs/configurations.


### Acknowledgments
Thanks to Per Larsen (University of California, Irvine), Christian Wimmer and Mason
Chang (Oracle Labs), and Michael Bebenita (Mozilla Research) for extensive feedback,
discussions and additional comments.

### Disclosure
I have been working on optimizing interpreters, specifically the Python 3.x
interpreters, and while I use results of that work (reported as "optimized
interpreter"), it is at no point my intention to advocate my own work.


## References

### Mike Pall
Mike Pall is the implementer of the LuaJIT system. The quote comes from his
discussion of LuaJIT 2, which is a tracing just-in-time compiler plus interpreter,
i.e., a mixed-mode virtual machine.

### Baseline
Christian Wimmer mentioned that this is primarily an argument against baseline
compilers (such as template JITs), and not against optimizing JIT compilers. This
seems plausible (though I have not looked at the LuaJIT 1 source code), but I
believe it does not affect the argument for interpreters and their optimization.

### hsinterp
Christian Wimmer pointed out that heavily optimized interpreters, such as the one in
Sun’s HotSpot JVM, are not portable, as it consists of hand-tuned assembly
instructions. This is obviously true. However, an interesting question is, whether
porting these hand-written interpreters is compares favorably with the effort
necessary to create a new backend.
### Memory

I received some comments on the abundance of memory in most devices which weakens
the argument, since memory clearly cannot be an important consideration when you
have enough of it. My personal view on this matter is, however, different: Having
more memory translates to higher energy requirements. The relevant question is: What
requires more energy, the interpreter or powering the additional memory? (I have not
yet found a paper with a satisfactory answer.)


### GCeval
This is a pure black-box evaluation, i.e., it is entirely possible that some of the
overheads correspond to differences in memory management (e.g., generational garbage
collector vs. reference counting, which is well known to be space-efficient.)
While this is true for all numbers, I think it is still illustrative, because a JIT
compiler usually requires a more "spacious" memory management technique, i.e., I
have never seen a dynamic compilation subsystem using immediate reference
counting. In addition, there are other factors in the mix, such as difference in
object layout, libraries, internals (such as different String representation, etc.)
Since I do not have corresponding data available, I cannot quantify how these
factors skew the results in either direction.

Furthermore, my evaluation uses a modified nanobench testing program, i.e., timing
includes warm-up times and JIT compilation times. Therefore, because some of the
arguments are rather small, it might very well be that the benefits of running JIT
compiled code are probably not always visible. This is, however, irrelevant, as the
numbers primarily demonstrate memory consumption.


### JITs

It is very well possible that with the arguments chosen, the benefits of JIT
compilation cannot be reaped accordingly, e.g., the profiling plus code generation
takes longer (latency) than the execution in the interpreter. In fact, for
binarytrees I did a quick check on my machine with the argument 16 and found that
PyPy 1.8 is 2.6 times faster than the optimized interpreter (just using the “time”
command line utility), for the argument 500 spectralnorm is about 1.74 times faster
than the optimized interpreter (again using the “time” program.) It goes without
saying that I did not select arguments to display this effect intentionally. In
fact, I think this could be an indicator for applications where JITing is strictly
superior to interpreting (such as long running desktop software.)

### Libs
Michael Bebenita pointed out that this essentially reduces to calling a library
function to implement the functionality. It is true that an interpreter would have
to do this, too, but if most of the computational time is spent in library code that
is not inline-able by a just-in-time compiler, the corresponding optimization
potential is lower and therefore interpretation and just-in-time compilation seem
"equal" w.r.t. achievable performance.

### Java
Comparable data for Java-based systems would be very interesting for comparison
purposes. Unfortunately, no such data are available to me but would be highly
relevant and surely interesting.

Furthermore, my data collection cannot pinpoint whether the overhead is due to the
actual JIT compiler caching code, or due to overhead in the memory subsystem.
People may argue that for the JIT compiler to unfold its performance potential it
cannot use reference counting for memory management and therefore the JIT compiler
uses more memory, e.g., to accommodate multiple generations, etc.
A direct consequence of this line of thinking is that if I valued space efficiency
over performance, an interpreter using reference counting might be beat any dynamic
compilation subsystem.

### Webcmp

Christian Wimmer remarked that for Java like systems the memory consumption of a
JITed system might very well be lower than for an interpreter, primarily due to
beneficial effects of inlining and escape analysis. In addition, compiled code
scales better than interpreted code and additional memory consumption is only
necessary during compilation. These are reasonable remarks, and I cannot comment on
the situation for dynamically typed programming languages used in that scenario (as
I have no comparable data.)
### LLwa
Mason Chang mentioned that the big players in the field (Google, Facebook, etc.)
could not care less about the complexity and money involved, as the performance
improvement translates directly into benefits. This is certainly true, on the other
hand I believe that there are many companies who cannot rely on the same economics.