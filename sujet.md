# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

**1.** Un bug a touché l’application bancaire de LCL en février 2021. Ce bug a touché 72 000 clients entre 17h40 et 18h40. Lorsqu’un client se connectait à son application, il n’avait pas accès aux opérations de son compte mais celles d’un autre client, sans pour autant avoir accès aux données personnelles de ces dits-comptes. Le bug est survenu à la suite d' une mise à jour, selon nous le bug devait être local et devait être quelque chose du genre que lorsque l’application récupérait l’identifiant de l’utilisateur dans la base de données elle ne récupérait pas le bon, et donc affichait les opérations de l’identifiant récupéré. 
On aurait pu testé le fait que l’identifiant espéré était égale à l’identifiant utilisé dans la requête, selon nous un test de ce genre aurait permis de détecter la faille.

**2.** Ce [bug](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-796?filter=doneissues) est local, en effet lors d’une modification quelqu’un a par inadvertance supprimé une ligne qui ne devait pas l’être. La méthode fonctionnait toujours cependant la ligne supprimée empêchait la méthode d’accomplir ce qu’elle était censée faire, le retour de la méthode était donc toujours incorrect par rapport à ceux qui était attendu. La solution a été simplement de recommit la ligne supprimée. Conjointement à la correction du bug, un test a été ajouté afin de prévenir que ce bug ne survienne une nouvelle fois, ce test vérifie que ce que retourne la méthode est égale à ce qui est attendu.

**3.** 

Expériences Concrètes Réalisées:
- Chaos Monkey : Termination aléatoire d'instances de machines virtuelles pour encourager la conception de services résistants aux pannes individuelles.
- Chaos Kong : Simulation de la défaillance d'une région entière d'Amazon EC2.
- Testing d'Injection de Défaillance (FIT) : Simulation d'échecs de requêtes entre services Netflix pour vérifier la dégradation gracieuse du système.

Exigences pour les Expériences:
- Les expériences doivent être menées dans un environnement de production réel.
- Doivent être automatisées et exécutées continuellement.
- Les hypothèses doivent être basées sur le comportement stable du système.

Variables Observées:
- SPS (Stream Starts Per Second) : Principal indicateur de la santé du système, reflétant le nombre d'utilisateurs démarrant un streaming chaque seconde.
- D'autres métriques fines comme la charge CPU, le temps de réponse des requêtes, etc., sont également surveillées pour identifier les impacts au niveau des services.

Principaux Résultats:
- Amélioration de la résilience et de la tolérance aux pannes du système.
- Meilleure préparation et réactivité face aux défaillances réelles.
- Les services Netflix sont conçus pour résister aux défaillances individuelles et maintenir l'opérabilité globale.

Autres Entreprises et Spéculation sur les Expériences dans Diverses Organisations:
- D'autres grandes entreprises technologiques comme Amazon, Google, Microsoft et Facebook utilisent des approches similaires.
- Dans d'autres organisations, ces expériences pourraient varier en fonction de la complexité et de l'échelle de leurs systèmes. Les variables à observer seraient également déterminées par les caractéristiques et les exigences spécifiques de chaque système, comme les métriques de performance, la disponibilité, ou la réactivité du service.


**4.**

Avantages d'une Spécification Formelle pour WebAssembly
- Fiabilité et clarté: La spécification formelle fournit une base solide pour comprendre et mettre en œuvre le langage de manière uniforme et prévisible. Cela contribue à un design propre et clair, réduisant les incertitudes liées à l'interprétation du langage.

- Déterminisme et Portabilité: WebAssembly vise à offrir une cible de compilation portable pour le code de bas niveau sans sacrifier la performance. La spécification formelle garantit un comportement déterministe sur différents matériels et systèmes d'exploitation, à l'exception de quelques cas spécifiques comme la gestion des NaNs, l'épuisement des ressources et les fonctions hôtes, qui peuvent présenter un comportement non déterministe.

- Sécurité et Efficacité: WebAssembly offre une représentation compacte, une validation et une compilation efficaces, ainsi qu'une exécution sûre avec peu ou pas de surcharge. La spécification formelle contribue à ces objectifs en fournissant des règles et des structures claires pour la conception et l'implémentation.

Implication pour le Test des Implémentations de WebAssembly
Bien que la spécification formelle de WebAssembly offre des avantages considérables en termes de clarté, de portabilité et de fiabilité, cela ne signifie pas que les implémentations de WebAssembly ne doivent pas être testées. Au contraire, les tests restent une étape cruciale dans le développement de logiciels pour plusieurs raisons :

- Couverture des Cas Non Déterministes: Comme mentionné, il existe des cas où le comportement de WebAssembly peut être non déterministe. Les tests sont nécessaires pour s'assurer que ces cas sont gérés correctement par les implémentations.

- Validation de l'implémentation: Une spécification formelle décrit ce que doit faire un langage, mais pas comment le faire. Les tests vérifient que l'implémentation concrète se conforme aux spécifications et fonctionne correctement dans divers scénarios d'utilisation.

- Détection des erreurs et des failles de sécurité: Même avec une spécification formelle, des erreurs de programmation et des vulnérabilités de sécurité peuvent se glisser dans l'implémentation. Les tests aident à identifier et à corriger ces problèmes avant la mise en production.

En conclusion, bien que la spécification formelle de WebAssembly fournisse une base solide pour la création d'une implémentation fiable et uniforme, les tests restent un élément indispensable du processus de développement pour garantir la qualité et la sécurité des implémentations.


**5.**

La mécanisation de la spécification de WebAssembly offre plusieurs avantages clés et a contribué à l'amélioration de la spécification formelle originale du langage. Voici les points principaux :


Avantages de la mécanisation de la spécification
- Preuve de solidité complète: Le travail a produit une preuve mécanisée complète de la solidité des propriétés du système de types de WebAssembly. Cela n'était pas disponible auparavant, ni sous forme mécanisée ni autrement​​.


- Identification et correction des déficiences: La mécanisation a révélé plusieurs problèmes dans la spécification officielle de WebAssembly, notamment des défauts qui rendaient le système de types initialement non solide. Ces découvertes ont conduit à des corrections nécessaires dans la spécification officielle​​.


- Dialogue constructif avec le groupe de travail: La mécanisation a facilité un dialogue constructif avec les membres du groupe de travail de WebAssembly, permettant la vérification et la mécanisation de nouvelles fonctionnalités au fur et à mesure de leur ajout à la spécification​​.


Artéfacts dérivés de cette mécanisation
- Interpréteur exécutable vérifié: Un interpréteur exécutable a été défini et vérifié, nécessitant une intégration avec un parseur et un lieur externes pour fonctionner en tant que programme autonome​​.
- Vérification expérimentale: La mécanisation a permis de valider expérimentalement la spécification en utilisant l'interpréteur exécutable, à la fois grâce à la suite de tests de conformité officielle de WebAssembly et par des expériences de fuzzing​​.

Vérification de la spécification
- La vérification de la spécification a été réalisée par induction sur les relations de réduction ou de typage. Ceci a nécessité l'établissement de nombreux lemmes auxiliaires, notamment dans les cas inductifs impliquant des contextes d'exécution récursifs et des flux de contrôle​​.


Implications pour le besoin de tests
- Même avec une spécification mécanisée et une preuve de solidité complète, la nécessité de tester les implémentations de WebAssembly reste. Les tests permettent de vérifier le comportement réel des implémentations dans divers contextes et scénarios, ce qui est crucial étant donné la complexité et la diversité des environnements d'exécution. De plus, les tests aident à identifier les problèmes pratiques et les différences entre la théorie et la pratique, comme le montre la nécessité d'intégrer l'interpréteur mécanisé avec un parseur et un lieur non vérifiés​​.


En résumé, la mécanisation de la spécification de WebAssembly a non seulement amélioré la spécification formelle existante, mais a également souligné l'importance des tests pratiques et de la validation expérimentale, même en présence d'une spécification formelle solide.
