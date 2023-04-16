## CardController
  - il n'est pas conseillé de renvoyer directement le message de l'exception, en l'occurence ici des informations sensibles pourraient être retournées `res.status(500).send(error.message)`, plutôt renvoyer une message générique
  - Ligne 44 `const deck = req.session.cards.filter(card => !(card.id === cardId))`;
    La condition est alambiquée, on peut appliquer la négation directement avec `card.id !== cardId`
  - Une petite astuce, tu peux convertir les chaines en nombre juste en préfixant par un "+": `+req.params.id`, plus court que `Number(req.params.id)` ou `parseInt(req.params.id)`
  - dans la méthode deckPage: verifier si la session contient bien un tableau "decks" n'est plus nécessaire, en effet il y a un middleware qui s'en charge /middlwares/middleware, d'ailleurs je suggère de renommer ce ficher en initSession.js, et d'exporter une function, un seul middleware par fichier. Il suffit donc de faire
    `res.render('card/deck', { cards: req.session.cards })`
  
  - Méthode addCard: Attention, ici si la carte n'existe pas en base on ne retourne aucune réponse, la solution la plus simple consiste à faire la redirection en dehors de la condition, il ne se passe donc rien de spécial si la carte n'existe pas en base, il serait plus adapté de renvoyer un message informant
     l'utilisateur avec la technique déjà utilisée ailleurs
     
## Datamapper
  - la méthode **searchByElementNull** est inutile, c'est ce qu'on appelle du **code mort**
  - dans la méthode **searchByValues**: La requête n'est pas mal, mais il faut garder à l'esprit que tout ce qui vient de l'utilisateur peut etre dangereux,
        en effet la requête est sensible aux **injections SQL**; exemple:
        **/search/values?direction=east=1 OR 1=1 OR id=$1 --&value=1**
        Je modifie la requête que l'application exécute au final: Bien sûr ce n'est pas dangéreux ici en particulier, mais il faudra faire attention.
        google (ou chatGPT) est ton ami pour te donner plus d'infos
        
## SearchController
  - ligne 22:  `(searchedElement === 'null' ? ' sans élément' : + searchedElement)`, attention le **+** n'est pas nécessaire, revoir les ternaires,
        En effet ici le + va convertir **searchedElement** en entier (voire astuce plus haut), ce qui donnera toujours NaN, c'est d'ailleurs ce qui s'affiche sur la page de résultat
        
## Pour aller plus loin
  - on pourrait ex traire la manipulation de la session dans une classe ou dans fonctions à part, afin d'aléger le controleur
  - se renseigner sur docker, je l'utilise pour ne pas avoir à installé PostrgeSQL sur mon ordinateur, c'est pratique
