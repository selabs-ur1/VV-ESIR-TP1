# Validation and Verification: Practical Session #1

## Exercice 1

Voici l'article choisi pour le bug : [Google Home Bug](https://www.igen.fr/domotique/2023/01/une-faille-permettait-de-transformer-un-google-home-mini-en-micro-espion-134715). Ce bug concernait les produits Google Home, les enceintes connectées de Google. Par ce bug, un utilisateur externe au réseau wifi de l'enceinte pouvait s'y connecter et en prendre le contrôle. Ce bug est dit global car il exploite des intéractions entre les composants qui n'ont pas été pensées. 

Pour reproduire le bug, il fallait attaquer le réseau Wifi auquel l'appareil était connecté pour le déconnecter. Ensuite, l'attaquant devait se connecter à l'enceinte pour ensuite réussir à y lier son compte Google. Une fois cette étape réussie, l'attaquant pouvait envoyer toutes sortes d'ordres à l'enceinte depuis son smartphone par l'application Google : écouter les micros, manipuler les objets connectés à l'enceinte, récupérer certaines informations et ce par le biais d'injection de routines ou de petites application dans le Google Home. 

Ce bug représente donc une énorme faille de sécurité pour les utilisateurs du Google Home. Heureusement, celui qui a découvert la faille a prévenu Google, se faisant alors rétribué d'une prime de 100 000 dollars. A mon sens, ce bug aurait pu être évité si Google n'avait pas récupéré un OS non adapté (celui des télés Chromecast) comme base de l'OS du Google Home. Les ingénieurs Google aurait aussi pu mettre ce bug en évidence par certains tests d'intrusion sur ce scénario précis.

## Exercice 2  

Parmi les bugs résolus rapportés par Apache, nous avons choisi celui-ci : [Collection-802](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-802?filter=doneissues). On avait une classe AbstractReferenceMap<K,V> ayant un itérateur ReferenceBaseIterator. Le bug venait de la méthode hasNext() de l'itérateur qui ne fonctionnait pas correctement. En effet, le code changeait l’état de l’itérateur en modifiant les valeurs de la clé courante et de la valeur courante en null. Cette modification provoquait une erreur lors de la suppression du dernier élément d’une AbstractReferenceMap<K,V>. Les deux lignes de modification ont été supprimées et le bug était résolu. Un test a été ajouté pour vérifier le scénario qui faisait apparaître le problème. Cela a augmenté le coverage de l’application. 

## Exercice 3

Netflix utilise le "Chaos Engineering" pour tester la résilience de son système face aux problèmes qu'il peut rencontrer. Pour cela, les ingénieurs mettent à l'épreuve leur système par des expériences : éteindre une instance de machine virtuelle, mettre de la latence entre les services, injecter des erreurs dans les requêtes et les services ou encore rendre les serveurs d'une zone géographique indisponible. Mais avant de réaliser ces expériences il faut d'abord émettre des hypothèses sur le système et ensuite comparer avec les résultats. Ces résultats sont souvent extraits de l'observation de certaines variables car dire "le système fonctionne normalement" semble être trop peu précis. Ainsi on observe principalement le SPS (stream start per second), le nombre d'abonnements par seconde, la charge du CPU ou le temps pour résoudre une requête à la base de données.

On remarque que Netfix n'est pas la seule entreprise à utiliser le Chaos Engineering. En effet, il est sous-entendu dans le papier que beaucoup d'autres entreprises du numérique qui utilisent des "Large Distributed Systems" utilisent aussi ces méthodes, ex : Amazon, Google, Microsoft, and Facebook.

## Exercice 4

Avoir une spécification formelle permet de documenter le langage. Elle montre sa syntaxe, ses concepts de base (modules, fonctions, type de valeurs…), comment le langage fonctionne (gestion de la mémoire linéaire, structures de données, appels de fonctions…). La spécification peut également renseigner sur le type de validation effectué par le langage (statique et dynamique pour WebAssembly). Pour finir, nous pouvons connaître le format des fichiers du langage, son interopérabilité. 
Cependant, la spécification n’empêche pas de devoir réaliser des tests. En effet, le développeur est plus averti de l’utilisation du langage et de son fonctionnement, mais il peut toutefois effectuer des erreurs. Peu importe notre connaissance du langage, un programme peut toujours avoir des bugs locaux (code dupliqué, erreur dans une condition…) ou globaux (mauvaise réutilisation, bug de concurrence…). Ces erreurs viennent généralement d’une mauvaise conception/modélisation. Il est donc nécessaire de tester chaque programme, avec des tests unitaires, d’intégration et de système.

## Exercice 5

A partir de la spécification formelle, l’auteur est venu implémenter du code et vérifier s’il fonctionnait. Cela a pour objectif de tester la spécification et faire émerger des potentielles erreurs. Ce fut le cas, des erreurs sont apparus pendant la vérification et la spécification a dû être modifiée. La spécification mécanisée a créé deux éxécutables : un interpréteur et un type checker. Pour vérifier la spécification, l'auteur a utilisé le logiciel Isabelle, qui est un assistant de preuves et permet de démontrer intéractivement des théorèmes. Concernant la nécessité de tester son code, la réponse reste inchangée par rapport à la spécification formelle (voir réponse 4). Le developpeur est toujours susceptible de réaliser des erreurs et tester son code est obligatoire. 

Author : Alexandre LECOMTE et Sterenn LE HIR
