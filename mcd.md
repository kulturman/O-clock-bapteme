Le diagramme est globalement bon, il y a juste quelques petits soucis, à savoir
- La relation entre **Product** et **User** ont la mauvaise cardinalité, en effet il faudrait une cardinalité **0-N** des deux côtés
- La relation entre **Order** et **Product** nécessite l'ajout d'un attribut dans la relation **Contain**, parce qu'une commande peut avoir plusieurs mugs, **exemple**: j'achète 3 mugs de type "Mug Type 1" et 2 mugs de type "Mug Type 2" en une seule commande
la quantité achetée ne peut ni aller dans l'entité **Order** ni d'en l'entité **Product**, *la quantité n'a de sens que dans la relation*

- Pour ce qui est de l'entité **Order** mettre l'attribut **total_amount** est discutable, car elle est facilement calculable, quoique ce soit un choix qui peut se justifier mais il s'agit d'un compromis

Quand j'étais étudiant, j'utilisais http://www.jfreesoft.com/JMerise/, complet, il peut te générer le modèle logique et le code SQL de création de la base de données (ce qui n'est pas mal pour prototyper l'application), il est gratuit pour les étudiants.
Il y a aussi des outils en ligne comme **draw.io** qui est plus générique
