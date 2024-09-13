# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. 

Roundoff Error and the Patriot Missile
   
Le bug dans le système de missiles Patriot était causé par une troncature de la représentation binaire du temps dans l'arithmétique en virgule flottante. Le système suivait les missiles Scud à l'aide d'une horloge interne qui mesurait le temps en dixièmes de seconde sous forme d'entiers. Cependant, en raison des limites d'un registre de 24 bits, la conversion du temps de l'horloge en virgule flottante était inexacte. En particulier, la représentation binaire non terminée de 0,1 seconde (en base 2) provoquait une erreur de timing légère mais cumulative à mesure que le système fonctionnait plus longtemps. Cette erreur augmentait proportionnellement au temps écoulé. Après 100 heures de fonctionnement, l'écart de temps atteignait 0,3433 secondes, ce qui était suffisant pour qu'un missile Scud parcoure plus d'un demi-kilomètre et échappe à l'interception.


En fait, le bug est considéré comme local dans le sens où il affectait spécifiquement la conversion du temps dans certains points du programme Patriot. Il ne touchait pas toute l'infrastructure informatique du système, mais se manifestait dans les calculs de synchronisation précis qui étaient cruciaux pour le suivi des missiles.


L'échec s'est manifesté lorsqu'un missile Scud a frappé les casernes de l'armée américaine à Dhahran, en Arabie Saoudite, entraînant la mort de 28 militaires. L'erreur de suivi a empêché le missile Patriot d'intercepter correctement le Scud entrant.


Pour les clients (forces militaires) : Cet échec a soulevé des inquiétudes importantes quant à la fiabilité du système de défense Patriot, en particulier dans des situations de guerre à fort enjeu. L'erreur a révélé que le système n'était pas conçu pour gérer les missiles balistiques à haute vitesse qu'il a rencontrés lors de la guerre du Golfe, entraînant une perte de confiance dans le système.
Pour l'entreprise (Raytheon, le contractant derrière le Patriot) : Le bug a nui à la réputation de Raytheon en tant que contractant de défense, car le système a échoué lors d'une mission critique. De plus, il y a eu un examen public sur l'efficacité des systèmes de défense militaire et sur la question de savoir si des tests appropriés avaient été effectués avant le déploiement de ces systèmes en zone de guerre.


Rétrospectivement, des tests dans les bonnes conditions — en particulier pour de longues périodes de fonctionnement combinées à des scénarios d'interception de missiles à haute vitesse — auraient probablement révélé le bug. Comme l'erreur de synchronisation devenait significative uniquement après de nombreuses heures de fonctionnement continu, des simulations ou des tests en temps réel reproduisant ces conditions auraient pu signaler l'erreur cumulative avant le déploiement du système.

2. 

[COLLECTIONS-766] fix ci build (or to say, fix everything that wrongly blocked the ci build.)

Le bug est global car il est lié à la dépendance à une version de jdk plus récente que la version utilisée dans le code.
Une fonction a été mise à jour dans la nouvelle version de jdk mais le code n'avait pas été mis à jour en conséquence.
Ainsi les modification apportées permettent de passer de l'ancienne version (Math.floorMod(long,int)) à la nouvelle (Math.floorMod(long,long)) en forçant l'utilisation de variables de type "long". 
Le bug concerne directement un fichier de test, donc aucun ajout de test à été effectué.

3. 

Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

Expériences Concrètes :
- Chaos Monkey : Termine aléatoirement des instances de machines virtuelles en production pour s'assurer que les services peuvent gérer des pannes d'instances.
- Chaos Kong : Simule la panne d'une région entière d'Amazon EC2 afin de vérifier que les systèmes de Netflix peuvent supporter des pannes à grande échelle en redirigeant le trafic vers des régions saines.
- Tests d'injection de pannes (FIT) : Introduit des pannes de réseau et des latences entre les services de Netflix pour observer comment le système se dégrade de manière progressive sous contrainte.

Exigences des Expériences :
Les expériences sont réalisées dans des environnements de production pour capturer des interactions réelles.
Le système est surveillé pour observer des mécanismes de repli (fallback), où les services non-critiques se dégradent de manière progressive (par exemple, le contenu personnalisé revient à des valeurs par défaut raisonnables).

Variables Observées :
Les démarrages de flux par seconde (SPS), qui suivent le nombre d’utilisateurs qui commencent un flux vidéo chaque seconde. Le SPS est utilisé comme indicateur de la santé globale du système.
La charge CPU, la latence, et les performances au niveau des services pour détecter des dégradations.

Principaux Résultats :
Les ingénieurs de Netflix ont développé des systèmes robustes où les services peuvent se dégrader de manière progressive en cas de pannes.
Les pannes critiques du système ont été réduites grâce à une expérimentation continue, et les services non-critiques peuvent tomber en panne sans avoir un impact significatif sur l'expérience utilisateur.

Netflix n’est pas la seule entreprise à réaliser ces expériences. Des entreprises comme Amazon, Google, Microsoft et Facebook ont adopté des approches similaires pour vérifier la résilience de leurs systèmes en cas de panne​.

Dans d'autres secteurs (par exemple, les services financiers, le commerce électronique), les expériences de chaos pourraient se concentrer sur différentes métriques critiques, telles que :

Le taux de complétion des transactions dans une institution financière ou le taux de complétion des commandes dans le commerce.
La simulation de pannes comme des pannes de bases de données, des retards de réponse d'API, ou des pics inattendus de demandes utilisateurs pourrait aider à valider la résilience.
L'observation de la capacité de traitement des transactions, des temps de réponse, ou des taux d'erreurs peut mettre en lumière des goulots d'étranglement et des dépendances des services sous contrainte.

4.

Avantages d'une spécification formelle pour WebAssembly:

-  Sécurité: Elle garantit que le code s'exécute en toute sécurité et empêche des accès non autorisés à la mémoire.
-  Portabilité: Le code fonctionne de manière cohérente sur toutes les plateformes compatibles WebAssembly, quelle que soit l'architecture matérielle.
-  Validation rapide : Le code peut être vérifié rapidement et efficacement, assurant qu'il est correct avant son exécution.
-  Performance: WebAssembly peut être optimisé pour fonctionner à des vitesses proches du code natif.


Faut-il encore tester les implémentations ?

Oui, les implémentations WebAssembly doivent toujours être testées. Même avec une spécification formelle, des bugs peuvent apparaître dans le compilateur, le runtime ou dans l'interaction avec d'autres systèmes. Les tests garantissent la performance, la compatibilité et la robustesse des implémentations.


5. 

Avantages:
- Vérification formelle : La spécification mécanisée fournit une description précise et vérifiée par machine de WebAssembly, plus fiable que les méthodes traditionnelles de formalisation sur papier.
- Artefacts exécutables : L'auteur a développé un interpréteur exécutable et un vérificateur de types pour WebAssembly, garantissant la solidité du système de types.
- Identification des problèmes : Le processus a révélé plusieurs insuffisances dans la spécification originale de WebAssembly, telles que des aspects incorrects du système de types, qui ont ensuite été corrigées par le groupe de travail WebAssembly.

Améliorations:
Oui, la spécification mécanisée a permis d'améliorer la spécification formelle originale. Des problèmes tels que des erreurs dans le système de types et des problèmes de propagation d'exceptions ont été corrigés grâce aux informations obtenues lors de la mécanisation.

Artefacts Dérivés:
- Interpréteur exécutable : Un interpréteur exécutable vérifié pour WebAssembly a été dérivé de la spécification mécanisée, permettant ainsi des tests et une validation face aux implémentations industrielles.
- Vérificateur de types : Un vérificateur de types a été développé, vérifié et optimisé pour répondre aux attentes des implémentations industrielles.

Vérification:
La spécification a été vérifiée à l'aide de preuves mécanisées dans Isabelle, notamment la démonstration de la solidité du système de types de WebAssembly, avec les propriétés de préservation et de progression.

Supprime-t-elle le Besoin de Tests ?
Non, la nouvelle spécification n'élimine pas le besoin de tests. Bien que la vérification formelle offre des garanties solides, les tests restent nécessaires pour valider l'implémentation dans des scénarios réels et par rapport aux normes industrielles​