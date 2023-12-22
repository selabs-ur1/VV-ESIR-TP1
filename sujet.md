# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

## Rapport sur un bug logiciel

- - En mars 2023, la fonction historique des conversations de ChatGPT à été hors-ligne pendant quelques jours car un incident a eu lieu. En effet, certains utilisateurs avaient accès aux informations d’autres utilisateurs. Le bug a exposé de brèves descriptions des conversations d'autres utilisateurs. Même si le fait d’exposer des informations des conversations est un problème, on a trouvé que le bug exposait également des informations de paiement des utilisateurs de GPT Plus, l’abonnement que l’on peut souscrire chez OpenAI.
Il a été estimé qu’environ 1,2% des utilisateurs de GPT Plus auraient été touchés, ayant leurs noms, prénoms, adresse mail et dernier numéros de carte bancaire exposés.
Bien qu’il s’agisse d’un bug majeur, OpenAI n’a pas subi de conséquences importantes à la suite du bug. Je pense que le test de multiples scénarios aurait pu éviter cet incident. En effet, malgré la criticité du bug, OpenAI est resté assez discret et peu informatif sur leur site dédié : status.openai.com. Ils ont néanmoins été assez réactifs et ont corrigé le bug rapidement, en détaillant les suites de test qu’ils ont effectué pour s’assurer que le bugfix était effectif.

##Revue de correction de bug

Le bug sur lequel je me suis renseigné est un bug critique qui a été corrigé en Novembre 2018. Lien vers l’historique : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-701?filter=doneissues
Le bug provenait d’une récursion infinie, causant le plantage du programme. Pour corriger cela, une condition de retour a été ajoutée afin de sortir de la boucle. En plus de la correction du bug, une suite de test a été rajoutée dans le même commit (dans un fichier test préexistant).
Lien vers le commit : https://github.com/apache/commons-collections/pull/57/commits/be0cea3c907bb4ab1083384377521942d17e15bd
Je pense qu’il s’agit d’un bug local car le problème provient de l’implémentation de la fonction. Le bug provient du fait qu’elle n’ait pas de condition de retour, ce qui la fait boucler indéfiniment, causant un StackOverflowError.
- Comme mentionné précédemment, le développeur à l’origine du fix a également ajouté une suite de tests pour vérifier que cette erreur soit détectée plus facilement si elle était amenée à revenir.

##Chaos Engineering

- Afin de s’armer contre les interruptions de service, Netflix utilise un service interne appelé Chaos Monkey. Ce service sélectionne de manière aléatoire les instances de machines virtuelles qui hébergent leurs services de production et les shutdown. Cela force les ingénieurs à concevoir des technologies qui sont résistantes aux erreurs et qui sont capables de se rediriger vers les services qui fonctionnent toujours.
Pour faire fonctionner une méthode pareille, il faut comprendre ce que “fonctionner” signifie pour les services proposés. Pour le cas de Netflix, il s’agit principalement du nombre d’utilisateurs en ligne. Grâce aux données qu’ils récoltent, les ingénieurs de chez Netflix peuvent mieux suivre l’évolution des services (en suivant les courbes d’utilisateurs en fonctionnement normal notamment) en fonction des mesures qu’ils prennent. Ils utilisent le nombre de streams par seconde pour valider le caractère stable de leur service. Comme décrit dans le document, d’autres domaines pourraient utiliser différentes metrics pour caractériser la stabilité des services. 
- On pourrait imaginer différents scénarios pour des services en ligne. Par exemple, on pourrait introduire intentionnellement de la latence réseau ou encore isoler des segments du réseau pour tester leur résistance à l’échec. De même, on pourrait s’intéresser aux bases de données et rendre leur accès momentanément indisponible pour tester la gestion d’erreur du service. Ensuite, de nos jours les applications utilisent de plus en plus des API tierces et il serait intéressant de les rendre indisponible. Toutes ces méthodes seraient à approfondir avant de les ajouter. Il faut surtout établir les conditions de stabilité du service pour que cette méthode d’ingénierie soit efficace.

##Webassembly
Le texte fourni est un extrait de l'article intitulé "Bringing the Web up to Speed with WebAssembly" rédigé par Andreas Haas, Andreas Rossberg, Derek L. Schuff et Ben L. Titzer. Cet article explore le développement et les fonctionnalités de WebAssembly, un bytecode de bas niveau conçu pour répondre aux défis d'efficacité et de sécurité du code sur le Web.

Voici les principaux avantages d'avoir une spécification formelle pour WebAssembly, tels que décrits dans le texte :
Sécurité :
- WebAssembly vise à fournir un environnement d'exécution sécurisé pour le code mobile sur le Web. Une spécification formelle garantit que le langage est sûr à exécuter, protégeant contre les menaces de sécurité pouvant provenir de sources non fiables.

Efficacité :
- Le code de bas niveau généré par des langages tels que C/C++ est généralement optimisé en amont pour le code machine natif. WebAssembly est conçu pour une exécution rapide, permettant aux développeurs d'atteindre des niveaux de performance comparables à du code natif.

Portabilité :
- WebAssembly est indépendant du langage, du matériel et de la plateforme. Cela signifie que le code écrit en WebAssembly peut s'exécuter de manière cohérente sur différentes architectures de machines, systèmes d'exploitation et navigateurs, offrant un haut niveau de portabilité.

Déterminisme et Facilité de Compréhension :
- La sémantique formelle de WebAssembly rend le langage déterministe et facile à comprendre. Ceci est essentiel pour les développeurs qui ont besoin de comprendre et de prédire le comportement de leur code.

Interopérabilité avec la Plateforme Web :
- WebAssembly offre une interopérabilité simple avec la plateforme Web, permettant une intégration transparente avec les technologies web existantes.

Quant à la question de savoir si le fait d'avoir une spécification formelle signifie que les implémentations de WebAssembly ne devraient pas être testées, la réponse est non. Bien qu'une spécification formelle contribue à assurer la correction et la clarté de la conception du langage, elle ne garantit pas l'absence de bugs d'implémentation. Les tests restent cruciaux pour vérifier que les implémentations réelles respectent les spécifications et pour identifier et résoudre les problèmes éventuels pendant le processus de mise en œuvre. Les spécifications formelles fournissent une base solide, mais des tests dans des conditions réelles sont essentiels pour valider les implémentations pratiques du langage.

##Isabelle
Le document décrit les efforts déployés pour automatiser et vérifier la spécification de WebAssembly à l'aide d'Isabelle, un outil de preuve formelle. Conrad Watt, de l'Université de Cambridge, présente une spécification mécanisée de WebAssembly dans Isabelle, accompagnée d'un interpréteur exécutable et d'un vérificateur de types vérifiés. L'objectif est d'améliorer la spécification formelle initiale du langage, qui reposait principalement sur une formalisation papier.
Les principaux avantages de cette spécification mécanisée incluent la possibilité de mener des preuves formelles, d'identifier des erreurs dans la spécification officielle de WebAssembly et d'influer sur son développement. L'auteur met également en avant les problèmes révélés par la preuve de type, qui ont conduit à des corrections dans la spécification officielle.
En plus de l'interpréteur et du vérificateur de types, l'auteur a soumis l'interpréteur à des tests différentiels par rapport à des implémentations industrielles, renforçant ainsi la fiabilité du mécanisme spécifié.
En résumé, la spécification mécanisée contribue à accroître la vérifiabilité et la fiabilité de WebAssembly en fournissant des preuves formelles et en identifiant d'éventuelles erreurs dans la spécification du langage. Toutefois, bien que la spécification mécanisée renforce la confiance dans la correction du langage, elle ne remplace pas la nécessité de procéder à des tests.

