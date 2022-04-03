# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. Dirty Pipe is a software bug discovered by Max Kellermann on February 19, 2022. It is a Linux vulnerability which occured on Linux kernel allowing an underprivileged user to obtain writing rights on files. The research on this bug began when files on log servers were corrupted. 

The bug is using the piping mechanism of Linux. Piping mechanism is used to allow one process to inject data into another. Dirty Pipe is using this mechanism with the splice function to overwrite sensitive read only files. For example it can modify the /etc/passwd to gain a non password root shell. For that, we have to create a pipe and insert datas in it in order to set a flag in all the entries of the pipe allowing pipe buffer merges. Then we must remove all the datas contained in the pipe. After that we have to be connected to the target file using a splice function before the target file offset. For that, we are using Linux's memory management : the pages. Indeed, Linux is copying 4kb chunks of datas from the hard disk to the pages. And so we use the splice() function that redirects the page references into a pipe. Finally, we can enter the data we want to insert into the file in the pipe. The data inserted in the pipe will replace the data contained in the target file. 
There are limitations to this bug : we must have read permissions on the target file, the file can’t be resized, etc.. So this failure is manifesting locally because you must have local access to the system.

It is a very dangerous bug because it can allow you to remove privileges on the system, but also to steal passwords, private datas, etc.. and it is very easy to produce. In fact, it is considered as the biggest threat in linux since 2016 (dirty cow bug, concerning pipes as well). Furthermore, it is a bug that occured on all systems based on a Linux kernel which concerns versions from 5.8 to 5.10.101 of Linux. So this bug can happen on a lot of systems such as android devices, servers, etc.. And updating a server or any system running on these sensible kernel versions of Linux is the only way to be safe from Dirty Pipe.

The origin of this vulnerability is from a previous bug introduced in Linux 4.9 allowing to create arbitraire references to pages cache and from the modification of functionment of the merging of pipe buffers in the 5.8 Linux version.

Testing the right scénario was essential in finding the source of the bug because to find it, Mark Kellermann has created a process writing “AAAAA” continuously in a file and another which was transfering the datas in the pipe, and then writing “BBBBB” in this pipe. After that, only “BBBBB” was written in the file while the second program had the read-only permission on the file.


Source : 
Article of Max Kellermann explaining how he discovered the bug : https://dirtypipe.cm4all.com/

2.  Appache Commons Collections bug: [link to the bug](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-799?filter=doneissues)

description of the bug :
pollFirst() and pollLast() are successfully executed and do not throw an UnsupportedOperationException on an UnmodifiableNavigableSet instance.
In my opinion, org.apache.commons.collections4.set.UnmodifiableNavigableSet should have an implementation similar to java.util.Collections.UnmodifiableNavigableSet, where both methods throw an UnsupportedOperationException: https://github.com/openjdk/jdk/blob/708407eddc9d52c01de02e3986c05e1c6225fa85/src/java.base/share/classes/java/util/Collections.java#L1278-L1279
This was detected during working on https://github.com/assertj/assertj-core/pull/2328.

This bug is local as it is an error of implementation in conjunction with a lack of testing. 
 
New tests have been added. We can read them in the file :
src/test/java/org/apache/commons/collections4/set/UnmodifiableNavigableSetTest.java
 
We can also see what was exactly modified with the pull request in the “file” section :
https://github.com/apache/commons-collections/pull/250/files

3. Netflix is using Chaos Engineering to test their entire system in production to ensure that the system provides a minimum of service for the customers, even when a server shutdown occurs. 

For that, companies using Chaos Engineering need to perform concrete experiments. These experiments consist in establishing hypotheses for the behavior of the system in very specific steady states. Then we modify real-world events like server shutdown or surging in incoming requests to run the experiments we want. And finally we check the metrics which indicates whether customers are encountering issues in accessing the services or not. For example for Netflix it is the stream start per second (SPS) because if a lot of customers have issues, the SPS will show that a lot of customers are disconnecting from the website and that would be a sign for malfunctions or unavailabilities in the services. 
A final state would be to automate these experiments.

So Netflix is not the only company that is using Chaos Engineering : Google, Amazon, Microsoft or Facebook are using similar tests to ensure the resilience of their systems. For example Amazon is doing “Chaos Kong” exercises to simulate the failure of an entire Amazon region.
As well as the experiments, the metrics used as data to see the resilience of a system differ following the system to test :  it can be the number of sign ups, the number of completed purchases for an e-commerce site, etc…

4. WebAssembly is the first language in the web environment being formally specified. The aim is to match a number of properties :
 
• Safe, fast, and portable semantics: 
safe to execute
     fast to execute
language -, hardware-, and platform-independent
     deterministic and easy to reason about simple
     interoperability with the Web platform 

• Safe and efficient representation:
compact and easy to decode
easy to validate and compile
easy to generate for producers 
streamable and parallelizable

Formal specification is helping to achieve and prove these goals.

It does not mean it is unnecessary to test the system. Implementation errors can be found and some aspects of the system are not formally specified. In addition, as we will see in the next exercise errors were found in the original formal specification. We also need to check if its specification matches the expectations. 

5. This paper is about mechanization of the WebAssambly specification using Isabelle which is a generic proof assistant.

The original specification was done by hand which is prone to errors. Mechanised specification is using software to make proof that it is correct. It helped to discover issues in the type system original specification. They also used Isabelle to define a type checker and an interpreter which were proven sound.

They also did a validation of the new model with the official conformance tests. It is necessary because it is required to check if the mechanized specification was the “right” specification (eg: what system we wanted to build). A fuzzing was also conducted.

Testing is still needed to avoid implementation errors. Moreover WebAssembly is interfacing with unsafe environments such as javascript so testing those interfaces is a good idea. 
