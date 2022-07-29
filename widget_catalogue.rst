====================
Application Widget Catalogue
====================

Le site d'OPenIG permet d'intégrer un **catalogue de données en marque blanche sur un site web externe**. Cette fonctionnalité est également intitulée 'widget'. Elle offre une solution technique pour valoriser le catalogue de données d'une organisation et plus largement de tout sous ensemble du catalogue de données OPenIG filtré par une ou plusieurs facettes (organisations, thématiques, formats, licences, recherche par mot clé...).

**La marque blanche est accessible sans restriction et sans autorisation préalable à tout utilisateur, contributeur ou développeur d'OPenIG.**

Pour pouvoir tester et installer le widget en local, voici la procédure :

* Télécharger le zip du code sur Github https://github.com/neogeo-technologies/ckan-widget

* Télécharger Node.js et l’installer si ce n’est pas déjà le cas https://nodejs.org/fr/download/

* Redémarrer l’ordi pour prendre en compte l’installation

* Effectuer les commandes: ::

    cd /CHEMIN_VERS_LE_FICHIER_DEZIPPE/ckan-widget (pour se positionner dans le dossier)
    npm install && npm start (pour installer)

* Tester l’URL local pour voir si ça fonctionne : http://localhost:3000/


Ensuite, une fois le widget installé, cela passe par l'intégration de quelques lignes de code HTML à l'endroit souhaité sur une page web externe ainsi que deux appels à un fichier Javascrit (.JS) et une feuille de style CSS (.CSS).



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


**Si l'adhérent souhaitant disposé d'un catalogue en marque blanche ne dispose pas d'une solution (serveur) pour héberger le widget, il peut contacter l'adresse webmestre@openig.org. Une aide lui sera apportée. **

La marque blanche OPenIG a été développée par Neogeo Technologies. Elle est distribuée sur Github sous licence MIT. Le code source peut être utilisé pour afficher tout catalogue CKAN sur un site tiers.

* Code source :
  https://github.com/neogeo-technologies/ckan-widget

* Licence :
  https://github.com/neogeo-technologies/ckan-widget/blob/master/LICENSE

* Les fichiers à inclure et un exemple de code HTML sont disponibles ici :
  https://github.com/neogeo-technologies/ckan-widget/tree/master/build
