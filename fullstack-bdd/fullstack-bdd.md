footer: \#fullstackcon
slidenumbers: false

# [fit] Fullstack **BDD**
### _and its side effects_

^ This is not yet another talk about how to use .feature files to test the pages of your website. That may be the examples you have seen and thought, hey I can do that in my existing testing tools. This seems expensive and brittle, it will break the moment I update the UI. The overhead is not worth any discernible gain.

^ Where is the value in doing this? It is not that there is no value it is just that the value is not visible to the people it should be or it is only valuable to the technical people.

---

### Alistair Stead
#### CTO
## ![](https://static1.squarespace.com/static/ta/55bb7996e4b0682032281f3c/207/assets/images/logo.png)
### **@alistairstead**

^ I'm CTO at the Inviqa group. We are a system integrator that provides software consultancy and delivery primarily with Open Source technology.

^ I'm a polyglot programmer with an inquisitive disposition. "Why?" is the question that drives me. Why write this code? To what end? Why can't this be better? Why is this not good enough?

---

# [fit] `src.match(/(B|T|D)DD/).should.be.true`

^ I'm not saying that if you are not doing BDD, TDD, DDD, or any other DD, that you are doing it wrong.

^ If it is a quick experiment then *hack* away. The key for me is to use the tools available to you that fit your context and allow you to write the software that is needed with the quality that is appropriate to its life span and how it will be applied.

---

# [fit] **tools** _>_ practices _>_ principles

^ I'm going to make the wild assumption that we all in this room are technologists? We seek to solve problems with technology.

^ This means to some extent that we fixate on technical solutions.

---

# [fit] **⚠︎** This is somewhat dangerous

^ Higher education tends to focus on building knowledge and understanding of core principles. But in the space available within an online tutorial and the attention span of the engineer looking for an answer to their immediate problem, we find a focus on the tools, the API and the simple practices of how to use them not how to apply them at their best. Not how to understand how to apply the principles.

^ If you are familiar with the Dryfus model of skill acquisition it describes a mode of skill development where at the beginning you rely upon rigid structure and practices. You need clear instructions and guidance to accomplish a task. Only when you progress towards proficiency can you appreciate the high level principles to their full.

---

# [fit] 🔨 _Tool_ selection

^ Select tools that make your life easier, that inspire you to deliver the right solution and facilitate the creation of great code. That protect you from poor code architecture that promote good practices.

---

# [fit] `mocha.js | jasmine.js | cucumber.js | ...`

^ There are many tools to JavaScript and Node engineers to accomplish our BDD / TDD goals but I'm going to focus on cucumber.js as I think that is possibly the most maligned and miss represented from my experience.

---

# [fit] BDD is about **translation**

^ For me BDD is about dealing with the cost if translation, about forming ubiquitous language allowing greater conversation. Not just the language of the business people or the language of tech but one that makes sense to all involved and forces it to the front in any interactions. Anyone familiar with Domain Driven Design (DDD) and having read anything by Eric Evans will say that sounds like a goal of DDD. It is, and this is where many concepts start to be addressed from different angles. The principle is the key not the practice or the tool.

---

# [fit] BDD `!==` functional testing

^ BDD is about gaining and understanding of the outward behavior of your code in the context of the value it delivers. It is about understanding the interactions between consumers and the code. If you can't attach your story to some value then you are looking at it from the perspective of the wrong Actor .

---

```gherkin
# features/myFeature.feature

Feature: Example feature
  As a user of cucumber.js
  I want to have documentation on cucumber
  So that I can concentrate on building awesome applications

  Scenario: Reading documentation
    Given I am on the Cucumber.js GitHub repository
    When I go to the README file
    Then I should see "Usage" as the page title
```

^ This is the example from the cucumber.js repository and you would be forgiven for forming the initial opinion that BDD is about functional testing. Specifically about testing an application by driving it through the UI and the browser.

---

```javascript
// features/support/world.js
var zombie = require('zombie');
function World() {
  this.browser = new zombie(); // this.browser will be available in step definitions

  this.visit = function (url, callback) {
    this.browser.visit(url, callback);
  };
}

module.exports = function() {
  this.World = World;
};
```

---

```javascript
// features/step_definitions/myStepDefinitions.js

module.exports = function () {

  this.Given(/^I am on the Cucumber.js GitHub repository$/, function (callback) {
    this.visit('https://github.com/cucumber/cucumber-js', callback);
  });

  this.When(/^I go to the README file$/, function (callback) {
    callback.pending();
  });

  this.Then(/^I should see "(.*)" as the page title$/, function (title, callback) {
    var pageTitle = this.browser.text('title');
    if (title === pageTitle) {
      callback();
    } else {
      callback(new Error("Expected to be on page with title " + title));
    }
  });
};
```

---

# [fit] Using gherkin for **translation**

^ A better way to use this tool is to use it as a point at which you translate between the business and the development team. Working collaboratively to understand the details of a requirement is far more effective.

---

# [fit] Using gherkin to define a **ubiquitous language**

^ Use this document as the place where ongoing conversation and debate happens.

---

# [fit] Using gherkin to describe the **domain**

^ Model the domain through scenarios and examples.

---

# Domain features

```gherkin
Feature: Break slow service requests early
  In order to prevent slow upstream systems from impacting the performance of my application
  As an engineer calling an upstream system
  I want a circuit breaker to divert a call that is taking too long

  Scenario: Service responding within limits
    Given a breaker that wraps request
    When I make a request of service "http://service.dev/will/succeed"
    Then the callback should be invoked with the response "Success"
```

^ In this form even though it is a technical example you can start to see the domain come to the fore.

---

```javascript
  this.Given('a breaker that wraps request', () => {
    this.breaker = Breaker.wrap(request, 'unique.test.request.key');
  });

  this.When(/^I make a request of service "([^"]*)"$/, (url, callback) => {
    this.breaker.get(url, (err, response, body) => {
      this.err = err;
      this.response = response;
      this.body = body;
      callback();
    });
  });

  this.Then(/^the callback should be invoked with the err "([^"]*)"$/, (err, callback) => {
    this.response.statusCode.should.equal(500);
    this.err.should.equal(err);
    callback();
  });
```

^ The development team glue these steps to the code that shows how the code will be used through the form of exercising the code.

---

# [fit] Development workflow

^ How does this all fit into the development workflow?

^ It starts the conversation and facilitates the ongoing dialogue with all the stakeholders.

---

# [fit] BDD & TDD

![](http://cdn.meme.am/instances/500x/55794716.jpg)

^ BDD describes the business domain and the external behaviors

^ TDD drives the development of the code components.

^ If it a purely technical component, the unit tests may be enough?

---

## BDD ~ Business domain
##
## TDD ~ Developer domain

^ These different practices are facilitated by different tools but are born of the same principles.

---

# [fit] RED -> GREEN -> REFACTOR♻︎

^ Adopting them into your workflow you would ideally fall into a steady cycle of small iteration of approximately 20 minutes where you write a failing test, write only enough code to make this test pass, then you go back and refactor your code.

---

# [fit] refactoring[^1]

[^1]: Changing the design of the software without changing it's behavior

---

# [fit] ✏️ Design

^ I'm not suggesting you resort to UML and model your code on a white board. What I'm talking about here is emergent desgin and the ability refactor allows that.

^ Refactoring gives you the opportunity to design your code to build that elegant maintainable solution you hoped you would make first time round. Lets be honest look at the code you wrote last week, would you write it the same way this week after another 7 days of learning? However do you feel you can change it easily and achieve what you want with confidence?

---

# [fit] Design patterns

![top right](http://t0.gstatic.com/images?q=tbn:ANd9GcQYYgj_qlNHWQecLsaHLSQxzb4Cgkh1JjsJAAGlFyh1gtkVnK0J)
![bottom right](http://addyosmani.com/resources/essentialjsdesignpatterns/cover/cover.jpg)

^ Design patterns are useful but I'm certainly not saying you should refactor towards design patterns. Patterns are a common vocabulary that you can use to solve problems but use them with thought.

---

# [fit] SOLID principles[^2]

[!2]:http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod

^ The SOLID principles were originally codified by Robert “Uncle Bob” Martin, bringing a set of existing design principles together in a more easily-understandable format. Ultimately the goal of SOLID is clean code that can be easily re-used.

^ Single Responsibility (SRP)
^ Open-Closed (OCP)
^ Liskov Substitution (LSP)
^ Interface Segregation
^ Dependency Inversion

---

# [fit] Beware accidental complexity

^ Simply throwing all of these concepts are you project will likely result in one scenario, complexity. Abstraction for abstractions sake. Use of design patterns because that is what this opinionated person stood up and said were good.

^ Complexity has a translation cost. Complex makes sense now in this moment when you created it. That seemingly elegant solution you spent the last 3 hours byte shifting. In 3 months will make little to know sense to you let alone others.

^ Striving for clean code protects us from creating complexity or at least it can with some simple rules.

---

# [fit] Abstract and simplify

* All tests pass
* Expresses intent
* No duplication
* Small

![right](https://s3.amazonaws.com/titlepages.leanpub.com/4rulesofsimpledesign/large?1425550529)

^ Understanding the 4 rules of simple design by Corey Haines is in my opinion a much more useful entry to these considerations. By starting to consider these rules you will happen upon the large concepts of SOLID and Design Patterns.

^ These rules give you the opportunity ask "Why?" and to assess the code you have crafted. To ensure that you are prioritizing the principles over the tools.

^ Duplication is not simply code duplication but shared knowledge or duplication of concepts.

---

# [fit] Write just enough tests

^ More code is more cost and complexity.

---

# [fit] Write just enough code

^ More code is more cost and complexity.

---

# [fit] Build tiny module[^3]

[^3]:@substack http://substack.net/how_I_write_modules

^ This is the node philosophy. Before that it was the UNIX philosophy

---

# [fit] Write clean code

^ This isn't about defining getter and setters.

^ It is not about using a specific programming paradigm OO or Functional. The same applies across all of this.

---

# [fit] Write code with a clear intent

^ All code has an external surface a UI it exposes methods and it should make sense

^  UX / DX / UI / API

^ Readability, Readability, Readability - Defend against the Caveman Coder (Kitson Kelly)

---

# [fit] Identify duplication and complexity

^ Don't forget this applies to your tests as well as your code.

---

# [fit] **Common smells** 💩

---

# [fit] Hard to test

^ Hard to test means tightly coupled

---

# [fit] Tests are expensive the maintainable

^ Lots of tests need to be updated when you change the API means too much awareness of other components.

^ Inappropriate intimacy within the code or in the tests, use test doubles. If using doubles is hard then code is again too tightly coupled.

---

# Brittle scenarios

```gherkin
Feature: Example feature
  As a user of website.com
  I want to register for an account
  So that I can use the service

  Scenario: New user successfully registers
    Given I a new user
    When I go to the register page
    And I fill 'name' with 'Peter Parker'
    And I fill 'email' with 'p.parker@omni.corp'
    And I click 'submit'
    Then I should seen 'Thank you for registering'
```

^ Why may this be brittle? A: It is tied to a specific UI

^ This is a scenario where "re-use" is causing people to bring implementation detail to the top rathe than push it down into the supporting glue code.

^ Developers should be building the glue to the code and connecting the domain to the outside world

---

# [fit] Test fail for unrelated components

^ Test fail when you change unrelated components indicates a lack of cohesion.

^ Law of Demeter - At its heart, the LoD is about encapsulation. We don’t want to reach inside an object and manipulate its insides; that’s just mean. Instead, we want to ask objects to perform some action for us. Let the object deal with its collaborators.

^ Only one dot per statement!

---

# [fit] **Should you adopt BDD?**

---

# [fit] There is a cost!

^ If you simply follow examples and commit some of the errors I described here. It will not only add costs initially but also longer term maintenance costs that will likely exceed the value.

---

# [fit] Sustainable agility

^ Done well and with a healthy level of skepticism then it can help you achieve a sustainable pace and ability to handle change with very little impact.

---

# [fit] Constantly pay down the technical debt

^ Refactoring is at the heart of the workflow and the ability to change the inner functionality while being confident that you are maintaining the external behavior

---

# [fit] Automation

^ Testing and validation of acceptance criteria that you have developed in collaboration with the all the stakeholders.

---

# [fit] Regression tests

^ Free regression testing being built as you work not as an after thought.

---

# [fit] Documentation

^ Free documentation that lives and grows with your code. It stays in sync and its value is maintained without significant additional investment.

---

# [fit] How do you apply this at different levels?

^ Okay we are at fullstack so how do you apply this to the different layers of the stack

---

# [fit] Use different Worlds...

* Domain
* API
* UI

^ You can build your features through all the layers developing them using the same guiding steps.

^ You glue the implementation to these contexts

---

### World

```javascript
// features/support/world.js
var world = process.env.WORLD;
if (!world) {
  console.warn("Please set $WORLD before running. Defaulting to 'domain'...");
  world = 'domain';
}

module.exports = function () {
  var worldSource = './' + world + '_world';
  this.World = require(worldSource).World;
};
```

---

### UI World

```javascript
// features/support/ui_world.js
...
function UiWorld(ready) {
  var shop = browser.params.shop;
  var self = {
    setPrice: shop.setPrice,
    scan: function (scannedQuantity, callback) {
      var quantityInput = element(by.css('#quantity'));
      var scanButton = element(by.css('#scan'));
      browser.get('/');
      quantityInput.clear();
      quantityInput.sendKeys(scannedQuantity);
      scanButton.click().then(callback);
    },
    totalIs: function (expectedTotal, callback) {
      var total = element(by.css('#total'));
      ...
    }
  };
  ...
}
module.exports = { World: UiWorld };
```

---

### API World

```javascript
// features/support/api_world.js
...
function ApiWorld(ready) {
  var shop = new Shop();
  var webApp = new WebApp(shop);
  var self = {
    ...
    scan:
      function (scannedQuantity, callback) {
        request(webApp)
          .post('/basket/cucumber')
          .send({ quantity: scannedQuantity })
          .expect(201)
          .end(callback);
      },
    totalIs:
      function (expectedTotal, callback) {
        request(webApp)
          .get('/basket')
          .expect(200)
          .expect(function (res) {
            assert.equal(expectedTotal, res.body.total);
          })
          .end(callback);
      }
  };
  ...
}
module.exports = { World: ApiWorld };
```

---

### Domain World

```javascript
// features/support/domain_world.js
...
function ShopWorld(ready) {
  var shop = new Shop();
  var self = {
    ...
    scan: shop.scan,
    totalIs:
      function (expectedTotal, callback) {
        shop.calculateTotal(function (err, actualTotal) {
          assert.equal(actualTotal, expectedTotal);
          callback();
        });
      }
  };
  ...
}
module.exports = { World: ShopWorld };
```

---

### Steps

```javascript
module.exports = function () {

  this.Given(/^a Cucumber costs \$(\d+)$/, function (price, callback) {
    this.setPrice(parseInt(price), callback);
  });

  this.When(/^I buy (\d+) Cucumbers$/, function (quantity, callback) {
    this.scan(parseInt(quantity), callback);
  });

  this.Then(/^the total should be \$(\d+)$/, function (expectedTotal, callback) {
    this.totalIs(parseInt(expectedTotal), callback);
  });
};
```

---

# The worst effects of TDD

* Initial slow progress
* More tools to learn
* New habits to form

---

# The best effects of TDD

* Working from the outside in, test first
* Write the code you wish you had
* Use examples to clarify understanding of a requirement and your understanding
* Provides living documentation and examples for developers

^ The main side effect of TDD for me is that you write just the right amount of tests and code. No code exists without tests. Tests deepen the understanding of the problem you are trying to solve.

---

# The worst effects of BDD

* Need to get the business involved
* It can add huge overhead if done badly

---

# The best effects of BDD

* Focused on **value**
* Discover examples collaboratively with the business
* Develop and use a ubiquitous language
* Domain driven design (modeling by example)
* Sustainable agility
* Provides living documentation of the domain

^ A reduction in the cost of translating code you set down for a period of time.

^ BDD examples show you how it works and more importantly why it is there and what value it brings.

---

# 👎🏻
## [fit] Tools _>_ Practices _>_ **Principles**

^ The guiding theme I'm trying to convey is regardless of your tool selection or your desire to adopt BDD. You should recognize the default stance we may have as technologists.

^ There is a risk as technologists that we focus way too much on the tools

---

# 👍🏻
## [fit] **Principles** _>_ Practices _>_ Tools

^ Regardless of your context try and redress the balance towards the principles that inspire the practices that the tools facilitate.

---

# Questions?

---

## Credits

* http://everzet.com/post/99045129766/introducing-modelling-by-example
* https://cucumber.io/blog/2014/09/10/when-cucumbers-go-bad
* https://github.com/cucumber-ltd/cukeup-2014-js-workshop
* https://github.com/cucumber/cucumber-js
