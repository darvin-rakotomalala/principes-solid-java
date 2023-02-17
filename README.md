## SOLID example Java | Spring Boot
Dans ce repo, nous allons voir des exemples des principes SOLID avec Java - Spring Boot.

*Notez que je suis toujours dans le processus d'apprentissage - probablement sans fin :)-*

### Qu'est-ce que Les principes SOLID ?
---
**Les principes SOLID** sont des concepts de conception orientés objet pertinents pour le développement de logiciels. 
SOLID est l'acronyme de cinq autres principes de conception de classe : **S**ingle Responsibility Principle, **O**pen-Closed Principle, **L**iskov Substitution Principle, **I**nterface Segregation Principle, et **D**ependency Inversion Principle.

- **Principe Single Responsibility** : Chaque classe doit être responsable d'une seule partie ou fonctionnalité du système.
- **Principe Open-Closed** : Les composants logiciels doivent être ouverts aux extensions, mais pas aux modifications.
- **Principe Liskov Substitution** : Les objets d'une superclasse doivent pouvoir être remplacés par des objets de ses sous-classes sans casser le système.
- **Principe Interface Segregation** : Aucun client ne devrait être contraint de dépendre de méthodes qu'il n'utilise pas.
- **Principe Dependency Inversion** : Les modules de haut niveau ne doivent pas dépendre des modules de bas niveau, les deux doivent dépendre d'abstractions.

SOLID est une approche de conception structurée qui garantit que votre logiciel est modulaire et facile à entretenir, à comprendre, à déboguer et à refactoriser. Suivre SOLID permet également d'économiser du temps et des efforts en matière de développement et de maintenance. SOLID empêche votre code de devenir rigide et fragile, ce qui vous aide à créer des logiciels durables.

### Exemples
---
1. **Principe Single responsibility** - 
Chaque classe en Java devrait avoir un seul travail à faire. Pour être précis, il ne devrait y avoir qu'une seule raison de changer de classe. Voici un exemple de classe Java qui ne respecte pas le principe de responsabilité unique (SRP) :
```
public class Vehicle {
    public void printDetails() {}
    public double calculateValue() {}
    public void addVehicleToDB() {}
}
```
La classe `Vehicle` a trois responsabilités distinctes : printDetails, calculateValue et addVehicleToDB. En appliquant SRP, nous pouvons séparer la classe ci-dessus en trois classes avec des responsabilités distinctes.

2. **Principe Open-closed** - 
Les entités logicielles (par exemple, les classes, les modules, les fonctions) doivent être ouvertes pour une extension, mais fermées pour modification.

Considérez la méthode ci-dessous de la classe VehicleCalculations :
```
public class VehicleCalculations {
    public double calculateValue(Vehicle v) {
        if (v instanceof Car) {
            return v.getValue() * 0.8;
        if (v instanceof Bike) {
            return v.getValue() * 0.5;

    }
}
```
Supposons que nous voulions maintenant ajouter une autre sous-classe appelée `Truck`. Nous devrions modifier la classe ci-dessus en ajoutant une autre instruction if, qui va à l'encontre du principe ouvert-fermé.
Une meilleure approche serait pour les sous-classes `Car` et `Truck` pour remplacer la méthode `calculateValue` :
```
public class Vehicle {
    public double calculateValue() {...}
}
public class Car extends Vehicle {
    public double calculateValue() {
        return this.getValue() * 0.8;
}
public class Truck extends Vehicle{
    public double calculateValue() {
        return this.getValue() * 0.9;
}
```
L'ajout d'un autre type `Vehicle` est aussi simple que de créer une autre sous-classe et de l'étendre à partir de la classe `Vehicle`.

3. **Principe de Liskov substitution** - 
Le Liskov Substitution Principle (LSP) s'applique aux hiérarchies d'héritage telles que les classes dérivées doivent être complètement substituables à leurs classes de base.

Prenons un exemple typique d'une classe dérivée `Square` et d'une classe de base `Rectangle` :
```
public class Rectangle {
    private double height;
    private double width;
    public void setHeight(double h) { height = h; }
    public void setWidht(double w) { width = w; }
    ...
}
public class Square extends Rectangle {
    public void setHeight(double h) {
        super.setHeight(h);
        super.setWidth(h);
    }
    public void setWidth(double w) {
        super.setHeight(w);
        super.setWidth(w);
    }
}
```
Les classes ci-dessus n'obéissent pas à LSP car vous ne pouvez pas remplacer la classe de base `Rectangle` par sa classe dérivée `Square`. La classe `Square` a des contraintes supplémentaires, c'est-à-dire que la hauteur et la largeur doivent être identiques. Par conséquent, le remplacement `Rectangle` par la class `Square` peut entraîner un comportement inattendu.

4. **Principe d'Interface segregation** - 
Le Interface Segregation Principle (ISP) stipule que les clients ne doivent pas être forcés de dépendre de membres d'interface qu'ils n'utilisent pas. En d'autres termes, ne forcez aucun client à implémenter une interface qui ne lui est pas pertinente.

Supposons qu'il existe une interface pour le véhicule et une classe `Bike` :
```
public interface Vehicle {
    public void drive();
    public void stop();
    public void refuel();
    public void openDoors();
}

public class Bike implements Vehicle {

    // Can be implemented
    public void drive() {...}
    public void stop() {...}
    public void refuel() {...}
    
    // Can not be implemented
    public void openDoors() {...}
}
```
Comme vous pouvez le voir, cela n'a pas de sens qu'une lasse `Bikec` implémente la méthode `openDoors()` car un vélo n'a pas de portes ! Pour résoudre ce problème, ISP propose que les interfaces soient décomposées en plusieurs petites interfaces cohérentes afin qu'aucune classe ne soit obligée d'implémenter une interface, et donc des méthodes, dont elle n'a pas besoin.

5. **Principe d'inversion de dépendance** - 
Le Dependency Inversion Principle (DIP) stipule que nous devrions dépendre d'abstractions (interfaces et classes abstraites) plutôt que d'implémentations concrètes (classes). Les abstractions ne doivent pas dépendre de détails ; au lieu de cela, les détails devraient dépendre d'abstractions.

Prenons l'exemple ci-dessous. Nous avons une classe `Car` qui dépend de la classe `Engine` concrète; par conséquent, il n'obéit pas au DIP.
```
public class Car {
    private Engine engine;
	
    public Car(Engine e) {
        engine = e;
    }
    public void start() {
        engine.start();
    }
}

public class Engine {
   public void start() {...}
}
```
Le code fonctionnera, pour l'instant, mais que se passerait-il si nous voulions ajouter un autre type de moteur, disons un moteur diesel ? Cela nécessitera de refactoriser la classe `Car`.

Cependant, nous pouvons résoudre ce problème en introduisant une couche d'abstraction. Au lieu de dépendre `Car` directement de `Engine`, ajoutons une interface :
```
public interface Engine {
    public void start();
}
```
Nous pouvons maintenant connecter n'importe quel type de `Engine` qui implémente l'interface Engine à la classe `Car` :
```
public class Car {
    private Engine engine;
	
    public Car(Engine e) {
        engine = e;
    }
    public void start() {
        engine.start();
    }
}

public class PetrolEngine implements Engine {
   public void start() {...}
}

public class DieselEngine implements Engine {
   public void start() {...}
}
```

*Bon apprentissage... Bon codage...*

[Source d'article](https://www.educative.io/answers/what-are-the-solid-principles-in-java)
