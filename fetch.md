# Explications sur fetch

fetch est une fonction Javascript permettant de faire des requêtes asynchrones, ce n'est pas très clair j'imagine alors voyons un peu de théorie

Dans le parcours que tu viens de réaliser il y a plusieurs pages, quand tu ouvres le nagigateur par exemple à l'URL: http://localhost:2001 ou http://localhost:2001/card/18 le navigateur envoie une
requête vers le serveur, et ce dernier renvoie une page HTML. Alors supposons que maintenant nous souhaitions afficher les informations de la carte dans une boite de dialogue au click sur une carte
, c'est à dire qu'au lieu de charger une autre page pour afficher les détails de la carte nous affichons les données sur la même page. Comment t'y prendrais tu? C'est là que fetch vient à la rescousse
la fonction te permet de faire une requête vers le serveur, de récupérer les données et des afficher sur la page (ou quoi que tu souhaites) sans recharger la page, magique non?

Si tu as déjà entendu parle d'**AJAX** c'est exactement ce que fait la fonction fetch: des **requêtes AJAX**

## Implémentons cette fonctionnalité
Pour cela tu peux juste rajouter ce script dans la vue (.ejs) qui liste les cartes et essayer de cliquer sur une carte

```
<script>
  const cardListsElements = document.querySelectorAll('.link-to-card');

  cardListsElements.forEach(cardElement => {
    cardElement.addEventListener('click', e => {
      e.preventDefault();
      const link = e.target.closest('.link-to-card').getAttribute('href');
      fetchCardDetails(link);
    })
  });

  function fetchCardDetails(link) {
    fetch(link) // fetch fait la requête vers le serveur
      .then(data => data.text()) // nous récupérons les donnés sous forme de text
      .then(data => {
        alert(data); //Nous affichons les données reçues
      })
  }
</script>
```

Hmm, il semble y avoir un problème, en effet ce qui s'affiche dans la boite de dialogue est le code HTML de la page de détails, cela est normal car le serveur nous renvoie une page HTML: `res.render("card/card-item", { card })`, mais nous ne voulons plus renvoyer de page HTML complète, nous voulons
juste les cartes, on se charge de les afficher dans la boite de dialogue avec du JavaScript, nous devons juste modifier la méthode du controleur pour renvoyer uniquement les données, ce qui donne: `res.send(card);`

Le résultat est déjà mieux, tu peux cliquer sur différentes cartes pour vérifier que les données affichées sont bien celles de la carte sur laquelle tu clickes

On peu encore faire mieux, pour cela on modifiera un peu la fonction **fetchCardDetails** comme suit: 

```
function fetchCardDetails(link) {
    fetch(link)
      .then(data => data.json()) // Nous savons que le serveur renvoie les données sous forme d'objet javascript, du JSON: https://www.w3schools.com/js/js_json_intro.asp
      .then(data => {
        //Il ne nous reste plus qu'à afficher les données
        alert(`
          Element: ${data.element || 'Aucun'}
          Niveau: ${data.level}
          Nom: ${data.name}
          Valeur Est: ${data.value_east}
          Valeur Nord: ${data.value_north}
          Valeur Sud: ${data.value_south}
          Valeur Ouest: ${data.value_west}
        `);
      })
  }
```

Voilà, j'espère que cet exemple t'aura aider à comprendre un peu mieux fetch, pour en apprendre plus tu peux regarder ici: https://developer.mozilla.org/fr/docs/Web/API/Fetch_API/Using_Fetch
Tu auras aussi remarquer une syntaxe assez étrange, par exemple pourquoi nous n'avons pas fait

```
const data = fetch(link);
alert(`
    Element: ${data.element || 'Aucun'}
    Niveau: ${data.level}
    Nom: ${data.name}
    Valeur Est: ${data.value_east}
    Valeur Nord: ${data.value_north}
    Valeur Sud: ${data.value_south}
    Valeur Ouest: ${data.value_west}
  `);
})
```

C'est parce que fetch ne renvoie pas les données directement, mais renvoie une *Promise*, tu peux en apprendre plus ici: https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Promise
