# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. Un exemple d'article récent : 
La panne informatique mondiale du 19 juillet 2024  causée par une mise à jour défectueuse du logiciel de sécurité Falcon Sensor de CrowdStrike. 


Description du bug :
Le bug a été causé par une mise à jour défectueuse du logiciel de sécurité Falcon Sensor, utilisé pour protéger les systèmes Windows. Cette mise à jour a introduit une anomalie qui empêchait les systèmes de démarrer correctement et de se connecter au réseau. 


Nature du bug : 
⇒ Bug global : 
- Des milliers de systèmes Windows utilisant le logiciel Falcon Sensor ont été touchés.
- Interaction complexe entre la mise à jour et le système, affectant tout le réseau
→ dysfonctionnements critiques à grande échelle partout dans le monde.


Conséquences du bug : 
    *Impact sur les utilisateurs :
        -Interruption des services : Les systèmes Windows ne pouvaient plus démarrer, causant des pertes financières importantes. 
        -Ce bug a paralysé de nombreux secteurs, notamment les compagnies aériennes (des avions ont été cloués au sol), les banques, et les marchés financiers..
    *Impact sur l’entreprise (CrowdStrike) :
        -Perte de confiance : Les clients doutent de la fiabilité de ses solutions.
        -Des coûts de réparation importants pour créer des correctifs et gérer les réclamations des entreprises touchées.


Spéculation sur les tests : Les tests qui auraient pu éviter le bug
    -La relecture du code, tout simplement
    -Les tests unitaires (tester chaque module indépendamment des autres)
    -Les tests d’intégration (suivant un ordre pour bien intégrer les classes les unes après les autres, s’assurant qu’elles interagissent sans problèmes)
    -Les test système , en tant qu’ensemble pour vérifier qu’il fonctionne correctement dans son environnement final
    -Tests de Régression : Voir si les nouvelles modifications ont introduit de nouveaux bugs. Ce qui est crucial pour les mises à jour logicielles !!!



2. Identifier les bugs

Nom du bug : A potential misleading comment

Ce bug concerne un problème dans la documentation JavaDoc. La bibliothèque concernée s’appelle Apache Commons Collections. Dans la situation présente, le commentaire associé à la méthode ne correspond pas au comportement réel de la méthode. Résultat : les développeurs sont induits en erreur. Il s’agit d’un bug global car il affecte tous les utilisateurs de la librairie Apache Commons Collections. Pour corriger cette erreur, il suffit de modifier la documentation JavaDoc de la manière suivante : 
@throws IndexOutOfBoundsException if index < 0 or index >= size().
Ainsi, le commentaire est cohérent avec le comportement de la méthode. Comme il s’agissait d’un bug lié à un commentaire mal rédigé, aucun test n’a été mis en place.


3. Netflix et le Chaos Engineering
( Selon le papier intitulé “Chaos Engineering” publié par Netflix dans IEEE Software et l’article “The Business Case for Chaos Engineering” publié par la IEEE Computer Society )


Types d’expériences : 
    -Simulation de pannes régionales (simuler la défaillance d’un centre de données) pour tester la gestion du trafic si une région entière tombe.
    -Injection de latence (ajouter de la latence entre les services) pour observer l’impact sur les performances.
    -Suppression partielle de données (supprimer des données) pour vérifier la récupération. 
    -Défaillance fonctionnelle (provoquer des erreurs aléatoires dans certaines fonctions) pour évaluer la gestion des erreurs.
    -Tests de charge (augmenter artificiellement le trafic) pour simuler des charges élevées et tester la capacité du système.


Variables observées : 
    -Disponibilité du service : Capacité du système à rester opérationnel malgré les pannes.
    -Taux de réponse : Temps de réponse du système sous stress.
    -Taux d’erreur : Fréquence des erreurs pendant les tests.
    -Résilience des données : Capacité à récupérer et maintenir l'intégrité des données après des pertes.


Résultats obtenus : 
→ Meilleure résilience (Correction des faiblesses avant qu'elles ne causent des pannes)
→ Moins d’interruptions (Réduction des temps d'arrêt)
→ Performances améliorées (Optimisation pour mieux gérer les charges élevées)


Netflix est-elle la seule entreprise à réaliser ces expérimentations ?
→ Non, Netflix n'est pas la seule à utiliser le Chaos Engineering. D'autres entreprises : Google, Facebook, Amazon ..

→ Applications dans d'autres organisations :
    *Systèmes bancaires :
        -Expériences possibles : Simuler des pannes de serveurs, des défaillances de base de données ou des interruptions de réseau.
        -Variables à observer : Disponibilité des services financiers, temps de réponse des transactions, sécurité et intégrité des données.
    *Secteur de la santé :
        -Expériences possibles : Tester la résilience des systèmes de gestion des dossiers médicaux électroniques en simulant des pannes de serveur ou des erreurs de données.
        -Variables à observer : Continuité de l'accès aux dossiers médicaux, temps de réponse des systèmes, et précision des informations médicales.
    * E-commerce :
        -Expériences possibles : Simuler des pics de trafic lors de promotions ou des interruptions de paiement pour tester la gestion de la charge.
        -Variables à observer : Performance des sites web, temps de réponse lors des pics de trafic, et intégrité des transactions.
    *Fournisseurs de cloud :
        -Expériences possibles : Simuler des pannes de machines virtuelles, des interruptions de réseau ou des défaillances de stockage.
        -Variables à observer : Disponibilité des services cloud, temps de récupération des données, et capacité à gérer les pics de charge.



4. Les avantages d’avoir une spécification formelle pour WebAssembly sont : 
    → La rapidité d’exécution
    → Exécution sécurisée
    → Interopérabilité simple avec la plate-forme Web
    → Compact et facile à décoder
    → Facile à valider et compiler
    → Facile à générer pour les producteurs

**Code de bas niveau rapide**
Il est généré par un compilateur C/C++ et est généralement optimisé avant l’exécution.

**Portabilité**
Le Web s’étend sur de nombreuses classes d’appareils. Le code destiné au Web doit être indépendant du matériel pour garantir que les applications fonctionnent sur tous les types de navigateurs avec le même comportement.

**Code compact**
Le code transmis via le réseau doit être aussi compact que possible pour réduire les temps de chargement

**Sécurité**
La sécurité est indispensable sur le Web car le code provient de sources non fiables.

Les implémentations de WebAssembly devraient être testées, même si elles sont basées sur une spécification formelle. Voici quelques raisons pour lesquelles le test est nécessaire :
    → le code provient de sources non fiables,
    → des erreurs d’implémentation peuvent survenir
    → WebAssembly doit pouvoir être utilisé en combinaison avec d’autres langages comme C++ et Rust. Il faut vérifier que ces interactions se déroulent correctement.

