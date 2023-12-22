# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. On 20minutes, a French news website, we found an article about a bug in the Firefox browser. 

source : https://www.20minutes.fr/high-tech/4017153-20230103-bug-enfin-corrige-firefox-18-ans-apres-decouverte
https://bugzilla.mozilla.org/show_bug.cgi?id=290125

The bug was discovered in Firefox version 1.0, eighteen years ago. It affected text rendering, specifically with a style description where a large initial capital letter did not display correctly. The issue was related to the interpretation of a CSS, specifically the pseudo-element "CSS ::first-letter" when coupled with a float property. This bug is a global bug of concurrency between these two properties. According to Mozilla's tracking system, the bug was quickly discovered but considered a non-priority, resulting in the large initial letters being displayed incorrectly on websites using Firefox and causing aesthetic problems. Despite being reported, the resolution was delayed for eighteen years. The impact was mainly visual, affecting the presentation of initial letters on certain websites (but without hindering the use of websites). The only repercussion for Mozilla could be an eventual loss of users. The bug's correction is expected with the release of Firefox 110 in next February. Testing wouldn't have been relevant in this case, because the bug was already known and just ignored.

2. "CollectionUtils.retainAll() not throwing proper NullPointerException(NPE)" is an example of issue considered as solved for the Apache Commons Collections project. The resolution of this issue is a fix.
The bug was a local bug, local to the function CollectionUtils.retainAll(). The bug was that the function did not throw a NullPointerException when one of its parameters was null, contrary to what was specified in the documentation. 
The bug was patched by throwing a NullPointerException when one of the parameters is null (and updating the comments). In the commit, we can see that the same patch has also been made in several other functions of the same class.
The contributor added new tests to verify that each updated function throws a NullPointerException when one of its parameters is null (one test for each parameter), and ensure that the bug is detected if it reappears in the future.
For the function in the title of the issue, the new tests are entitled "testRetainAllNullSubColl" and "testRetainAllNullSubColl".
source : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-813?filter=doneissues


3. The concrete experiments performed by Netflix revolve around Chaos Engineering and are the following:

- Chaos Monkey: Randomly terminates virtual machine instances that host production services during normal working hours. 

- Chaos Kong: Simulates the failure of an entire Amazon EC2 region.

- Failure Injection Testing (FIT) exercises: Cause requests between Netflix services to fail deliberately.

For each of these experiments, the system needs to handle the failure and continue to operate correctly. 
This kind if experiments is only possible on distributed systems.
In order to adopt the described system perspective, the system needs to have been deployed into production some time ago and to have active users.

Seeing the system as a whole entity, allows to observe the `steady­state behavior`, wich can be affected by users traffic, engineers interventions, and failures.
The variable mesured by Netflix aren't the match of the specification, but rather the observation of if the system is still working as intended, availibility and how much the users are complaining about a failure. During an experiment, Netflix just need to measure the difference in steady state behavior between the normal state and the state with the failure injected.

From these experiments, two kinds of results can be obtained:
- The system is resilient and the experiment has no impact on the users. We can then have more confidence in our system and its ability to handle failures.
- A weakness has been uncovered and should be fixed. The experiment has helped to find a path for improvement.

Netflix is not the only company performing this kind of experiments. In fact, and as stated in the paper: "Amazon, Google, Microsoft, and Facebook, were applying similar techniques to test the resilience of their own systems".

Chaos monkey and FIT could be carried in other organization using distributed systems and microservices.
However, Chaos Kong is only possible and relevant for services deployed over multiple regions.

Examples of variables that could be relevant for any kind of system using Chaos Engineering would be:
- Users retention time
- Latency
- Availability

4.

The formal specification of WebAssembly allows it to be independant from the web and JavaScript environments.
Web assembly is more of a standard and can be used in different environments, not only the web. 
It can be used to implement low or high level languages (only low level languages are currently implemented). 

It is less ambiguous than natural languages wich will make it easier to implement across several browsers and more reliable. It also allows consistency across implementations and thus compatibility.
In short, it allows what the authors of the paper call a "notably clean design".

The expected behavior has already been stated (like in test driven development) so it is easier to test the implementation of the specification.
Test are easier to determine and to write because the specification is more precise and less ambiguous. 
Testing is evidently still necessary to ensure that the implementation is correct.
In fact, to our mind, we could say that WebAssembly implementation tests should mainly focused on the verification side of testing (to verify that the implementation is correct) rather than the validation side (to verify that the implementation is what is needed). 
However, the more these implementations will build over the standard, the more they will need to be tested for validation purposes.



5.
Mechanized specification of WebAssembly using the Isabelle proof assistant has many advantages:
It allowed formally proving properties of the WebAssembly language, such as type soundness.
Maintaining "eyeball closeness" to the original specification helped relate proven properties back to the original text.
The process of developing proofs uncovered issues in the original specification that could be addressed, improving the specification.
The mechanized specification can guarantee properties like type soundness in a way not possible for a "light-weight" specification.

Yes, it improves on the original WebAssembly formal specification. This proof has highlighted
several problems with the official WebAssembly specification, and thus has influenced its development.

In addition to the mechanized specification itself, there are two other verified artifacts from the specification that derived from this mechanized specification :
A verified executable interpreter that implements the WebAssembly semantics.
A verified type checker that checks WebAssembly programs for type safety.

The author used the Isabelle proof assistant to mechanically verify key properties of the WebAssembly specification.

No, the mechanized specification didn’t remove the need for testing. While it helped improve the original specification and provided additional verified artifacts like a type checker, testing stay important for fully validating implementations.

------------------------------
------------------------------
Authors:
- Baptiste AMICE
- Ulysse-Néo LARTIGAUD