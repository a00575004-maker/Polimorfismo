# Polimorfismo en C++: Definiciones y Casos de Uso Esenciales

## Propósito de este repositorio

Este repositorio tiene como objetivo ayudarte a comprender uno de los conceptos más importantes de la programación orientada a objetos: el **polimorfismo**.

La idea central es sencilla:

> **Un mismo nombre de función, una misma operación o un mismo tipo puede comportarse de diferentes formas dependiendo del contexto.**

En C++, el polimorfismo aparece principalmente de dos maneras:

1. **Polimorfismo en tiempo de compilación**  
   El compilador decide qué versión usar antes de que el programa se ejecute.
   - Sobrecarga de funciones.
   - Plantillas o `templates`.

2. **Polimorfismo en tiempo de ejecución**  
   El programa decide qué comportamiento ejecutar mientras está corriendo.
   - Herencia.
   - Funciones virtuales.
   - Clases abstractas e interfaces.

Este README explica cada caso de uso con más detalle para que puedas entender no solo **qué hace el código**, sino también **por qué se escribe de esa forma**.

---

## 1. ¿Qué es el polimorfismo?

La palabra **polimorfismo** viene del griego y significa “muchas formas”. En programación orientada a objetos, significa que una misma acción puede tomar diferentes comportamientos.

Por ejemplo, la acción `hablar()` puede existir en varios tipos de objetos:

- Un perro puede responder con `Guau!`.
- Un gato puede responder con `Miau!`.
- Un animal genérico puede responder con `Animal genérico`.

La llamada puede tener el mismo nombre:

```cpp
hablar();
```

pero el resultado puede cambiar según el tipo real del objeto.

### Idea clave

El polimorfismo permite escribir código más flexible porque evita depender de clases concretas todo el tiempo. En lugar de programar pensando en “un perro”, “un gato” o “un círculo”, podemos programar pensando en conceptos más generales como “animal” o “figura”.

Esto es muy importante en software profesional porque permite construir sistemas que pueden crecer sin reescribir todo el código.

---

## 2. Polimorfismo en tiempo de compilación

El **polimorfismo en tiempo de compilación** ocurre cuando el compilador decide qué función o implementación usar antes de ejecutar el programa.

Se llama “en tiempo de compilación” porque la decisión se toma cuando el código se traduce a un programa ejecutable.

Los dos casos más comunes son:

1. Sobrecarga de funciones.
2. Plantillas o `templates`.

---

## 2.1. Sobrecarga de funciones

La **sobrecarga de funciones** permite tener varias funciones con el mismo nombre, pero con diferente lista de parámetros.

A esa lista de parámetros se le conoce como **firma** de la función.

La firma considera principalmente:

- La cantidad de parámetros.
- El tipo de cada parámetro.
- El orden de los parámetros.

### Ejemplo

```cpp
int suma(int a, int b) { return a + b; }
double suma(double a, double b) { return a + b; }

suma(1, 2);      // invoca la versión int
suma(1.5, 2.3);  // invoca la versión double
```

### ¿Qué está pasando?

Aquí existen dos funciones llamadas `suma`, pero no son iguales para el compilador:

```cpp
int suma(int a, int b)
```

recibe dos enteros, mientras que:

```cpp
double suma(double a, double b)
```

recibe dos números decimales.

Cuando se ejecuta:

```cpp
suma(1, 2);
```

el compilador observa que `1` y `2` son enteros, por lo tanto selecciona la versión que recibe `int`.

Cuando se ejecuta:

```cpp
suma(1.5, 2.3);
```

el compilador observa que los argumentos son decimales, por lo tanto selecciona la versión que recibe `double`.

### ¿Por qué esto es polimorfismo?

Porque el mismo nombre, `suma`, puede tener diferentes formas de operar dependiendo de los tipos de datos que recibe.

No tienes que crear nombres como:

```cpp
sumaEnteros();
sumaDecimales();
```

Puedes usar una sola idea:

```cpp
suma();
```

y dejar que C++ seleccione la versión adecuada.

### Caso de uso profesional

La sobrecarga se usa para crear APIs más limpias y fáciles de recordar. Por ejemplo, podrías tener una clase `Calculadora` que permita sumar enteros, decimales o incluso objetos más complejos.

### Instrucciones clave para leer este ejemplo

1. Observa que ambas funciones se llaman igual.
2. Revisa que los parámetros no son iguales.
3. Identifica cuál función se invoca según los argumentos usados.
4. Recuerda: el tipo de retorno por sí solo no basta para sobrecargar una función.

### Errores comunes

No es válido hacer esto como única diferencia:

```cpp
int suma(int a, int b);
double suma(int a, int b);
```

Aunque el tipo de retorno cambia, los parámetros son iguales. Para C++, eso genera ambigüedad.

---

## 2.2. Plantillas de función (`templates`)

Las **plantillas** permiten escribir código genérico. Esto significa que puedes escribir una función una sola vez y permitir que funcione con diferentes tipos de datos.

### Ejemplo

```cpp
template<typename T>
T maximo(T a, T b) {
    return (a > b) ? a : b;
}

maximo(3, 7);       // deduce int
maximo(2.5, 1.2);   // deduce double
```

### ¿Qué significa `template<typename T>`?

Esta línea:

```cpp
template<typename T>
```

le dice al compilador:

> “Voy a escribir una función que trabaja con un tipo genérico llamado `T`.”

`T` no es una clase específica. Es un marcador de posición. Más adelante, el compilador reemplaza `T` por el tipo real que se necesite.

Por ejemplo:

```cpp
maximo(3, 7);
```

hace que el compilador deduzca que `T` es `int`.

En cambio:

```cpp
maximo(2.5, 1.2);
```

hace que el compilador deduzca que `T` es `double`.

### ¿Qué significa esta línea?

```cpp
return (a > b) ? a : b;
```

Es un operador ternario. Se lee así:

> Si `a > b` es verdadero, regresa `a`; si no, regresa `b`.

Para que esta función funcione con un tipo `T`, ese tipo debe poder usar el operador `>`.

Por eso funciona con `int` y `double`. También puede funcionar con otros tipos si tienen definida una forma de compararse.

### ¿Por qué esto es polimorfismo?

Porque la función `maximo` puede adoptar diferentes formas según el tipo de dato usado. No se escribe una función para `int`, otra para `double` y otra para `std::string`. Se escribe una función genérica.

### Caso de uso profesional

Las plantillas son la base de muchas herramientas modernas de C++, como:

- `std::vector<T>`
- `std::sort`
- `std::map<Key, Value>`
- `std::unique_ptr<T>`

La letra `T` representa el tipo que será definido después.

### Instrucciones clave para leer este ejemplo

1. Identifica el tipo genérico `T`.
2. Observa dónde aparece `T` en los parámetros y en el retorno.
3. Revisa con qué tipos se llama la función.
4. Comprueba que esos tipos soporten las operaciones usadas dentro de la función.

---

## 3. Polimorfismo en tiempo de ejecución

El **polimorfismo en tiempo de ejecución** ocurre cuando el programa decide qué método ejecutar mientras está corriendo.

Este tipo de polimorfismo se logra principalmente con:

- Herencia.
- Funciones virtuales.
- Punteros o referencias a la clase base.
- Métodos sobrescritos en clases derivadas.

La diferencia clave es que aquí la decisión no se basa solo en el tipo declarado de la variable, sino en el **tipo real del objeto**.

---

## 3.1. Herencia y funciones virtuales

La herencia permite crear una clase nueva a partir de una clase existente.

En este ejemplo:

- `Animal` es la clase base.
- `Perro` es la clase derivada.
- `Perro` hereda de `Animal`.

### Ejemplo

```cpp
struct Animal {
    virtual void hablar() const { std::cout << "Animal genérico\n"; }
    virtual ~Animal() = default;
};

struct Perro : Animal {
    void hablar() const override { std::cout << "Guau!\n"; }
};

Animal* a = new Perro();
a->hablar();  // imprime "Guau!"
delete a;
```

### ¿Qué significa `struct Animal`?

`Animal` representa una clase base. Define un comportamiento general:

```cpp
hablar()
```

La clase base dice:

> “Todo animal puede hablar, aunque sea de forma genérica.”

### ¿Qué significa `struct Perro : Animal`?

Esta línea:

```cpp
struct Perro : Animal
```

significa que `Perro` hereda de `Animal`.

En otras palabras:

> Un `Perro` es un tipo específico de `Animal`.

Por eso, un objeto `Perro` puede ser tratado como un `Animal`.

### ¿Qué significa `virtual`?

La palabra clave `virtual` es fundamental para el polimorfismo en tiempo de ejecución.

Cuando escribimos:

```cpp
virtual void hablar() const
```

le estamos diciendo a C++:

> “Si una clase derivada redefine este método, y el objeto se usa a través de un puntero o referencia a la clase base, ejecuta la versión del tipo real del objeto.”

Sin `virtual`, C++ usaría la función de acuerdo con el tipo del puntero. Con `virtual`, C++ revisa el tipo real del objeto.

En este caso:

```cpp
Animal* a = new Perro();
```

el tipo del puntero es `Animal*`, pero el objeto real creado es `Perro`.

Por eso:

```cpp
a->hablar();
```

imprime:

```text
Guau!
```

porque el objeto real es un `Perro`.

### ¿Qué es la vtable?

Cuando una clase tiene métodos virtuales, C++ usa internamente una estructura llamada **tabla virtual** o **vtable**.

No necesitas manipularla directamente. La idea conceptual es esta:

> La vtable permite que el programa encuentre, en tiempo de ejecución, cuál versión del método debe llamar.

Por eso se dice que `virtual` habilita el despacho dinámico.

### ¿Qué significa `override`?

La palabra clave `override` se usa en la clase derivada:

```cpp
void hablar() const override
```

Sirve para decirle al compilador:

> “Esta función debe sobrescribir una función virtual de la clase base.”

Esto ayuda a detectar errores.

Por ejemplo, si escribes accidentalmente:

```cpp
void hablar() override
```

pero en la clase base el método era:

```cpp
virtual void hablar() const
```

entonces falta el `const`. Sin `override`, el error podría pasar desapercibido. Con `override`, el compilador te avisa que realmente no estás sobrescribiendo el método correcto.

### ¿Qué significa `virtual ~Animal() = default;`?

Esta línea declara un **destructor virtual**:

```cpp
virtual ~Animal() = default;
```

Es importante porque se está eliminando un objeto derivado usando un puntero a la clase base:

```cpp
delete a;
```

Si la clase base no tiene destructor virtual, puede ocurrir que no se llame correctamente el destructor de la clase derivada. Eso puede producir errores de memoria o liberación incompleta de recursos.

La parte:

```cpp
= default
```

significa:

> “Usa la implementación normal que C++ generaría automáticamente.”

### ¿Qué significa `Animal* a = new Perro();`?

Esta línea combina varias ideas importantes:

```cpp
Animal* a = new Perro();
```

- `new Perro()` crea dinámicamente un objeto de tipo `Perro`.
- `Animal* a` declara un puntero de tipo `Animal`.
- Como `Perro` hereda de `Animal`, el puntero base puede apuntar a un objeto derivado.

Esta es una de las formas más comunes de usar polimorfismo.

### ¿Por qué esto es poderoso?

Porque puedes tener muchas clases derivadas:

- `Perro`
- `Gato`
- `Vaca`
- `Pato`

Todas pueden heredar de `Animal` y redefinir `hablar()`.

Luego podrías recorrer una lista de `Animal*` y llamar:

```cpp
animal->hablar();
```

sin importar qué tipo específico de animal hay detrás.

### Caso de uso profesional

Este patrón se usa en:

- Frameworks gráficos.
- Videojuegos.
- Plugins.
- Simuladores.
- Sistemas con múltiples tipos de usuarios, sensores, formas, vehículos o componentes.

### Instrucciones clave para leer este ejemplo

1. Identifica la clase base: `Animal`.
2. Identifica la clase derivada: `Perro`.
3. Observa que el método de la base está marcado como `virtual`.
4. Observa que la clase derivada usa `override`.
5. Observa que se usa un puntero de clase base: `Animal*`.
6. Revisa que el objeto real creado es `Perro`.
7. Comprueba que la llamada ejecuta la versión de `Perro`.

### Error común: olvidar `virtual`

Si `hablar()` no fuera virtual, esta llamada:

```cpp
a->hablar();
```

podría ejecutar la versión de `Animal`, no la de `Perro`.

Eso rompería el comportamiento polimórfico.

---

## 3.2. Clases abstractas e interfaces

Una **clase abstracta** es una clase que no se puede instanciar directamente porque contiene al menos un método virtual puro.

Una **interfaz** es una clase abstracta que se usa principalmente para definir un contrato: qué métodos debe implementar cualquier clase derivada.

### Ejemplo

```cpp
struct IShape {
    virtual double area() const = 0;
    virtual ~IShape() = default;
};

struct Circulo : IShape {
    Circulo(double r) : radio(r) {}
    double area() const override { return 3.1416 * radio * radio; }
private:
    double radio;
};
```

### ¿Qué significa `IShape`?

El nombre `IShape` representa una interfaz para figuras geométricas.

La letra `I` suele usarse como convención para indicar “Interface”. No es obligatorio, pero ayuda a leer el diseño.

`IShape` no representa una figura concreta. Representa la idea general de una figura que puede calcular su área.

### ¿Qué significa `virtual double area() const = 0;`?

Esta línea es el corazón del ejemplo:

```cpp
virtual double area() const = 0;
```

Se compone de varias partes:

- `virtual`: permite polimorfismo en tiempo de ejecución.
- `double`: indica que el método regresará un número decimal.
- `area()`: es el nombre del método.
- `const`: indica que el método no debe modificar el objeto.
- `= 0`: convierte el método en un método virtual puro.

Un método virtual puro significa:

> “Esta clase declara que este método debe existir, pero no proporciona la implementación.”

Por eso `IShape` se vuelve una clase abstracta.

### ¿Por qué no puedo crear un objeto `IShape` directamente?

No puedes hacer esto:

```cpp
IShape shape;
```

porque `IShape` tiene un método sin implementar:

```cpp
area()
```

C++ no sabría cómo calcular el área de una figura genérica.

Lo que sí puedes hacer es usar punteros o referencias a `IShape`:

```cpp
IShape* shape = new Circulo(3.0);
```

porque el objeto real sí es una clase concreta: `Circulo`.

### ¿Qué hace `Circulo : IShape`?

La clase `Circulo` implementa el contrato definido por `IShape`.

Cuando escribe:

```cpp
double area() const override
```

está proporcionando la fórmula concreta para calcular el área de un círculo.

### ¿Por qué esto es polimorfismo?

Porque podrías tener muchas clases que implementen `IShape`:

- `Circulo`
- `Rectangulo`
- `Triangulo`

Todas tendrían un método `area()`, pero cada una lo calcularía de manera diferente.

El código cliente podría trabajar con `IShape*` sin conocer el tipo exacto:

```cpp
std::cout << shape->area();
```

La llamada ejecutará la versión correcta según el objeto real.

### Caso de uso profesional

Las interfaces se usan para diseñar software basado en contratos.

Por ejemplo, en un proyecto real podrías tener:

```cpp
class AIProvider {
public:
    virtual std::string sendPrompt(const std::string& prompt) = 0;
    virtual ~AIProvider() = default;
};
```

Luego diferentes clases podrían implementar ese contrato:

- `OllamaProvider`
- `MockAIProvider`
- `OpenAIProvider`

El resto del programa podría usar `AIProvider*` sin depender de una implementación específica.

### Instrucciones clave para leer este ejemplo

1. Busca el método que termina en `= 0`.
2. Identifica que la clase con ese método es abstracta.
3. Revisa qué clase concreta hereda de la interfaz.
4. Confirma que la clase concreta implementa el método obligatorio.
5. Observa el uso de `override`.
6. Recuerda: una interfaz define qué debe hacerse, no necesariamente cómo hacerlo.

---

## 4. Otros patrones polimórficos

Además de herencia y funciones virtuales, C++ tiene otras formas de lograr comportamiento polimórfico.

En este repositorio aparecen dos casos importantes:

1. Polimorfismo paramétrico con plantillas de clase.
2. Operadores sobrecargados.

---

## 4.1. Polimorfismo paramétrico con plantillas de clase

Una **plantilla de clase** permite crear una clase que puede trabajar con diferentes tipos de datos.

### Ejemplo

```cpp
template<typename T>
class Caja {
    T valor;
public:
    Caja(T v): valor(v) {}
    T get() const { return valor; }
};
```

### ¿Qué significa `template<typename T>` en una clase?

Igual que en las funciones plantilla, `T` representa un tipo genérico.

La clase `Caja` no está limitada a guardar enteros o textos. Puede guardar cualquier tipo compatible.

Por ejemplo:

```cpp
Caja<int> cajaEntera(10);
Caja<std::string> cajaTexto("Hola");
Caja<double> cajaDecimal(3.14);
```

Cada una usa la misma plantilla, pero con un tipo diferente.

### ¿Qué significa `T valor;`?

Esta línea:

```cpp
T valor;
```

significa que el atributo `valor` será del tipo que se indique al crear la caja.

Si escribes:

```cpp
Caja<int>
```

entonces `valor` será `int`.

Si escribes:

```cpp
Caja<std::string>
```

entonces `valor` será `std::string`.

### ¿Qué hace el constructor?

```cpp
Caja(T v): valor(v) {}
```

Este constructor recibe un valor de tipo `T` y lo guarda en el atributo `valor`.

La parte:

```cpp
: valor(v)
```

se llama **lista de inicialización**. Es una forma eficiente y profesional de inicializar atributos en C++.

### ¿Por qué esto es polimorfismo?

Porque la misma clase `Caja` puede adoptar distintas formas:

- `Caja<int>`
- `Caja<double>`
- `Caja<std::string>`

El comportamiento general es el mismo, pero el tipo interno cambia.

### Caso de uso profesional

Las plantillas de clase son esenciales para construir contenedores genéricos, estructuras de datos y componentes reutilizables.

Ejemplos profesionales:

- `std::vector<int>`
- `std::vector<std::string>`
- `std::shared_ptr<Objeto>`
- `std::optional<double>`

### Instrucciones clave para leer este ejemplo

1. Ubica el `template<typename T>`.
2. Revisa dónde se usa `T` dentro de la clase.
3. Observa que la clase no define un tipo específico.
4. Identifica el tipo concreto al crear el objeto: `Caja<int>`, `Caja<std::string>`, etc.

---

## 4.2. Operadores sobrecargados

La **sobrecarga de operadores** permite definir cómo deben comportarse operadores como `+`, `==`, `<`, `<<` o `[]` cuando se usan con clases creadas por nosotros.

### Ejemplo

```cpp
struct Punto {
    int x, y;
    Punto operator+(const Punto& o) const {
        return {x + o.x, y + o.y};
    }
};
```

### ¿Qué representa `Punto`?

`Punto` es un tipo de dato propio. Tiene dos atributos:

```cpp
int x, y;
```

Estos representan una coordenada en dos dimensiones.

### ¿Qué significa `operator+`?

Esta línea:

```cpp
Punto operator+(const Punto& o) const
```

le dice a C++ cómo debe comportarse el operador `+` cuando se suman dos objetos de tipo `Punto`.

Por ejemplo:

```cpp
Punto p1{1, 2};
Punto p2{3, 4};
Punto p3 = p1 + p2;
```

C++ interpreta:

```cpp
p1 + p2
```

como una llamada a:

```cpp
p1.operator+(p2)
```

### ¿Qué significa `const Punto& o`?

El parámetro:

```cpp
const Punto& o
```

significa que la función recibe otro `Punto` por referencia constante.

Esto es importante porque:

- Evita copiar innecesariamente el objeto.
- Garantiza que el punto recibido no será modificado.

### ¿Qué significa el `const` al final?

```cpp
Punto operator+(const Punto& o) const
```

El `const` final significa que esta función no modificará el objeto actual.

Es decir, al hacer:

```cpp
p1 + p2
```

ni `p1` ni `p2` cambian. Se crea un nuevo punto como resultado.

### ¿Qué significa `return {x + o.x, y + o.y};`?

Esta línea crea y regresa un nuevo `Punto`.

Si:

```cpp
p1 = {1, 2}
p2 = {3, 4}
```

entonces:

```cpp
p1 + p2
```

produce:

```cpp
{4, 6}
```

### ¿Por qué esto es polimorfismo?

Porque el operador `+` ya existe para tipos como `int` y `double`, pero aquí le damos una nueva forma para funcionar con un tipo propio: `Punto`.

El símbolo es el mismo, pero el comportamiento depende del tipo de dato.

### Caso de uso profesional

La sobrecarga de operadores se usa cuando hace que el código sea más natural.

Buenos ejemplos:

- Sumar vectores matemáticos.
- Comparar fechas.
- Imprimir objetos con `operator<<`.
- Comparar objetos con `operator==`.

Debe usarse con cuidado. No conviene sobrecargar operadores si el significado no es intuitivo.

### Instrucciones clave para leer este ejemplo

1. Identifica qué operador se está redefiniendo.
2. Revisa qué tipo de objeto recibe como parámetro.
3. Observa si modifica o no modifica los objetos originales.
4. Verifica que el significado del operador sea natural.

---

## 5. Buenas prácticas y consejos

### 5.1. Marca siempre los métodos sobrescritos con `override`

Usa `override` cuando una clase derivada redefine un método virtual de la clase base.

Esto permite que el compilador detecte errores de firma.

Correcto:

```cpp
void hablar() const override;
```

Si olvidas `const` o cambias mal los parámetros, el compilador te avisará.

---

### 5.2. Declara destructores de base como `virtual` si hay herencia

Cuando una clase se usará como base polimórfica, su destructor debe ser virtual.

Ejemplo:

```cpp
virtual ~Animal() = default;
```

Esto es especialmente importante si vas a eliminar objetos derivados usando punteros a la clase base.

---

### 5.3. Evita el slicing

El **slicing** ocurre cuando copias un objeto derivado dentro de una variable de tipo base y se pierde la parte específica de la clase derivada.

Ejemplo conceptual:

```cpp
Perro perro;
Animal animal = perro;  // posible slicing
```

Aquí `animal` conserva solo la parte de `Animal`. La parte específica de `Perro` puede perderse.

Para evitarlo, usa punteros o referencias:

```cpp
Animal& animal = perro;
Animal* animalPtr = &perro;
```

---

### 5.4. Prefiere templates cuando no necesitas polimorfismo en ejecución

Si todos los tipos se conocen en tiempo de compilación, los `templates` suelen ser una buena solución.

Si necesitas cambiar comportamientos dinámicamente durante la ejecución, usa herencia y funciones virtuales.

---

### 5.5. Documenta claramente tus interfaces y contratos

Una interfaz debe dejar claro:

- Qué métodos deben implementarse.
- Qué espera recibir cada método.
- Qué debe regresar.
- Qué responsabilidades tiene cada clase derivada.

Esto ayuda a que el diseño sea más profesional y mantenible.

---

# Ejemplo de Polimorfismo y Templates en C++

En este proyecto encontrarás un ejemplo completo que ilustra varios conceptos clave de C++ orientado a objetos y plantillas genéricas.

La estructura general del proyecto es:

```text
Polimorfismo/
├── include/
│   ├── Example.h
│   └── Example.hpp
├── src/
│   ├── Example.cpp
│   └── main.cpp
├── docs/
│   └── README.md
├── Makefile
└── README.md
```

## 6. Lectura guiada del código del proyecto

Esta sección explica cómo se relacionan los conceptos anteriores con los archivos reales del repositorio.

---

## 6.1. Archivo `include/Example.h`

Este archivo contiene las declaraciones principales.

Aquí se definen:

- La interfaz `IShape`.
- La clase concreta `Circle`.
- La plantilla de clase `Box<T>`.
- La clase `Example` con métodos sobrecargados y una función plantilla.

### `IShape`: clase abstracta o interfaz

```cpp
class IShape {
public:
    virtual ~IShape() = default;
    virtual double area() const = 0;
};
```

#### ¿Qué debes observar?

1. `IShape` tiene un método virtual puro:

   ```cpp
   virtual double area() const = 0;
   ```

2. Eso significa que `IShape` no se puede instanciar directamente.
3. Cualquier clase que herede de `IShape` debe implementar `area()`.
4. El destructor es virtual para permitir destrucción segura mediante punteros a la clase base.

#### Interpretación conceptual

`IShape` funciona como un contrato:

> “Toda figura que herede de mí debe saber calcular su área.”

No dice cómo calcular el área. Solo obliga a que el método exista.

---

### `Circle`: clase concreta que implementa la interfaz

```cpp
class Circle : public IShape {
public:
    Circle(double r);
    ~Circle();
    double area() const override;
    double getRadius() const;
    void setRadius(double r);
private:
    double radius_;
};
```

#### ¿Qué debes observar?

1. `Circle` hereda de `IShape`:

   ```cpp
   class Circle : public IShape
   ```

2. `Circle` implementa el método obligatorio:

   ```cpp
   double area() const override;
   ```

3. Usa `override` para confirmar que realmente está sobrescribiendo un método virtual de la clase base.
4. Tiene un atributo privado:

   ```cpp
   double radius_;
   ```

5. Usa `getRadius()` y `setRadius()` para acceder y modificar el radio.

#### Interpretación conceptual

`Circle` sí es una figura concreta. Por eso puede implementar la fórmula del área:

```text
área = π · radio²
```

La interfaz `IShape` exige que exista `area()`. La clase `Circle` decide cómo calcularla.

---

### `Box<T>`: plantilla de clase

```cpp
template<typename T>
class Box {
public:
    Box(const T& content);
    ~Box();
    T getContent() const;
    void setContent(const T& content);
private:
    T content_;
};
```

#### ¿Qué debes observar?

1. `Box` es una clase genérica porque usa:

   ```cpp
   template<typename T>
   ```

2. `content_` no tiene un tipo fijo. Su tipo depende de `T`.
3. El constructor recibe el contenido como referencia constante:

   ```cpp
   const T& content
   ```

4. `getContent()` permite consultar el contenido.
5. `setContent()` permite modificar el contenido.

#### Interpretación conceptual

`Box<T>` es como una caja adaptable. Puede guardar diferentes tipos de contenido.

Por ejemplo:

```cpp
Box<int> intBox(123);
```

crea una caja que guarda enteros.

También podría existir:

```cpp
Box<std::string> textBox("Hola");
```

que sería una caja para texto.

---

### `Example`: clase con sobrecarga y plantilla de función

```cpp
class Example {
public:
    Example(int v);
    ~Example();

    void setValue(int v);
    int getValue() const;

    int add(int a, int b) const;
    double add(double a, double b) const;

    template<typename T>
    static T maxValue(T a, T b) {
        return (a > b) ? a : b;
    }

private:
    int value_;
};
```

#### ¿Qué debes observar?

1. Hay dos métodos llamados `add`:

   ```cpp
   int add(int a, int b) const;
   double add(double a, double b) const;
   ```

   Esto demuestra sobrecarga de funciones.

2. Hay una función plantilla:

   ```cpp
   template<typename T>
   static T maxValue(T a, T b)
   ```

   Esto demuestra polimorfismo en tiempo de compilación.

3. `maxValue` es `static`, por lo que puede llamarse sin crear un objeto `Example`.

   Por ejemplo:

   ```cpp
   Example::maxValue<int>(4, 7);
   ```

4. `value_` es privado, por lo que se accede mediante `setValue()` y `getValue()`.

#### Interpretación conceptual

`Example` concentra dos ideas:

- Sobrecarga: mismo nombre, diferentes parámetros.
- Template: misma función, diferentes tipos posibles.

---

## 6.2. Archivo `include/Example.hpp`

Este archivo contiene las definiciones de la plantilla `Box<T>`.

En C++, las plantillas normalmente se implementan en archivos `.h` o `.hpp` porque el compilador necesita ver la definición completa cuando genera la versión concreta de la plantilla.

### Constructor de `Box<T>`

```cpp
template<typename T>
Box<T>::Box(const T& content) : content_(content) {
    // Constructor de Box
}
```

#### ¿Qué significa?

- `template<typename T>` indica que seguimos trabajando con un tipo genérico.
- `Box<T>::Box` indica que estamos implementando el constructor de la clase plantilla.
- `: content_(content)` inicializa el atributo privado.

### Getter

```cpp
template<typename T>
T Box<T>::getContent() const {
    return content_;
}
```

Este método regresa el contenido guardado dentro de la caja.

El `const` final indica que consultar el contenido no modifica el objeto.

### Setter

```cpp
template<typename T>
void Box<T>::setContent(const T& content) {
    content_ = content;
}
```

Este método reemplaza el contenido actual por uno nuevo.

Usar `const T&` evita una copia innecesaria y protege el parámetro recibido.

---

## 6.3. Archivo `src/Example.cpp`

Este archivo contiene la implementación de `Circle` y `Example`.

### Implementación de `Circle`

```cpp
Circle::Circle(double r) : radius_(r) {
    std::cout << "[Circle] Constructor, radius = " << radius_ << "\n";
}

Circle::~Circle() {
    std::cout << "[Circle] Destructor\n";
}

double Circle::area() const {
    return 3.141592653589793 * radius_ * radius_;
}
```

#### ¿Qué debes observar?

1. El constructor inicializa `radius_`.
2. El destructor imprime un mensaje para mostrar cuándo se destruye el objeto.
3. `area()` implementa el método que la interfaz `IShape` exigía.

### Implementación de `Example`

```cpp
int Example::add(int a, int b) const {
    return a + b;
}

double Example::add(double a, double b) const {
    return a + b;
}
```

#### ¿Qué debes observar?

Ambas funciones se llaman `add`, pero reciben parámetros diferentes.

Cuando se llama con enteros, se usa la versión `int`.

Cuando se llama con decimales, se usa la versión `double`.

---

## 6.4. Archivo `src/main.cpp`

El archivo `main.cpp` ejecuta las demostraciones.

Cada bloque imprime un título para separar los conceptos.

---

### Demo 1: Sobrecarga

```cpp
std::cout << "\n=== Demo Sobrecarga (2.1) ===\n";

Example ex(5);

std::cout << "add(int): 2 + 3 = " << ex.add(2, 3) << "\n";
std::cout << "add(double): 2.5 + 3.7 = " << ex.add(2.5, 3.7) << "\n";
```

#### ¿Qué se demuestra?

Se crea un objeto `Example` y se llama dos veces a `add()`:

```cpp
ex.add(2, 3)
```

usa la versión de enteros.

```cpp
ex.add(2.5, 3.7)
```

usa la versión de decimales.

#### Resultado esperado

```text
add(int): 2 + 3 = 5
add(double): 2.5 + 3.7 = 6.2
```

---

### Demo 2: Template de función

```cpp
std::cout << "\n=== Demo Template Función (2.2) ===\n";

std::cout << "maxValue<int>(4,7) = " << Example::maxValue<int>(4, 7) << "\n";

std::cout << "maxValue<string>(\"foo\",\"bar\") = "
          << Example::maxValue<std::string>("foo", "bar") << "\n";
```

#### ¿Qué se demuestra?

Se llama a una función plantilla con dos tipos diferentes:

- `int`
- `std::string`

En el caso de enteros:

```cpp
Example::maxValue<int>(4, 7)
```

el resultado es `7`.

En el caso de strings:

```cpp
Example::maxValue<std::string>("foo", "bar")
```

C++ compara las cadenas según el orden lexicográfico.

#### Nota importante

El uso explícito de:

```cpp
<int>
<std::string>
```

ayuda a visualizar el tipo con el que se está instanciando la plantilla.

En muchos casos, C++ puede deducir el tipo automáticamente, pero escribirlo explícitamente es útil para aprendizaje.

---

### Demo 3: Clase abstracta, interfaz y polimorfismo

```cpp
std::cout << "\n=== Demo Clase Abstracta e Interface (3.2) ===\n";

IShape* shape = new Circle(3.0);

std::cout << "Circle radius = " << static_cast<Circle*>(shape)->getRadius() << "\n";
std::cout << "Circle area = " << shape->area() << "\n";

delete shape;
```

#### ¿Qué se demuestra?

Esta es una de las partes más importantes del proyecto.

```cpp
IShape* shape = new Circle(3.0);
```

significa que un puntero de tipo `IShape*` apunta a un objeto real de tipo `Circle`.

Esto es polimorfismo en tiempo de ejecución.

La llamada:

```cpp
shape->area();
```

usa la interfaz `IShape`, pero ejecuta la implementación concreta de `Circle`.

#### ¿Por qué funciona?

Funciona porque `area()` fue declarado como virtual puro en `IShape`:

```cpp
virtual double area() const = 0;
```

y fue implementado en `Circle`:

```cpp
double area() const override;
```

#### ¿Qué hace `static_cast<Circle*>(shape)`?

Esta parte:

```cpp
static_cast<Circle*>(shape)->getRadius()
```

convierte el puntero `IShape*` a un puntero `Circle*` para poder llamar a `getRadius()`.

¿Por qué se necesita convertir?

Porque `getRadius()` pertenece a `Circle`, no a `IShape`.

La interfaz `IShape` solo garantiza que existe `area()`.

#### Nota didáctica importante

Este `static_cast` funciona en este ejemplo porque sabemos que el objeto real es un `Circle`:

```cpp
new Circle(3.0)
```

En programas más grandes, se debe tener cuidado con conversiones de este tipo. En diseños más robustos, normalmente se evita depender de métodos específicos de la clase derivada cuando se trabaja mediante una interfaz.

---

### Demo 4: Plantilla de clase

```cpp
std::cout << "\n=== Demo Plantilla de Clase (4.1) ===\n";

Box<int> intBox(123);

std::cout << "Box<int> holds = " << intBox.getContent() << "\n";

intBox.setContent(456);

std::cout << "Box<int> now = " << intBox.getContent() << "\n";
```

#### ¿Qué se demuestra?

Se crea una caja genérica especializada para enteros:

```cpp
Box<int>
```

Eso significa que, para este objeto, `T` se convierte en `int`.

Primero se guarda:

```text
123
```

Luego se reemplaza por:

```text
456
```

#### Resultado esperado

```text
Box<int> holds = 123
Box<int> now = 456
```

---

## 7. Conceptos demostrados en el repositorio

### 7.1. Sobrecarga de funciones

La clase `Example` implementa dos versiones de `add()`:

```cpp
int add(int a, int b) const;
double add(double a, double b) const;
```

La primera opera con enteros y la segunda con decimales.

Esto permite comprobar cómo el compilador selecciona automáticamente la función adecuada según el tipo de los argumentos.

---

### 7.2. Plantilla de función

El método estático:

```cpp
template<typename T>
static T maxValue(T a, T b)
```

calcula el máximo de dos valores de cualquier tipo compatible con el operador `>`.

Se ejemplifica con:

- `int`
- `std::string`

---

### 7.3. Clases abstractas e interfaces

La interfaz `IShape` define un contrato obligatorio:

```cpp
virtual double area() const = 0;
```

La clase `Circle` implementa ese contrato.

Esto demuestra herencia, abstracción y polimorfismo en tiempo de ejecución.

---

### 7.4. Plantilla de clase

La clase genérica `Box<T>` encapsula un valor de tipo `T`.

Incluye:

- Constructor.
- Destructor.
- Getter.
- Setter.

Se prueba con:

```cpp
Box<int>
```

---

### 7.5. Uso de constructores, destructores, getters y setters

Las clases del proyecto usan constructores y destructores para mostrar el ciclo de vida de los objetos.

`Circle` y `Box<T>` incluyen métodos para acceder y modificar sus atributos de forma controlada.

Esto refuerza el principio de **encapsulación**.

---

## 8. ¿Cómo ejecutar el proyecto?

### Opción con `make`

Desde la carpeta raíz del proyecto:

```bash
make
```

Esto compila los archivos fuente usando el `Makefile` del proyecto.

Después, ejecuta:

```bash
./build/app
```

### Opción directa con `g++`

También puedes compilar manualmente con:

```bash
g++ -std=c++20 src/*.cpp -Iinclude -o build/app
```

Y ejecutar:

```bash
./build/app
```

### En Windows

Si estás usando MinGW/MSYS2, el ejecutable puede generarse como:

```bash
build/app.exe
```

Para ejecutarlo desde terminal:

```bash
./build/app.exe
```

Si la carpeta `build` no existe, créala antes de compilar:

```bash
mkdir build
```

---

## 9. Resultado esperado aproximado

El programa debe mostrar mensajes parecidos a estos:

```text
=== Demo Sobrecarga (2.1) ===
[Example] Constructor, value = 5
add(int): 2 + 3 = 5
add(double): 2.5 + 3.7 = 6.2

=== Demo Template Función (2.2) ===
maxValue<int>(4,7) = 7
maxValue<string>("foo","bar") = foo

=== Demo Clase Abstracta e Interface (3.2) ===
[Circle] Constructor, radius = 3
Circle radius = 3
Circle area = 28.2743
[Circle] Destructor

=== Demo Plantilla de Clase (4.1) ===
Box<int> holds = 123
Box<int> now = 456
[Example] Destructor
```

El valor exacto del área puede variar ligeramente por el número de decimales mostrado.

---

## 10. Checklist de comprensión para estudiantes

Antes de cerrar esta sesión, deberías poder responder:

- ¿Qué significa que una función esté sobrecargada?
- ¿Qué es la firma de una función?
- ¿Qué significa `template<typename T>`?
- ¿Cuál es la diferencia entre polimorfismo en tiempo de compilación y en tiempo de ejecución?
- ¿Para qué sirve `virtual`?
- ¿Para qué sirve `override`?
- ¿Por qué una clase con un método `= 0` es abstracta?
- ¿Por qué se recomienda un destructor virtual en clases base?
- ¿Qué significa usar un puntero de clase base apuntando a un objeto derivado?
- ¿Qué es el slicing y por qué debe evitarse?
- ¿Por qué `Box<int>` y `Box<std::string>` son versiones distintas de una misma plantilla?

---

## 11. Mini reto sugerido

Sin cambiar la estructura principal del proyecto, intenta extenderlo de forma controlada:

1. Crea otra clase que implemente `IShape`, por ejemplo `Rectangle`.
2. Implementa su método `area()`.
3. En `main.cpp`, crea un puntero `IShape*` que apunte a un `Rectangle`.
4. Llama a `area()` desde el puntero base.
5. Compara el comportamiento con `Circle`.

La meta del reto no es escribir mucho código, sino comprobar que una misma interfaz puede tener varias implementaciones.

---

## 12. Cierre conceptual

El polimorfismo es una herramienta esencial para construir software flexible.

En esta sesión viste que C++ permite polimorfismo de varias formas:

| Tipo de polimorfismo | Mecanismo | Decisión ocurre en |
|---|---|---|
| Sobrecarga de funciones | Mismo nombre, diferente firma | Compilación |
| Templates | Código genérico con `T` | Compilación |
| Herencia + virtual | Clase base y clase derivada | Ejecución |
| Clases abstractas | Contratos con métodos puros | Ejecución |
| Operadores sobrecargados | Operadores adaptados a tipos propios | Compilación |

La idea más importante es esta:

> El polimorfismo permite escribir código que trabaja con ideas generales, pero ejecuta comportamientos específicos cuando corresponde.

Eso es una base fundamental para diseñar software orientado a objetos de forma profesional.
