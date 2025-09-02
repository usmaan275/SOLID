## **1. Single Responsibility Principle (SRP)**

**Martin’s summary:**
"A class should have one, and only one, reason to change."

**Nouman Ali Khan explanation (conversational):**
You know, sometimes we try to do too much at once, and it becomes messy. The same happens with classes. If a class is trying to cook, serve, and clean all at the same time, every time one thing changes, it can break something else. It’s much simpler and safer if a class focuses on just **one responsibility**. Then you know exactly what will change when you need to make an update.

**Technical explanation:**
Single Responsibility Principle ensures that a class has a single, well-defined functionality. When a class combines multiple responsibilities, modifying one part may inadvertently affect the other parts. Keeping a class focused on one responsibility increases cohesion, reduces coupling, and makes the code easier to maintain, test, and extend.

**Analogy:**
Think of a chef in a kitchen. If the chef has to cook, serve, clean, and manage accounts, chaos ensues. But if each person has one responsibility—one cooks, one serves, one cleans—then the kitchen runs smoothly.

**Java implementation:**

```java
// ❌ BAD: One class handling multiple responsibilities
class KitchenStaff {
    void cook() { System.out.println("Cooking food"); }
    void serve() { System.out.println("Serving food"); }
    void clean() { System.out.println("Cleaning kitchen"); }
}

// ✅ GOOD: Each class has a single responsibility
class Chef {
    void cook() { System.out.println("Cooking food"); }
}

class Waiter {
    void serve() { System.out.println("Serving food"); }
}

class Cleaner {
    void clean() { System.out.println("Cleaning kitchen"); }
}
```



---

## **2. Open-Closed Principle (OCP)**

**Martin’s summary:**
"You should be able to extend a class's behavior, without modifying it."

**Nouman Ali Khan explanation (conversational):**
Think about this—every time you want to add something new, you don’t want to break what already works. If you go and change the existing class, even a tiny change could create a bug somewhere else. It’s much safer if you can **extend the class** to add new features without touching the old code. That way, the original works perfectly, and you just build on top of it.

**Technical explanation:**
Open-Closed Principle states that software entities should be open for extension but closed for modification. This is typically achieved using inheritance or polymorphism. The idea is to add new functionality by extending existing code rather than modifying it, which protects verified functionality and reduces risk of bugs.

**Analogy:**
Think of a tree. You don’t cut the trunk to add a new branch. You grow the branch on top. The trunk remains solid, and the tree grows safely without compromising what’s already there.

**Analogy 2:**
Think of a car. You don’t need to take apart an entire car just to add a spoiler. The car already works perfectly. Instead, you can attach the spoiler onto it without modifying the original car. The original car remains functional, and the new feature is added safely.  

**Java implementation:**  

```java
// ❌ BAD: Modifying the Car class to add a spoiler
class Car {
    void startEngine() { System.out.println("Engine started"); }
    void drive() { System.out.println("Driving..."); }
    
    // Later added spoiler here → modifying old class
    // void addSpoiler() { System.out.println("Spoiler added"); }
}

// ✅ GOOD: Extend Car class to add a spoiler
class Car {
    void startEngine() { System.out.println("Engine started"); }
    void drive() { System.out.println("Driving..."); }
}

class SpoilerCar extends Car {
    void addSpoiler() { System.out.println("Spoiler added"); }
}
```

---

## **3. Liskov Substitution Principle (LSP)**

**Martin’s summary:**
"Derived classes must be substitutable for their base classes."

**Nouman Ali Khan explanation (conversational):**
Here’s the point—you should be able to replace a parent class with a child class without breaking anything. If the child behaves differently in a way the parent didn’t promise, the code that uses the parent will break. So subclasses should **honor the expectations** of their base classes.

**Technical explanation:**
Liskov Substitution Principle ensures that subclasses can replace their base classes without altering expected behavior. Violating LSP leads to fragile code and breaks polymorphism. Proper inheritance maintains substitutability and predictable behavior.

**Analogy:**
Think of a wheelchair ramp. Any wheelchair should be able to use the ramp safely. If a new type of wheelchair suddenly doesn’t fit, the ramp has failed its purpose. The same goes for subclasses—they must fit the expectations set by the base class.

**Analogy 2:**
Think of a car. Some cars have engines (petrol cars), and some don’t (electric cars). If the base Car class requires a startEngine() method, forcing all cars—including electric cars—to implement it, this would violate this principle. Electric cars don’t have engines, so they can’t fulfill that expectation, so we shouldn't put a startEngine() method in Car. Subclasses should only promise behavior that makes sense for them.

**Java implementation:**

```java
// ❌ BAD: ElectricCar breaks expectations of Car
interface Car {
    void startEngine();
}

class PetrolCar implements Car {
    public void startEngine() { System.out.println("Engine started"); }
}

class ElectricCar implements Car {
    public void startEngine() { throw new UnsupportedOperationException(); }
}

// ✅ GOOD: Separate responsibilities
interface EngineCar { void startEngine(); }

class PetrolCar2 implements EngineCar {
    public void startEngine() { System.out.println("Engine started"); }
}

class ElectricCar2 {
    void startMotor() { System.out.println("Motor running"); } // no engine assumption
}
```

---

## **4. Interface Segregation Principle (ISP)**

**Martin’s summary:**
"Make fine grained interfaces that are client specific."

**Nouman Ali Khan explanation (conversational):**
Sometimes we give people things they don’t need, and it just makes their job harder. In code, if a class is forced to implement methods it doesn’t use, it becomes messy and harder to maintain. We should give each class **just what it needs**.

**Technical explanation:**
Interface Segregation Principle says that large, fat interfaces should be split into smaller, client-specific interfaces. Classes should only implement interfaces relevant to their responsibilities, reducing unnecessary coupling and promoting maintainability.

**Analogy:**
Think of a zookeeper. Some zookeepers feed and wash the bears. Others just pet them. Don’t make someone who only feeds bears also pet them—they shouldn’t have that extra responsibility.

**Java implementation:**

```java
// ❌ BAD: Fat interface
interface BearKeeper {
    void washBear();
    void feedBear();
    void petBear();
}

class BearCarer implements BearKeeper {
    public void washBear() {}
    public void feedBear() {}
    public void petBear() { throw new UnsupportedOperationException(); }
}

// ✅ GOOD: Small, client-specific interfaces
interface BearFeeder { void feedBear(); }
interface BearCleaner { void washBear(); }

class BearCarer2 implements BearFeeder, BearCleaner {
    public void feedBear() {}
    public void washBear() {}
}
```

---

## 5. Dependency Inversion Principle (DIP)

**Martin’s summary:**  
> "Depend on abstractions, not on concretions."

**Nouman Ali Khan explanation (conversational):**  
Think about this: if your high-level code is tied directly to a specific detail, like a particular keyboard, changing that detail becomes a nightmare. Instead, rely on a general idea—a contract. Then you can swap in any specific implementation without breaking anything. This makes your code flexible and easy to test.  

**Technical explanation:**  
Dependency Inversion Principle means high-level modules should depend on abstractions (interfaces or abstract classes), not concrete implementations. Low-level modules should also depend on these abstractions. This decouples the system, allows for easier testing, and makes replacing or extending functionality simple.  

**Analogy:**  
Think of a computer and its keyboard. You don’t want the computer to only work with one brand of keyboard. If it’s tied to a specific keyboard, changing to a better keyboard breaks everything. Instead, the computer should depend on the idea of a keyboard. Any keyboard that implements this idea can be plugged in, and everything still works perfectly.  

**Java implementation:**  

```java
// ❌ BAD: High-level class depends on a specific keyboard
class Computer {
    StandardKeyboard keyboard = new StandardKeyboard();
    void type() { System.out.println("Typing on standard keyboard"); }
}

// ✅ GOOD: High-level class depends on abstraction
interface Keyboard {
    void type();
}

class StandardKeyboard implements Keyboard {
    public void type() { System.out.println("Typing on standard keyboard"); }
}

class MechanicalKeyboard implements Keyboard {
    public void type() { System.out.println("Typing on mechanical keyboard"); }
}

class Computer2 {
    private Keyboard keyboard;

    Computer2(Keyboard keyboard) {
        this.keyboard = keyboard;
    }

    void type() {
        keyboard.type();
    }
}

// Usage
Computer2 myComputer = new Computer2(new MechanicalKeyboard());
myComputer.type(); // Typing on mechanical keyboard
```
