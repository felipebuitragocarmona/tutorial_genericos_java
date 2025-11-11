# 🧩 GUÍA: Tipos de Datos Genéricos en Java (con ejemplos prácticos)

---

## 1️⃣ ¿Qué es un tipo genérico?

Los **tipos genéricos** en Java permiten escribir **clases y métodos reutilizables** que funcionan con **diferentes tipos de datos**, sin tener que duplicar código.

Ejemplo:

```java
class Caja<T> {
    private T valor;
    public void setValor(T valor) { this.valor = valor; }
    public T getValor() { return valor; }
}
```

* `T` es un **parámetro de tipo**.
* Puede ser cualquier clase: `Integer`, `String`, `Estudiante`, etc.

Esto evita escribir varias clases como `CajaEntero`, `CajaString`, `CajaEstudiante`.


---

## 🧮 Ejemplo 1: Caja con `Integer`

```java
public class MainEntero {
    public static void main(String[] args) {
        // Caja que guarda un número entero
        Caja<Integer> cajaEntero = new Caja<>();
        cajaEntero.setValor(100);

        System.out.println("El valor en la caja es: " + cajaEntero.getValor());
    }
}
```

📤 **Salida:**

```
El valor en la caja es: 100
```

---

## 🧮 Ejemplo 2: Caja con `String`

```java
public class MainString {
    public static void main(String[] args) {
        // Caja que guarda una cadena de texto
        Caja<String> cajaTexto = new Caja<>();
        cajaTexto.setValor("Hola Mundo");

        System.out.println("Mensaje guardado: " + cajaTexto.getValor());
    }
}
```

📤 **Salida:**

```
Mensaje guardado: Hola Mundo
```

---

## 🧮 Ejemplo 3: Caja con una clase personalizada (`Estudiante`)

Primero definimos la clase `Estudiante`:
## 🧱 Clase auxiliar: `Estudiante`
```java
public class Estudiante implements Comparable<Estudiante> {
    private String nombre;
    private int edad;
    private double promedio;

    public Estudiante(String nombre, int edad, double promedio) {
        this.nombre = nombre;
        this.edad = edad;
        this.promedio = promedio;
    }

    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }
    public double getPromedio() { return promedio; }

    @Override
    public String toString() {
        return nombre + " (Edad: " + edad + ", Promedio: " + promedio + ")";
    }

    @Override
    public int compareTo(Estudiante otro) {
        // Ordenar por promedio
        return Double.compare(this.promedio, otro.promedio);
    }
}
```

📤 **Salida:**

```
Contenido de la caja: Ana (Promedio: 4.6)
Promedio del estudiante: 4.6
```

---

## 📘 Conclusión

| Tipo de Caja       | Tipo de dato guardado  | Ejemplo de uso                             |
| ------------------ | ---------------------- | ------------------------------------------ |
| `Caja<Integer>`    | Números enteros        | Guardar edad, cantidad, ID                 |
| `Caja<String>`     | Texto                  | Guardar nombres, mensajes                  |
| `Caja<Estudiante>` | Objetos personalizados | Guardar y acceder a un estudiante completo |




---

## 2️⃣ Clase genérica para ORDENAR elementos

```java
public class Ordenador<T extends Comparable<T>> {

    public void ordenar(T[] arreglo) {
        for (int i = 0; i < arreglo.length - 1; i++) {
            for (int j = 0; j < arreglo.length - i - 1; j++) {
                if (arreglo[j].compareTo(arreglo[j + 1]) > 0) {
                    T temp = arreglo[j];
                    arreglo[j] = arreglo[j + 1];
                    arreglo[j + 1] = temp;
                }
            }
        }
    }

    public void mostrar(T[] arreglo) {
        for (T elem : arreglo) {
            System.out.println(elem);
        }
    }
}
```

### 💡 Ejemplo de uso:

```java
public class MainOrdenar {
    public static void main(String[] args) {

        Integer[] numeros = {8, 3, 5, 1};
        String[] palabras = {"uva", "manzana", "banana"};
        Estudiante[] estudiantes = {
            new Estudiante("Ana", 20, 4.5),
            new Estudiante("Luis", 22, 3.8),
            new Estudiante("Marta", 19, 4.9)
        };

        Ordenador<Integer> ordInt = new Ordenador<>();
        Ordenador<String> ordStr = new Ordenador<>();
        Ordenador<Estudiante> ordEst = new Ordenador<>();

        System.out.println("---- Ordenar enteros ----");
        ordInt.ordenar(numeros);
        ordInt.mostrar(numeros);

        System.out.println("\n---- Ordenar cadenas ----");
        ordStr.ordenar(palabras);
        ordStr.mostrar(palabras);

        System.out.println("\n---- Ordenar estudiantes (por promedio) ----");
        ordEst.ordenar(estudiantes);
        ordEst.mostrar(estudiantes);
    }
}
```

---

## 3️⃣ Clase genérica para BUSCAR un elemento

```java
public class Buscador<T> {

    public int buscar(T[] arreglo, T valor) {
        for (int i = 0; i < arreglo.length; i++) {
            if (arreglo[i].equals(valor)) {
                return i;
            }
        }
        return -1;
    }
}
```

### 💡 Ejemplo de uso:

```java
public class MainBuscar {
    public static void main(String[] args) {
        Buscador<Integer> buscInt = new Buscador<>();
        Buscador<String> buscStr = new Buscador<>();
        Buscador<Estudiante> buscEst = new Buscador<>();

        Integer[] nums = {1, 3, 5, 7};
        String[] frutas = {"manzana", "pera", "uva"};
        Estudiante[] ests = {
            new Estudiante("Ana", 20, 4.5),
            new Estudiante("Luis", 22, 3.8)
        };

        System.out.println("Buscar 5 → posición " + buscInt.buscar(nums, 5));
        System.out.println("Buscar 'pera' → posición " + buscStr.buscar(frutas, "pera"));
        System.out.println("Buscar estudiante Luis → posición " + buscEst.buscar(ests, ests[1]));
    }
}
```

---

## 4️⃣ Clase genérica para OBTENER EL MAYOR elemento

```java
public class MayorElemento<T extends Comparable<T>> {

    public T obtenerMayor(T[] arreglo) {
        if (arreglo == null || arreglo.length == 0) return null;

        T mayor = arreglo[0];
        for (int i = 1; i < arreglo.length; i++) {
            if (arreglo[i].compareTo(mayor) > 0) {
                mayor = arreglo[i];
            }
        }
        return mayor;
    }
}
```

### 💡 Ejemplo de uso:

```java
public class MainMayor {
    public static void main(String[] args) {

        MayorElemento<Integer> mayorInt = new MayorElemento<>();
        MayorElemento<String> mayorStr = new MayorElemento<>();
        MayorElemento<Estudiante> mayorEst = new MayorElemento<>();

        Integer[] numeros = {4, 10, 6, 8};
        String[] frutas = {"uva", "banana", "pera"};
        Estudiante[] estudiantes = {
            new Estudiante("Ana", 20, 4.5),
            new Estudiante("Luis", 22, 3.8),
            new Estudiante("Marta", 19, 4.9)
        };

        System.out.println("Mayor entero: " + mayorInt.obtenerMayor(numeros));
        System.out.println("Mayor cadena: " + mayorStr.obtenerMayor(frutas));
        System.out.println("Mayor estudiante (por promedio): " + mayorEst.obtenerMayor(estudiantes));
    }
}
```

---

## 5️⃣ Clase genérica para SUMAR y PROMEDIAR elementos

### 🧱 Clase genérica `Calculadora`

```java
import java.util.function.ToDoubleFunction;

public class Calculadora<T> {

    private ToDoubleFunction<T> extractor;

    // El extractor indica cómo obtener el valor numérico del objeto
    public Calculadora(ToDoubleFunction<T> extractor) {
        this.extractor = extractor;
    }

    public double sumar(T[] arreglo) {
        double suma = 0;
        for (T elem : arreglo) {
            suma += extractor.applyAsDouble(elem);
        }
        return suma;
    }

    public double promedio(T[] arreglo) {
        if (arreglo.length == 0) return 0;
        return sumar(arreglo) / arreglo.length;
    }
}
```

---

## 🧮 Ejemplo 1: Calcular promedio de estudiantes (usando atributo `promedio`)

```java
public class Estudiante {
    private String nombre;
    private int edad;
    private double promedio;

    public Estudiante(String nombre, int edad, double promedio) {
        this.nombre = nombre;
        this.edad = edad;
        this.promedio = promedio;
    }

    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }
    public double getPromedio() { return promedio; }

    @Override
    public String toString() {
        return nombre + " (" + promedio + ")";
    }
}
```

### Uso:

```java
public class MainEstudiantes {
    public static void main(String[] args) {
        Estudiante[] estudiantes = {
            new Estudiante("Ana", 20, 4.5),
            new Estudiante("Luis", 22, 3.8),
            new Estudiante("Marta", 19, 4.9)
        };

        // Indicamos que el atributo numérico será el promedio del estudiante
        Calculadora<Estudiante> calcEst = new Calculadora<>(e -> e.getPromedio());

        System.out.println("Suma de promedios: " + calcEst.sumar(estudiantes));
        System.out.println("Promedio general: " + calcEst.promedio(estudiantes));
    }
}
```

---

## 🧮 Ejemplo 2: Calcular promedio de materias (usando atributo `notaFinal`)

```java
public class Materia {
    private String nombre;
    private double notaFinal;
    private int creditos;

    public Materia(String nombre, double notaFinal, int creditos) {
        this.nombre = nombre;
        this.notaFinal = notaFinal;
        this.creditos = creditos;
    }

    public String getNombre() { return nombre; }
    public double getNotaFinal() { return notaFinal; }
    public int getCreditos() { return creditos; }

    @Override
    public String toString() {
        return nombre + " (" + notaFinal + ")";
    }
}
```

### Uso:

```java
public class MainMaterias {
    public static void main(String[] args) {
        Materia[] materias = {
            new Materia("Matemáticas", 4.2, 3),
            new Materia("Historia", 3.6, 2),
            new Materia("Programación", 4.8, 4)
        };

        // Usamos el atributo notaFinal como base del cálculo
        Calculadora<Materia> calcMat = new Calculadora<>(m -> m.getNotaFinal());

        System.out.println("Suma de notas finales: " + calcMat.sumar(materias));
        System.out.println("Promedio de notas: " + calcMat.promedio(materias));
    }
}
```

---

## 🧮 Ejemplo 3: Calcular promedio de productos (usando atributo `precio`)

```java
public class Producto {
    private String nombre;
    private double precio;

    public Producto(String nombre, double precio) {
        this.nombre = nombre;
        this.precio = precio;
    }

    public String getNombre() { return nombre; }
    public double getPrecio() { return precio; }

    @Override
    public String toString() {
        return nombre + " ($" + precio + ")";
    }
}
```

### Uso:

```java
public class MainProductos {
    public static void main(String[] args) {
        Producto[] productos = {
            new Producto("Teclado", 80_000),
            new Producto("Mouse", 50_000),
            new Producto("Monitor", 350_000)
        };

        // Indicamos que el atributo a sumar/promediar será el precio
        Calculadora<Producto> calcProd = new Calculadora<>(p -> p.getPrecio());

        System.out.println("Suma de precios: " + calcProd.sumar(productos));
        System.out.println("Promedio de precios: " + calcProd.promedio(productos));
    }
}
```

---

## 📘 Conclusión

| Clase        | Atributo usado | Descripción                  | Resultado calculado             |
| ------------ | -------------- | ---------------------------- | ------------------------------- |
| `Estudiante` | `promedio`     | Calcular promedio académico  | Promedio general de estudiantes |
| `Materia`    | `notaFinal`    | Calcular promedio de notas   | Promedio simple de notas        |
| `Producto`   | `precio`       | Calcular promedio de precios | Promedio y suma de precios      |


---

# 📚 RESUMEN General

| Clase              | Restricción               | Propósito         | Tipos probados                    |
| ------------------ | ------------------------- | ----------------- | --------------------------------- |
| `Ordenador<T>`     | `T extends Comparable<T>` | Ordenar elementos | `Integer`, `String`, `Estudiante` |
| `Buscador<T>`      | Ninguna                   | Buscar un valor   | `Integer`, `String`, `Estudiante` |
| `MayorElemento<T>` | `T extends Comparable<T>` | Obtener el mayor  | `Integer`, `String`, `Estudiante` |
| `Calculadora<T>`   | `T extends Number`        | Sumar y promediar | Dado el atributo de una clase       |

---
