# Welcome to the official blog of [μCSRL](https://www.unibw.de/ucsrl-en)!

The [Munich Computer System Research Laboratory](https://www.unibw.de/ucsrl-en), μCSRL for short, is a research lab at the [National Cyber Defense Research Institute CODE](https://www.unibw.de/code) at the [Universität der Bundeswehr, München](https://www.unibw.de), directed by Stefan Brunthaler.
This web page hosts all public artifacts of the research group, including blog posts, essays, source code, and tools.

## Research
As of Q4 2023, μCSRL conducts the following research projects.

### μ-fuzz
Bundles our research activities in automated vulnerability identification via *fuzzing*.
Our objective for this project is the investigation of combinatorial optimization of fuzzing on clusters.
To support this project, we have a state-of-the-art fuzzing cluster with 1,200+ CPUs.

### μ-proteus
Bundles our research activities in software diversification. Our recent milestones include:
- New defense against Address-Oblivious Code Reuse (AOCR) through novel diversification techniques that prevent the code harvesting stage of the attack.
- New defense against Counterfeit-Object Oriented Programming (COOP) through new spatial diversification techniques.

### μ-dc
We examine novel techniques in decompiling programs, i.e., the process of producing source code from programs in binary form.

### μ-python
Bundles our research activities in *interpreter optimization*.
Our present research efforts deal with purely-interpretative optimizations, i.e., trying to avoid dynamic code generation altogether.


## Essays
The following essays are available:
- [Triaging Boring Publications, a response to Daniel Lemire's post](essays/20210102-academic-ills.md)
- [Why interpreters matter (at least for high abstraction level languages)](essays/20120712-why-interpreters.md)


## Publications
* [R2C: AOCR-Resilient Diversity with Reactive and Reflective Camouflage](publications/r2c-eurosys23.pdf)\
  Felix Berlakovich, Stefan Brunthaler.\
  In **EuroSys '23**: Proceedings of the Eighteenth European Conference on Computer Systems ([source](https://github.com/ucsrl/r2c-llvm), [artifact](https://doi.org/10.5281/zenodo.7728972))
  **Note**: This paper extends prior work of the Readactor system to mitigate AOCR and PIROP attacks.

* [Look Ma, no constants: practical constant blinding in GraalVM](publications/constant-blinding-eurosec22.pdf)\
  Felix Berlakovich, Matthias Neugschwandtner, Gergö Barany.\
  In **EuroSec '22**: Proceedings of the 15th European Workshop on Systems Security

* [Towards efficient and verified virtual machines for dynamic languages](publications/verified-machines-cpp21.pdf)\
 Martin Desharnais, Stefan Brunthaler.\
 In **CPP '21**: Proceedings of the 10th ACM SIGPLAN International Conference on Certified Programs and Proofs

## Presentations

* [Multi-Level Quickening (Invited talk, University of Kent, 2021)](https://www.youtube.com/watch?v=Ww59mOcrfzc&pp=ygURc3RlZmFuIGJydW50aGFsZXI%3D)
* [MAD: Memory Allocation meets Software Diversity (DRAMSec, 2021)](https://www.youtube.com/watch?v=WsoOpLv7i18&pp=ygURc3RlZmFuIGJydW50aGFsZXI%3D)