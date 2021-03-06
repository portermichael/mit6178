- [Announcements](#sec-1)
  - [Problem Set 1](#sec-1-1)
  - [Lecture repos](#sec-1-2)
- [Inheritance](#sec-2)
  - [Terminology](#sec-2-1)
  - [Single Inheritance Multiple Implementation](#sec-2-2)
  - [Basic Inheritance](#sec-2-3)
    - [Example](#sec-2-3-1)
    - [Access Modifiers](#sec-2-3-2)
    - [Exercise](#sec-2-3-3)
  - [Subtyping](#sec-2-4)
    - [Example](#sec-2-4-1)
    - [Casting](#sec-2-4-2)
  - [Overriding](#sec-2-5)
    - [`@Override`](#sec-2-5-1)
  - [`super`](#sec-2-6)
    - [Ordinary methods](#sec-2-6-1)
    - [Constructors](#sec-2-6-2)
    - [Exercise](#sec-2-6-3)
  - [`Object` class](#sec-2-7)
    - [`toString()`](#sec-2-7-1)
    - [Overriding `toString()`](#sec-2-7-2)
- [Interfaces](#sec-3)
  - [What are interfaces, abstractly?](#sec-3-1)
  - [What are interfaces, concretely?](#sec-3-2)
  - [Interface example](#sec-3-3)
  - [Exercise](#sec-3-4)
  - [Coming full circle](#sec-3-5)
- [Important Standard Library Classes](#sec-4)
  - [`List`](#sec-4-1)
    - [`ArrayList`](#sec-4-1-1)
    - [Example](#sec-4-1-2)
    - [Exercise](#sec-4-1-3)
  - [`Map`](#sec-4-2)
    - [&ldquo;Key&rdquo; operations](#sec-4-2-1)
    - [`HashMap`](#sec-4-2-2)
    - [Example](#sec-4-2-3)
  - [`Set`](#sec-4-3)
    - [Important operations](#sec-4-3-1)
    - [`HashSet`](#sec-4-3-2)
    - [Example](#sec-4-3-3)
  - [Iterators and Iterables](#sec-4-4)
    - [&ldquo;Enhanced&rdquo; `for` loops](#sec-4-4-1)
  - [Collections](#sec-4-5)


# Announcements<a id="sec-1"></a>

## Problem Set 1<a id="sec-1-1"></a>

Problem Set 1 staff solutions have been made available at [6178-2017/ps1-staff-solutions](https://github.mit.edu/6178-2017/ps1-staff-solutions). Click on the commit message or the commit SHA to see the solution code itself.

<div class="NOTES">
Demo this.

</div>

## Lecture repos<a id="sec-1-2"></a>

Make sure to make your personal lecture repos private. Select &ldquo;Private&rdquo; when you&rsquo;re creating the repository so that only you can see your lecture exercises.

# Inheritance<a id="sec-2"></a>

## Terminology<a id="sec-2-1"></a>

-   Classes in Java are arranged in a **class hierarchy**, which can be imagined as a tree of classes
-   Each parent-child edge represents the relationship between a class and its **subclass**.
-   The parent class of a subclass is called its **superclass**.
-   The subclass is said to **inherit** from its superclass.

## Single Inheritance Multiple Implementation<a id="sec-2-2"></a>

A class can have multiple subclasses, but each class may only have **one** parent class from which it inherits. This is known as **single inheritance**, as opposed to **multiple inheritance**, which is the system used in some other technologies like C++, Perl, and the Common Lisp Object System.

## Basic Inheritance<a id="sec-2-3"></a>

Instances of a subclass include the state and behavior of the subclass, as well as its parent class (and so on up the hierarchy). That is to say, if we have class `Animal` with subclass `Dog`, every `Dog` instance will *inherit* the properties and methods of the `Animal` class.

### Example<a id="sec-2-3-1"></a>

```java
package examples;
class Animal {
    public String name;
    public Animal(String name) { this.name = name; }
    public void breathe() { System.out.println("in, out"); }
}
class Dog extends Animal {
    public String breed;
    public Dog(String name, String breed) {
        super(name);
        this.breed = breed;
    }
    public void bark() { System.out.println("woof!"); }
}
public class Example1 {
    public static void main(String[] args) {
        Dog fido = new Dog("Fido", "Golden Retriever");
        fido.breathe();
        fido.bark();
        System.out.println(fido.name);
        System.out.println(fido.breed);
    }
}
```

### Access Modifiers<a id="sec-2-3-2"></a>

-   Fields and methods of the superclass will be *inherited*, but if they are `private` or have no access modifier, they will not be &ldquo;visible&rdquo; in the subclass.

<div class="NOTES">
Internally the object will still include those fields and methods, allowing it to still act like an instance of the superclass.

</div>

-   If the fields are visible to the subclass, e.g., `protected` or `public`, then they can be accessed within the subclass as normal.

### Exercise<a id="sec-2-3-3"></a>

```java
package exercises;

class Appliance {
    public int serialNumber;

    public static int NUMBER_OF_APPLIANCES;

    /**
     * Create a new Appliance.
     *
     * Each new appliance gets the next serial number.
     */
    public Appliance() {
        serialNumber = NUMBER_OF_APPLIANCES;
        NUMBER_OF_APPLIANCES++;
    }
}

/*
 * TODO Create a class to represent your favorite type of appliance.
 *
 * - Rename the class to something else
 * - Make the class inherit from `Appliance`
 * - Give it a method unique to that kind of appliance
 * - In main, call that method and print out the serial number of the appliance.
 */
class MyFavoriteKindOfAppliance {
}

public class BasicInheritance {
    public static void main(String[] args) {
    }
}
```

## Subtyping<a id="sec-2-4"></a>

An instance of a subclass can be treated, for all intents and purposes, as an instance of the superclass. For example, all `Dog` objects can be simply treated as `Animal` objects.

We can call `Dog` a **subtype** of `Animal`. Since a **type** just specifies a set of possible values, and all possible `Dog` values are `Animal` values too, we can say that the set of values specified by `Dog` is a subset of that specified by `Animal`.

### Example<a id="sec-2-4-1"></a>

```java
package examples;
public class Example2 {
    public static void main(String[] args) {
        Animal myDog = new Dog("Fido", "Golden Retriever");
        myDog.breathe();
        // this line does not compile:
        // myDog.bark();
    }
}
```

We can also put multiple different kinds of `Animal` into the same array, by declaring the array type to be `Animal[]`.

In the previous example, we cannot treat `myDog` as a `Dog` instance even though it is! This is because we have declared the type of the variable `myDog` to be `Animal`, so the Java compiler requires us to treat `myDog` as only an `Animal`.

### Casting<a id="sec-2-4-2"></a>

We can get around this restriction in the following way:

```java
Dog iSwearItsADog = (Dog) myDog;
iSwearItsADog.bark();
```

By forcibly **casting** the value of `myDog` to type `Dog`, we make the compiler treat the expression as an instance of `Dog` instead of as an `Animal`, so then we can call `.bark()` on the resulting expression.

If we tried to cast an `Animal` that wasn&rsquo;t actually an instance of `Dog` to type `Dog`, then when we run the program, Java will encounter that line, detect that `myDog` mysteriously had type, say, `Cat` instead of `Dog`, and would throw a `ClassCastException`, basically telling you that you can&rsquo;t do that.

## Overriding<a id="sec-2-5"></a>

You can override the behavior of a superclass method in the subclass by simply defining a method with the same name and signature:

```java
package examples;
class Vegetable {
    public String color() { return "green"; }
}
class Carrot extends Vegetable {
    @Override public String color() { return "orange"; }
}
public class Example3 {
    public static void main(String[] args) {
        Vegetable lettuce = new Vegetable();
        System.out.println(lettuce.color()); // green

        Carrot carrot = new Carrot();
        System.out.println(carrot.color()); // orange

        Vegetable carrotAsAVegetable = carrot;
        System.out.println(carrotAsAVegetable.color()); // orange
    }
}
```

Note that the correct `color()` method is called for the `Carrot` object even though the variable that refers to it has a declared type of `Vegetable`!

### `@Override`<a id="sec-2-5-1"></a>

You can annotate overridden methods with an `@Override` tag. This tag is optional but recommended because it will help catch typos (learn more in 6.031).

## `super`<a id="sec-2-6"></a>

You can access the original implementation of overridden methods using the `super` keyword. This allows you to override a method by just **slightly** altering the original behavior.

### Ordinary methods<a id="sec-2-6-1"></a>

```java
package examples;

class Parent {
    public int generation() { return 0; }
}
class Child extends Parent {
    @Override public int generation() { return super.generation() + 1; }
}
class Grandchild extends Child {
    @Override public int generation() { return super.generation() + 1; }
}
public class Example4 {
    public static void main(String[] args) {
        int generation = new Grandchild().generation();
        System.out.println(generation); // 2
    }
}
```

### Constructors<a id="sec-2-6-2"></a>

```java
package examples;
class Animal {
    public String name;
    public Animal(String name) { this.name = name; }
    public void breathe() { System.out.println("in, out"); }
}
class Dog extends Animal {
    public String breed;
    public Dog(String name, String breed) {
        super(name);
        this.breed = breed;
    }
    public void bark() { System.out.println("woof!"); }
}
public class Example1 {
    public static void main(String[] args) {
        Dog fido = new Dog("Fido", "Golden Retriever");
        fido.breathe();
        fido.bark();
        System.out.println(fido.name);
        System.out.println(fido.breed);
    }
}
```

### Exercise<a id="sec-2-6-3"></a>

```java
package exercises;

class Ship {
    private int length;

    public Ship(int length) {
        this.length = length;
    }

    public int getLength() { return length; }

    public boolean isSunk(int hits) {
        return hits >= length;
    }
}

/*
 * TODO Create a class for submarines.
 *
 * - Make it inherit from `Ship`
 * - Give it a constructor:
 *   - `length` is private, so you won't be able to initialize it directly.
 *   - Use `super()` to call `Ship's` constructor from your class's constructor
 * - Override the isSunk() method. Submarines should print out "SOS" before
 *   reporting that they are sunk.
 *   - Don't recompute line 13. Use `super`!
 */

class Submarines /* TODO */ {
    // TODO constructor here

    // TODO override isSunk()
}

public class OverrideAndSuper {
    public static void main(String[] args) {
        /*
         * TODO Create an instance of your new class and use it.
         *
         * - Create an instance of your new class.
         * - Call the isSunk() method with various arguments.
         */
    }
}
```

## `Object` class<a id="sec-2-7"></a>

All classes in Java are part of the same hierarchy, and the root of the hierarchy is class `Object`. Every class is a subclass of `Object` (perhaps through several layers), and every object is therefore an instance of `Object`.

`Object` has several methods, but the most useful one is `toString()`.

### `toString()`<a id="sec-2-7-1"></a>

`Object.toString()` returns a &ldquo;human-readable&rdquo; string representation of the object. For objects whose class does not override `toString()`, the value returned by `toString()` includes the name of the class of the instance as well as a unique(-ish) hexadecimal ID for that object.

### Overriding `toString()`<a id="sec-2-7-2"></a>

```java
package examples;
class Person {
    private String name;
    private int age;
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    @Override public String toString() {
        return "(Person named " + name + ", " + age + " years old)";
    }
}
public class Example5 {
    public static void main(String[] args) {
        Person me = new Person("Aaron", 19);
        String meAsAString = me.toString();
        System.out.println(meAsAString);
        System.out.println(me); // println() automagically calls .toString() on Objects
    }
}
```

# Interfaces<a id="sec-3"></a>

Say you have a class `WaterBottle` and a class `MetalObject`. `WaterBottle` has a bunch of useful methods for objects that contain water, such as pouring out the water into a drinking cup, emptying the water, asking how much water is left in the vessel, etc. `MetalObject` has a bunch of useful methods for objects made out of metal, e.g., `recycle()`.

What if you have a `MetalWaterBottle` in your system, which you would like to treat sometimes as a `WaterBottle` and sometimes as a `MetalObject`?

You cannot make `MetalWaterBottle` inherit from both `WaterBottle` and `MetalObject`, because Java prohibits multiple inheritance.

    #-------------#    #-------------#
    | WaterBottle |    | MetalObject |
    #-------------#    #-------------#
               ^           ^
               |           |
           #------------------#
           | MetalWaterBottle |
           # -----------------#

          ...                                                           .          ..                  s       .                                       ..      .
       xH88"`~ .x8X                                                    @88>  x .d88"                  :8      @88>                                  x88f` `..x88. .>
     :8888   .f"8888Hf        u.      ..    .     :     .d``           %8P    5888R                  .88      %8P          u.      u.    u.       :8888   xf`*8888%     .u    .      .u    .          u.      .u    .
    :8888>  X8L  ^""`   ...ue888b   .888: x888  x888.   @8Ne.   .u      .     '888R         u       :888ooo    .     ...ue888b   x@88k u@88c.    :8888f .888  `"`     .d88B :@8c   .d88B :@8c   ...ue888b   .d88B :@8c
    X8888  X888h        888R Y888r ~`8888~'888X`?888f`  %8888:u@88N   .@88u    888R      us888u.  -*8888888  .@88u   888R Y888r ^"8888""8888"    88888' X8888. >"8x  ="8888f8888r ="8888f8888r  888R Y888r ="8888f8888r
    88888  !88888.      888R I888>   X888  888X '888>    `888I  888. ''888E`   888R   .@88 "8888"   8888    ''888E`  888R I888>   8888  888R     88888  ?88888< 888>   4888>'88"    4888>'88"   888R I888>   4888>'88"
    88888   %88888      888R I888>   X888  888X '888>     888I  888I   888E    888R   9888  9888    8888      888E   888R I888>   8888  888R     88888   "88888 "8%    4888> '      4888> '     888R I888>   4888> '
    88888 '> `8888>     888R I888>   X888  888X '888>     888I  888I   888E    888R   9888  9888    8888      888E   888R I888>   8888  888R     88888 '  `8888>       4888>        4888>       888R I888>   4888>
    `8888L %  ?888   ! u8888cJ888    X888  888X '888>   uW888L  888'   888E    888R   9888  9888   .8888Lu=   888E  u8888cJ888    8888  888R     `8888> %  X88!       .d888L .+    .d888L .+   u8888cJ888   .d888L .+
     `8888  `-*""   /   "*888*P"    "*88%""*88" '888!` '*88888Nu88P    888&   .888B . 9888  9888   ^%888*     888&   "*888*P"    "*88*" 8888"     `888X  `~""`   :    ^"8888*"     ^"8888*"     "*888*P"    ^"8888*"
       "888.      :"      'Y"         `~    "    `"`   ~ '88888F`      R888"  ^*888%  "888*""888"    'Y"      R888"    'Y"         ""   'Y"         "88k.      .~        "Y"          "Y"         'Y"          "Y"
         `""***~"`                                        888 ^         ""      "%     ^Y"   ^Y'               ""                                     `""*==~~`
                                                          *8E
                                                          '8>
                                                           "

## What are interfaces, abstractly?<a id="sec-3-1"></a>

An **interface** defines a set of classes with shared behavior. Instances of these classes can then be treated the same with respect to that behavior, without knowledge of the concrete class of those instances.

## What are interfaces, concretely?<a id="sec-3-2"></a>

An `interface` defines a set of methods (names and signatures). Classes that have all of the methods in an interface are said to **implement** that interface.

Interfaces can also be treated as a type. Classes that **implement** the interface are then subtypes of the interface, because their instances have all of the behavior specified by the interface (and then some).

## Interface example<a id="sec-3-3"></a>

```java
package examples;
interface SoundMaker {
    void makeASound();
}
class Bird implements SoundMaker {
    public void makeASound() { System.out.println("tweet"); }
    public void catchWorms() {}
}
class Person implements SoundMaker {
    public void makeASound() { System.out.println("howdy"); }
    public void philosophize() {}
}
public class Example6 {
    SoundMaker[] things = new SoundMaker[] { new Bird(), new Person() };
    for (int i = 0; i < things.length; i++) {
        SoundMaker soundMaker = things[i];
        soundMaker.makeASound();
        // these two lines don't compile
        // soundMaker.catchWorms();
        // soundMaker.philosophize();
    }
}
```

## Exercise<a id="sec-3-4"></a>

```java
package exercises;

interface Person {
    String getName();
    int getWage();
}

class Employee {
    private String name;
    private int wage;

    public Employee(String name, int wage) {
        this.name = name;
        this.wage = wage;
    }
}

class Manager {
    private String name;
    private int wage;

    public Manager(String name, int wage) {
        this.name = name;
        this.wage = wage;
    }
}

public class Interface {
    public static void main(String[] args) {
        /*
         * TODO Make this method compile. You may have to edit Person, Employee,
         * and/or Manager to get it to work.
         */

        Person ben = new Employee("Ben Bitdiddle", 15);
        Person alyssa = new Manager("Alyssa P. Hacker", 30);

        Person[] company = new Person[] { ben, alyssa };

        for (int i = 0; i < company.length; i++) {
            Person p = company[i];
            System.out.printf("%s makes $%d per hour.\n", p.getName(), p.getWage());
        }
    }
}
```

## Coming full circle<a id="sec-3-5"></a>

Now, we can define `MetalObject` as an interface with a `recycle()` method, and `WaterBottle` as an interface with an `howMuchLeft()` method, and so on.

`MetalWaterBottle` can now implement both of these interfaces, and can be treated as each of the different types in different contexts in your program.

# Important Standard Library Classes<a id="sec-4"></a>

All of these classes and interfaces are in `java.util`.

## `List`<a id="sec-4-1"></a>

`List`&rsquo;s are like Python lists. They represent ordered sequences of elements that can be accessed by 0-based numerical indices. Like Python&rsquo;s lists, they can be appended to, or inserted into. This makes them strictly more flexible than arrays because you can change the length of a `List` over the course of your program&rsquo;s execution.

`List` is actually an `interface`. It has many methods that you might expect, including `get()`, `set()`, `size()`, `add()` (append), and so on. There are several implementations of `List` in the Java standard library (i.e., classes that &ldquo;implement&rdquo; `List`), but the most important is `ArrayList`.

### `ArrayList`<a id="sec-4-1-1"></a>

An `ArrayList` is a `List` whose internal data structure is an array. When you run out of space and need to add more elements, it:

-   Creates a new, bigger, array;
-   Copies all of the elements from the old array; and
-   Inserts the new element at the end

### Example<a id="sec-4-1-2"></a>

```java
List<String> favoriteWords = new ArrayList<String>();

favoriteWords.add("teacup");
favoriteWords.add("pasta");
favoriteWords.add("java");

System.out.println(favoriteWords); // [teacup, pasta, java]

System.out.println(favoriteWords.size()); // 3

System.out.println(favoriteWords.get(1)); // pasta

favoriteWords.set(2, "Java");

System.out.println(favoriteWords); // [teacup, pasta, Java]
```

<div class="NOTES">
Turns out org-reveal has a bug that causes the angle brackets in src environments to fail to be escaped. Guess who had to file an [issue](https://github.com/yjwen/org-reveal/issues/265) at 01:20 a.m. this morning?

</div>

### Exercise<a id="sec-4-1-3"></a>

```java
public class ListExercise {
    public static void main(String[] args) {
        List<Integer> numbers = new List<Integer>();

        for (int i = 0; i < 20; i++) {
            int randomInt = (int)(Math.random() * 100);
            numbers.add(randomInt);
        }

        List<Integer> evenNumbers = new List<Integer>();

        // TODO add all of the even numbers in `numbers` to `evenNumbers`

        // TODO print out evenNumbers (use toString())

        // TODO print out how many elements evenNumbers has
    }
}
```

<div class="NOTES">
Mention that `println()` has an implicit `toString()` call.

</div>

## `Map`<a id="sec-4-2"></a>

A `Map<K, V>` is akin to a Python dictionary. Conceptually, it is an unordered table with key-value pairs, at most one per unique key.

<div class="NOTES">
K is the type of the key element, V is the type of the V element. For example, Map<String, Integer>

</div>

### &ldquo;Key&rdquo; operations<a id="sec-4-2-1"></a>

-   `get(key)` retrieves the value associated with a given key in the map, null if no such key
-   `put(key, val)` associates the given key with the value, replacing if already present
-   `size()` returns the number of key-value pairs
-   `containsKey(key)` returns `true` iff the map contains an entry for `key`

### `HashMap`<a id="sec-4-2-2"></a>

`HashMap` is the most commonly-used implementation of `Map`.

### Example<a id="sec-4-2-3"></a>

```java
Map<String, Integer> phonebook = getTheDataSomehow();

String name = "Jane Doe";
int phoneNumber = phonebook.get(name);
System.out.printf("%s can be reached at %d.\n", name, phoneNumber);
```

## `Set`<a id="sec-4-3"></a>

A `Set` represents an unordered set of elements.

### Important operations<a id="sec-4-3-1"></a>

-   `contains(e)` returns `true` iff the set contains an element equal to `e`
-   `add(e)` adds element `e` to the set
-   `remove(e)` removes element `e` from the set
-   `size()` returns the number of elements in the set

### `HashSet`<a id="sec-4-3-2"></a>

`HashSet` is the most commonly-used implementation of `Set`.

### Example<a id="sec-4-3-3"></a>

```java
Set<Integer> luckyNumbers = openFortuneCookie();

luckyNumbers.remove(13); // in case the fortune cookie is wrong

int myFavoriteNumber = 8;

boolean isLucky = luckyNumbers.contains(8);
```

## Iterators and Iterables<a id="sec-4-4"></a>

`Iterator<E>` and `Iterable<E>` are both interfaces.

An `Iterator<E>` is an object that can supply objects of type `E`. When you call `next()` on an `Iterator`, it returns the next object of type `E` in the iteration, or throws `NoSuchElementException` if there are no more elements.

You can call `hasNext()` on an `Iterator<E>` to find out if there are any more elements available. This can help you avoid `NoSuchElementException` errors.

```java
// this code prints out all of the names provided by namesIter

Iterator<String> namesIter = magic();

while (namesIter.hasNext()) {
    String name = namesIter.next();
    System.out.println(name);
}
```

An `Iterable<E>` is an object that has a method `iterator()`, which must return a value of type `Iterator<E>`. That is to say, it is an object that can be &ldquo;iterated over&rdquo;, such as `List`&rsquo;s, `Set`&rsquo;s, and even arrays.

### &ldquo;Enhanced&rdquo; `for` loops<a id="sec-4-4-1"></a>

`Iterable`&rsquo;s are useful because they enable a convenient syntax feature in Java: the &ldquo;enhanced&rdquo; `for` loop. This makes iterating over things easier:

```java
int[] numbers = new int[] { 1, 2, 3, 4, 5 };

int sum = 0;
for (int x : numbers) {
    sum += x;
}

System.out.println(sum); // 15
```

<div class="NOTES">
This can be a `List` or a `Set` too.

</div>

An enhanced `for` loop has the following form:

```java
for (<type> <name> : <iterable>) {
    // do something with the variable <name>
}
```

This iterates over the iterable until it is depleted, just like the previous `while`-loop example.

## Collections<a id="sec-4-5"></a>

`List`&rsquo;s and `Set`&rsquo;s are both `Collection`&rsquo;s. They represent a group of objects called *elements*. There are many useful methods that operate on all kinds of `Collection`&rsquo;s in general. All `Collection<E>`&rsquo;s are `Iterable<E>`&rsquo;s.
