# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
### 1.
Le bug que nous avons trouvé provient de l’article de [CNET](https://www.cnet.com/tech/services-and-software/chatgpt-bug-exposed-some-subscribers-payment-info/), on apprend que le chatbot de OpenAI, ChatGPT a été victime d’un bug entraînant l’exposition des informations de paiement de certains abonnés. Ce bug a été découvert lorsque certains utilisateurs ont notifié qu’ils pouvaient voir les titres de l’historique de chat d’autres utilisateurs. Ce qui a entraîné l’exposition de d’informations personnelles des clients. OpenAI a déclaré que le nombre de personnes dont les données ont été révélées était extrêmement faible. Le bug a été causé par une bibliothèque open-source Redis client utilisée par OpenAI pour mettre en cache les informations utilisateur sur son serveur, ce qui fait de ce bug un bug global. Les clients concernés ont été informés que leurs informations de paiement ont peut-être été exposées. Cela pourrait avoir un impact négatif sur la réputation de l’entreprise et la confiance des clients dans ses services.

### 2.
Problème :
Le problème COLLECTIONS-781 était un bogue qui a été résolu dans le projet Apache Commons Collections. Le bogue était lié à la classe MultiMapUtils. Le bogue a causé la méthode getCollection de la classe MultiMapUtils de ne pas fonctionner correctement avec les objets MultiMap qui ont des clés nulles.
Alors, on faisait “return -1;” au lieu de renvoyer un objet de clé nulle.

Le bug a été classé comme un bug local, car il n’a affecté que la classe MultiMapUtils.
![](https://drive.google.com/uc?export=download&id=1g-H-bU3QTg1c_84An-oEFK-VjvE4G0mE)

Solution :
L'idée est de créer une constante qui représente le fait que l'indice recherché n'a pas été trouvé. Le La solution était de remplacer “return -1;” par “return CollectionUtils.INDEX_NOT_FUND;”

Tout en modifiant les classes suivantes:
``
src/main/java/org/apache/commons/collections4/list/AbstractLinkedList.java
src/main/java/org/apache/commons/collections4/map/LinkedMap.java
src/main/java/org/apache/commons/collections4/ListUtils.java
src/main/java/org/apache/commons/collections4/IteratorUtils.java
``
La figure ci dessous illustre le changement dans le cas de la classe : src/main/java/org/apache/commons/collections4/ListUtils.java

![](https://drive.google.com/uc?export=download&id=1KGt4MKlwq4L5W2X72J3ZUbqbVP_R6ASo)

vous pouvez voir le changement appliqué aux autres classes en visitant le lien ci-après: https://github.com/apache/commons-collections/pull/210/files

Il n’est pas clair si les contributeurs du projet ont ajouté de nouveaux tests pour s’assurer que le bug est détecté s’il réapparaît à l’avenir.

### 3.
Dans l’article publié en 2016, on apprend que Netflix réalise différentes expérimentations tel que :
``
Instance Termination : Chaos Monkey choisie un serveur et le shutdown afin de s’assurer de la bonne prise en charge des requêtes malgré le shutdown.
Chaos Kong Exercises : Ces exercices simulent des défaillances plus importantes, telles que la défaillance sur l’ensemble d’une région
Failure Injection Testing (FIT) : Les exercices FIT consistent à causer délibérément des défaillances ou à injecter des erreurs dans le système afin d’observer son comportement. Cela peut inclure de l’injection de latence dans les demandes entre les services, la provocation d'échecs de demandes, ou la rendant temporairement indisponible pour des services internes.
Geographic Failover Testing : L’objectif de cet exercice est de tester la capacité du système à passer d’une région à une autre en cas d'interruption d’une région.
Dependency Failures : Simuler des problèmes de dépendances telles que rendre un service inutilisable ou mettre des erreurs dans les réponses des services externes.
``

Leurs expérimentations consistent à :
Commencez par définir un "état stable" comme une sortie mesurable d'un système indiquant un comportement normal.
Formulez l'hypothèse selon laquelle cet état stable persistera à la fois dans le groupe témoin et dans le groupe expérimental.
Introduisez des variables reflétant des événements du monde réel, tels que des serveurs qui tombent en panne, des disques durs qui dysfonctionnent, des connexions réseau qui sont coupées.
Tentez de réfuter l'hypothèse en recherchant une différence d'état stable entre le groupe témoin et le groupe expérimental.

Netflix n’est pas la seule entreprise à utiliser cette approche, d’autres entreprises à l’échelle d’internet l'utilisent selon des discussions informelles.

On pourrait utiliser ce type d’expérience dans d'autres organisations tel que,

Sécurité :
``
Expérience : Simuler des incidents de sécurité, tels que des attaques DDoS ou des tentatives d'accès non autorisées.
Variables système : Évaluer la capacité du système à détecter et atténuer les menaces de sécurité, surveiller les temps de réponse et identifier les vulnérabilités potentielles.
``

Intégrité des données :
``
Expérience : Introduire des données mal formées ou incorrectes dans le système.
Variables système : Observer comment le système gère les entrées invalides, évaluer l'impact sur l'intégrité des données et l'expérience utilisateur.
``


### 4.

Le principal avantage d'avoir une spécification formelle pour WebAssembly réside dans le fait qu'elle fournit une définition précise et sans ambiguïté du langage. Cette clarté facilite la mise en œuvre du langage dans différents environnements et garantit un comportement uniforme sur toutes les implémentations.

Notre réponse est OUI. Car avoir une spécification formelle n'inclurait pas le fait que ses implémentations ne doivent pas être testées. Cette spécification ne peut pas assurer la correction des implémentations. Les tests restent nécessaires pour détecter et corriger les bugs dans les implémentations.



### 5.
Les principaux avantages de la spécification mécanisée sont que la spécification mécanisée est plus précise et moins sujette à l'ambiguïté, est plus modulaire et plus facile à entretenir ainsi que plus extensible et plus facile à mettre à jour que la première tentative de formalisation. De plus, la spécification mécanisée a contribué à améliorer la spécification formelle initiale du langage. Elle a mis en évidence plusieurs problèmes avec la spécification officielle de WebAssembly, ce qui a permis d'influencer son développement.
La spécification mécanisée a conduit à la création de plusieurs artefacts tels que la création d’un interpréteur et d’un vérificateur de type exécutables vérifiés pour le langage WebAssembly. Cela a conduit aussi à une preuve entièrement mécanisée de la cohérence du système de types de WebAssembly. Ainsi qu’à la réalisation d’un fuzzing différentiel de l'interpréteur par rapport aux mises en œuvre industrielles.
L’auteur vérifie la spécification en utilisant le theorem prover Isabelle/HOL.
La spécification mécanisée ne supprime pas le besoin de tests. Elle vient en complément de la première tentative de formalisation et offre une base plus fiable et plus extensible pour le langage WebAssembly.
