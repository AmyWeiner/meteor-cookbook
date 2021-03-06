## Refactoring Test Driven Development
Refactoring test during test-driven-development is essentially the same as normal refactoring.  We're just moving code around between files and directories.  



#### A)  Initial Steps  
In both cases, we begin with a simple ``meteor create`` command, and then proceed to refactor to server/client architecture.


````js
// meteor create helloWorld
helloWorld/
  helloWorld.html
  helloWorld.css
  helloWorld.js

// refactor to server/client architecture
helloWorld/
  client/
    helloWorld.js
    helloWorld.html
    helloWorld.css
  server/
    methods.js
````


#### B1)  Tests In Same Repository (RDT)
The one major difference between regular coding, and coding for test-driven-development (TDD) or behavior-driven-development (BDD), is that we don't want to have our tests actually be served up in production.  We want to separate out our tests from production code.  So, we need to create a separate directory for our tests.  

````js
// initial repository
helloWorld/
  client/
    helloWorld.js
    helloWorld.html
    helloWorld.css
  server/
    methods.js
  tests/
    helloTest.js
````

#### B2)  Tests In Separate Repository (Safety Harness)
However, we could also separate out our tests by putting them in an entirely different repository, and running them as a separate application altogether.  
````js
// initial repository
helloWorld/
  client/
    helloWorld.js
    helloWorld.html
    helloWorld.css
  server/
    methods.js

// testharness repository
helloTests/
  helloTests.js
````




#### Z1)  Advanced Tests Within the Repository (RTD)
Our decision will affect how we later structure our code.  After a bit of time, we go to advanced topics, like stubbing and dependency injections.  

````js
// initial repository
helloWorld/
  client/
    helloWorld/
      helloWorld.html  
      helloWorld.js
      helloWorld.css
    niftyGizmo/
      niftyGizmo.html
      niftyGizmo.css
      niftyGizmo.js
  packages/
  server/
    methods.js
  tests/
    helloTestjs.js
    niftyGizmoTests.js
    /gizmo
      niftyGizmoStub.js
      niftyGizmoDependencyInjections.js
````

#### Z2)  Advanced Tests In Separate Repository (Safety Harness)  
And we have the choice of having all those stubs and mock objects in our main application, or in a separate application.  
````js
// initial repository
helloWorld/
  client/
    helloWorld/
      helloWorld.html  
      helloWorld.js
      helloWorld.css
    niftyGizmo/
      niftyGizmo.html
      niftyGizmo.css
      niftyGizmo.js
  packages/
  server/
    methods.js

// testharness repository
helloTestjs/
  helloTestjs.js
  niftyGizmoTests.js
  /gizmo
    niftyGizmoStub.js
    niftyGizmoDependencyInjections.js
````


## Leaderboard Example  

#### RDT and Web-Mocha Approach of Having Tests in the Same Repository  
````feature  
Feature: Player score can be increased manually

  As a score keeper in some hyperthetical game
  I want to manually give a player five points
  So that I can publicly display a up-to-date scoreboard

  Scenario: Give 5 points to a player
    Given I authenticate
    And "Grace Hopper" has a score of 10
    When I give "Grace Hopper" 5 points
    Then "Grace Hopper" has a score of 15
````


#### Selenium / Test Harness of External Tests  
````feature  
Feature: Player score can be increased manually

  As a score keeper in some hyperthetical game
  I want to manually give a player five points
  So that I can publicly display a up-to-date scoreboard

  Scenario: Give 5 points to a player
    Given I can connect to page "http://leaderboard.meteor.com"
    And "Grace Hopper" has a score of 10
    When $("#niftyWidgetButton").click()
    foo = $("#niftyWidgetText").val()
    Then foo.should.have.value(20)
````

#### Acceptance Tests Have 3 Essential Key Features

1.  Load a page  
2.  Trigger an event / simulate a user interaction  
3.  Inspect DOM elements  

Which, when translated to JQuery (and a bit of Chai), look something like this:
````js
  $(window).open("http://leaderboard.meteor.com")
  $('#niftyWidgetButton').click()
  $('#niftyWidgetText').val().should.have.value(20)
````  

#### Same-Origin-Policy Messes Everything Up  
The jQuery example is great, but it doesn't run on separate machines.  Which is why technologies like Selenium and PhantomJS have been invented.  So, what's needed is a stepping-stone between the jQuery and Selenium or PhantomJS tests.  Something that can translate the three acceptance tests features from jQuery, which is testing locally, and Selenium/PhantomJS which test remotely.  


#### Testing Locally  
[meteorServer] <===> [(meteorClient)jquery]  

#### Testing Remotely
[meteorServer] <===> ([meteorClient] <===> [phantomJS])selenium  
 
