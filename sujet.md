# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

TP1 de Axel MARTIN et Théo VINCENT

### 1.
Le 4 juin 1996. Après 36,7 secondes de vol, les fortes accélérations produites par la trajectoire de la fusée Ariane 5 provoquent un dépassement d’entier dans les registres mémoire du système de guidage inertiel principal, qui est immédiatement mis hors service. Le système de guidage de secours, identique au système principal, subit les mêmes effets et s'arrête à la même seconde. Le pilote automatique, qui s'appuie sur les informations de ces systèmes de guidage inertiel, n'a alors plus aucun moyen de contrôler la fusée.
La fusée a explosé à 4 000 mètres d'altitude au-dessus du centre spatial de Kourou, en Guyane française. Aucune victime n'est à déplorer, mais les quatre satellites de la mission Cluster qui se trouvaient à bord de la fusée sont partis en fumée lors de l'explosion.
C'était un bug global, une erreur de réutilisation. Un composant fonctionnel pour Ariane 4 avait été réutilisé.
Coûts engendrés : 370 millions de dollars.
Si on avait testé dans les bonnes conditions, en ayant recalculé les valeurs pour Ariane 5 et cet ancien composant, on aurait vite remarqué l'erreur.

### 2.
Il y avait un problème dans la bibliothèque "util", plus précisément dans la fonction MapUtils.toProperties, https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-516?filter=doneissues.

Le bug identifié était le suivant : lorsqu'on appelait la fonction MapUtils.toProperties avec une map contenant une entrée null, cela provoquait une erreur de type NullPointerException. Cependant, la javadoc de la fonction ne mentionnait pas ce comportement et indiquait plutôt que la fonction renverrait une Properties null. Ainsi, il s'agissait d'un bug global, surtout si l'on considère que l'utilisation d'entrées null dans une map est courante.

Deux solutions étaient envisageables pour résoudre ce problème : soit clarifier explicitement ce comportement dans la javadoc, soit ajouter une vérification pour traiter les entrées null. Les contributeurs ont jugé que la solution de la vérification masquerait les utilisations incorrectes de la fonction, et par conséquent, seule la javadoc a été modifiée pour refléter ce comportement inattendu.

Compte tenu de ces modifications visant à "résoudre" le bug, il est probable qu'aucun test n'ait été ajouté pour valider ces changements.

### 3.
Quelques exemples d'expériences courantes chez Netflix :

a) Mettre fin à des instances de machines virtuelles.

- Variables observées :
    Temps de redémarrage des instances.
    Impact sur la disponibilité des services.
- Principaux résultats :
    Évaluation de la résilience des services face à la perte d'instances.
    Mesure de la récupération et de la répartition de la charge de travail sur d'autres instances.*

b) Injecter de la latence dans les demandes entre services

- Variables observées :
    Temps de réponse des services.
    Taux d'erreur ou de défaillance des requêtes.
- Principaux résultats :
    Évaluation de la tolérance aux retards des services.
    Analyse de la performance des mécanismes de réplication et de récupération des données.
  
c) Faire échouer les demandes entre les services

- Variables observées :
    Taux d'échec des requêtes.
    Temps de récupération des services.
- Principaux résultats :
    Vérification de la résilience des services face à des défaillances.
    Évaluation de la fiabilité des mécanismes de détection et de gestion des erreurs.
  
d) Faire échouer un service interne

- Variables observées :
    Impact sur les autres services dépendant du service en panne.
    Temps de reprise ou de basculement vers des mécanismes de secours.
- Principaux résultats :
    Mesure de la capacité à isoler les pannes et à éviter la propagation des erreurs.
    Évaluation de la robustesse des solutions de basculement.
  
e) Rendre indisponible une région Amazon entière

- Variables observées :
    Temps de basculement vers des régions de secours.
    Impact sur la disponibilité globale des services.
- Principaux résultats :
    Évaluation de la résilience face à des défaillances majeures d'infrastructure.
    Vérification de l'efficacité des stratégies de récupération et de reprise sur des sites distants.

Netflix n'est certainement pas la seule organisation à utiliser une telle méthode. Google, Apple et Amazon, parmi tant d'autres, font appel à cette pratique. 
On pourrait imaginer chez eux des coupures des serveurs afin d'observer la récupération des données, la synchronisation des appareils et les réactions des apps, ou bien des retards entre centres de données pour évaluer la latence des services, les erreurs des requêtes et la migration du trafic.

### 4. 

Il y a plusieurs avantages principaux à avoir une spécification formelle pour WebAssembly:

Interopérabilité: Avoir une spécification formelle signifie qu'il existe une définition précise et non ambiguë du langage WebAssembly. On garantit l'interopérabilité entre les différentes implémentations de WebAssembly. Les développeurs peuvent être certains que le code fonctionnera de la même manière partout.

Sécurité: La spécification formelle permet d'analyser plus en détail les propriétés de sécurité du langage, ce qui est important étant donné qu'il s'agit d'un langage de bas niveau exécuté dans les navigateurs. Cela aide à identifier et corriger les vulnérabilités potentielles.

Ouverture: Quiconque peut implémenter WebAssembly en se basant sur la spécification ouverte, ce qui favorise l'innovation et la concurrence. De nouveaux outils et environnements d'exécution peuvent être développés indépendamment.

Bien que la spécification formelle soit importante, cela ne signifie pas que les implémentations de WebAssembly ne devraient pas être testées. Des tests rigoureux sont toujours nécessaires pour s'assurer que chaque implémentation est conforme à la spécification et ne présente pas de bugs. C'est la seule façon de garantir la qualité et la fiabilité du langage.

### 5.

Selon l'auteur du document, un point positif majeur de la specification mecanisée serait la possibilité de vérifier automatiquement si la specification est respectée, avec par exemple des artefacts comme un interpreteur executable, ou bien un type-checker exectutable. Cela permet donc d'accélerer grandement cette vérification. Il est aussi expliqué que la mécanisation permet d'améliorer plus facilement la spécification, et facilite également la détection d'erreurs dans celle-ci. De nombreux autres points positifs sont exposés dans le document.
On a effectivement pu améliorer la spécification grâce à cette mécanisation grâce au repérage facilité d'erreurs (on trouve un exemple Figure 9), rendant la spécification plus "sound" (sensée, fiable), mais également la collaboration a été rendue plus simple par rapport à la spécification "manuscrite".
Cette mécanisation a été validée à l'aide de tests de conformités officiels WebAssembly et du fuzzing.

Néanmoins, tout cela ne supprime pas la nécessité de tester. Bien qu'une meilleure qualité de code soit garantie par cette spécification améliorée, on ne couvre pas tous les scenarii possibles, qui peuvent être aussi basiques qu'une simple erreur d'implémentation, ou autre.
