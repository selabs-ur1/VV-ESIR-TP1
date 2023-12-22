# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
1.
Bug : Bug logiciel sur TikTok réduisant le nombre d'abonnés à zéro

Description : Ce bug permet à toute personne ayant accès au code de TikTok et sachant comment le manipuler (comme un employé) de supprimer tous les abonnés d'un autre utilisateur sans leur permission ni leur connaissance.

Portée : Il s'agit d'un bug mondial.

Répercussions : Les utilisateurs dont le nombre d'abonnés a été réduit ne s'en rendraient même pas compte avant d'examiner ultérieurement leurs comptes, et même alors, seulement s'ils faisaient suffisamment attention pour remarquer ce qui s'est passé avec leur liste d'abonnés.

Entité responsable : TikTok

Spéculation de test : Je pense que vérifier les autorisations d'accès nécessaires pour utiliser le code de l'application serait utile pour prévenir ce bug.

2.Bug: CollectionUtils.retainAll() not throwing proper NullPointerException(NPE)
**Classification du bug**

Le bug est local car il ne touche qu'une méthode spécifique, CollectionUtils.retainAll(). Il n'affecte pas l'ensemble du système ou de l'application.

Explication du bug

Le bug se produit parce que la méthode retainAll() n'effectue pas de vérification appropriée de la nullité des paramètres. Lorsque le dernier paramètre, equator, est nul, la méthode ne lève pas d'exception NullPointerException (NPE). Cela peut entraîner des comportements inattendus et des erreurs dans d'autres parties du système qui utilisent la méthode retainAll().

**Solution au bug**

Les contributeurs du projet ont corrigé le bug en ajoutant un contrôle de nullité sur le paramètre equator dans la méthode retainAll(). Ils ont utilisé la méthode Objects.requireNonNull() pour lever une NPE si le paramètre est nul.

Les collaborateurs n'ont pas ajoutés de test mais le code à été refactoré.


3.
**Expérience concrète**

Netflix utilise Chaos Monkey pour simuler des pannes de serveurs en production. D'autres expériences incluent des attaques DDoS, des pannes de pods et de nœuds Kubernetes, ou l'injection de défauts spécifiques.

**Exigences**

Les expériences de chaos doivent être visibles, contrôlées et agiles.

**Variables observées**

Netflix observe la santé de l'application, le comportement du trafic, la santé du cluster et les journaux système.

**Résultats principaux**

L'ingénierie du chaos a permis à Netflix d'améliorer la résilience de ses systèmes, de réduire les temps d'indisponibilité et d'augmenter la sensibilisation des développeurs.

**Autres entreprises**

Google, Amazon Web Services et Microsoft Azure utilisent également l'ingénierie du chaos.

**Application dans d'autres organisations**

L'ingénierie du chaos peut être appliquée à tout type d'organisation, quel que soit son secteur ou sa taille. Les expériences et les variables à observer dépendront des systèmes et de l'infrastructure de l'organisation.

**Exemples**

Les sites Web de commerce électronique peuvent simuler des pannes de réseau, des pannes de passerelle de paiement ou des pics de trafic. Les services financiers peuvent simuler des attaques DDoS, des corruptions de bases de données ou des transactions qui ne parviennent pas à s'exécuter. Les systèmes de fabrication peuvent simuler des dysfonctionnements des équipements, des perturbations de la chaîne d'approvisionnement ou des pannes de capteurs.

4.

La spécification formelle de WebAssembly présente plusieurs avantages. Tout d'abord, elle offre une clarté et une précision, éliminant les ambiguïtés pour une compréhension précise du langage. En favorisant l'interopérabilité, elle assure une exécution cohérente sur divers navigateurs. De plus, elle facilite la vérification des compilateurs, garantissant la conformité aux règles du langage. La spécification formelle aide à détecter les bugs tôt, favorisant la fiabilité du code. Elle constitue une base stable pour l'évolution du langage. Cependant, malgré la spécification formelle, le test reste crucial pour valider les implémentations réelles, découvrant des bugs spécifiques à l'implémentation et assurant la robustesse dans divers environnements.

5.
 Selon l'auteur de cette deuxième étude, les principaux avantages de la spécification mécanisée sont qu'elle permet d'automatiser la vérification de la spécification, de détecter des erreurs dans la spécification initiale et de produire des artefacts de validation, tels que des preuves de correction et des énoncés de propriétés. La spécification mécanisée a également permis de corriger et d'améliorer la spécification initiale. L'auteur a vérifié la spécification en utilisant des preuves formelles dans Isabelle. La nouvelle spécification ne remplace pas le besoin de tests, mais elle permet de tester les implémentations de WebAssembly plus efficacement et de réduire le nombre de tests nécessaires.
