# TodoListApp

## Etape 1 : Corrigez les bugs

### 1er bug trouvé

On remarque dès lors de la première utilisation qu'à l'ajout d'une première tâche, rien ne se passe visuellement. Un tour du côté de la console permet d'identifier explicitement le premier bug avec cette entrée : "Uncaught TypeError: self.addItem is not a function", au passage ligne 17 sur le fichier controller.js
En se rendant à cette ligne là, un essai d'accès à la méthode via F12 ou clic droit "aller à la définition" ne nous retourne rien non plus, c'est un appel à une méthode fantôme. En parcourant le fichier, nous trouvons bel et bien la méthode qui était appelée mais qui contient une faute de frappe :

```javascript
	/**
	 * An event to fire whenever you want to add an item. Simply pass in the event
	 * object and it'll handle the DOM insertion and saving of the new item.
	 */
	Controller.prototype.adddItem = function (title) {
```
En ôtant le 'd' en trop, on retrouve une fonctionnalité qui marche.

### 2ème bug trouvé

On remarque un conflit potentiel du côté de l'attribution d'un id à toute nouvelle tâche créée : il s'agit d'une concaténation de 6 caractères chacun défini grâce à la méthode Math.random() qui, comme vu ci-dessous, va renvoyer un chiffre entre 0 et 9 compris. Un number de valeur 10 aurait pu être mis à la place de la string charset, qui ici n'a pas beaucoup d'intérêt vu qu'on utilise simplement sa longueur et non pas les éléments de sa valeur.

```javascript
// Generate an ID
	    var newId = ""; 
	    var charset = "0123456789";

        for (var i = 0; i < 6; i++) {
     		newId += charset.charAt(Math.floor(Math.random() * charset.length));
        }
```

Je propose à la place de cette méthode qui a comme risque de créer des ids identiques une méthode qui va concaténer deux strings :
    - La première qui est créée suite à l'appel de la méthode Date.now() qui correspond au nombre de millisecondes écoulées depuis le 1er Janvier 1970 00:00:00 UTC
    - La seconde, qui vient compléter la première dans le doute où deux tâches seraient créées à la même milliseconde, un Math.random() pouvant aller de 0 à 999, afin de compléter notre id en une valeur totalement unique.

```javascript
// Generate an ID
		var newId = Date.now().toString() + Math.floor(Math.random() * 1000);
```

    ex: 
    *1574089302679666
    *1574089341614461


## Etape 2 : où sont les tests ?!

Les tests à réaliser ont été effectués