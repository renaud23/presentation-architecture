
# COMMENTAIRES
--------------

## SLIDE 1
  - le monolithe et les appli maison  

## SLIDE 2

## SLIDE 3
  définition
  ----------
  Une application monolithique et par extension une architecture monolithique est 
  une application dont l'ensemble du code et des fonctionnalités sont implémentés 
  dans un seul programme, au travers d'une seule base de données.

  commentaires
  ------------
  - l'ensemble des couches applicatives sont liées au travers d'une seule réalisation.
  - une base, le plus souvent relationnelle fédère l'ensemble de la donnée au travers d'un modèle métier unique.
  - un seul dépôt de code.
  - dans sa version la plus hard, une seule instance de déploiement.

## SLIDE 4
  - à l'institut, comme dans le reste du monde la situation dans la pratique est plus nuancée. 
  - un peu réducteur. Ne reflète pas la diversité du parc applicatif de l'institut.

## SLIDE 5
  - depuis quelques années déjà, de nombreuses petites application.
  - séparation physique de la couche présentation. Cette isolation franche facilite l'évolutivité du client de présentation, voir sa refonte intégrale (onyxia par ex).
  - le web-service conçu, s'il est conçu comme non couplé à l'ihm pourrait-être ouvert à d'autres usages, d'autres utilisateurs.
  - plus un monolithe au sens strict du terme.
  - toutefois toute la logique métier concentrée dans le même code et les mêmes données.
  - petit besoin, petite appli : au fond c'est suffisant pour pas mal de système.
  - toutefois, cette première expérience illustre déjà le bénéfice potentiel d'une architecture plus modulaire(même si ici on isole juste une couche)  

## SLIDE 6
  - la logique métier souvent répartit derrière plusieurs sous-modules : le back, le front, l'appli de collecte, ....
  - toujours une seule base de données et un seul modèle.
  - dans la pratique ça reste un monolithe ou chaque composant est prévu pour servir un système conçu et modélisé comme un seul bloc.
  - refaire un composant reste lourd et risqué pour les autres modules.

## SLIDE 7 : Les avantages
  commentaires
  -----------
  - c'est ce que l'on sait faire et ce à quoi on nous prépare au travers des formations attachés et contrôleurs
  - plus simple à développer (code et version) et à deployer. 
  - pas besoin de services transverses (broker, gestion de containers, ...)
  - moins coûteux : moins de fragmentation = moins de code = moins de travail(donc moins d'ETP), moins d'env, pas d'inv transversaux.

## SLIDE 8 : les limites
  commentaires
  ------------
  - gros ensemble ensemble de code avec une forte intrication des fonctionnalités devient toujours plus difficile à maintenir au fil du temps (correction et évolution) avec le temps.
  - redéploiement en production complexe, parfois pour quelques lignes seulement.
  - socle technique verrouillé à la conception.
  - obsolescence d'une pièce entraîne l'obsolescence de l'ensemble.
  - au final, une refonte tout les 5 ans = coûteux à long terme.

## SLIDE 9 : parties 2, principes et patterns
Un simple aperçu, bien d'autres existent

## SLIDE 10
  Principe de réalisation qui consiste à tracer des frontières pour isoler les grands ensembles fonctionnels ou techniques.
  - ce traduit en général par l'isolation d'élément cohérent d'information du système.
  => principe applicable à plusieurs degrés : même pour un monolithe dans un seul dépôt le principe est envisageable.

## SLIDE 11
  - Le 3 tiers est une séparation technique des couches applicatives, largement appliquée.
  - MVC, MBP, tout les patterns MV*

## SLIDE 12
   Séparation métier
  - isolation des logiques distinctes selon les spécificités propre à chaque acteur.
    isolation peut se formaliser :
      . au niveau du code par des règles de nommage de classes/packages, des modules autonomes (Séparation logique).
      . au niveau de l'architecture par des services isolées encapsulant données et fonctionnalités(Séparation physique).

## SLIDE 13
  Principe de réalisation selon lequel chaque module, classe ou fonction ne doit être porteur que d'une seule fonctionnalité et l'encapsule.
  Dit plus vulgairement un module, une classe ou une fonction ne fait " qu'une chose " 
  - SOC peut-être perçu comme un ensemble cohérent de responsabilités. 
  - principe bien connu des dev Java : une classe n'a qu'une seule responsabilité.
  - Au fond, tout est affaire de sémantique : c'est le concepteur qui pose les mots définissant les responsabilités.

## SLIDE 14
 - SOC et SRP : ce sont 2 principes coutumiers pour tout développeur, appliqués au quotidien dans l'écriture de code.
 - séparation en module souvent effective à l'Insee, mais sans séparation physique (des données).

## SLIDE 15
  Le pattern CQRS (Command Query Responsibility Segregation) repose sur un principe simple : 
  la séparation, au sein d’une application, des composants de traitement métier de l’information (“command” / écriture) et de restitution de l’information (“query” / lecture).
  (extrait d'octo talk : https://blog.octo.com/cqrs-larchitecture-aux-deux-visages-partie-1/) 
  - Grosso modo : on sépare les opérations de lecture des opérations d'écriture.
  - Selon ce principe, la représentation de la donnée dans un système efficace n'est pas unique :
    - une vision métier pour COMMANDS : facilitant la lisibilité et la compréhension des opérations d'écriture, jugées souvent plus complexes.
    - une vision orienté présentation pour QUERIES : accélérant le processus de restitution de la données, en passant le modèle pour l'affichage
      (dénormalisation de la data pour éviter de grosse jointure, souvent croisé des vues matérialisées ou des tables dont c'est la vocation)
  - Pattern libre d'implémentation. : s'applique à des degrés différents :
    - une petite application peu se contenter de séparer les responsabilités d'écriture au sein de classe distinctes
    - un gros système pourra formaliser des services et des systèmes de stockage de données distincts. 

## SLIDE 16
- à gauche les opérations d'écriture : une base construite autour du modèle métier
- à droite les opérations de lecture :  une base structurée autour des objets de présentation (DTO), un modèle métier orienté vers la
présentation.

## SLIDE 17 Event sourcing
l'ES est un pattern d'architecture dans lequel la trace de l'état de chaque
entité n'est pas assuré au travers d'une seule base relationnelle, mais en validant et conservant
l'ensemble des évènement qui affecte ces états, dans un store spécialisé.
- L'état courant est l'aggregation de tout les évènement, depuis un état initial donné.
- on ne conserve pas le solde de ton compte, mais l'ensembles des mouvements sur ton compte. Le solde c'est juste la somme de ces mouvements.
- pour les comptables, c'est un peu la différence qui sépare le bilan et le compte de résultat.

## SLIDE 18 : le pattern en lui-même
- command -> fonction de décision -> events -> fonction d'évolution -> state -> action ...

## SLIDE 19 

## SLIDE 20 partie 3 : l'architecture de microservices
  définition
  ----------
  Technique d'architecture et de conception logiciel où le système est structuré en un ensemble indépendant de services simples, 
  faiblement couplés les un les autres.
  - chaque fonctionnalité du système,
    - est isolé dans son propre runtime.
    - possède son propre système de données.
    - est conçu comme une application à part entière.
  - un mode d'organisation innovant : un service = une team (squad produit : une équipe auto-organisée. 
  (Les membres se sont choisis et décident de la construction du produit)

## SLIDE 21 un système plus distribué
- de la donnée éclatée entre les MS
- plusieurs petites application inter-agissent au travers du réseau (HTTP)
- on remplace les appels de méthodes/fonctions par des invocations réseau

## SLIDE 22 : vers une plus grande isolation
Les microservices pousse à l'isolation potentiellement jusqu'à l'extrême.
- isolation au mieux des responsabilités : portage de SRP à l'échelle de l'architecture.
- isolation du code : jusqu'au runtime.
- isolation des données : séparation physique et conceptuelle de la données.
        
## SLIDE 23 : le réseau NETFLIX
- un appel client se solde par un processus de résolution non prédictible par avance.

## SLIDE 24 
  A mesure où le système gagne en souplesse et en isolation, il perd en consistance. 
  - La même information peut exister à des endroits différent selon les besoins propre de chaque service.
  - Le système peut, sur un interval de temps se trouver à un état incohérent, le temps que l'information se propage dans le nuage de service.
  quelques pistes :
    - garder un seul runtime = monolithe forever
    - un système transactionnel logiciel : ça existe mais c'est complexe et pas à l'abris des pannes.
    - assumer les certains risques (ou relativiser leur gravité) et proposer des règles côté métier pour contourner certaines situations.

  un exemple : 
  ------------
  un site marchand avec une dispo d'une seule unité, dans un système de microservice peu se solder par la commande 
  d'un produit indisponible, car vendu à l'instant même à un autre. Mais à bien y réfléchir, ce n'est pas un problème
  de consistance et de gestion transactionnelle : comment a-t-on pu laisser le stock tomber à un niveau si bas ! 
  L'ajout d'un simple règle métier peut prévenir ce genre de situation et même éviter des non ventes en limitant les indisponibilités 
  de stock. Faire passer le métier devant la technique et ne pas voir les contraintes techniques comme un rampart systématique.

## SLIDE 25 orchestration
- système placé au dessus du graph de service pour assurer sa cohérence globale.
- assure l'invocation/synchronisation des services jusqu'à l'obtention d'un état.
- assure les mesures correctives en cas de défaillance.

Dans la pratique consiste à décrire des workflows de processus métier, au travers de charts (BPMN specification) ou
directement dans un langage. Au fond cela ressemble un peu au scheduler (ordonnanceur) gros système, pour ceux qui ont connu ;)

- recouplage des services au travers du système d'orchestration (en contradiction avec l'esprit initiale des MS).
  -> Le monolithe distribué. 
  Arbitrage isolation/consistence.

## SLIDE 26 illustration de la orchestration
- système placé au dessus du graph de service pour assurer sa cohérence globale.
- assure l'invocation/synchronisation des services jusqu'à l'obtention d'un état.
- assure les mesures correctives en cas de défaillance.

Dans la pratique consiste à décrire des workflows de processus métier, au travers de charts (BPMN specification) ou
directement dans un langage. Au fond cela ressemble un peu au scheduler (ordonnanceur) gros système, pour ceux qui ont connu ;)

- recouplage des services au travers du système d'orchestration (en contradiction avec l'esprit initiale des MS).
  -> Le monolithe distribué. 
  Arbitrage isolation/consistence.

## SLIDE 27 la chorégraphie
En s'inspirant des systèmes réactifs (events driven), la chorégraphie est un moyen d'assurer la mise en cohérence des microservices

## SLIDE 28 illustration

## SLIDE 29 avantages et limites
  - ++
    - ne plus lier le destin de chacune des fonctionnalités au travers d'un seul gros système.
    - plus complexe d'un point de vu technique, mais en divisant la complexité, plus facile à concevoir.
  - --
    - la complexité technique. plus d'env, plus d'outils pour coordonner l'ensemble des briques.
    - complexité transactionnelle, rupture radicale de l'approche de la données pour nous.
