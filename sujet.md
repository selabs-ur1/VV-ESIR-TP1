# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
1. La célèbre pressse [computeWorld](https://www.computerworld.com/article/2567198/eds--it-upgrade-caused-software-glitch-at-u-k--agency.html) a publié un article qui faisait mention d'un bug informatique en rapport avec la célèbre société Electronics Data System Corp (EDS) qui, aujourd'hui fait partie intégrante de la société HP entreprise. En 2004, le gouvernement britannique a mis en place un nouveau système complexe pour gérer les opérations l’Agence des pensions alimentaires pour enfants (ASE). Du 22 au 26 novembre 2004, le service Department for Work and Pensions (DWP) a été interrompu, dû à la misa à jour logicielle. En effet EDS a introduit un système informatique vaste et complexe dans le programme Child support Agency (CSA) alors que l'agence était en restructuration. Cependant le problème ne peut pas être entièrement imputés à la coomplexité du système informatique et l'incompatibilité de la struture de l'agence, mais à l'absence d'une gestion adéquate à également exagéré le problème. Il s'agit donc ici d'une erreur globale. Le système devait coûter environ 450 millions de livres sterling, mais a fini par coûter environ 768 millions de livres sterling au gouvernement. EDS a également annoncé une perte de 153 millions de dollars. Nous pouvons spéculer en disant que si nouveau système avait était sérieusement testé, avant son déploiement, il aurait pu être adapté à la structure de l'agence, ce qui aurait pu éviter toutes ces pertes d'argent.

2. Sur le site nous avons constaté un problème : [" BloomFilter: Converting Double to Int "](https://issues.apache.org/jira/browse/COLLECTIONS-817) la collection-817; Il s'agit ici d'un bug global, pour le coup le problème est dû au fait que BloomFilter se voyait souvant perdre certaine valeur vu qu'il est question pour elle de faire la conversion d'un double en entier. Elle rencontrait souvent des collisions. La solution apportée est de modifier la façon dont la conversion double en int est gérée pour éviter les pertes de précision. Non les utilisateurs n'ont pas ajouté de test pour s'assurer que le bug est détecté s'il réapparait à l'avenir.

3. Les expériences concrètes réalisées sont :
      - Utilisation de Chaos Monkey, on fait la sélection de manière aléatoire des instances de machines virtuelles hébergeant les services de production de Netflix et les termine.
      - Réalisation d'exercices Chaos Kong simulant la défaillance d'une région entière d'Amazon EC2.
      - Exécution de tests d'injection de défaillance (FIT) où des requêtes entre les services de Netflix sont délibérément causées pour échouer.
      - Utilisation d'hypothèses autour du comportement en état stable pour concevoir des expériences.

     les conditions requises pour ces expériences sont : 
      - Les équipes d'ingénieurs chez Netflix modifient fréquemment le code et les paramètres de configuration en production.
      - Les expériences sont conçues pour simuler des événements du monde réel, tels que des défaillances matérielles, des pics inattendus de demandes client
      - Netflix considère son système comme une collection de services fonctionnant comme un seul système distribué
   
      les variables observées et quels sont les principaux résultats obtenus sont :
       - Mesure principale de l'expérience : SPS (stream starts per second), qui indique combien d'utilisateurs commencent à diffuser une vidéo chaque seconde.
       - Les autres métriques fines observées pendant les expériences, telles que la latence des requêtes, l'utilisation du CPU
   
      Les principaux resultats obtenus sont : 
       - L'utilisation de Chaos Monkey a conduit les ingénieurs de Netflix à concevoir des services capables de résister aux défaillances d'instances individuelles.
       - L'extension de l'approche d'injection de défaillance à des scénarios plus complexes, tels que la simulation de la défaillance d'une région entière d'Amazon EC2, a amélioré la résilience globale du système.
       - Les équipes ont développé une intuition pour évaluer les fluctuations normales par rapport aux problèmes potentiels en observant la métrique SPS.
       - Les principes de l'ingénierie du chaos, tels que la formulation d'hypothèses, la variation des événements du monde réel, l'exécution d'expériences en production, et l'automatisation des expériences, sont     
          énoncés comme des pratiques clés de cette discipline émergente.
      
      Non Netflix, n'est pas la seule entreprise à utiliser cette technologie. Il y a aussi Amazon, Google, Microsoft et Facebook qui appliquent des techniques similaires pour tester la résilience de leurs systèmes
      
4. Les principaux avantages pour WebAssembly d'avoir une spécification formelle :                                                                                                        
       - Clean design :
         La spécification formelle permet une conception claire et détaillée. Cela évite les ambiguités et permet une implémentation plus aisée.

      - Semantique plus simple
	 " For example, JVM bytecode verification takes more
	 than 150 pages to describe in the current JVM specification,
	 while for WebAssembly it fits on one page "
	 
     - Semantique déterministe sur tout type de hardware et avec un coût d'exéution minime

     - Langage prouvé :  Cela nous assure la fiabilité, la sécurité et la conformité des programmes

     - Analyse formelle et verification
  
Les implémentations WebAssembly doivent-elles être testées ?                                                                                                                         
       Oui, les implémentations devraient être testées.
La spécification formelle de WebAssembly qui lui permet d'être un langage claire et sans ambiguité, et evite les divergences lors de l'implément nous prouve qu'il est bien conçu (Que le langage est bon). Cependant reste à savoir si le resultat de l'implémentation est ce que nous souhaitions (Qu'il s'agit du bon langage implémenté). 

 
5. Les principaux avantages de la spécification mécanisée :
   D'après l'article, la spécification mécanisée du langage webassembly a plusieurs avantages principaux. Tout d'abord elle permet de vérifier la correction de la spécifiation formelle originale du langage.

   a t-elle permis d'améliorer la spécification formelle originale du langage?
   De plus, elle a permis de découvrir plusieurs problèmes dans la spécification originale, ce qui a influencé son développement.

   Les dérivés de la spécification?
   Enfin, la spécification mécanisée a permis de dériver un interpréteur et un vérificateur de type exécutables vérifiés.

   Comment l'auteur vérifie la spécification?
   L’auteur a vérifié la spécification en utilisant la preuve de la correction du système de types de WebAssembly.

   La spécification supprime-t-elle la nécessité de procéder à des tests ?
   Cependant, cette nouvelle spécification ne supprime pas la nécessité de procéder à des tests 
   


