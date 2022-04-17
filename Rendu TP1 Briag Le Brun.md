# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

https://raygun.com/blog/costly-software-errors-history/
http://www.righto.com/2014/03/the-programming-error-that-cost-mt-gox.html

In 2011, someone lose lot of Bitcoin because a bug.
It's a local bug. In a normal script:
OP_DUP
OP_HASH160
f2e63314c350094550c703fcdcd4850ad37d8310(adress variable)
OP_EQUALVERIFY
OP_CHECKSIG

whereas the script of Mt gox look like:
OP_DUP
OP_HASH160
0
OP_EQUALVERIFY
OP_CHECKSIG

So they lost a thousand of bitcoins because of that.

If we testing an appropriate scenario, we could found this error.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-775?filter=doneissues

it's a global bug. Every time we call CollectionUtils.get(), it's instantiate a new Iterator. So there is a lot of iterator and the expected value could be compare with lot of possible values.

"This is a rather simple fix -- instead of adding CollectionUtils.get(expected, 0) and CollectionUtils.get(expected, 1) to a new HashMap (found) and compare the two maps, assert the 0 and 1 entries are either zeroKey=zero or oneKey=one." That's how they solved this issue.

They didn't make more test to verify this error in the future.

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

Netflix call that "Chaos Monkey", they randomly select a server of their service and shutdown him to see how the system react to a failures of individual instances. and now they extend the 
approach of injecting failures into the production system in order to improve reliability.
In result, Netflix see how the customers react, if they can continue to watch or not, if they log out or not. 
Amazon , Google , Microsoft , and Facebook used chaos engineering to test them system.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

WebAssembly addresses the problem of safe, fast, portable
low-level code on the Web.
the main advantages are:
  Safe, fast, and portable semantics:
    safe to execute
    fast to execute
    language-, hardware-, and platform-independent
    deterministic and easy to reason about
    simple interoperability with the Web platform
  Safe and efficient representation:
    compact and easy to decode
    easy to validate and compile
    easy to generate for producers
    streamable and parallelizable

No, even if it' a binary language, we need to test him if we want avoid bug ou unexpected behavior.

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

The main benefits of the mechanized specification are the creation of a mechanized proof for the strength properties of the type and the correction of the specification that was not strong in some cases.
Author successfully passes all core language conformance tests available in the WebAssembly repository but it does not mean we doesn't need test.
## Answers
