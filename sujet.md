# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. Link of article (https://www.cnbc.com/2024/01/26/tesla-recalls-nearly-200000-vehicles-in-us-over-rearview-camera-bug.html)

The bug is from a bad communication between two onboard chips after a software update of the FSD (Full Self-Driving 10.3). It is a global bug because it's a software/hardaware improbable interaction. This bug caused a false forward-collision warning or unexpected activation of the emergency brakes. The repercussion for clients is the recall of their vehicles and for the brand, it is a bad communication. To avoid this bug the brand could have make tests of this system before the beta-test launch.

2. Link of bug (https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-802?filter=doneissues)

This bug is local because it's a wrong condition. The code compare a couple of value and a integer value. The solution it's to get the value of the current item and compare him to a another integer value. The contributors didn't add other tests because the bug come from an assertion.

3. Netflix performs several Chaos Engineering experiments to test the resilience of its distributed systems such as Chaos Monkey, Chaos Kong and Failure Injection Testing(FIT).The first one randomly shuts down virtual machines in production, the second tend to simulate the failure of an entire AWS region, and the last one which introduces failures in service communication.

The requirements for the experiments are a  distributed system (AWS), monitoring systems to track metrics , mechanisms to ensure graceful degradation and finally  controlled environments to prevent major disruptions.
The variables observed are SPS, latency, error rates.
These experiments enhance the systems' resilience, recovery capabilities, graceful degradation and increased confidence in production system stability

Netflix is not the only company using these practices, there is also Amazon, Google, Microsoft and Facebook conducting similar experiments.

In Financial institutions for eg these experiments could involve testing the failure of a key transaction service or data inconsistency scenarios. The focus could be on transaction processing times or error rates.

4. The formal specification of WebAssembly offers several advantages. 
First there is a clear and reliable design with precise rules for execution and validation
Then  The specification ensures the language is independent of hardware and platforms, allowing for seamless integration across different environments
Finally the proof of Soundness, in fact the formal semantics include proofs of correctness and security, ensuring that the language maintains its core properties, such as memory safety.

Although the formal specifications guarantee theoretical correctness, practical testing is still needed for verifying implementations. Testing ensures that the language not only in phase with its theoretical model but also performs optimally and complies with real-world use cases across different contexts

