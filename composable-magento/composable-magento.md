# [fit] The **how** and the **why** of composable Magento

---

### James Cowie
#### Technical Team Lead
##### t/**@jcowie** gh/**jcowie**

### Alistair
#### CTO Session Digital
##### t/**@alistairstead** gh/**alistairstead**

---

# Reuse...

**verb** |riːˈjuːz| _[ with obj. ]_
use again or more than once: the tape could be magnetically erased and reused.

**noun** |riːˈjuːs| _[ mass noun ]_
the action of using something again: the ballast was cleaned ready for reuse.

^ What is reuse? Programmers are taught this from day one. This is watchword for everything that drives our industry. Write code so it can be re-used re-purposed. This is why we have modular patterns, inheritance, extension points. This drives the idea of don't repeat yourself. In fact I have been heard to state many times both in public to any audience that would listen to me, and privately that I would rather hire a lazy engineer than one prepared to merrily keep typing and building the same features over and over and over...

^ Is re-use a myth? A fallacy driven from misunderstanding or the principles by which DRY was defined? A big industry misunderstanding?

---

### **D**on't **R**epeat **Y**ourself

![](http://c.fastcompany.net/multisite_files/coexist/article_feature/1280-dry-land-farming.jpg)

^ DRY is not about simplistic code duplication. It is about duplication of knowledge and intent.

---

### “Every piece of knowledge should have one and only one representation.”[^1]

[^1]:The Pragmatic Programmer, by Dave Thomas and Andy Hunt

^ This is the most subtle of principles. We tend to think of duplication at a code level — a mechanical “this looks like that, so duplication!” level. However, this guiding principle isn’t about code duplication; it is about knowledge duplication.

^ A lot of people are introduced to this idea through the DRY principle, or Don’t Repeat Yourself. This was established in the book, The Pragmatic Programmer, by Dave Thomas and Andy Hunt.

^ The DRY principle states “Every piece of knowledge should have one and only one representation.” This rule also has been expressed as “Once and Only Once.” http://pragprog.com/book/tpp/the-pragmatic-programmer

^ Instead of looking for code duplication, always ask yourself whether or not the duplication you see is an example of core knowledge in the system.

---

### Inheritance

^ Inheritance, objects and the promised land of reuse...

^ Inheritance gives us reuse... It lets us use fewer key strokes in order to be share behavior between objects. But this type of reuse is lying to you. Inheritance creates tight coupling and eventually limits the scope to which you can reuse your beautifully crafted code.

---

### Traits and Mixins?

^ Oh wait, then the language authors and fellow programmers go and provide another way to misrepresent reuse and tempt us into thinking we are following DRY principles. This is closer but can provide a false sense of accomplishment.

---

### Modules for reuse

^ We have modules for reuse but it is only reuse within the context of Magento and at that only reuse with Magento as of the major version when you wrote the module.

---

### Composition

---

### Composer
^ Magento 1 module authors had the problem of installing modules in a reliable and repeatable way. As such in 2010 Colin Mollenhour went out and created modman.

^ Modman allowed us to take individual modules and put them in there own source control system and use modman to link them into a Magento 1 install. For a
series of bash scripts this worked really really well and is still in use for many projects today. However in 2011 Jordi Boggiano started out on creating
a solution for the PHP community. His goal was to have a reliable package manager for PHP so anyone could create packages that could be used in any
project. Composer was going to be the tool that managed a projects dependency by pinning versoins of installed software and could over time manage the
dependencies of dependencies.

^ One of the problems we as Magento developers faces was that Magento 1 shipped with a non standard and easily extensible autoloader so any
opportunity to load or use these packages was not going to be an easy journey. Code was loaded via lib or a code pool. Composer only knows about loading content from src.

^ Welcome to composer-hackathon project. This was a massive step forward in the composer Magento history. Based on this work and the foudning work by Modman we now had a reliabel way to install Magento 1 modules in a reliable way.

^ Note problems on modules not being composable ? 
---

### Interfaces

---

### Service Contracts

---

### Composable Magento

---

### Abstraction away from the framework

^ If you are using composer and the generated autoloading functionality it is practical to write code that is unaware of Magento, that sits in some other location than a Magento module but is wired into the structures and deep inheritance chain that Magento defines. The wiring however is minimal and isolated away from the code that solve the actual problem. This means you have created a solution prime for re-use because it can be re-wired or re-composed into other projects and applications.

---

### The naked Magento module

^ The idea of the naked module is to create code that is completely unaware that it will be applied within a Magento application. This would mean that it can also be applied to a new version of Magento with a greatly reduced amount of re-work. We get to apply DRY in the true form and only create the new wiring to Magento 2 or 3 or 4. Magento will last for ever you know!

---

### Your **value** as an engineer is not the sum of the code you have written

^ The value of engineer is their ability to write code to solve problems. However their value is not attached to that code. Deleting code should not be painful and you should not be protective of your code beyond it's ability to solve a problem.

^ A higher value of an engineer is the ability to solve problems using the most appropriate solution available to them which may indeed involve choosing to not write code but instead assess and select an appropriate solution written by someone else.

---

### Ephemeralisation

---

### Doing more with less

![](http://blog.lucid.berlin/wp-content/uploads/sites/4/2015/02/r-buckminster-fuller-5.jpg)

^ The true power of an engineer is doing more with less. Using composable components to deliver functionality should be seen far superior to re-crafting code to solve a problem already solved by someone else. Not least because they may well know more about the problem than you.

---

### Credits

* https://skillsmatter.com/skillscasts/6433-business-logic-a-different-perspective
* https://www.youtube.com/watch?v=X8lqnO7aYe0
