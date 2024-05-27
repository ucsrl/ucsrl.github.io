# Welcome to the official blog of [μCSRL](https://www.unibw.de/ucsrl-en)!

The [Munich Computer System Research Laboratory](https://www.unibw.de/ucsrl-en), μCSRL for short, is a research lab at the [National Cyber Defense Research Institute CODE](https://www.unibw.de/code) at the [Universität der Bundeswehr, München](https://www.unibw.de), directed by Stefan Brunthaler.
This web page hosts all public artifacts of the research group, including blog posts, essays, source code, and tools.

## Research
As of Q1 2024, μCSRL conducts the following research projects.
We have been quite active over the past couple of years and thus expect to publish multiple papers in 2024.


### *Fuzzing*: μ-fuzz
Bundles our research activities in automated vulnerability identification via *fuzzing*.
Our objective for this project is the investigation of combinatorial optimization of fuzzing on clusters.
To support this project, we have a state-of-the-art fuzzing cluster with 1,200+ CPUs.

### *Language-based Security -- Software Diversity*: μ-proteus
Bundles our research activities in software diversification. Our recent milestones include:
- New defense against Address-Oblivious Code Reuse (AOCR) through novel diversification techniques that prevent the code harvesting stage of the attack.
  [R2C: AOCR-Resilient Diversity with Reactive and Reflective Camouflage](publications/r2c-eurosys23.pdf)
- New defense against Counterfeit-Object Oriented Programming (COOP) through new spatial diversification techniques. *Submitted for publication*.

#### Leakage-resilient Diversity
The goal of this line of research is to mitigate advanced code-reuse attacks, such as both direct and indirect JIT-ROP, COOP, AOCR, and PIROP.
Broadly speaking, the idea is to combine software diversity with so-called, *execute-only memory* (XOM).
Prof. Brunthaler co-authored one of the most highly cited articles in this area, called [Readcator](publications/sp15.pdf), which used the first hardware-supported XOM with advanced code diversification, including *code-pointer hiding*.
Due to emergent security problems of code-pointer hiding, which resulted in the Address-Oblivious Code Reuse (AOCR) attack, our research group continued improving diversification techniques to mitigate even the most-advanced code reuse attacks.
In 2023, we were able to publish this defense, [R2C - Reactive and Reflective Camouflage](publications/r2c-eurosys23.pdf), which to the best of our knowledge, is the only effective and efficient defense to date.

#### Versatile Diversity
Besides code-reuse attacks, we published the first paper aimed at preventing Rowhammer attacks with principles underlying software diversity.
Similarly, we published a defense against timing-based cache side-channels through our discovery of a new defense called *control-flow diversity*.

### *Supply-Chain Attacks*: μ-c
We are actively investigating how to address supply-chain attacks at compile time through developing our own compiler infrastructure.
This compiler combines our state-of-the-art software diversification techniques and offers support for C and multiple backends.


### *Decompilation*: μ-dc
We examine novel techniques in decompiling programs, i.e., the process of producing source code from programs in binary form.

### *Interpreter Optimization*: μ-python
Bundles our research activities in *interpreter optimization*.
Our present research efforts deal with purely-interpretative optimizations, i.e., trying to avoid dynamic code generation altogether.
The key insight of Prof. Brunthaler's work from 2010 until 2014 was that an interpreter can do pretty much the same things as a JIT compiler.
A series of optimizations addressed various shortcomings in isolation, such as providing [type feedback via inline caching](publications/ecoop10.pdf), or eliminating [reference count operations](publications/dls10.pdf).
Later on, these techniques were combined to also eliminate the overhead of operating on boxed objects (see [Multi-Level Quickening](https://arxiv.org/pdf/2109.02958.pdf)).
Multi-level quickening provided substantial speedups of up to 5.5x, but did not convince the reviewers in 2012, 2013, and 2014.

At present, Python adopted the former optimization techniques, i.e., the quickening-based inline caching, since version 3.10, and will adopt the latter technique in future versions.
As a result, this line of research, although academically unsuccessful, is used by millions of people on a daily basis.



## Recent Publications
* [R2C: AOCR-Resilient Diversity with Reactive and Reflective Camouflage](publications/r2c-eurosys23.pdf)\
  Felix Berlakovich, Stefan Brunthaler.\
  In **EuroSys '23**: Proceedings of the Eighteenth European Conference on Computer Systems ([source](https://github.com/ucsrl/r2c-llvm), [artifact](https://doi.org/10.5281/zenodo.7728972))\
  **Note**: This paper extends prior work of the Readactor system to mitigate AOCR and PIROP attacks.\
  (*Acceptance rate: 16.2%*)

* [Look Ma, no constants: practical constant blinding in GraalVM](publications/constant-blinding-eurosec22.pdf)\
  Felix Berlakovich, Matthias Neugschwandtner, Gergö Barany.\
  In **EuroSec '22**: Proceedings of the 15th European Workshop on Systems Security

* [Towards efficient and verified virtual machines for dynamic languages](publications/verified-machines-cpp21.pdf)\
 Martin Desharnais, Stefan Brunthaler.\
 In **CPP '21**: Proceedings of the 10th ACM SIGPLAN International Conference on Certified Programs and Proofs

[Full list of publications](publications/)

## Presentations

* [Multi-Level Quickening (Invited talk, University of Kent, 2021)](https://www.youtube.com/watch?v=Ww59mOcrfzc&pp=ygURc3RlZmFuIGJydW50aGFsZXI%3D)
* [MAD: Memory Allocation meets Software Diversity (DRAMSec, 2021)](https://www.youtube.com/watch?v=WsoOpLv7i18&pp=ygURc3RlZmFuIGJydW50aGFsZXI%3D)

## Essays
The following essays are available:
- [Triaging Boring Publications, a response to Daniel Lemire's post](essays/20210102-academic-ills.md)
- [Why interpreters matter (at least for high abstraction level languages)](essays/20120712-why-interpreters.md)
