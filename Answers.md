# Practical Session #1: Introduction

Ce TP, ainsi que les autres, furent effectués dans un autre Repository. Je n'avais pas compris à la base qu'il fallait fork. L'historique des commits n'est donc pas à jour ici, mais vous pouvez aller voir dans ce repo Git : https://github.com/TheKingHydra/VV.git.

# Exercice 1. (Guillaume)
https://cointelegraph.com/news/thorchain-network-halted-following-software-bug

Un réseau virtuel de cryptomonnaie subit une panne après une mise à jour, pendant 15 heures. Un bug global dans le code fait que toutes les machines ont été affectées. C'est un problème de non-déterminisme entre plusieurs nœuds d'un graphe.
Une panne sur ce genre d'infrastructure bloque le réseau de monnaie, et peut donc avoir une influence négative sur les particuliers ou les entreprises qui ont un business basé sur celui-ci. De plus, une mauvaise réputation de cette technologie peut lui faire perdre de la valeur. Même avec des tests approfondis, ce bug aurait été particulièrement difficile à découvrir dans le code, car ce n'est pas un cas qui est censé apparaître dans ces circonstances.

# Exercice 2. (Hywel)  
https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-521?filter=doneissues Dans ce rapport de bug, il est question d'une erreur de typographie. Dans la fonction isEqualKey(entry,key1,key2), on return une valeur booléenne qui vaut (multi.size()==2) && (key1 == multi.getKey(0) || key1 != null && key1.equals(multi.getKey(0))) && (key2 == multi.getKey(1) || key1 != null && key2.equals(multi.getKey(1))). Ici, on vérifie que la taille est bien de 2, que la clé donnée en entrée key1 correspond bien à la première clé de multi et qu'elle est pas nulle, et que la clé key2 correspond bien à la deuxième. Cependant, il y a une typo et la vérification de key2!=null n'est pas faite. Cela cause un bug local?. La solution proposée et adoptée est de juste remplacer la dernière partie du booléen par (key2 != null && key2.equals(multi.getKey(1))) ce qui checkerai sa valeur. Un test a été ajouté pour vérifier si le deuxième paramètre est null, en renvoyant une NPE. Le test : MultiKeyMap<String, Long> map = new MultiKeyMap<String, Long>();
map.put("test", null, 2L);

# Exercice 3. (Hywel)
Netflix expérimente avec ses systèmes en utilisant un service interne appelé Chaos Monkey, qui ferme aléatoirement des machines virtuelles hostant les services. Cela leur permet de tester la fiabilité de leur système en simulant des coupures de serveurs. Pour mesurer l'impact de la fermeture aléatoire d'un serveur, ils utilisent une valeur appelée SPS (Start Per Second) qui correspond au nombre de vidéos ouvertes/lancées par seconde. Ils la comparent avec celle précédemment enregistrée afin de réperer une éventuelle baisse anormale de l'indice, ce qui signifierait que la fermeture de ce host a un impact énorme, et qu'ils ne sont pas préparés.
D'autres méthodes qu'ils utilisent sont l'injection de latence entre les requêtes inter-services, couper complètement un service (exemple : le bookmarking qui te permet de reprendre la vidéo là où tu t'étais arrêté, la vidéo sera donc relancée depuis le début), rendre une région Amazon complètement offline (en redirigeant les requêtes d'une région vers une autre).
Netflix n'est pas la seule organisation à mettre en place le Chaos Engineering. Amazon, Google, Microsoft et Facebook appliquent des techniques similaires pour évaluer leurs systèmes.
Amazon, peut-être, simule le shutdown d'une de ses régions. On peut supposer que Google fasse pareil. Facebook peut potentiellement shutdown une partie de sa messagerie, et Microsoft doit pouvoir shutdown une partie de sa zone de déploiement de mise à jour pour vérifier le comportement post problème.

# Exercice 4. (Guillaume)

Avoir une spécification formelle du langage permet de définir une structure solide dès le départ. On peut prouver certaines propriétés avant de se lancer dans la programmation de ce langage. De plus, cette description supprime toute ambiguïté quant à l'implémentation par plusieurs compilateurs ou interpréteurs de ce langage.
Cependant, cela n'implique pas qu'il ne faille pas tester. Une bonne spécification et un bon modèle n'empêchent pas les erreurs d'implémentation. Il se peut également qu'il y ait des erreurs dans des parties non définies par le modèle, comme les accès en mémoire ou les appels réseaux.

# Exercice 5.  (Guillaume)

La Spécification mécanique assure que tous les aspects de la spécification ont été définis et vérifiés par un logiciel, en rédigeant des preuves dans un langage spécifique (comme Isabelle dans ce papier). Cette technique a permis de mettre en lumière des spécifications non renseigné dans le 1er papier, comme l'interaction avec le host (des appels aux fonctions non web-assembly).
Dans tous les cas, les preuves ne permettent pas de s'abstenir de tests lors du développement. Un langage correct n'implique pas que le programme écrit avec ce langage se comporte comme voulu.
