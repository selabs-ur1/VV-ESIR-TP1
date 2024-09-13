# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. A recent critical bug was discovered in Ivanti Endpoint Management (EPM) software, affecting global users. This flaw allowed unauthenticated attackers to remotely execute code on the server by exploiting a vulnerability in deserializing untrusted data. No customers were impacted by this bug because there is no known public exploitation of this vulnerability, but the bug posed a serious threat to businesses relying on EPM for managing IT systems. Ivanti quickly patched the issue. Improved testing, especially focused on deserialization scenarios, could have identified the flaw earlier.

2. The bug reported to Collection 796, is a bug related to a list that doesn't add list elements to return value. It's a local bug that haven't big impact. The bug is due to an accidentally deleted call to a function named 'addAll'. To solve the bug, the contributor found the commit where the call was removed, he implements the fonctonality again, he solved a typo error and he add tests to prevent other bugs like it.

3. Concrete experiments:
Chaos monkey: terminates virtual machines hosting production services randomly, ensuring that engineers design services to handle instance failures.
Chaos kong: simulates the failure of an entire Amazon EC2 region to test whether the system can handle such a large-scale outage by redirecting requests to healthy regions.
Failure injection testing : causes requests between Netflix services to fail, testing if the system degrades gracefully without severe user impact.
Requirements for the experiments:
The system must be distributed and able to operate with a high level of availability.
The experiments need to be run in production to simulate real-world failures.
Systems must have fallback mechanisms to handle service failures and maintain functionality.
Variables observed:
Stream starts per second: the primary metric used to measure system health, representing the number of users starting video streams per second.
Other steady-state metrics: metrics such as CPU load, request latency, and memory utilization are monitored during the experiments to ensure services are operating correctly, even under stress.
Main results: the experiments showed that Netflix could build resilience into its system by exposing it to various simulated failures. This allowed Netflix engineers to uncover weaknesses and fix them, improving overall system reliability. Over time, these exercises became an integral part of maintaining high availability for Netflix’s streaming services.
Are other companies using chaos engineering? No, Netflix is not the only company using these techniques. Other large organizations like Amazon, Google, Microsoft, and Facebook also conduct similar experiments to verify the resilience of their distributed systems​.
Other companies can implement Chaos Engineering by adapting experiments relevant to their systems. For example:
An e-commerce platform could simulate server crashes during peak sales events to observe how it impacts order processing and customer experience.

4. Main advantages of a formal specification for WebAssembly:
Safety and security: WebAssembly ensures that programs compiled into this language are guaranteed to avoid common runtime errors such as invalid memory access and undefined behaviors.
Portability: The formal specification ensures that WebAssembly behaves consistently across different platforms and devices.
Optimization: With a clear, well-defined formal specification, developers and compiler creators can optimize WebAssembly for performance while adhering to its safety guarantees.
Efficiency: A formal specification enables WebAssembly to be compact and fast.
Ease of validation: The formal specification simplifies validation.
In our opinion, all implementations which will be used by a lot of people should be tested.

5. The main advantages of the mechanized specification are the identification and correction of errors, the soundness proof, the executable artifacts and the validation through testing.
The mechanized specification significantly helped improve the original formal specification of the WebAssembly language. Through the mechanized proof, several deficiencies and errors in the official specification were identified and corrected.
The mechanized specification of WebAssembly led to the creation of several important artifacts : the verified executable interpreter, the verified type checker, and the mechanized proof of type soundness
The author verified the specification by providing a fully mechanized proof of the WebAssembly type system’s soundness. This proof was achieved through induction over the language’s reduction and typing rules, ensuring the correctness of the type system.
The mechanized specification does not eliminate the need for testing. While it ensures that the core parts of the specification are sound, fuzzing and conformance tests remain critical to catch issues in practical implementations and edge cases.

Tommi LUCAS and Nicolas BARON
