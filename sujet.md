# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

---
**Student**  
<table align="left">
<tr>
  <th> First Name </th>
  <th> Last Name </th>
  <th> @Mail adress </th>
</tr>
<tr>
  <td> Léo </td>
  <td> Thuillier </td>
  <td> leo.thuillier@etudiant.univ-rennes1.fr </td>
</tr>
<tr>
  <td> Thibaut </td>
  <td> Le Marre </td>
  <td> thibaut.le-marre@etudiant.univ-rennes1.fr </td>
</tr>
</table>
<br><br><br><br><br><br>

---

1. Well known bug - Patriot missile: error 404, scud not found, 28 dead

An Iraqi Scud strikes the Dhahran barracks in Saudi Arabia, killing 28 soldiers of the US Army's 14th Quartermaster Detachment.

The attack should have been intercepted by the radar system of the Patriot missiles in use.
But a software bug in the system's handling of time stamps got in the way.

As a result, the system scanned the wrong part of the sky and found no target.
If there was no target, the initial detection was assumed to be a false trail and the missile was removed from the system.

The Patriot missile battery had been in service for 100 hours, during which time the system's internal clock had drifted by a third of a second.
Due to the speed of the missile, this was equivalent to a missing distance of 600 metres.

It represent a `global` bug because it's due to a malfunction of the coordination of different elements in the system.

As we know some values can not be represented in binary form. To ensure our system does not make approximations on a long time period we should have tested it by simulating the long time use of the system.

---

2. Apache bug

Here is the link to the corrected bug we have chosen : [AbstractLinkedMap firstKey/lastKey JavaDoc reversed](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-353?filter=allissues)

The bug comes from the javaDoc of two methods of the `AbstractLinkedMap` class:
- .firstKey()
- .lastKey()

Indeed, when executing a test, the expected result corresponding to the doc fails.
```java
LinkedMap map = new LinkedMap();
map.put("one", "one");
map.put("two", "two");
assertEquals("one", map.lastKey());
assertEquals("two", map.firstKey());
```

The two javadocs are reversed, this bug is `local` because it does not come from the interaction of elements between them but from a local error during the implementation of the two methods. It is probably due to a copy/paste error.

This bug has been reported by several people in different reports, it is difficult to say if a test has been added to avoid regression. (any bug found must be tested to avoid regression and reappearance of the error).

The solution is rather simple: 
- reverse the names of the two JavaDocs methods so that their behaviour matches their JavaDocs.

---

3. Netflix Chaos Engineering

Chaos engineering is popularized by Netflix with its Simian Army. The main concept is to perform a controlled experiment in production to study how the entire system behaves under unexpected conditions. For example, in Netflix, they would simulate random server shutdowns to see how the system responds to this phenomenon. This is a form of testing in production. Main challenges are to design the experiments in a way that the system does not actually fail and to pick the system properties to observe. In the case of Netflix, they want to preserve the availability of the content even when the quality has to be reduced.

It is known that the SNCF team is very well known in this field when it comes to simulating route changes. These techniques can be used by companies that develop software where the environment is very difficult to simulate and where the consequences of a drop in performance have very few serious consequences.

---

4. Bringing the Web up to Speed with WebAssembly

**Formal specification**
> This approach tries to guarantee the absence of bugs by construction. It involves the manual or automatic formal proof of all the components of the system, and their final integration. It is usually based on logical modeling and reasoning and it is used on specific parts of critical software.

In this paper they describe the advantages of this specification as :
- Safe, fast, and portable semantics:
  - safe to execute
  - fast to execute
  - language-, hardware-, and platform-independent
  - deterministic and easy to reason about
  - simple interoperability with the Web platform
- Safe and efficient representation:
  - compact and easy to decode
  - easy to validate and compile
  - easy to generate for producers
  - streamable and parallelizable

Constructive approach involves formal modeling is one of the three main general approaches to construct reliable software:
- It guarantees the reliability and correctness by construction.

**The main problem with formal proofs comes from the `assumptions` they make to abstract the real world.**
So it'll always be interresting to try catching those bugs with analytical analyse.

---

5. Mechanising and Verifying the WebAssembly Specification

According to the author of this second paper, what are the main advantages of the mechanized specification? 
- They can now guarantee, through their proofs, that the type system is sound in a way that would not be possible for a “light-weight" specification. They also discovered some issues during it's implementation.

Did it help improving the original formal specification of the language?
- Yes, they detail how our work on this proof has exposed several issues with the official WebAssembly specification, influencing its development.

What other artifacts were derived from this mechanized specification? 
- Thanks to this they can offer a verified executable interpreter and type checker, which the paper-based formalisation did not.

How did the author verify the specification? Does this new specification removes the need for testing?
- They successfully passes all core language conformance tests available in the WebAssembly repository, the also conducted differential testing
of our executable interpreter against several major WebAssembly engines.
- No it doesn't mean that no failures could happen.





