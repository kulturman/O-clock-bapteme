## mainController
  - il n'est pas conseillé de renvoyer directement le message de l'exception, en l'occurence ici des informations sensibles pourraient être retournées `res.status(500).send(error.message)`, plutôt renvoyer une message générique

## Page d'accueil
  - Les cartes sur la page d'accueil ne renvoie pas sur le détails d'une carte, pour rajouter ce lien:
    `<a href="/card/<%= card.id%>"></a>`, applique la même technique sur le lien permettant d'ajouter une carte au deck
    
## router.js
  La route de détails d'une carte n'est pas définie, en effet, la route a besoin de l'ID de la carte à afficher sinon on ne sait pas quelle carteafficher sur la page donc:
  `router.get('/card/:id', mainController.cardPage)`;   
  C'est cet id qu'il faut récupérer dans la méthode du controller (cardPage) en faisant `req.params.id`, et non dans le dataMapper où
  **req** n'existe pas, et enfin il faut passer cet id à la méthode **getCard** du **dataMapper**:     
  `const card = await dataMapper.getCard(+req.params.id);`.   
  Si tu te demandes pourquoi le +, c'est juste pour convertir l'id en entier, en effet tout ce qui est passé dans l'URL est considéré comme "string",
  c'est la même chose que Number(request.params.id)
  J'espère que cette explication t'aide à mieux comprendre la correction
