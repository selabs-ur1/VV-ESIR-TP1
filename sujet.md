# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
1. 
Bug: https://www.theregister.com/2015/12/18/software_glitch/ 

We did some research about a bug that happened at the Co-op Bank in April. It was a local bug into the Bank program that handled the sending of loan statements to clients. The law said that banks need to send the loan statements days after the previous annual statements. But the program sent them 3 days too late to a certain group of clients. Co-op Bank has been forced to refund million £ to their clients. It has a huge impact on the Bank economy and it happened when the Bank was cash-strapped. 
This bug was probably introduced by a software engineer who surely did a miscalculation. This bug could have been avoided by doing tests on the calculation function or by adding an assertion to make sure of the date.

2.
https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-796?filter=doneissues&orderby=cf%5B12310200%5D+DESC%2C+updated+ASC

SetUniqueList.createSetBasedOnList doesn't add list elements to return value.
We classify this bug as local because it is due to a removal of the “addAll” method which it seemed there was no reason to remove. To resolve this bug the method was added again. The user fixed a typo error at the same time on UnmodifiableListIterator.unmodifiableListIterator (previous 'UnmodifiableListIterator.umodifiableListIterator').
The user that fixed this added new tests to ensure that the function is working

3. 
To test their system, Netflix has to use chaos engineering. They want to guarantee the availability of their service under different circumstances. They have a metric to analyze and measure the Netflix service’s health. This metric is the SPS, stream per second. The engineers, in charge of chaos, know the SPS curve during 24h and they can detect an anomaly. 
    To start a chaos engineering test, you need to define a stable state and have a control group and an experimental group. Next you introduce some variables that represent real issues like server shutdown or network disconnection. After that, you compare SPS between the 2 groups and you decide if the variables have had an impact on the Netflix system. 
    Facebook, Google, Amazon or Microsoft also use chaos engineering to test resilience of their system. For example, a metric for Amazon could be the number of sales per minute. 


4.
WebAssembly is safe and fast to execute, portable semantics. It brings assembly to web technologies.

The fact that the language has a formal semantics means that we can automate some parts of the code so that we can assure it is running as intended.
Furthermore, the functions can’t access the memory of a running program meaning that it cannot inject code and avoid malicious attack.
We still need to verify the code because the specification was made by humans and we can’t say for sure if there are errors or not. Then we need to verify that the code written follows the specification.



5.
The main advantage of Isabelle is that its mechanized proof brings soundness to the specification of WebAssembly. During the completion of the proof, several issues with the WebAssembly specification were discovered. These issues influenced the development of WebAssembly. This proof comes with an executable interpreter and a type checker.
To verify the specification, the author created an interpreter following the mechanized specification, so that it can validate it and see if there is any error.
This new specification removes the need for testing because it is mechanized, however it should be tested for the new updates of WebAssembly if they add new functionalities.

