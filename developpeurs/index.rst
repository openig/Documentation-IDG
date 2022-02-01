.. _action developpeurs:

-------------------------------
Espace développeurs
-------------------------------

Service API Ckan
================

Le site **https://ckan.openig.org** est construit à partir du système d'information OpenSource dédié à la gestion de catalogues de données `CKAN <http://www.ckan.org/>`_.

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


Marque blanche / Widget
==============

Le site d'OPenIG permet d'intégrer un **catalogue de données en marque blanche sur un site web externe**. Cette fonctionnalité est également intitulée 'widget'. Elle offre une solution technique pour valoriser le catalogue de données d'une organisation et plus largement de tout sous ensemble du catalogue de données OPenIG filtré par une ou plusieurs facettes (organisations, thématiques, formats, licences, recherche par mot clé...).

**La marque blanche est accessible sans restriction et sans autorisation préalable à tout utilisateur, contributeur ou développeur d'OPenIG.**

Techniquement, la marque blanche d'OPenIG passe par l'intégration de quelques lignes de code HTML à l'endroit souhaité sur une page web  externe ainsi que deux appels à un fichier Javascrit (.JS) et une feuille de style CSS (.CSS).

La marque blanche OPenIG a été développée par Neogeo Technologies. Elle est distribuée sur Github sous licence MIT. Le code source peut être utilisé pour afficher tout catalogue CKAN sur un site tiers.

* Code source :
  https://github.com/neogeo-technologies/ckan-widget

* Licence :
  https://github.com/neogeo-technologies/ckan-widget/blob/master/LICENSE

* Les fichiers à inclure et un exemple de code HTML sont disponibles ici :
  https://github.com/neogeo-technologies/ckan-widget/tree/master/build




* Exemple de code d'implémentation: ::

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <link href="./app.css" rel="stylesheet">
        <link href="./static/css/main.css" rel="stylesheet">
        <title>Catalogue CKAN</title>
      </head>

      <body>
        <div id="ckan-widget"></div>
      </body>

      <script src="./static/js/main.js" type="text/javascript"></script>
      <script type="text/javascript">
        var config = {
           // URL du catalogue CKAN cible
           ckan_api: 'https://ckan.openig.org',
          // Filtres complémentaires optionnels :

          //ckan_organizations: ['org1', 'org2'],
          //ckan_groups: ['group1'],
          //ckan_tags: ['tag1'],
          //ckan_facets: {
            //res_format: 'HTML',
        //    datatype: 'type'
        //  },

        // paramétrages de l'affichage :
          data_sort: 'title_string asc',
          result_page_size: 25,
          thumbnails_display: true
        }

        ckanWidget.init(config)
      </script>
    </html>

**Paramètres d'intégration de la marque blanche :**

Le code d'inclusion html et son appel javascript permettent :

- 1/ De **charter l'interface graphique** à travers la modification de la feuilles de styles **app.css**.

- 2/ De **spécifier les facettes à filtrer** : les organisations (ckan_organizations), les thématiques (ckan_groups), les mots clés (ckan_tags) et plus généralement toute facette (ckan_facets) identifiable dans l'url des résultats d'une recherche effectuée sur OPenIG.
- 3/ De **spécifier comment afficher les résultats** : tri (data_sort), nombre de résultats par page (result_page_size), et intégration d'un vignette (thumbnails_display: true).


.. **Exemples d'intégration :**

.. - Sur le site des Parcs Naturels Régionaux :
.. http://geo.pnrpaca.org/geoservices/catalogue-de-donnees/

.. - Sur le site internet du Département des Alpes-Maritimes :
.. https://www.departement06.fr/l-information-du-departement/opendata-29882.html

.. - Sur le site internet du la ville de Digne les Bains :
.. https://www.dignelesbains.fr/coordonnees-et-horaires-de-la-mairie/open-data/



Service WMS et WFS
===========

Pour accéder aux flux OGC (Web Map Service et Web Feature Service) des données publiées sur OPenIG, il existe plusieurs façons selon le type de service :

**1. Flux Mapserver**

Lorsqu'on se situe sur la fiche d'un jeu de données, il suffit de cliquer soit directement sur la ressource soit sur l'oeil.

.. image:: acces_flux.png

Il faut ensuite sélectionner "API Géo".

.. image:: API_geo.png

.. note:: Si le bouton "API Géo" ou l'aperçu cartographique n'apparaît pas, cela peut provenir du fait que la ressource géographique déposée n'a pas été reconnue comme telle. Cela est souvent dû au format choisi lors de la publication de la ressource. Attention, pour un Shapefile zippé il faut choisir le format "ESRI Shapefile (Fichier ZIP)"" et non pas "ZIP".

.. image:: Shapefile_zip.PNG

Une fois que vous avez cliqué, un menu contextuel apparait pour vous donner toutes les informations que vous souhaitez.

.. image:: API_geo2.png

Pour une utilisation dans QGIS, il suffit d'ajouter une nouvelle connexion WMS ou WFS en collant l'URL suivante :
 "https://mapserver.openig.org/maps/" + l'identifiant de l'organisation

L'identifiant d'une organisation peut être facilement récupérer :

* Se rendre sur la page : https://ckan.openig.org/organization
* Sélectionner l'organisation
* Récupérer l'identifiant à la suite de l'URL : https://ckan.openig.org/organization/region-occitanie-pyrenees-mediterranee --> "region-occitanie-pyrenees-mediterranee"

Exemple 1 : departement-du-gard -> https://mapserver.openig.org/maps/departement-du-gard

Example 2 : departement-des-pyrenees-orientales -> https://mapserver.openig.org/maps/departement-des-pyrenees-orientales

**2. Flux Mapcache**

Réservés aux adhérents, ce flux permet d'accéder aux orthophotographies et certains fonds IGN. Cela nécessite d’avoir un compte sur openig.org pour les consommer.

Depuis votre SIG il faut renseigner l’adresse suivante  https://mapserver.openig.org/mapcache/ ainsi que vos identifiants et mot de passe utilisés pour vous connecter à https://www.openig.org/.

Les couches sont visibles à partir de l’échelle 1:250’000.

Le tuilage des couches n’est pas pré-calculé ; il est calculé à l’affichage. C’est pourquoi on peut rencontrer des lenteurs lors des premières utilisations. Les performances s’amélioreront progressivement à l’usage.

Liste des couches disponibles :

.. image:: donnees_mapcache.png

En plus de cette documentation, des **tutoriels vidéos** existent sur le site internet d'OPenIG (service accessible uniquement aux adhérents) : https://www.openig.org/flux

Catalogue Service for the web (CSW)
===========

En cours.


Créer ses propres cartes avec MViewer
===========

Ce service est uniquement accessible aux **adhérents** d'OPenIG.

Il faut vous rendre sur le site internet d'OPenIG, vous authentifier et accéder à la page "créer sa carte" via la rubrique "services avancés" de l'onglet "Accès rapides".
Sinon directement via cette URL : https://www.openig.org/creer-sa-carte

Via cette page, vous avez accès au MViewer Studio :

.. image:: mviewer_studio.PNG

.. Attention:: L'outil MViewer a été développé avec le serveur cartographique GeoServer. OPenIG utilisant MapServer, certaines fonctionnalités ne sont pas disponibles. La plupart des paramétrages pourra se faire mais via des fichiers externes stockés sur serveur et non pas directement avec l'interface Studio.

**Onglet Application :**

Spécification des paramètres globaux de l'application (emprise géographique, titre, couleur, etc.). Pour pouvoir utiliser une page d'aide ainsi qu'un logo spécifique, ceux-ci doivent être déposés sur un serveur. Il suffira ici de renseigner l'adresse URL de ces fichiers.

Exemple de page d'aide .HTML de base::

    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#h_app" aria-controls="profile" role="tab" data-toggle="tab">Application</a></li>
        <li role="presentation" ><a href="#h_credits" aria-controls="settings" role="tab" data-toggle="tab" i18n="help.modal.credits">Crédits</a></li>
    </ul>
    <!-- Tab panes -->
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active" id="h_app"><h4 i18n="help.modal.about">A propos de l'application</h4>Application qui présente ...</div>
        <div role="tabpanel" class="tab-pane" id="h_credits"><h4 i18n="help.modal.credits">Crédits </h4>`Application réalisée par ... </div>
    </div>

**Onglet Thématiques & données :**

*Ajout d'une thématique :*

.. image:: mviewer_thematiques.PNG

*Ajout d'une donnée :*

.. image:: mviewer_donnees.PNG

*Paramétrage de la donnée :*

.. image:: mviewer_donnees_modif.PNG

.. Attention:: Il ne faut pas oublier de cocher la dernière case ("utiliser le proxy") afin que la couche puisse s'afficher dans le visualiseur.

La rubrique "fiche" va vous permettre de modifier la fiche d'information relative à la ressource lorsque vous allez sélectionner une entité.
Du fait de MapServer, la seule possibilité de paramétrer cette fiche sera d'appeler un fichier .MST stocké sur votre serveur. Si vous souhaitez en savoir plus : https://mviewerdoc.readthedocs.io/fr/latest/doc_tech/config_tpl.html

.. image:: mviewer_donnees_modif2.PNG

L'affichage peut se personnaliser mais encore une fois avec des fichiers externes : URL d'un fichier .SLD ou URL d'une image pour la légende.

.. image:: mviewer_donnees_modif3.PNG

Les autres rubriques (filtre, choix, recherche) ne sont pas paramétrables dans notre cas.

Enfin, vous allez pouvoir sauvegarder votre application sur le serveur, télécharger le fichier de paramétrage .XML de votre application et prévisualiser l'application définie.
"Charger" vous permettra d'utiliser un fichier .XML stocké en local ou de charger une application sauvegardée sur le serveur.

.. image:: mviewer_admin.PNG

Liens utiles :

* Github de MViewer : https://github.com/geobretagne/mviewer

* Applications développées par la Région Bretagne : https://mviewer.netlify.app/fr/

* La documentation MViewer : https://mviewerdoc.readthedocs.io/fr/latest/index.html
