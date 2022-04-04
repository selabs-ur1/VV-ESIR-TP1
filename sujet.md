# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. Bug : Critical Bugs Expose Hundreds of Thousands of Medical Devices and ATMs

Description : 
Seven vulnerabilities were found in an internet of things remote management tool, CyberMDX found the seven vulnerabilities, collectively dubbed Access:7, in the IoT remote access tool PTC Axeda. The tool Axeda is a popular IoT tool in the medical field as well as with ATMs, vending machines, barcode scanning systems, and some industrial manufacturing equipment. The Access:7 vulnerabilities are in hundreds of thousands of devices in all and in a review of its own customers, Forescout Technologies alone found more than 2,000 vulnerable systems. 
Three of the seven vulnerabilities rate as critical. Attackers could potentially exploit the bugs to grab patient data, alter test results or other medical records, launch denial of service attacks that could keep health care providers from accessing patient data when they need it, disrupt industrial control systems, or even gain a foothold to attack ATMs.

The Bugs :
The two most severe RCE vulnerabilities (CVSS scores of 9.8) relate to hard-coded, guessable system passwords shared by multiple Axeda users. This might not be testable, I think having guessable passwords shaed by multiple users is a bad practice and even more so if they are hard coded into a product, but I don't know how those vulnerabilities would be tested. (CVE-2022-25246, CVE-2022-25247)

The next most critical vulnerabilitiy (CVSS score of 9.4) relate to issues with how Axeda processes undocumented and unauthenticated commands. This issue is very testable as it realtes to testing the reachability and security of commands. (CVE-2022-25251)

Global or Local? :
Both of the bugs we coverred are most likely global since article says it is related to the use of hardcoded credentials and guessable system passwords. The fact that some of the system passwords are hard-coded into the product implies that they could maybe be used at some point but were clearly not meant to be used by some other parts of the product. This means that there are multiple parts of the product that interact to cause the bug.

Would testing in the right scenario have helped find the fault? : 
I guess...
Some of the bugs like CVE-2022-25251, seem easier to test for. I think checking for unauthenticated command restrictions in a program that allows commands should be common place.
For the CVE-2022-25246, having hard coded credentials seems very dangerous in it of itself so if they are left in i should think that they are esssential to the functioning of the program. In this case thourough testing on these credentials is undoubtetly necessary.
The CVE-2022-25247 that involves full system access trough a bug on the other hand seems harder to test for. But i think that if a full system access potentially exists in a program there should be of course tests that focus hard on its accessibility.
However we cannot say if the developpers already thought of all of this and that the bugs are simply a consequence of the fact that it is impossible to test for every possibility, of course testing the right scenario would have helped prevent all of these bugs probably but finding and testing the right scenario for each bug will only leave open another scenario for another bug.

2. Apache Commons projects

Among those issues it is the collections issues that is considered like a bug and only some of them have been resolved. So the collections 701: “StackOverflowError in SetUniqueList.add() when it receives itself” is one of the resolved bug with a critical priority.
It is a global bug because the program crashes when the add method reuse the same list. It is an error in reusing of this method (recursion). 
The execution of add () gets trapped in an infinite recursion that crashes the program.
From the stack trace, we found that infinite recursion occurs
to AbstractList.hashCode() since it invokes hashCode() of each of its elements.
The fix of the bug should be so that a SetUniqueList behaves like a ArrayList and HashSet.
Some test classes have been set up to prevent the reappearance of the problem if it happens in the future like the testSetUniquelist() class.


3. Netflix's chaos engineering

The experiments in chaos engineering consist of these steps:
Build a hypothesis around steady state behavior -> Vary real world events -> Run experiments in production -> Automate experiments to run continuously

A concrete experiment will follow this path:
- decide which variables will characterize steady-state behaviour in this experiment. (ex: SPS)
- make a hypothesis about a the previoously chosen metric regarding the chaos engineering experiment. (ex: SPS will remain steady)
- use historical data to introduce into the environement changes that could potentially disrupt the steady state behavior of the
system. (ex: terminate virtual machine instances, inject latency into requests between services, make an entire Amazon region unavailable)
- observe the previoously chosen metric's reaction to the introduced changes and conclude on the hypothesis, (generally we want to disprove the hypothesis to expose flaws in the system). (ex: making an entire Amazon region unavailable lowered the SPS => the hypothesis is false)

The variables Netflix uses for their chaos engineering experiments are:
SPS (stream starts per second) and New account signups per second to characterize steady state behavior / system availability.
Finer-grain metrics, CPU utilization and Request latency to characterize the functionality of their system.

chaos engineering also used by : Amazon , Google , Microsoft and Facebook 
Amazon : Amazon probably monitors metrics like : Purchases per second, and New account signups per second. Similarly to Netflix i expect them to simulate failures in their servers to observe the effect on user purchases.
Google : Google probably monitors metrics like : Searches per second and Videos Started if we count Youtube. Similarly to Netflix i expect them to simulate failures in their servers, inject latency in the queries and purposefully fail requests to observe the effect on user search frequency.
Microsoft : Microsoft probably tests the resillience of its network through metrics like CPU usage in VM and experiments that shut down regions and servers.
Facebook : Facebook definitively monitors user Logins and Signups per second as well as probably Posts/Uploads per second. Apparently Facebook turned off a data center to test the tesiliency of their network.

4. WebAssembly

Advantages of having a formal specification :
The main advantage of having a formal specification is the new easyness it creates for the validation part of programming.
According to 4. Validation : the typing rules of WebAssembly create a property of standard soundness and through the extension of the typing rules to stores and configurations we can state the soundness theorems. These properties ensure that all valid programs either diverge, trap, or terminate with values of the correct types.
Having such theorems indicates that the WebAssembly is much more easy to validate.

Does this mean that WebAssembly implementations should not be tested? :
Having a formal specification for WebAssembly will indeed help validation tremendously but there is no way for a specification to help verify a human programmer's logic. The validation part that this formal specification will help work on will most likely be local bugs that exist on specific locations in the code and that focus mostly on the correct typying, but not much for global bugs that don't exist individually but are the result of faulty iteractions between bits of different code. In fact a global bug can exist through the interaction of two perfectly well-written programs and in this case the formal specification of WebAssembly will not be of much use.
Because of this the testing on a WebAssembly code should focus less on typying testing and more on other local bugs and global bugs, but bug testing and verifications should never be omitted.

5. The mechanical specification of webAssembly

The mechanical specification of webAssembly has the advantage of solid formal properties of webAssembly and it defined a separate verified executable interpreter and type checker However we have the improvement of the original formal specification of the language in this specifics points:
•	adding a hypothetical “dup" opcode to WebAssembly 
•	definition of two inductive relations, which correspond to the WebAssembly specification’s reduction and typing rules 
•	changing the structure of the Label and Local operations 
•	altering the semantics of Return so that it now breaks directly to the outside of the function call instead of being defined in terms of Br. 
•	Implementation of an executable interpreter 
These artefacts need the integration of an external parser and linker to pass independently, so they introduce an untrusted interface.
And the validation and fuzzing permit them to test all the functionality and the interpreter where no errors were found either in our implementation or any commercial engine, although a crash bug was discovered within the Binaries toolchain itself, which was reported and fixed by the developers. Also, some tests have been done to validate the model of mechanized specification. All these tests don’t remove the need of test in the future


