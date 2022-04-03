# PR - Thomas BAUQUIN & Erwan DUFOUR

# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### Question 1

We have chosen the bug : [CURA-8365](https://github.com/Ultimaker/CuraEngine/pull/1474)

The unretract was placed before the move, which can cause dimensional accuracy issues and leave visual artefacts since the nozzle is sitting on top of the outer wall on the printing.

Instead, the printer should unretract before the last travel move when going to print an outer wall. After the unretraction is complete, it can apply the last travel move and start printing the outer wall.

There is no repercussions on the company behind this bug because that’s not a fatal bug. Moreover, this is an OpenSource project where everyone can be a contributor. The bug was found and solved by the community. The bug is just a minor local bug which decreases the quality of the printing. The error is due to some human error while they wrote the code.

To discover this problem we should manually create an oracle and compare it with the output of cura’s engine. That’s the only solution to be sure about our output.

### Question 2

We have chosen the bug : [COLLECTIONS-796](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-796?filter=doneissues) 

SetUniqueList.createSetBasedOnList doesn't add list elements to return value

This bug occured after a new released version. A call to a function (called addAll) was accidentally deleted by an humain error. 

To resolve this problem, the contributor created a [pull request](https://github.com/apache/commons-collections/pull/255) where he corrects the bug and adds a test to avoid that the bug reappers in the future.

### Question 3

This paper presents experiments in chaos engineering, used to make systems safe again.

Netflix wants a resilient system to have a service that works 24/7. To test this resilience, they are experimenting with shutting down the servers of an entire region of AWS. After the servers are shut down, they test whether subscribers in the region where AWS was shut down can access Netflix and watch their movies. To do this, they developed their service on a distributed system.

The variables that the developer team observes are “are users complaining ?” and “does everything seem to be working correctly ?”.

Netflix was the first company to develop the use of chaos engineering. They have also created a software: Chaos Monkey. Through this article, they aim to extend its use to as many companies as possible. 

### Question 4

WebAssembly has several advantages : 
- safe to execute
- fast to execute
- language and platform independent
- deterministic 
- simple interoperability with the Web platform
- compact and easy to decode
- easy to validate and compile
- easy to generate for producers
- streamable and parallelizable

Having a formal specification for the WebAssembly, “not only demonstrates the “real world” feasibility of such an approach, but also leads to a notably clean design".

### Question 5

According to the authors, the main advantages of the mechanism Isabelle are to have a verifiable executable and a type checker. Moreover, their model is more resilient, they have fully proven the mechanism.

The mechanism helps to improve the mechanism because the authors of this article have exposed several issues about the official WebAssembly specification.

We have a separate verified executable interpreter and type checker as news artefacts.

The author verified the specification through a semantic behaviour in English prose and with an accompanying natural deduction style formal rule. The designers of WebAssembly created their model with the goal to be capable of being proven through formal analysis and verification.

This new specification needs to be tested. The core proof of correctness and the official WebAssembly conformance test suite allow to experimentally validate the mechanised specification.


# PR - Thomas BAUQUIN & Erwan DUFOUR
