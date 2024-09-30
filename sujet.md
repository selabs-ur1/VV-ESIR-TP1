# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. ([CBS Article](https://www.cbsnews.com/news/microsoft-internet-outages-reported-worldwide/)) A faulty update of Crowdstrike Falcon, an widely used antivirus system for Windows, caused a blue screen error on all Windows devices that used it. This caused a global outage that impacted several companies, for example Delta Airlines and several other airline companies, who saw the majority of their flights get cancelled. Another type of company that was impacted was delivery services, such as FedEx or Starbucks.
The main source of the bug comes from erroneous data that was introduced into a Falcon configuration file - the channel file. This file was treated by trusted, and consequently elevated parts of Falcon, which could run code from within the OS to validate the contents of the configuration. However, a mismatch of entry counts caused the failure of the treatment procedure, which led to a BSOD.
Even if the failure was caused by erroneous data, Falcon was unequipped to deal with such cases. The bug comes from either defunct exception handling, or the absence of it. 
Therefore, we can say that it was a global bug.
In our opinion, testing the right scenario could have definitely helped to discover the fault, because the failure was most likely caused by a poorly handled exception.

2. We have chosen the bug [UnmodifiableMultiValuedMapTest is flaky](https://issues.apache.org/jira/browse/COLLECTIONS-769) from the tracker. This bug concerns three test functions. This is due to the fact that the `toString` of the `MultiValuedMap` type returns a string formed with all the values contained in the map, but the problem is that the values are arranged in an arbitrary order. Indeed, a map operates with keys that can be of any type, so it is not obvious to have an order for all these types. However, in the tests, the `MultiValuedMap.toString()` is compared to a hardcoded string that follows an order we find logical, with the keys "one, two, three" in ascending numerical order. These keys could also be arranged in alphabetical order, which would result in "one, three, two", making this comparison unreliable. 
We categorize this bug as local because the `toString` function is used correctly with correct input, and the same goes for `assertEqual`. Here, it’s a lack of caution regarding the behavior of the `MultiValuedMap.toString()`. 
The solution was to create a helper to test the value of all the map's keys individually, so as not to have to use `MultiValuedMap.toString()`.

3. Concrete experiments carried out by Netflix include: *the use of the internal Chaos Monkey service which is a tool that randomly terminates virtual machine instances in the production system in order to test the service's ability to handle individual instance failures.

    *the use of chaos kong Chaos kong simulates the failure of an entire Amazon EC2 region to assess the system's resilience to large-scale failures.

    *the introduction of errors in communications between Netflix services to verify that the system continues to operate even if requests fail.

                the requirements for these experiments__

    the requirements for these experiments are first of all to be able to define a Steady State this means that Netflix engineers must be able to identify a basic metric that reflects the normal behavior of the system, such as the number of videos streamed per second (SPS). They must also be able to formulate hypotheses on the fact that the stable state will be maintained despite the disturbances introduced. In addition, they must be able to simulate real events such as server failures or network errors and be able to observe the differences in behavior between the control group (without disturbance) and the experimental group (with disturbance).

                        the variables they observe_

    the observed variables are the number of videos observed per second which is then used as the main indicator to determine whether or not the system is healthy as well as internal performance metrics such as query latency and CPU usage, to detect problems at the individual service levels

                        the results obtained__

    the observed experiments make it possible to identify the various weaknesses of the system and to make suggestions for its improvement. Also, they make it possible to obtain assurance that the system is functioning correctly despite the various failures that have been simulated and that it manages at the same time to maintain acceptable performance despite these failures

            Is Netflix the only one to use this technique__

    No Netflix is ​​not the only company to adopt Chaos engineering we can cite Amazon, Google, Microsoft and Facebook also use similar techniques to test the resilience of their systems.

    These experiments could be used in e-commerce companies to simulate the different failures that could occur in payment systems.

4. The main advantage of having a formal specification for WASM is that it drastically simplifies its semantics. For example, the JVM (Java Virtual Machine) bytecode verification is about 150 pages (?????) whereas WASM's fits in one. So the verification is quicker and more functional. For example JVM, CIL and Android Dalvik verification allow irreducible loops while WASM does not.
However, it does not mean that WASM implementations should not be tested. Indeed, a formal specification does not keep any of its implementations to be bug-free.

5. The main advantages of the mechanized specification is that specification reveal deficiencies in the WebAssembly specification. Yes, the mechanization exposed several issues that were then corrected by the specification authors of WebAssembly. The author derived a verified executable interpreter and a type checker from the mechanized specification and he verified the specification through a combination of fuzzing tests and the official WebAssembly test suite. No, the new specification does not eliminate the need for testing. The author still conducted differential fuzzing and testing against industry implementations, which indicates the need for ongoing testing.