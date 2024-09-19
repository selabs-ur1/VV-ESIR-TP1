# CHAUVEL Tom & JOSSO Célia, ESIR2 Informatique SI, VV : Practical Session 1 

# 1. Article reporting the discovery of a software bug

We have chosen to find an article about the Crowdstrike bug. Here is the [link](https://www.theverge.com/2024/7/19/24201864/crowdstrike-outage-explained-microsoft-windows-bsod) of the article. 

## 1.1. Description of the bug

Crowdstrike company is one of the leader to help companies to prevent security breaches. This bug was due to an update being pushed without any verication on every Windows operating system where Crowdstrike was installed on the 19th of July 2024. As Crowdstrike has access to the kernel, the damages were critical.

## 1.2. Manifested failures

The impacted systems were stuck in a boot loop with an error message that says, “It looks like Windows didn’t load correctly” while giving users the option to try troubleshooting methods or restart the PC.

## 1.3. Repercussions

### 1.3.1. For client/customers

Crowdstrike has around 29,000 customers, with more than 500 on the list of the Fortune 1000. So a lot of companies were impacted, it means some of the biggest airlines, TV braodcasters and other essential companies.

Globally, 8.5 millions of Windows based machines were impacted. 

### 1.3.2. For the company

The company lost its credibility because it decided to make an update firstly on a friday : the worst day of the week as it is just before the weekend, right before an important holidays departure and also before the begginning of the Olympic Games. 

For that, Crowdstrike's finances have decreased by 68 percents. (Source [here](https://www.google.com/finance/quote/CRWD:NASDAQ?sa=X&ved=2ahUKEwjIz6Sxv7-IAxX1TaQEHSvZKK4Q3ecFegQIRBAf&window=6M))

## 1.4. Local or global bug ?

This is a global bug because it impacted a lot of systems all around the world for at least one hour in the best scenario. Also, other company weren't directly impacted by that, but as some suppliers were, they were too.

## 1.5. Is testing would have helped ?

According to this (document)[https://www.lemondeinformatique.fr/actualites/lire-une-entree-de-trop-a-l-origine-du-bug-de-crowdstrike-94462.html], tests weren't made as the program tried to access to a 21th value in a set of 20. And it seems pretty logical that testing would have help to spot this ridiculous bug.

# 2. Apache Commons projects tracking systems

## 2.1. Bug that has been solved

Here is the bug that we studied : 

[COLLECTIONS-697 : JavaDoc for FixedSizeList should warn that modifying underlying list is still allowed and is not prevented](
https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-697?filter=doneissues)

## 2.2. Bug explanation

A list called _B_ is constructed from a list called _A_ thanks to a method from the _FixedSizeList_ class. This list is supposed, according to its noun, to be fixed. 

However, if we add an element in the _A_ list, the _B_ list is also modified. It brings confusion with the noun of the method.

## 2.3. Local or global 

This is an example of a global bug since it can impact a lot of developers. Furthermore, as no test was provided to clearify the behaviour of the function, a code modification could lead to confusion and potentially break a lot of programs.

## 2.4. Provided solution

Here is the [Pull Request](https://github.com/george-ranjan/commons-collections/pull/1) that illustrates the provided solution.

The JavaDoc has been updated to explicit this observation and explain the behavior of the method to be sure that no similar issue will be seen in the future. 

## 2.5. Presence of tests by the contributors

A test has been added to ensure that the behavior of this method will not be changed in the future.

# 3. Netflix Chaos Engineering

## 3.1. Concrete experiments they perform

Here all some concrete experiments Netflix performs by using Chaos Engineering :
- Terminate virtual machine instances
- Inject latency into requests between services
- Fail requests between services
- Fail an internal service
- Make an entire Amazon region unavailable

## 3.2. Requirements for these experiments

To practice a Chaos Engineering, here are some of the requirements :
- Make a hypothesis (it means define how the system should behave under the given failure conditions).
- Determine the region of the test
- Have a services based system like AWS in order to test each services efficiently

## 3.3. What are the variables they observe

The observed variables are :
- The **SPS** (starts per second). That is the most important indicator for them because it permits to see if the behavior of the users is stable or not.
- The hypothesis depends on the **region**. In fact, an experiment does not likely have the same impact in Europe and in North America.
- **finer grain metric** such as CPU load or time to complete a database query. To understand better the interaction between users and the system

## 3.4. What are the main results they obtained

These experiments helped improve system resilience and fault tolerance. Netflix found weaknesses and reinforced its infrastructure by simulating failures and their metrics permit to understand their users' behavior, so that make them confident in handling real-world failures more effectively.

## 3.5. Is Netflix the only company performing these experiments? 

Based on this tool (https://litmuschaos.io/), we learnt that chaos engeneering is used for exemple by Orange, RedHat (IBM), VMWare (BroadCom), or Kubesphere which are companies or tools that are using big scale infrastructures. 

## 3.6. Adapting Experiments to Other Organizations: Types and Key Variables

When adapting chaos engineering experiments to other organizations, the process could be :

1. Build a hypothesis around steady state behavior
2. Vary real­world events
3. Run experiments in production 
4. Automate experiments to run continuously

# 4. WebAssembly Formal Specification

## 4.1. Should WebAssembly implementations not be tested?

A formal specification is useful in order to get the same behaviour on different operating systems, architectures and web browsers. It is also useful for each language in order to follow the same specific guidelines.

So the implementation has to be tested, otherwise the program won't run exactly the same. Moreover, WASM isn't meant to be platform-specific, so testing is necessary in order to get the same output on each platform.

# 5. Mechanized specification of the language using Isabelle

## What are the main advantages of the mechanized specification?

The mechanized specification helps to find problems in the formal implemention as it proves it in a mathematical and theorical way (Constructive Approach). 

## Did it help improving the original formal specification of the language?

They have identified issues and have helped in fixing a bunch of those.

## What other artifacts were derived from this mechanized specification?

From this, they have provided a verified executable interpreter and type checker.

## How did the author verify the specification?

The author properly formulated lemmas as no one was created before.

## Does this new specification removes the need for testing?

No, this is just proving the specification, not testing the implementation. And also, as Isabelle is not bug-free, we can't be 100% sure that there is no issue in the theory. 
