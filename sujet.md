# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. https://www.eveonline.com/news/view/about-the-boot.ini-issue
The bug occured in a video game called "Eve Online", the developpers released a patch that modified the installer script. The game had a local boot.ini file which was temporary, so the script was deleting it. The issue was that the script was setting the path to the current directory used by the script(using SetOutPath) and then deleting the boot.ini file, but the delete function wasn't actually using the current path set earlier. So it was deleting the system boot.ini file resulting in the computer not being able to reboot.
The developpers assumed that the Delete function used the path, but the full path must be specified, meaning that the line "Delete "boot.ini"" would delete the Windows boot.ini file instead, this one being at the root.
This bug was a local bug because it only concerned one file.
The repercusions of this bug are hardly measurable because the users could not guess what was deleting the boot.ini and why their computers were not booting. On their website the game developpers say that only 215 clients contacted the customer service, but we can guess that the impact was much bigger. The company lost money by calling back developpers in the middle of the night and making arrangements with external support technicians.
In our opinion, the bug could have been found, as said in the article:
"We also discovered that we didn't have enough variation in our hardware and operating system setups"
So with the right scenario the fault would have been discovered.

2.https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-799?filter=doneissues
The bug is a local bug that involves 2 functions "pollFirst()" and "pollLast". These two functions retrieve and remove an element from a NavigableSet, the issue was that these functions were successfully executed on an unmodifiable set were it should have thrown an exception. They solved the bug by just throwing an exception in the class functions "UnmodifiableNavigableSet". In fact, before solving the issue, the developper added a test to check if these functions fail successfully.

3.Netflix runs different experiments such as:
- Terminate virtual machine instances
- Inject latency into requests between services
- Fail requests between services
- Fail an internal service
- Make an entire Amazon region unavailable
In order to make these experiments, it is required to follow some steps, first define a steady state, secondly, make an hypothesis that this state will continue during the experiment, then introduce variables that reflect real world events and finally try to disprove the hypothesis by looking for a difference in steady state.
The main variables observed are the number of stream start per second or the signups per seconds.
The results obtained differ depending on the experiments, if the variables observed have a sufficient change it can show that the system has an issue. 
The different results help to have more confidence in the system's ability to maintain behavior in the presence of variables, or show that the system  has a weakness and suggests a path for improvement.
Netflix is not the only company using chaos engineering, for example there is google, amazon, microsoft and facebook that use it as well.
These experiments could be performed in other companies, such as e-commerce by checking the number of purchases per second or ad-services companie by checking the numbers of ads viewed by users per seconds.The same kind of experiments could be performed like shutting down servers.

4.Having a formal specification enables the language to nearly be an executable specification and to have a clean design. This leads to traduction in bytecode and its execution extremely fast.
The formal aspect of WebAssembly includes a proof of soundness, which is the "absence of undefined behavior in the execution semantics".
Testing is still important because the language only approximates executable specification, and in WebAssembly, it is possible to test the specification to be sure the program is validated. Some programs use mathematically unproved calculations, meaning the tests are the only solution to this.
Also, proving the correctness of a program is really hard, thus the models which the formal methods are based on can differentiate from the machines executing the programs.

5.The main advantage of mechanisation is to reveal some flaws in language specifications by using mathematical formulas. Even though the first paper specified that the language was soundness prooved, The author of this second paper helped to change WebAssembly in a way which guarantees proof of soundness with the use of Isabelle.
The other artefacts derived from the mechanised specification are an executable interpreter and a type checker, the former which helped to prove the specification of WebAssembly by first successfully passing all WebAssembly core language conformance tests, then by being tested against other WebAssembly engines.
This mechanisation does not remove the need of testing because although soundness has been proved, it doesn't prevent the language from having unwanted behaviours based on the specification in edge cases.
