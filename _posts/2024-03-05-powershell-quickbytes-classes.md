---
title: "PowerShell (QuickBytes) classes"
categories:
  - PowerShell
  - QuickBytes
tags:
  - powershell
---

A few months ago I came across this question on X.

*'What is the output of the last line if you run the code pasted below?'*

1. Brown
2. SyntaxError
3. InvalidOperation
4. Nothing.

```powershell
class Dog {

    [string]
    $Hair

    Dog() {
        $this.Color = 'Brown'
        $this.Hair = 'Long'
    }
}

([Dog]::new()).Color
```

In this case, the answer would be **3** `InvalidOperation`. 

But why?

## Table of contents

- [Table of contents](#table-of-contents)
  - [Classes](#classes)
  - [Instantiation](#instantiation)
  - [Constructor](#constructor)
  - [Properties](#properties)
  - [But why do we get an `InvalidOperation` error?](#but-why-do-we-get-an-invalidoperation-error)
  - [Methods](#methods)
  - [Wrapping Up](#wrapping-up)

### Classes

You can think of classes as a sort of blueprint that specifies how an object will look and behave.

### Instantiation

Classes need to be instantiated before we can perform any actions with them. Instantiating means that we first have to create an object that holds an instance of our class. In other words, we create an object based on our blueprint. To instantiate our `Dog`  class we simply type: `[Dog]::new()` 

### Constructor

*Constructors* are special methods that are called whenever a class is instantiated. They are **optional**, meaning you can define a class without a constructor. The constructor must **always** have the same name as the class. In our `Dog` class, it is the `Dog(){}` code block where the values of the properties `Color` and `Hair` are set.

```powershell
    # Constructor											
    Dog() {
        $this.Color = 'Brown'
        $this.Hair = 'Long'
    }
```

**TIP**
Methods are similar to functions. In class terminology, we always refer to a function as a method.
{: .notice--info}

### Properties

Our `Dog()` constructor are responsible for setting the value of two properties. `Color` and `Hair`. Both referred to by the automatic variable `$this`. `$this` is nothing more then a reference to the current instance of the property. 

### But why do we get an `InvalidOperation` error?

Looking at our class we’ve only defined the property `Hair` . The property `Color` is missing. Hence  the `InvalidOperation` error. 

```powershell
    # Properties			
    [string]
    $Hair
```

### Methods

Our `Dog` class currently does not contain any methods. Methods must always have a *return* type. If you’re method is returning a string, you’re class would be something like:

```powershell
class Dog {

    [string]
    $Hair

    [string]
    $Color

    Dog() {
        $this.Color = 'Brown'
        $this.Hair = 'Long'
    }

    # Method Bark() that will return a string
    [string] Bark(){
		return 'Woof'
	}
}

([Dog]::new()).Bark()
```

### Wrapping Up

Need more info on classes, check this awesome series: https://4sysops.com/archives/powershell-classes-part-1-objects/