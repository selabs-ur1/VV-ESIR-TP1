# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. Nous allons dans cette question parler du bug / exploit que représente la manipulation de RNG (random number generator) dans les jeux vidéos en s’attaquant au sujet de pokemon https://www.thegamer.com/pokemon-rng-manipulation-explained/

Pour cela il faut commencer par adresser la manière dont la RNG est créé au sein d’un outil informatique. Puisqu'on ne peut pas créer de réel nombre aléatoire informatiquement, on va faire de notre mieux pour simuler celle-ci à l’aide de différentes valeurs au sein du jeu / logiciel prises séparément les unes des autres afin de générer une suite de nombre qui crée une clé. Par exemple pour le cas de pokémon on peut utiliser le nombre de pas du joueur, ou encore l’ordre et le timer avec lesquels on réalise des interactions avec certains personnages non joueurs. C’est pour ça qu’en réalisant des tests empiriques (ou en analysant le code directement si possible) on peut déterminer différents facteurs permettant de créer cette clé afin de pouvoir derrière modifier ces valeurs afin de pouvoir manipuler la valeur de la RNG elle même.

Ce problème résultant de la réutilisation de différentes plages de données au sein du code est donc un bug global.

Ce type de bug n’est pas nécessairement un problème pour l’entreprise car il n’impacte pas les ventes du jeu ni l'expérience du joueur lambda qui s' il ne souhaite pas utiliser ces éléments ne sera jamais réellement confronté.

Néanmoins cela change du côté utilisateur pour une catégorie de joueur qui pratique le speedrun, un domaine du jeu vidéo consistant à se lancer dans une compétition afin de finir un jeu le plus vite possible. Dans ce domaine la manipulation de RNG devient vitale et même un moteur permettant de rendre le jeu plus intéressant, devenant non plus une prouesse de patience à essayer d’obtenir aléatoirement le résultat le plus optimale mais une épreuve technique consistant à réussir à manipuler ces valeurs en réalisant un chemin précis dans un temps donné. Les meilleurs exemple sont Pokémon diamant et perle sorties en 2006 qui utilisent notamment l’horloge interne de la console ainsi que le nom du joueur pour définir la clé originale du jeu et rendent le choix même des statistiques (valeurs lié à la puissance d’un pokémon) du pokémon de départ possible. Ce petit bug qui n’a que peux d’impacte pour le joueur lambda est donc bien une part vitale pour cette discipline.

Néanmoins si ce bug n’est pas majeur sur ces implications, game freak l’entreprise derrière pokémon est tout de même impactée par celui-ci en raison de leurs politiques très strictes qui veulent qu’un pokémon demande un certain niveau d’effort pour l’obtenir allant même à bloquer des “shiney”, pokemon dont la couleur est modifiée derrière des événements hors du jeu nécessitant parfois de mettre de l’argent dans cet événement (film, achat dans un magasin, etc…), malgré le peux d’impacte quel apporte corriger ce bug est donc quelque chose qu’ils voulaient résoudre.

Corriger un tel bug n’est néanmoins pas facile, et à l'époque des premiers jeux cela n'était même pas possible car celui-ci est là pour des raisons prévues par les développeurs, en effet cette valeur aléatoire doit bien être créée quelque part. Il à donc été présent de 1996 à 2017 sortie de pokémon ultra soleil et ultra lune. Néanmoins au cours de ces années on remarque que la manipulation est de plus en plus légère allant de manipuler l'entièreté du jeu à simplement modifier quelques interactions de temps à autre. Cependant cela change avec les dernières génération sorties sur nintendo switch la dernière console de jeu de nintendo. Si certaine manipulations sont faisable en manipulant la date du jeu sur pokémon épée et bouclier sorties en 2019 à partir de pokémon écarlate et violet sorties en 2022 cette possibilité à complètement disparue grâce à l’apparition d’un nouvel outil appelé “Crypto Secure”, cet outil provenant de la console elle même permet une sécurité complète sur la génération du nombre aléatoire, créé hors du jeu et qui est désormais impossible à déterminer même à l’aide d’outils externes.

Finalement ce bug ne pouvait pas jusqu’à quelque année être évité car celui-ci nécessitait une interaction directe avec la console. Néanmoins il est vrai que certaines facettes de ce bug aurait pu être éviter avec des tests préalables comme le fait de pouvoir manipuler l'entièreté d’un des jeu de la license à l’aide de l’horloge de la console en ajoutant d’autres paramètre pour créer la clé en lançant le jeu comme cela à été fait pour les jeux précédent et les sequels. Malgré tout, cette solution reste une percée dans le domaine, donnant des pistes sur l’utilisation de la console pour la création de l’aléatoire qui sera perfectionnée par la suite. 

2. Nous nous sommes interessé à la pull request #115 corrigeant un bug dans la classe Flat3Map lors de l’appel de la méthode remove() décrite ci-dessous.

    public void remove() {
        if (currentEntry == null) {
            throw new IllegalStateException(AbstractHashedMap.REMOVE_INVALID);
        }
        currentEntry.setRemoved(true);
        parent.remove(currentEntry.getKey());
        nextIndex--;
        currentEntry = null;
    }
Celle-ci à été corrigée par la pull request de cette manière :

    public void remove() {
        if (currentEntry == null) {
           throw new IllegalStateException(AbstractHashedMap.REMOVE_INVALID);
        }
        # currentEntry.setRemoved(true);
        parent.remove(currentEntry.getKey());
        currentEntry.setRemoved(true); # ←– new
        nextIndex--;
        currentEntry = null;
    }

La solution à ce bug est simple ce qui montre que même sur un logiciel tel que Apache une erreure est vite arrivée. C'est en effet une simple inversion de deux ligne car on a les fonction setRemoved() et getKey() de la classe static FlatMapEntry définies telle que :

        void setRemoved(final boolean flag) {
            this.removed = flag;
        }

        et

        @Override
        public K getKey() {
            if (removed) {
                throw new IllegalStateException(AbstractHashedMap.GETKEY_INVALID);
            }
            switch (index) {
                case 3:
                    return parent.key3;
                case 2:
                    return parent.key2;
                case 1:
                    return parent.key1;
            }
            throw new IllegalStateException("Invalid map index: " + index);
        }

Avec setRemoved(true) qui set la valeur de currentEntry.removed à true ce qui fait que currentEntry.getKey() utilisée dans parent.remove qui vérifie la condition :

            if (removed) {
                throw new IllegalStateException(AbstractHashedMap.GETKEY_INVALID);
            }


Et renvoie donc une exception.

En inversant ces lignes l’ordre est donc finalement respecté.

Ce problème est global car même si la classe statique est définie au sein de Flat3Map.java, le bug vient d’une erreur de compréhension sur l'ordre de fonctionnement de cette classe et donc de l’interaction entre deux classes (ou un simple problème d'inattention).

Finalement le contributeur à bien rajouter un test dans la classe Flat3MapTest :
    public void testEntrySet() {
        final Flat3Map<K, V> map = new Flat3Map<>();
        map.put((K) "A", (V) "one");
        map.put((K) "B", (V) "two");
        map.put((K) "C", (V) "three");
        Iterator<Map.Entry<K, V>> it = map.entrySet().iterator();

        Map.Entry<K, V> mapEntry1 = it.next();
        Map.Entry<K, V> mapEntry2 = it.next();
        Map.Entry<K, V> mapEntry3 = it.next();
        it.remove();
        assertEquals(2, map.size());
        assertEquals("one", map.get((K) "A"));
        assertEquals("two", map.get((K) "B"));
        assertEquals(null, map.get((K) "C"));
    }

Qui correspond au code qu'il a utiliser pour trouver le bug. Cela respecte donc bien le principe de l'analyse analytique qui dit que chaque erreur doit être integrée en tant que cas de test.

3. Netflix est un outil de visualisation de vidéo utilisé partout dans le monde à n'importe quel moment. Il est donc important pour cette entreprise de s'assurer que ce système fonctionne constamment ou du moins, qu'un problème n'affecte pas l'expérience des utilisateurs.
Cependant il est nécessaire de tester le système pour s'assurer qu'il fonctionne correctement, or il est impossible de simuler un serveur pouvant égaler celui de Netflix. La solution adoptée par Netflix est donc de travailler directement sur son serveur en production en y injectant des bug afin de vérifier si cela affecte les utilisateurs de manière considérable ou non.

Plus concrètement, le chaos engineering consiste tout d'abord à déterminer des variables à partir desquelles un état stable du système peut être déduit. Une de ces variables appelé "SPS" pour "stream Start Per Second", en effet cette variable connaît globalement les mêmes valeurs d'une journée à une autre. Ainsi, si ces valeurs ne correspondent pas à ce qui est habituellement observé, cela veut dire qu'il y a sûrement un problème. Une autre métrique pouvant aider à déterminer le bon fonctionnement du système est une augmentation de la latence des requêtes ou encore le nombre de nouvelles inscriptions par secondes. Ces métriques ont un réel lien avec le fait que le système est fonctionnel ou non.
Une fois ces hypothèses établies, il est nécessaire de déterminer les "événements" à injecter dans le système, en tentant de penser à tous les stimulus qui pourraient affaiblir le système. Netflix fait par exemple en sorte qu'un service interne ne fonctionne plus ou que des requêtes entre différents services échouent. Il peuvent également faire en sorte qu'une région AWS ne soit pas accessible.

Un exemple d'expérimentation donné par Netflix illustre l'utilisation de la métrique SPS afin de vérifier qu'une erreur dans le service de "marque page" n'affecte pas fortement cette dernière. Pour cela, ils sélectionnent un petit pourcentage d'utilisateurs qu'ils divisent en deux groupes. Un groupe de contrôle, qui ne recevra aucun changement, ainsi qu'un groupe d'expérimentation. Des simulations d'erreur dans le service de "marque-page" seront injectées dans ce deuxième groupe. Puis le SPS des deux groupes est observé pendant un certain laps de temps en supposant que cette métrique soit plus moins égale dans les deux groupes. Suite à cette expérience, les ingénieurs de Netflix peuvent déterminer si le système arrive à gérer différentes variables pouvant apparaître, ou si ce dernier possède des faiblesses qui nécessitent une amélioration. Dans notre cas, si le SPS des deux groupes reste équivalent, alors il semblerait que le système soit correct.

Malgré le fait que Netflix soit les fondateurs de cette technique de test, il semblerait que d’autres entreprises aient repris la méthode tel que Amazon, Google, Microsoft ou encore Facebook.

4. What are the main advantages of having a formal specification for WebAssembly ?

Tout d'abord cela permet d'avoir un code précis et rigoureux qui permet des facilités de lectures du côtés lecteurs / développeurs notament grace à ces structures de codes (Structures if, structures de boucle). Cela donne donc une compréhension relativement uniforme du code.
Cette lecture uniforme se traduit également côté système notament du côté des navigateurs qui peuvent traiter le code de la même manière peut importe le navigateur mais egalement être facilement portable exterieurement au domaine du web.
Le dernier point est sur la vérification facile et automatique du langage. La spécification formelle permet en effet des verifications plus rapide et plus sures.

Néanmoins il reste nécessaire de toujours tester et verifier le WebAssembly car comme l'a dit Djikstra, le test permet de detecter la présence et l'absence de bug
Par exemple un bug majeur sur la Meltdown ( une vulnérabilité matérielle découverte exclusivement dans les microprocesseurs Intel x86 qui permet à un processus non autorisé l'accès privilégié à la mémoire ) à marqué l'évolution du WebAssembly en 2018 provoquant une grande faille de sécurité pour un langage qui se veux poiurtant sécurisé.

5. Les avantages de la spécification mécanisée sont la précision et la rigeur qui permet à l'aide d'Isabelle d'eliminer plus d'erreurs humaines sur la spécification formelle qui est censée êre sure.
Elle permet ainsi de complementer le resultat obtenue avec le WebAssembly en assurant que le modèle est bien respecté. Cela à egalement permis de corriger certaines ambiguités contenues dans l'originale.

Encore une fois il reste nécessaire de toujours tester et verifier le code car on ne peux pas prouver l'absence de bug.

L'utilisation d'Isabelle à egalement permis d'être utilisée en temps que preuve sur différentes propriété du modèle comme la sureté de type ainsi que sur l'efficacité du WebAssembly.

L'auteur a ainsi utiliser Isabelle pour verifier la spécification en encodant la sémantique même du langage et en prouvant qu'elle respecte bien les propriétés qui lui etaient attribuées.