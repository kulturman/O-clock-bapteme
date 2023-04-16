## Page d'accueil
  - La page d'accueil a été retouchée il me semble, pas très beau mais hé, les gouts et les couleurs ne se discutent pas

## Fichier index.js
  - Utiliser une variable d'environnement pour la valeur "secret" de la configuration de la session car elle est critique et ne devrait pas apparaitre ainsi dans le code
  `
    app.use(sessionMiddleware({
      secret: process.env.SESSION_SECRET,
      ...
    }));
  `
  
  - La ligne 28 `app.use(mainController.cardPage);` n'est pas nécessaire et fait que mainController.notFound ne sera jamais appelé (ligne 29), l'ordre est important. Ce lien pourra te donner plus de détails: https://iq.opengenus.org/middlewares-in-express/

## Page de détails
La page ne marche pas, en effet:
 - `response.render('main/cardDetails', {oneCard});`, attention au nom sous lequel la vaiable est passée à la vue,
        cette syntaxe équivaut à {oneCard: oneCard}, ce qui veut dire sur la vue une variable "oneCard" sera disponible et représentera la carte retournée
        sous forme d'objet
 - Atttention, il n'y a pas de boucle à faire sur la vue, en effet il y a une seule carte à afficher, de plus aucune variable "singleCard" n'existe
        vu que la carte est accessible via une variable "oneCard", cf plus haut, il faut donc enlever la boucle, ci dessous une partie du code corrigé

    J'ai modifié la partie src de la balise img pour charger correctement l'image, il ne faut pas rajouter .jpg à la fin, et aussi éviter de modifier le nom avec
    toLowerCase (oneCard.visual_name.toLowerCase()) car les noms des fichiers sont sensibles à la casse sur les systèmes linux et Mac
        
        <img src="/visuals/<%= oneCard.visual_name %>" alt="<%= oneCard.name %> illustration">
            <p class="cardName">
                <%= oneCard.name %>
            </p>
            <p>
                <%= oneCard.element ? oneCard.element : 'Aucun élément' %>
            </p>
            ...
        </img>
    Aussi, le bouton d'ajout de la carte au deck ne fait rien d'autre que rester sur la page de détails, le lien n'est pas bon
    
## SearchController

Beaucoup trop de choses sont incorrectes, certains sont dûes à des oublis certainement, comme oublier d'importer le module datamapper ou appeler la mauvaise méthode: getCardByElement au lieu de cardElement

- la méthode devant se charger de rechercher les cartes par élément elle doit donc recevoir l'élément en paramètre, donc
       `getCardByElement: async (element) => {
            //Le code
        }`
- La page a besoin du résultat (les cartes répondant au critère) afin de les afficher sur la page et pas "l'élement" (aucun, feu...) qu'on recherche:
        `response.render('main/element', {cards: result});` // Note que j'ai aussi remplacé **'main/element/'** par **'main/element'** car le / de fin pose problème

- la vue doit maintenant être adaptée, le résultat de la recherche est accessible via une variable "cards", cf plus haut, il suffit maintenant de faire
    une boucle pour afficher les résultats, similaire à la page d'accueil, tu peux te référer à la correction, ça devrait être plus clair
- Bien sûr il faut également appliquer le même raisonnement sur les autres types de recherche (par nom...)


**Il manque la gestion du deck, cf la correction et n'hésite pas à me poser des questions**
