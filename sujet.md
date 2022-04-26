# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. 
Log4j est une bibliothèque logicielle utilitaire open source programmée en langage Java. Elle fait partie de Apache Logging Services, un projet de l'Apache Software Foundation.
Plutôt que de créer leur propre système de journalisation, de nombreux développeurs de logiciels utilisent l'open source Log4j, ce qui en fait l'un des packages de journalisation les plus courants au monde. Il est par exemple utilisé par des grandes sociétés telles qu’Apple (sur icloud),  Microsoft (Minecraft) pour ne citer que celles là. Il est également utilisé dans des millions d’applications Web.

Le bug de la bibliothèque de journalisation Java Apache Log4j  présente des risques pour de vastes pans d'Internet. La vulnérabilité du logiciel largement utilisé pourrait être utilisée par des cyber attaquants pour prendre le contrôle de serveurs informatiques, exposant potentiellement tout, de l'électronique grand public aux systèmes gouvernementaux et d'entreprise, à un risque de cyberattaque. Le bug est donc global.

Le problème avec Log4j a été remarqué pour la première fois dans le jeu vidéo Minecraft , mais il est rapidement devenu évident que son impact était beaucoup plus important.
Les attaquants peuvent inciter Log4j à exécuter du code malveillant en le forçant à stocker une entrée de journal qui inclut une chaîne de texte particulière. La façon dont les pirates le font varie d’un programme à l’autre, mais dans Minecraft, il a été rapporté que cela se faisait via des boîtes de discussion. Une entrée de journal est créée pour archiver chacun de ces messages, donc si la chaîne de texte dangereuse est envoyée d’un utilisateur à un autre, elle sera implantée dans un journal.

Dans un autre cas, les serveurs Apple ont créé une entrée de journal enregistrant le nom donné à un iPhone par son propriétaire dans les paramètres. Quoi qu’il en soit, une fois cette astuce réalisée, l’attaquant peut exécuter n’importe quel code qu’il aime sur le serveur, comme voler ou supprimer des données sensibles.

Un bon scénario de test aurait pu aider à trouver la faille d’autant plus qu’utiliser du texte pour exécuter un code est utilisé dans d’autres contextes comme sur les formulaires des pages web (Injection sql, XSS, etc.).

2. 
Le bug que nous avons trouvé est la COLLECTION-697 que nous considérons comme bug local. Il dénonce le fait que dans la JavaDoc, il n’a pas été mentionné que la modification d’une liste affecte la liste sous-jacente construite à partir de la première. Par exemple, ajouter un élément à liste principale engendre l’ajout d’un élément à la liste sous-jacente.
Pour résoudre ce souci, l’auteur a fait une pull request qui contient l’ajout d’une spécification dans JavaDoc qui mentionne clairement cette modification. 
Il a également ajouté un test pour éviter une future apparition d’un tel cas.

3. 
Voici brièvement les expériences effectuées.

- Définir un état qui montre que le système fonctionne correctement (normalement). Le 
  nombre d'utilisateurs présents par exemple

- Donner une hypothèse qui montrera que cet état est toujours normal dans les conditions de tests. 

- Simuler les défaillances réelles (les tests) telles que la panne d'un serveur, d'un service. 

- Essayer de voir la différence entre l'état qui se produit pendant la panne et l'état normal. Si les états sont approximativement pareils, le système se porte bien et les tests sont concluants.

- Enfin, il faut automatiser ce principe de manière à juste modifier les variables d'entrées.  Ce qui permettra donc de s'assurer que le système fonctionne bien au fil du temps et que les nouvelles fonctionnalités et ajouts ne l'affectent pas.

Netflix n’est pas la seule entreprise à effectuer ces expériences, le principe de ces expériences (Chaos Engineering) est utilisé par beaucoup d’autres.

D’autres organisations comme Youtube peuvent utiliser le même principe si ce n’est pas déjà le cas pour tester leur système. 
Youtube peut par exemple utiliser comme indicateur le nombre de visionnage par jour et comparer ce nombre à celui obtenu après des tests de pannes et autres pour évaluer leur système. 
Les sites de ventes en ligne et de publicités peuvent quant à eux utiliser respectivement le nombre d’achat effectués par seconde ou le nombre d'annonces vues par les utilisateurs par seconde.

4. 
En informatique, les méthodes formelles sont des techniques permettant de raisonner rigoureusement, à l'aide de logique mathématique, sur un programme informatique ou du matériel électronique numérique, afin de démontrer leur validité par rapport à une certaine spécification. Elles reposent sur les sémantiques des programmes, c'est-à-dire sur des descriptions mathématiques formelles du sens d'un programme donné par son code source (ou, parfois, son code objet).

Les méthodes formelles peuvent être utilisées pour donner une spécification du système que l'on souhaite développer, au niveau de détails désirés. Une spécification formelle du système est basée sur un langage formel dont la sémantique est bien définie 
Avantages : 
conduit à une conception particulièrement propre ce qui est déterministe et facile à raisonner,
sûr à exécuter , exécution rapide
indépendant du langage, du matériel et de la plateforme :  Le code destiné au Web doit être indépendant du matériel et de la plate-forme pour permettre aux applications de s'exécuter sur tous les types de navigateurs et de matériels avec le même comportement
interopérabilité simple avec la plate-forme Web : il est possible de lier plusieurs modules qui ont été créés par différents producteurs. Cependant, en tant que langage de bas niveau, WebAssembly ne fournit pas de modèle d'objet intégré.. C'est aux producteurs de faire correspondre leurs types de données à des nombres ou à la mémoire.
compacte et facile à décoder : Le code qui est transmis sur le réseau doit
être aussi compact que possible pour réduire les temps de chargement
facile à valider et à compiler

À votre avis, cela signifie-t-il que les implémentations WebAssembly ne doivent pas être testées ?
Les tests ne doivent pas être exclus car il peut y avoir des erreurs dans la spécification.

5. 
La spécification mécanisée Isabelle facilite la première spécification qui était manuelle en évitant les erreurs. 
La nouvelle spécification ne nécessite pas de test puisque lors de sa conception plusieurs tests ont été effectués et ont été des succès.


