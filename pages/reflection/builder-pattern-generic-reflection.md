---
layout: post
title: Builder Pattern genérico con reflexión
permalink: /notes/reflection/builder-pattern-generic-reflection/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---

Las implementaciones más básica del patrón builder hace que este sea dependiente de la clase para la cual se va a aplicar.

Dado una clase "C1", una implementación del patrón builder para "C1", véase "BuilderPattern_C1" no nos serviría para una clase "C2" que variase con respecto a "C1"
Es decir, esto supondría que cada clase necesita su propia implementación de builder pattern.

Una implementación sería la siguiente:

```java
package com.pruebaGuava.builderClasico;

public class C1<T> {
	
  private T attribute_1;
	
  /*
  ...
  ...
  */
	
  private T attribute_n;
  
  C1(BuilderPattern_C1 builder) {
    this.attribute_1 = (T) builder.getAttribute_1();
    /*
    ...
    ...
    */
    this.attribute_n = (T) builder.getAttribute_n();
  }

  @Override
  public String toString() {
    return "C1 [attribute_1=" + attribute_1 + ", attribute_n=" + attribute_n + "]";
  }

}
```

Su patrón builder asociado:

```java
package com.pruebaGuava.builderClasico;

public class BuilderPattern_C1<T> {
	
  private T attribute_1;
  /*
  ...
  ...
  */
  private T attribute_n;

  public BuilderPattern_C1() {}

  /*Setters*/
  public BuilderPattern_C1 attribute_1(T attribute_1) {
    this.attribute_1 = attribute_1;
    return this;
  }
	
  /*
  ...
  ...
  */

  public BuilderPattern_C1 attribute_n(T attribute_n) {
    this.attribute_n = attribute_n;
    return this;
  }

  // Getters
  public T getAttribute_1() {
    return attribute_1;
  };
	
  /*
  ...
  ...
  */
    
  public T getAttribute_n() {
    return attribute_n;
  };
	
  /*Final Operation*/
  public C1 build() {
    return new C1(this);
  }

}
```

Los tests:

```java
package com.pruebaGuava.prueba;

import org.junit.jupiter.api.Test;

import com.pruebaGuava.builderClasico.BuilderPattern_C1;
import com.pruebaGuava.builderClasico.C1;

public class BuilderPatternClasicoTests {
	
  @Test
  public void firstTest() throws Exception {
		
    C1 c1 = new BuilderPattern_C1()
                .attribute_1("valueAttribute_1")
                .attribute_n("valueAttribute_n")
                .build();  
		
  }
	
  @Test
  public void secondTest() throws Exception {
		
    C1 c1 = new BuilderPattern_C1()
                .attribute_n("valueAttribute_n")
                .attribute_1("valueAttribute_1")
                .build();  
		
  }
	
  @Test
  public void thirdTest() throws Exception {
		
    C1 c1 = new BuilderPattern_C1()
                .attribute_1("valueAttribute_1")
                .build();  
		
  }

}
```

Rápidamente nos damos cuenta de la problemática que presenta este patrón. "BuilderPattern_C1" hace uso intrínseco de los mismos atributos que presenta "C1", luego está ligado a dicha clase,
y no nos servirá para una supuesta clase "C2"

Entonces surge la pregunta: ¿Se puede hacer un builder genérico?

Es posible mediante reflexión.

El siguiente código es autoría de:

$$ \href{http://derrickbowen.com/blog/content/generic-builder-pattern-class-using-generics-reflection}{Derrick Bowen}\\$$

quien originalmente ha implementado el patrón builder mediante genéricos y reflexión para que sea válido para cualquier clase.

La instanciación comienza con el método "start()", que recibe como parámetro la clase para la cual queremos crear una nueva instancia. "start()" es "static", luego se puede llamar desde la clase, sin necesitar
de una instancia previa para llamarlo. Internamente, "start()" llama al método constructor del builder genérico.

```java
public static Builder <?> start(Class <?> clazz) {
  return new Builder <> (clazz);
}
```

Desde el constructor del builder genérico, se guarda la clase para la cual estamos instanciando, y se crea un mapa del cual hablaremos

```java
public Builder(Class <T> clazz) {
  super();
  this.clazz = clazz;
  this.map = new HashMap <> ();
}
```

La implementación hace uso de un mapa de tipos genéricos. Dicho mapa funcionará como una memoria donde se irá almacenando el atributo de la clase, y el valor que le queremos dar a dicho atributo.
Cada vez que queramos darle valor a un atributo, se hará uso del método ".with()", que insertará en el mapa el valor dado al atributo dado.

```java
public Builder <T> with(String name, Object value) {
  map.put(name, value);
  return this;
}
```

finalmente, el método ".build()" es el responsable de construir el objeto. Este método recorrerá nuestro mapa, y para cada entrada del mapa, tratará de setear el valor en el atributo a través
del método ".setProperty()".

```java
public T build() {
  @SuppressWarnings("unchecked")
  T instance = (T) ArbitraryInstances.get(clazz);
  try {
    for (Entry <String, Object> val: map.entrySet()) {
      setProperty(instance, val.getKey(), val.getValue());
    }
  } catch (Exception e) {
    throw new RuntimeException("Unable to set value with builder", e);
  }
  return instance;
}
```

Para cada entrada del mapa(par clave-valor), el método ".setProperty()" es ejectuado. Dicho método se apoyará en otro, ".invoke()". 

```java
  /**
  * Attempts to find the setter for the given property name
  * 
  * @param instance
  * @param name
  *            the property
  * @param value
  * @return
  * @throws IllegalAccessException
  * @throws IllegalArgumentException
  * @throws InvocationTargetException
  * @throws NoSuchMethodException
  * @throws SecurityException
  */
  private Builder <T> setProperty(T instance, String name, Object value)
  throws IllegalAccessException,
  IllegalArgumentException,
  InvocationTargetException,
  NoSuchMethodException,
  SecurityException {
    try {
      if (value != null) {
        invoke(instance, name, value, value.getClass());
      } else {
        findMethodAndInvoke(instance, name, value);
      }
    } catch (NoSuchMethodException nm) {
      if (value.getClass() == java.lang.Integer.class) {
        invoke(instance, name, value, int.class);
      } else if (value.getClass() == java.lang.Long.class) {
        invoke(instance, name, value, long.class);
      } else if (value.getClass() == java.lang.Float.class) {
        invoke(instance, name, value, float.class);
      } else if (value.getClass() == java.lang.Double.class) {
        invoke(instance, name, value, double.class);
      } else if (value.getClass() == java.lang.Boolean.class) {
        invoke(instance, name, value, boolean.class);
      } else {
        findMethodAndInvoke(instance, name, value);
      }
    }
    return this;
  }
```

Este buscará mediante la reflexión el setter de la clase para el atributo dado, y una vez lo obtenga, lo ejecutará para setear el valor al atributo de la clase.

```java
private void invoke(T instance, String name, Object value, Class < ? > claz)
  throws NoSuchMethodException,
  SecurityException,
  IllegalAccessException,
  IllegalArgumentException,
  InvocationTargetException {
    Method method = clazz.getMethod(getSetterName(name), claz);
    method.invoke(instance, value);
}
```

Ejemplos de uso con Tests. Dos clases diferentes que hacen uso del mismo patron builder genérico.

```java
package com.pruebaGuava.prueba;

import java.io.IOException;

import org.junit.jupiter.api.Test;

import com.google.common.testing.EqualsTester;
import com.google.common.testing.SerializableTester;
import com.pruebaGuava.buildPattern.Builder;
import com.pruebaGuava.models.EmployeePojo;
import com.pruebaGuava.models.StateProvince;

public class GenericReflectionBuilderPatternTest {
	
  @Test
  public void StateProvinceTest() {
    StateProvince sp1 = (StateProvince) Builder.start(StateProvince.class)
                                        .with("stateProvinceValue", 2)
                                        .with("stateProvinceName", "TX")
                                        .with("stateProvinceAbbreviation", "Big T")
                                        .build();
    StateProvince sp2 = (StateProvince) Builder.start(StateProvince.class)
                                        .with("stateProvinceValue", 2)
                                        .with("stateProvinceName", "TX")
                                        .with("stateProvinceAbbreviation", "Big T")
                                        .build();
    StateProvince sp3 = (StateProvince) Builder.start(StateProvince.class)
                                        .with("stateProvinceValue", 3)
                                        .with("stateProvinceName", "LA")
                                        .with("stateProvinceAbbreviation", "Big L")
                                        .build();
    EqualsTester etester = new EqualsTester();
    etester.addEqualityGroup(sp1, sp2);
    etester.addEqualityGroup(sp3);
    etester.testEquals();
  }

  @Test
  public void EmployeePojoTest() {
    EmployeePojo ep1 = (EmployeePojo) Builder.start(EmployeePojo.class)
                                        .with("firstName", "FJ")
                                        .with("lastName", "BP")
                                        .build();
    EmployeePojo ep2 = (EmployeePojo) Builder.start(EmployeePojo.class)
                                        .with("firstName", "FJ")
                                        .with("lastName", "BP")
                                        .build();
    EmployeePojo ep3 = (EmployeePojo) Builder.start(EmployeePojo.class)
                                        .with("firstName", "FK")
                                        .with("lastName", "BP")
                                        .build();
    EqualsTester etester = new EqualsTester();
    etester.addEqualityGroup(ep1, ep2);
    etester.addEqualityGroup(ep3);
    etester.testEquals();
  }
    
}
```

Ciertamente, esta implementación de builder genérico tiene un gran inconveniente y es que a la hora de ir construyendo el objeto, tenemos que sabernos de antemano como se llaman los atributos de la clase, para poder
pasárselos al método ".with()". En la primera implementación vista, la cual era dependiente de la clase "C1", podías ayudarte del IDLE para que este te fuese diciendo los métodos que tiene el builder, cuyos nombres
eran iguales a los del los atributos que tenías que setear, luego de alguna manera te chivava el nombre de los atributos y no te hacía falta tener que consultarlos en la propia clase.

Otra opción es usar Lombok con "@Builder", y que él nos inyecte la clase "{nameClass}Builder"

```java
package com.pruebaGuava.builderClasico;

import java.io.Serializable;

import lombok.Builder;
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
@Builder
public class C2 implements Serializable {

  /**
  * 
  */
  private static final long serialVersionUID = 2175829166696132703L;

  private String attribute_1;

  /*
  ...
  ...
  */
	
  private String attribute_n;

  public static C2Builder builder() {
    return new C2Builder();
  }

  @Override
  public String toString() {
    return "C2 [attribute_1=" + attribute_1 + ", attribute_n=" + attribute_n + "]";
  }

}
```

```java
  @Test
  public void fourthTest() throws Exception {
		
    C2 c2 = C2.builder()
              .attribute_n("valueAttribute_n")
              .attribute_1("valueAttribute_1")
              .build();
		
  }
```