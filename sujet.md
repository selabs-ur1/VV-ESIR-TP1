# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### 1. Article about the discovery of a software bug

- Article : [Next – Firefox 112.0.1 corrige un bug gênant avec les cookies](https://next.ink/brief_article/firefox-112-0-1-corrige-bug-genant-avec-cookies/)
- Le [rapport de bug](https://bugzilla.mozilla.org/show_bug.cgi?id=1827669) sur le tracker de bug de Mozilla

Le bug a été trouvé après la mise à jour 112 de Firefox. Firefox nettoyait les cookies du navigateurs sans que l'utilisateur le souhaite. Les cookies ont eu leur date de validité changée. C'est un bug local.

Répercussions sur les utilisateurs : préférences et statut de login oubliés, c'est un peu gênant.
Probablement qu'un test précis aurait aidé, même si on imagine que Firefox est déjà bien testé (en effet : [Mozilla Code Coverage](https://coverage.moz.tools/)).

### 2. Apache Bug

- [Rapport de bug](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-709?filter=doneissues)
- [Pull request](https://github.com/apache/commons-collections/pull/66)

Le bug a été découvert en 2019. Il concerne la collection MultiSet qui, lorsqu'on a retiré le dernier élément, ne renvoie pas 0 comme nombre d'éléments lors de l'appel de la fonction getCount(). C'est un bug local puisque le bug vient d'une mauvaise programmation. La solution trouvée est d'affecter un 0 à un entier. Un test a été ajouté afin de vérifier la correction de ce bug.  

### 3. Chaos Engineering

Netflix effectue plusieurs manipulations comme faire échouer un service interne, rendre une région entière du cloud Amazon inaccessible ou encore ajouter de la latence dans les requêtes entre services.

Les variables observées sont par exemple les crashs des serveurs, les erreurs de disques.

Amazon, Google, Microsoft et Facebook utilisent des techniques de test de résilience pas trop différentes selon le papier.

Facebook pourrait l'utiliser pour tester comment le système gère les erreurs de latence, la congestion du réseau à travers la planète en observant les taux d'erreur et de réponse.

### 4. Web Assembly

Une spécification formelle permet aux développeur.euse.s de pouvoir faire des analyses statiques plus efficaces et donc de coder plus rapidement, efficacement et d'éviter beaucoup d'erreurs. Même avec une spécification précise et formelle, on ne peut pas se dispenser de faire des tests sur notre programme. 

### 5. Mechanized specification

Les principaux avantages de le spécification mécanisée sont :
- faire une preuve mécanisée de l'intégrité des types de WebAssembly (apparemment ça a permis de dévoiler plusieurs problèmes dans la spécification, qui a été améliorée depuis)
- un interpréteur d'exécutable vérifié et un vérificateur de typage distincts

La spec mécanisée a été vérifiée à l'aide des tests de conformité mis dans le dépôt WebAssembly (tests générés avec CSmith et convertis en WebAssembly avec Binaryen).

Non ça ne remplace pas le besoin de tester le langage. Il peut y avoir des aspects du langage ou des implémentations qui ne sont pas couverts par la spécification. Mais plus généralement, les tests peuvent montrer des erreurs du monde réel. En effet, l'auteur du papier parle de la pratique du fuzzing sur des implémentations réelles provenant de l'industrie.
