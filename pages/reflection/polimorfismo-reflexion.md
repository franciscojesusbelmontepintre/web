---
layout: post
title: El polimorfismo y la reflexión
permalink: /notes/reflection/polimorfismo-reflexion/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---

Caso de estudio del problema que plantea:

$$ \href{https://stackoverflow.com/q/4866873}{Charles}\\$$

Cuando se usa el polimorfismo para tratar de eliminar los condicionales, lo que se está haciendo realmente es trasladar
un problema de un sitio a otro, ya que los condicionales no llegan a desaparecer del todo, sino que se pasan a la zona de instanciación
de las clases hijas que extienden mediante herencia a la clase abstracta padre.

Inicialmente, partimos de la siguiente situación inicial:

```java
package main;

import java.util.Scanner;

public class MainClass4 {
	
  public static void main(String[] args) {
    // TODO Auto-generated method stub
    Scanner myObj = new Scanner(System.in);
    System.out.println("Enter operation");
    String operation = myObj.nextLine();
	    
    switch(operation){
      case "view":
        System.out.println("doSomething ViewAction");
        break;
      case "edit":
        System.out.println("doSomething EditAction");
        break;
      case "sort":
        System.out.println("doSomething SortAction");
        break;
    }
	    
    myObj.close();
	    
  }

}
```

Lo que buscamos es eliminar ese Switch de 3 condiciones, así que se nos ocurre crear 1 clase padre, abstracta, y 3 clases hijas que extiendan de la clase padre y anulen el método .doSomething(), como sigue:

Clase padre:

```java
package polimorfismo;

public abstract class BaseAction {
	
  public abstract void doSomething();

}
```

Clase hija 1:

```java
package polimorfismo;

public class ViewAction extends BaseAction {
	
  public ViewAction() {}

  @Override
  public void doSomething() {
    // TODO Auto-generated method stub
    System.out.println("doSomething ViewAction");
  }

}
```

Clase hija 2:

```java
package polimorfismo;

public class EditAction extends BaseAction {
	
  public EditAction() {}
	
  @Override
  public void doSomething() {
    // TODO Auto-generated method stub
    System.out.println("doSomething EditAction");
  }

}
```

Clase hija 3:

```java
package polimorfismo;

public class SortAction extends BaseAction {
	
  public SortAction() {}
	
  @Override
  public void doSomething() {
    // TODO Auto-generated method stub
    System.out.println("doSomething SortAction");
  }

}
```

En base al nuevo esquema, planteariamos esta nueva situación:

```java
package main;

import java.util.Scanner;
import polimorfismo.*;

public class MainClass3 {

  public static void main(String[] args) {
    // TODO Auto-generated method stub
    Scanner myObj = new Scanner(System.in);
    System.out.println("Enter operation");
    String operation = myObj.nextLine();
		
    BaseAction theAction = null;

    switch (operation){
      case "view":
        theAction = new ViewAction();
        break;

      case "edit":
        theAction = new EditAction();
        break;

      case "sort":
        theAction = new SortAction();
        break;
    }
		
    theAction.doSomething();
		
    myObj.close();
		
  }

}
```

Pero claro, nos damos cuenta de que no nos hemos podido desprender de los condicionales. Es más, lo único que hemos hecho es pasarlos a la zona de instanciación de las clases hijas.

Llegados a este punto, Java sí nos permite eliminar los condicionales que hemos trasladado a la zona de instanciación de las clases hijas
mediante la reflexión, como sigue:

```java
package main;

import java.lang.reflect.*;
import polimorfismo.*;
import java.lang.Class;
import java.util.Scanner;

public class MainClass {

  public static void main(String[] args) throws Exception, ClassNotFoundException, NoSuchMethodException, SecurityException, InstantiationException, IllegalAccessException, IllegalArgumentException, InvocationTargetException {
    // TODO Auto-generated method stub
    Scanner myObj = new Scanner(System.in);
    System.out.println("Enter operation");
    String operation = myObj.nextLine();
	
    operation = operation.substring(0, 1).toUpperCase()+operation.substring(1, operation.length()).toLowerCase();
    String path = "polimorfismo."+operation+"Action";
    Object object = Class.forName(path).getConstructor().newInstance();
    BaseAction base = (BaseAction) object;
    base.doSomething();
	
    myObj.close();
  }
  
}
```

Compilamos y probamos:

```java
Enter operation
view
doSomething ViewAction
```