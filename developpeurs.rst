.. _action developpeurs:

-------------------------------
Espace développeurs - API
-------------------------------

Service API Ckan
================

Le `« site d'OPenIG » <https://idgo.openig.org//>`_ est construit à partir du système d'information OpenSource dédié à la gestion de catalogues de données `CKAN <http://www.ckan.org/>`_.

Requêter l'API CKAN Catalogue
========

CKAN propose une API permettant d'interroger et de consulter le catalogue des données et leurs ressources. L'API permet également de requêter directement le contenu des ressources tabulaires (CSV, XLS) lorsque celles-ci ont été correctement intégrées au Datastore (https://openig.readthedocs.io/fr/latest/contributeurs.html#datastore-et-donnees-intelligentes).

Ainsi, il est par exemple possible de réaliser ce qui suit.

* Obtenir au format JSON :

  la liste totale des jeux de données : https://ckan.openig.org/api/3/action/package_list

  les groupes thématiques : https://ckan.openig.org/api/3/action/group_list

  les mots-clés utilisés : https://ckan.openig.org/api/3/action/tag_list

  les organisations du catalogue : https://ckan.openig.org/api/3/action/organization_list

* Obtenir un flux des jeux de données récemment mis à jour :

  https://ckan.openig.org/api/3/action/recently_changed_packages_activity_list

* Obtenir une réprésentation détaillée d'un des objets (jeu de données, organisation, ressource), toujours au format JSON :

* Obtenir une représentation détaillée d'un jeu de données :
  https://ckan.openig.org/api/3/action/package_show?id=lme-orange-2017

* Obtenir une représentation détaillée d'une organisation :
  https://ckan.openig.org/api/3/action/organization_show?id=departement-du-gard

* Obtenir la liste de tous les jeux de données d'une organisation :
  https://ckan.openig.org/api/3/action/package_search?fq=organization:(departement-du-gard)&rows=150

* Obtenir une liste de jeux de données "géographiques" :
  https://ckan.openig.org/api/3/action/package_list?datatype=donnees-geographiques

* Obtenier des informations sur la thématique "Urbanisme".
  https://ckan.openig.org/api/3/action/group_show?id=urbanisme

* Rechercher de jeux de données à partir d'un mot clé :
  https://ckan.openig.org/api/3/action/package_search?q=energies

* Rechercher des jeux de données "géographiques", au format CSV, associés à la thématique Mobilité et Transports :
  https://ckan.openig.org/api/3/action/package_search?fq=+res_format:CSV+datatype:donnees-geographiques+groups:mobilite-et-transports


Requêter l'API CKAN DATA
========

Le site d'OPenIG permet également de requêter directement le contenu des jeux de données, ou plutôt de leurs ressources. Cette mécanique est rendue possible à travers l'interrogation de l'API de données de CKAN (API CKAN DATA).

Comme expliqué plus haut, le Datastore propose un service d'indexation des données tabulaires (CSV et XLS). L'API CKAN DATA permet d'exposer le contenu des ressources indexées dans le Datastore dont on peut ainsi interroger tout ou partie sans avoir à télécharger le jeu de données. Il est alors possible de faire des opérations de recherche sur les différents champs de données.

* Afficher les cinq enregistrements du jeu de données des points de rencontre de covoiturage dans les Pyrénées Orientales::

    https://ckan.openig.org/api/3/action/datastore_search?resource_id=1532854d-7f20-4408-b3d0-f2ae0a520477&limit=5

Cette requête utilise  la méthode datastore_search de l'API de CKAN avec la notion de filtres.

* Trouvez les arrêts de covoiturage dont le champ com_lieu est égal à PERPIGNAN::

    https://ckan.openig.org/api/3/action/datastore_search?resource_id=1532854d-7f20-4408-b3d0-f2ae0a520477&filters={"com_lieu":"PERPIGNAN"}

Cette requête utilise  la méthode datastore_search de l'API de CKAN avec la notion de filtres.

* Trouver tous les arrêts de covoiturage dans les Pyrénées Orientales qui disposent d'arrêts de bus et de lumière::

    https://ckan.openig.org/api/3/action/datastore_search?resource_id=1532854d-7f20-4408-b3d0-f2ae0a520477&filters={%22lumiere%22:%22true%22,%22comm%22:%22Pr%C3%A9sence%20d%27arceaux%20V%C3%A9lo%20et%20arr%C3%AAt%20de%20bus%22}

Cette requête utilise la méthode datastore_search de l'API de CKAN avec la notion de filtres.

* Trouver les points de rencontre avec un nombre de places supérieur à 20 et un nombre de place PMR supérieur à 1 (requête SQL)::

    https://ckan.openig.org/api/3/action/datastore_search_sql?sql=SELECT from "1532854d-7f20-4408-b3d0-f2ae0a520477"  WHERE "nbre_pl" > '20' AND "nbre_pmr" > '1'

Cette requête utilise la méthode datastore_search_sql de l'API de CKAN avec la notion de requête SQL .


Documentation de l'API (catalogue et ressources) et de l'API Datastore (requête sur les ressources) en anglais :

* http://docs.ckan.org/en/latest/api/

* http://docs.ckan.org/en/ckan-2.7.2/maintaining/datastore.html#the-datastore-api


.. note:: Le mot "package" qu'on trouve dans certaines requête et dans la documentation CKAN correspond à un jeu de donnée.


Construire une requête pour l'API
=================================

Pour appeler l'API CKAN, postez un dictionnaire JSON dans une requête HTTP POST sur l'une des URL d'API de CKAN. Les paramètres de la fonction API doivent être indiqués dans le dictionnaire JSON. CKAN retournera également sa réponse dans un dictionnaire JSON.

Une façon de publier un dictionnaire JSON sur une URL est d'utiliser le client HTTP en ligne de commande `HTTPie <http://httpie.org/>`_. Il existe également d'autres outils comme Postman. Par exemple, pour obtenir une liste des noms de tous les jeux de données du groupe ``environnment`` sur le site, installez HTTPie, puis appelez la fonction API ``group_list`` en exécutant cette commande dans un terminal::

    http https://ckan.openig.org/api/3/action/group_list

La réponse de CKAN ressemblera à ceci::

    {
      "help": "https://ckan.openig.org/api/3/action/help_show?name=group_list",
      "result": [
          "administration-et-action-publique",
          "agriculture-sylviculture-et-peche",
          "biodiversite-et-environnement",
          "citoyennete-et-democratie",
          "climat-air-et-energie",
          "culture-patrimoine-et-tourisme",
          "economie-et-entreprises",
          "energies-et-reseaux",
          "equipements-batiments-et-logements",
          "formation-education-et-emploi",
          "mobilite-et-transports",
          "occupation-des-sols",
          "referentiels",
          "social-sante-et-sports",
          "urbanisme",
          "vues-aeriennes-et-imagerie"
      ],
      "success": true
    }

La réponse est un dictionnaire JSON avec 3 clés :

1. ``"success"``: ``true`` or ``false``.

   L'API est conçue pour retourner à chaque fois un ``200 OK`` dans le code statut de sa réponse, qu'il y ait une erreur ou non dans la requête, il est donc important de toujours vérifier la valeur de la clé ``success`` dans le dictionnaire de réponse, et si elle est à false, de vérifier la valeur de la clé ``error``.

.. note::

    S'il y a vraiment un gros problème de syntaxe dans la requête à l'API, CKAN
    pourra retourner une réponse HTTP avec un status code ``409``, ``400`` or ``500``
    (dans l'ordre croissant de gravité). Dans les prochaines versions de CKAN, il est prévu
    d'essayer de supprimer ce type de réponse pour n'avoirà la place que des retours ``200 OK``
    et utiliser les valeurs ``"success"`` et ``"error"``.

2. ``"result"``: le résultat retournée par la fonction appelée. Le type et la valeur du résultat
   dépendent de la fonction appelée. Dans le cas de la fonction ``group_list``, il s'agit d'une liste
   de chaînes, les noms de tous les jeux de données qui appartiennent au groupe.

   Si c'est une erreur qui est retournée à la requête, le dictionnaire contiendra une clé ``"error"``
   avec le détail de l'erreur au lieu de la clé ``"result"``.
   Un dictionnaire de réponse contenant une erreur ressemblera à
   ceci::

       {
           "help": "Creates a package",
           "success": false,
           "error": {
               "message": "Access denied",
               "__type": "Authorization Error"
               }
        }

3. ``"help"``: le texte de documentation de la fonction appelée.

La même requête HTTP peut être effectuée en utilisant le module Python standard ``urllib2``
avec ce code Python::

    #!/usr/bin/env python
    import urllib2
    import urllib
    import json
    import pprint

    # Make the HTTP request.
    response = urllib2.urlopen('http://demo.ckan.org/api/3/action/group_list',
            data_string)
    assert response.code == 200

    # Use the json module to load CKAN's response into a dictionary.
    response_dict = json.loads(response.read())

    # Check the contents of the response.
    assert response_dict['success'] is True
    result = response_dict['result']
    pprint.pprint(result)



Versions de l'API
=================
Les API CKAN sont versionnées. Si vous faites une demande à une URL d'API sans
numéro de version, CKAN choisira la dernière version de l'API::

    https://ckan.openig.org/api/action/package_list

Vous pouvez également spécifier le numéro de version de l'API souhaité dans l'URL
que vous envoyez::

    https://ckan.openig.org/api/3/action/package_list

La version 3 est actuellement la seule version de l'API Action.

Nous vous recommandons de spécifier le numéro d'API dans vos demandes, car cela
garantit que votre client API continuera à fonctionner si un jour le site est mis à niveau
vers de nouvelles versions de CKAN).

.. _api authentication:


Authentification et clés
========================

Certaines fonctions de l'API nécessitent une autorisation, par exemple pour ajouter ou modifier des jeux de données et des ressources). L'API utilise la même fonction d'autorisation
et la configuration en tant qu'interface web, donc si un utilisateur est autorisé à
faire quelque chose dans l'interface web, il sera autorisé à le faire via l'API.

Lorsque vous appelez une fonction de l'API nécessitant une autorisation, vous devez vous authentifier
vous-même en fournissant votre clé API avec votre requête HTTP. Pour trouver votre clé API,
connectez-vous au site CKAN en utilisant son interface web et visitez votre profil utilisateur.

Pour fournir votre clé API dans une requête HTTP, incluez-la dans un En-tête `` Authorization`` ou `` X-CKAN-API-Key``.

Par exemple, pour demander si vous suivez actuellement l'utilisateur
`` markw`` sur demo.ckan.org en utilisant HTTPie, exécutez cette commande::

  https://ckan.openig.org/api/3/action/am_following_user id = markw Autorisation: XXX

(Remplacer `` XXX`` avec votre clé API.)

Par exemple, pour obtenir la liste des activités de votre tableau de bord utilisateur, on lance ce code Python::

    request = urllib2.Request('https://ckan.openig.org/api/3/action/dashboard_activity_list')
    request.add_header('Authorization', 'XXX')
    response_dict = json.loads(urllib2.urlopen(request, '{}').read())


Support JSONP
=============


Pour répondre aux scripts d'autres sites qui souhaitent accéder à l'API, les données peuvent
être renvoyé au format JSONP, où les données JSON sont 'complétées' avec une fonction
call. La fonction est nommée dans le paramètre 'callback'. Par exemple::

    https://ckan.openig.org/api/3/action/package_show?id=adur_district_spending&callback=myfunction

.. note :: Cela ne fonctionne qu'avec les requêtes GET
