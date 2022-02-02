2.1 L’approche service
oui je suis toalement d'accord : il faut d'abord pense à expérience utilisateur et non se demander qu'est ce que l'on veut exposer.(On fait des API pour les utilisateurs, pas pour soi !)
Je suppose quand ils parlent de service, ils ne font pas référence au service au sens SOA.

2.2L’exposition des ressources et le choix des actions
2.2.1 Choix des ressources

- REST est data centrique : on expose une représentation d'une ressource, pas la ressource elle-même (au sens métier du terme)
- REST dans la terminologie de Fielding est optimisé pour les échanges à " grain large " : je suppose que c'est ce qu'ils appelent granularité moyenne.
  Sur le reste de ce paragraphe, les remarques sont essentiellement affaire de bon sens : penser à la granularité des ressources en terme de perf et d'usages.

  2.2.2 Nommage des ressources

- les URI dans REST désignent les ressources et non les actions applicables sur celles-ci. OK.
- pour le pluriel systématique, c'est une pratique qu'on trouve assez largement sur le WEB. (c'est notre préco)
- pour la langue c'est aussi une pratique recommandable (j'ai toujours fait comme ça et pas seulement pour les API)
- pour la casse des URI, le camelCase ne tient pas la corde : on lui préfére le spinal ou snake case (toto-tata ou tutu_toto plutôt que titiTatat) : moins source d'erreur pour un oeil humain. Pour le corps de la requête, généralement désérialisé en JS, lower camelCase.
  On est tout d'accord avec eux.

  2.2.3 Les actions

- Pour l'usage des verbe on peut noter en effet une forte proximité de leur usage avec les pratiques CRUD de construction de classe d'accés aux données. Toutefois la comparaison ne doit pas suggérer que la vocation d'un WS REST est d'exposer des CRUD. Un WS expose des ressources et permet aussi d'en ajouter à un graph. A cette nuance près, on est tout à fait d'accord avec eux.
- REST est en effet sans état. C'est un point essentiel.

3 DEVELOPPEMENT D’API
3.1Spécifier et Documenter les API

- Je ne me souvient pas que nous ayons fait de recommandation officiel, mais dans la pratique nous utilisons tous swagger 2.0 et Open API.
- Le reste du paragraphe est de bon conseil. Nos recommandations ne me semblent pas aussi détaillées à ce jour.

  3.1.1 Fournir des exemples

- bonne idée, pourquoi pas.

  3.2Les requêtes
  3.2.1 Pagination

- c'est les mêmes recommandation que la note historique de la cellule sur la pagination. Dans la pratique, le système employé peut varier selon le format d'hypermédia utilisé. Nous n'avons pas de recommandation sur le sujet (personne ne fait d'hypermédia dans la boutique :()
  Quelques exemples chopés chez octo

https://blog.octo.com/transformez-votre-api-web-en-une-api-hypermedia/

Collection+JSON, créé par Mike Amundsen en 2011
UBER, créé par Mike Amundsen en 2014
HAL, créé par Mike Kelly en 2011
Siren, créé par Kevin Swiber en 2012
Mason, créé par Jorn Widlt en 2014
JSON-LD, créé par Manu Sporny & al. en 2010
Hydra, créé par Markus Lanthaler en 2012
JSON-API, créé par Steve Klabnik en 2013

3.2.2 Tri

- OK.

  3.2.3 Filtrage

- OK. les paramètre de requête sont en effet des filtres appliqués à une ressource. D'un point de vue sémantique on pourra toujours arguer que la ressource c'est la table de la base de donner, mais toutefois les QS ne sont pas les paramètres d'une requête SQL (puisqu'un WS REST n'est un f**\*\***g requêteur SQL !)

  3.2.4 Recherche

- oui, mais à la condition que les valeurs approchantes ne se soldent pas par des temps de réponse détestables. (CF API Sirène)

  3.3Les réponses
  3.3.1 Représentation des ressources

- OK. JSON et XML sont largement suffisant aujourd'hui. XML n'a d'ailleurs plus vraiment pas la quote...

  3.3.2 Pagination

- OK. Mais il est aussi possible d'étendre le système de pagination par l'hypermédia.
  3.3.3 Codes retour http
- OK. Liste classique des codes retour HTTP. Il manque peut-être 301 pour les redirections permanentes.
  3.3.4 Gestion des erreurs
- remarque de bon sens. OK. Je n'ai pas souvenance que c'était dans la vielle note(j'irais voir)
  3.3.5 Gestion des traitements longs
- OK. Ils proposaient d'offrir un système asynchrone.
  3.3.6 Contrôles hypermedia
  Tout le sel de REST en fait. Pas assez exploité dans la pratique.
  3.4Cross-domain
- OK. CORS et seulement CORS.
  3.5Version d’API et URI
- OK.
  3.6Internationalisation
- OK. Préférer le paramètre http (reconnu par les navigateur) plutôt qu'un paramètre ou l'URL (/fr/...)
  3.7Etat de santé de l’API
  OK.

  L'ensemble de ces recommandations sont conformes (et complètent) les recommandations que nous avons faites pour les API grand public (REST) à destination d'un portail. Les références OCTO sont celles dont on c'est souvent servies dans nos préconnisations.
