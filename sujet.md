# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improve the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
1.En Juin 2000, Nike a connecté son nouveau logiciel d'analyse avec son système de commande obsolète. Il fallait jusqu’à une minute pour enregistrer une seule entrée dans le logiciel. Recevant des dizaines de millions de numéros de produits de Nike, le système plantait fréquemment. De nombreux bug ont été observés comme l’ignorance de commandes, la duplication de certaines ou la suppression de données de commande. ce qui a dupliqué des commandes et a généré 500 millions de dollars de pertes.
I2, la compagnie qui a développé le système de chaine d'approvisionnement de Nike, a vu ses actions subir une importante chute après avoir été accusée à tort de l'incident.


2.Ce bug représente une faille de sécurité qui permet d’exécuter des commandes utilisateurs sur des machines hôtes.
C’est un bug local en ce que le problème vient du code en tant que tel qui n’a pas prévu cet usage.
Ce bug utilisait une faille dans le code de désérialisation de la classe InvokerTransformer.
Pour régler ce problème, la sérialisation de la classe a été désactivée par défaut.

https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-580?filter=doneissues&orderby=votes+DESC%2C+updated+DESC


3.what are the concrete experiments they perform?
	Netflix a un service interne, Chaos Monkey, qui arrête aléatoirement des machines hôtes de la compagnie pendant les heures normales de travail, forçant les ingénieurs a designer des logiciels résilients.
	
what are the requirements for these experiments?
	Il faut spécifier les hypothèses, les variables indépendantes, les variables dépendantes, le contexte.
	
what are the variables they observe?
	Ils regardent combien d'utilisateurs commencent une vidéo par seconde, ce qu'ils appellent SPS (Starts Per Second).

what are the main results they obtained?
	Les ingénieurs de Netflix conçoivent efficacement des services capable de gérer les problèmes ainsi rencontrés.

Is Netflix the only company performing these experiments?
	L'article évoque le fait qu'Amazon, Google, Microsoft et Facebook utilisent des techniques similaires.

Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.
	Une expérience très similaire pourrait être mené chez Google, en s'intéressant au nombre de recherches par seconde (au lieu du SPS).


4. Avoir une spécification formelle permet d'assurer une plus grande fiabilité en vérifiant des propriétés clefs avec des méthodes rigoureuses. Cela rend WebAssembly fiable, mais aussi rapide, indépendant de la plateforme, du langage et du hardware, déterministe, et facile à compiler et de raisonner sur.
	WebAssembly doit quand même être testé, car si sa spécification est formellement écrite, son implémentation peut toujours contenir des erreurs ; être sûr de ce que doit faire le langage ne signifie pas que l'on est sûr de comment le langage le fait.


5. La spécification mécanisée a permis de mettre en évidence des défauts de la spécification de WebAssembly qui a été faite à la main, et celle-là est donc une amélioration de celle-ci.
Isabelle est accompagnée d’un interpréteur exécutable vérifié et d’un vérificateur de types qui ont aidé à vérifier la spécification de WebAssembly.
Cette spécification mécanisée, quoique apparemment meilleure que l’originale, ne libère pas de la nécessité de tester. En effet, même avec une spécification parfaite, un programmeur utilisant WebAssembly peut toujours faire des erreurs de son côté qui respectent à priori la spécification (pour un exemple des plus simples, il peut avoir mis un ‘-’ au lieu d’un ‘+’).


# TP fait en binôme (Lucas Lemaitre et Jean-Loup Moll)
