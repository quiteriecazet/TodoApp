# TodoListApp

## 1. Correction des bugs

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