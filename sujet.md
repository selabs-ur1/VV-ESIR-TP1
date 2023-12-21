# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. ### Generale Motor 2014 

> In 2014, General Motors was accused of marketing cars whose ignition switch could, during a bump, cause the engine to stop completely (blocking power steering and preventing the deployment of airbags). This defect, known since 2005, by General Motors also earned it the subject of investigations by the Department of Justice, the financial markets gendarme, the Securities and Exchange Commission (SEC), and the United States Congress.

> The share of the software was significant. Although the initial defect resided in the defective physical design of the ignition switch, the software that monitored the operation of the switch contributed significantly to the serious consequences. The defective design of the switch could lead to an unexpected shutdown of the engine, disabling critical components such as airbags, in particular because of the weight of the keys or other factors. However, the software's response to this situation was also problematic. The software did not properly deploy the airbags in the event of an accident, which was a major safety failure. The software was responsible for monitoring the status of the ignition switch and taking appropriate measures in the event of a failure, which it did not do adequately in this particular case. As a result, software failure has contributed significantly to serious accidents and injuries, making the problem a complex incident involving both hardware and software aspects.

#### Local Or Global bug ?

> Rather than coming from a specific and localized error (Bug Local), some failures can emerge from the interactions of the modules that make up a system (Bug Global).

> Here we are facing a global bug due to unforeseen interactions between hardware, software and users. 

#### Describing the failure that manifested the presence of this bug

> The failure of the ignition switch could lead to an unexpected shutdown of the engine, cutting off the power supply to crucial components such as airbags, power steering and power brakes. However, the major failure was the inadequate response of the software responsible for monitoring the switch and activating security systems in the event of a failure.

#### Impact of the bug for consumers and for Generale Motor

> When a vehicle with the defective switch was involved in an accident, the software did not properly trigger the deployment of the airbags. This software failure had serious consequences, because the airbags were not deployed as they should have been to protect the occupants of the vehicle. As a result, many drivers and passengers have suffered serious injuries or even lost their lives in accidents that could have been less serious with a properly functioning safety system.
> General Motor lost a lot of money because of this bug, following the company's massive vehicle recalls and legal proceedings and government investigations.

> The failure of the software in this context has had a dramatic impact on vehicle safety, consumer confidence and General Motors' reputation. This also highlighted the crucial importance of software quality in the automotive sector and led to massive recalls, government investigations and reforms in vehicle design and testing practices.

#### My opinion about the test in this automotive context

> The concept of "testing the right scenario" involves evaluating the behavior of the software under normal and expected conditions of use of the product. In this automotive context, this would mean testing the functionality of the ignition switch and safety system in standard driving situations.
> If test scenarios of the right scenario had been correctly implemented, it would have been possible to detect anomalies in the operation of the ignition switch and in the responsiveness of the security software. This could have made it possible to identify potential failures earlier and to initiate preventive fixes before the vehicles were on the road.

> It is important to note that the detection of defects often depends on the quality of the test processes and vigilance in identifying relevant scenarios. Defects can sometimes escape testing, and in the case of the ignition switch problem, there were criticisms of General Motors' testing process and the management of information related to the problem.

2. ### Apache Commons projects :

> Among the solved bugs of Apache Commons Proposed, I chose the [COLLECTION-814](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-814?filter=doneissues).
>
> > Here, we are facing a local bug.

> *CollectionUtils* is a class of Apache commons that provides utility methods and decorators for collection instances. one of these methods is *removeAll* with the signature `public static <E> List<E> removeAll(Collection<E> collection, Collection<? > remove) {}`. this method Removes the elements in *"remove"* from *"collection"*.

> According to the documentation, *remmoveAll* must throw a NullPointerException (NPE) when one of the *"remove"* or *"collection"* parameters is null. Instead, we see that NPE is launched only if the *"collection"* parameter is empty.

> To solve this problem, contributors of this project used the `requireNonNull(T obj, String message)` method of the *Objects* class. This method checks that the reference of the specified object is not null and raises a custom NPE if this is the case.
> ```
> Objects.requireNonNull(collection, "collection");
> Objects.requireNonNull(remove, "remove");
> ```

> Pour s'assurer que le bogue est détecté s'il réapparaît à l'avenir, les contributeurs ont ajouté de nouveaux cas de tests à la suite de test.
> ```
> assertThrows(NullPointerException.class, () -> ListUtils.removeAll(null, new ArrayList<Object>()), "expecting NullPointerException");
> assertThrows(NullPointerException.class, () -> ListUtils.removeAll(new ArrayList<Object>(), null), "expecting NullPointerException");
> ```

3. ### Chaos Engineering :

> Chaos Engineering consists of experimenting on a distributed system to strengthen confidence in its ability to withstand turbulent production conditions. Netflix is particularly renowned for popularizing this discipline. For several years, Netflix has been using an internal service called Chaos Monkey, which randomly selects instances of virtual machines hosting production services and finishes them. Chaos Monkey's goal was to encourage Netflix engineers to design software services that can withstand the failures of individual instances. The success of this approach has prompted Netflix to expand the method of injecting failures into the production system to increase reliability. For example, they perform exercises called "Chaos Kong", simulating the failure of an entire Amazon EC2 region, and they also perform failure injection test (FIT) exercises where they induce failures in requests between Netflix services, thus verifying the system's resilience to these situations.
>
> These experiments are based on the following four processes :
>* Construire une hypothèse autour du comportement à l'état d'équilibre
>* Vary real‐world events
>* Run experiments in production
>* Automate experiments to run continuously
>
> The main requirement is that Chaos engineering is only active during normal working hours so that engineers can respond quickly if a service fails due to an instance termination. At Netflix, they are using Chaos Engineering to ensure that the system still works properly under a variety of different conditions. For this, there are certain variables that engineers observe. We can mention SPS (stream starts Per Second) which show how many users start streaming a video each second or new account signups per second.
>
> The SPS metric varies slowly and predictably over the course of a day, as shown in this screen :
>
> <img width="1223" alt="image" src="https://github.com/ZieAmara/VV-ESIR-TP1/assets/90223980/bf69c32a-420c-4b70-b417-a2340af6fb89">
>
> Other large companies such as Amazon, Google, Microsoft and Facebook also adopt the chaos engineering approach, it is not exclusive to Netflix. These organizations use similar techniques to assess the resilience of their systems.
>
> For companies offering services, chaos engineering proves to be an ideal method to test the resistance of their system. For example, an e-commerce site could choose to measure its system's ability to process a certain number of purchases per second.

4. ### WebAssembly :

> WebAssembly (Wasm) represents a binary instruction format intended for a stack-based virtual machine. It is designed to serve as a portable compilation target, adapted to programming languages, facilitating deployment for both client-side and server-side applications on the Web.
>
> The main objective of WebAssembly is to solve the challenge of providing low-level code that is safe, fast and portable on the Web. Previous solutions to this problem, from ActiveX to Native Client via asm.js, have failed to achieve the essential properties that a low-level compilation target should have:
>
```Js
.Safe, fast, and portable semantics:
	-safe to execute
	-fast to execute
	-language-, hardware-, and platform-independent
	-deterministic and easy to reason about
	-simple interoperability with the Web platform

.Safe and efficient representation:
	-compact and easy to decode
	-easy to validate and compile
	-easy to generate for producers
	-streamable and parallelizable
```

> There are many vulnerabilities. Which, due to common mitigation measures, are no longer exploitable in native binaries, are fully exposed in WebAssembly, but you can never be sure of a developer's error. It would therefore be wise to do tests.

5. ### The mechanized specification:

> The main advantage of the mechanized specification is the heart of mechanization is our definition of two inductive relationships, which correspond to the reduction and typing rules of the WebAssembly specification. These relationships are not directly executable, but we define separate executable functions for an interpreter and a type checker, and we prove that they are correct in relation to their corresponding relationship.
>
> The extension of our mechanization to model this feature revealed a gap in the WebAssembly specification that sabotaged the strength of the type system. This specification has therefore helped to improve it. Like many verified language implementations, these artifacts require integration with an external analyzer and linker to run as stand-alone programs, which introduces an unreliable interface.
>
> It is used to make our verified interpreter executable as a stand-alone program, because its internal state is more strongly based on the specification project. In particular, the formalization of paper stores the functions declared in WebAssembly programs within each instance itself. This is possible because the formalization only gives a summary description of the instantiation process. In the complete specification project, several instances can share the same store, and one can export a function that is imported by another. Instances cannot directly access each other, and therefore, a single copy of the function can be kept directly in the store, each instance retaining a reference to it.
>
> This new specification does not remove the need for tests because we have carried out differential tests of our executable interpreter compared to several large web assembly engines. This was done both in order to validate our interpreter and potentially to discover semantic bugs in commercial WebAssembly engines. The tests were generated using the CSmith tool [Yang et al. 2011], combined with the official Binaryen tool chain [WebAssembly Community Group 2017a] to convert the generated C tests into Web- Assembly. This imitates the way most WebAssembly programs will be produced "in the wild". No errors were found in our implementation or in any commercial engine, although a crash bug was discovered in the Binaryen tool chain itself, which was reported and fixed by the developer.


