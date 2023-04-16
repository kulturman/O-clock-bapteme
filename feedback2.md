## Fichier index.js
  - Utiliser une variable d'environnement pour la valeur "secret" de la configuration de la session car elle est critique et ne devrait pas apparaitre ainsi dans le code
  `
    app.use(sessionMiddleware({
      secret: process.env.SESSION_SECRET,
      ...
    }));
  `
## CardController
  - il n'est pas conseillé de renvoyer directement le message de l'exception, en l'occurence ici des informations sensibles pourraient être retournées `res.status(500).send(error.message)`, plutôt renvoyer une message générique
  
  - Ligne 5, `Number(req.params.id)` peut être remplacé par `+req.params.id`, plus court

## Page de détails d'une carte
  - Il manque de nombreux champs, l'élement étant nullable il serait bien de prévoir du texte précisant que la carte n'en a pas,
    exemple "Sans élement"
    
## Page de détils du deck
  - Il manque certains détails de la carte
  - Pas de bouton pour supprimer une carte du deck, la fonctionnalité est complètement absente
  - Revoir également la mise en page, les classes CSS ont aussi des noms assez bizarres, item-subtotal, celà n'a aucun sens dans le contexte actuel
  
  - Il est également possible d'ajouter plus de 5 cartes, ce qui ne répond pas aux contraintes du projet, il faudrait juste rajouter une simple condition comme dans la correction

## Fonction de recherche
  La fonction ne gère que la recherche par élement, mais elle possède un gros défaut, elle charge toutes les cartes en mémoire avant
  de filtrer, celà est très inéfficace, il faut filtrer côté base de données (exemple: WHERE element = "feu"), il faut gérer le cas où l'utilisateur choisi
  "aucun" comme élement, dans ce cas il faut bien faire **WHERE element IS NULL** et non **WHERE element = "aucun"**
