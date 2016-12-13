# DEV guidelines
This repo is a collection of development guidelines

* [Review](#review)
  + [On client project](#on-client-project)
  + [Generic review](#generic-review)
    - [Run the tests on your env. :)](#run-the-tests-on-your-env-)
    - [Create clear and structured commit header](#create-clear-and-structured-commit-header)
    - [Commit message body](#commit-message-body)
    - [Commit separation](#commit-separation)
    - [Architectural questions.](#architectural-questions)
    - [Tests](#tests)
    - [Clean code](#clean-code)
    - [Business logic.](#business-logic)
* [Version control](#version-control)
* [Development](#development)
* [Y2 Guidelines](#y2-guidelines)
  + [Managing tasks (services)](#managing-tasks-services)
  + [Executing tasks (modules)](#executing-tasks-modules)
* [Testing](#testing)

## Review
### On client project
  - Every commit is reviewed by at least one developer before push to master.
  - Every architectural changes is reviewed by the whole team.
    - To accept or improve that change is the responsibility of the whole team.
    - Needed for everyone (devs) be noticed about that change.
    
### Generic review
#### Run the tests on your env. :)

#### Commit Message Format

Use a similar apporach than the angular contribution guide described. Each commit message consists of a header, a body and a footer. The header has a special format that includes a type, a scope and a subject:

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

#### Create clear and structured commit header
```
// bad
add app

// good
feat(client/app.js): initial add of client app entry point
```
Type must be one of the following:

- feat: A new feature
- fix: A bug fix
- docs: Documentation only changes
- style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- refactor: A code change that neither fixes a bug nor adds a feature
- perf: A code change that improves performance
- test: Adding missing or correcting existing tests
- chore: Changes to the build process or auxiliary tools and libraries such as documentation generation

Scope

The scope could be anything specifying place of the commit change. For example featrues, src/client, services/user, components/root, world.js, etc...

Subject

The subject contains succinct description of the change:

use the imperative, present tense: "change" not "changed" nor "changes"
no dot (.) at the end

#### Commit message body
Create as detailed message body as you can if you have context
- Can the motivation for the change.
- Can be the context of the change.
- It should not be implementation details.

```
// bad
add the DI lib, you can register a module with
registerModule, if the DI do not find the module
then it throw an error

// bad
<no context>

// good
Create a DI system which is able to parse lab specific external systems.
We do not want to implement these business logic free but generic services
in ever new project, therefore we make a standardized DI system.
```

Footer

In case of someone modify the given patch then add sign-off to the footer which can show who is contributed on the given patch. 

```
Signed-off-by: <full name> <email address>
```

#### Commit separation
- Can the commit be separated into smaller commits?
- Do not commit the feature and the usage together, the implementation and the usage has a different context. You can push the tested implementation eariler the the usage, and from that point everyone can use it.
- It helps to the reviewer and later to developers to understand what was you intent with the commits. 

#### Architectural questions.
* [Check the Y2 Guidelines](#y2-guidelines)

#### Tests
* [Check the Y2 testing Guidelines](#testing)

#### Clean code
- classic review step where you should check that everything is clean and understandable.
https://cleancoders.com/

#### Business logic
- Try the new feature manually.
- Validate the business logic if you can. Every developer should understand the business logic, and they can help each other to check everything is ok and it helps to understand the business logic which not developed by you.

## Version control
- Use GIT
  - Work with feature branch
    - Before implement a new feature, think about that a refactor needed or not to implement the new feature.
    - If yes refactor in as such small steps as you can and commit it.
    - If it is an improvement for the whole project then merge it into the master after review.
    - Implement the new feature, if you can add it with only add a new file then you have a good architecture.
    - Commit the new feature without the usage and push it.
    - Implement the usage of the new feature.
    - Commit, review, push.
  - Do not start to commit and develop a POC (proof of concept) in the client project
    - Create an open source repository, and there find the solution for the given problem.
    - It has to be business logic free problem. 
      - Example: Pub/Sub with RabbitMQ
  - Use a git hook which do not allow the force push on master on every client project.

## Development
- Use Y2 approach for projects.
  - Separate the External Managing Tasks (EMT), Internal Managing Tasks (IMT) and Executing Tasks (ET).
    - Motivation:
      - It force you to separate the business logic from generic code, which generate a lot of open source and reusable code.
      - It facilitate to create clean and maintainable code.
      - It helps you to write proper test, which fasten the development.

## Y2 Guidelines
### Managing Tasks (services)
- Internal Managing Tasks can get External Managing Tasks and Internal Managing Tasks only through DI.
```javascript
// bad
const users = require(../../internal/user);
function authService () {
  users.getAll();
}

// bad, can not require the container module in a Managing Tasks
const container = require(../../../container);
function authService () {
  container.get('users').getAll();
}

// good
function authService (users) {
  users.getAll();
}
```

- Internal Managing Tasks can get Executing Task only through module system from its own directory, and the Executing task responsibility should be related to the Internal Managing Tasks.
```javascript
// bad
const tools = require(../../internal/messages/tools);

// good
const tools = require(./tools);
```

- External Managing Tasks can get External Managing Tasks only through DI.
- You cannot pass a Managing Task.
```javascript
// bad
customerService.getCustomers(userService);

// bad
tools.calculateCustomerPrice(customerService);
```

- In Managing Tasks you should not use control structures and modify, mutate state.
```javascript
// bad
  let user = userService.getUser();
  if (user.type === undefined) {
    user.type = 'developer';
  }

// good
const user = userService.getUser();
const initializedUser = tools.initUser(user);
```

### Executing Tasks (modules)
- An Executing task can import modules from ./ or using the module system. If you want to use a public module element from another module then you should promote it to a service.
```javascript
// bad
const tools = require('../../internal/messages/tools');

// good
const _ = require(lodash);
const tools = require('./tools');
```

- An Exnecuting task can not use the DI container.
```javascript
// bad 
const container = require('../../container');
function calculatePrice(id, discount) {
  price = container.get('priceService').getPriceById(id);
  return price * discount;
}

// good
function calculatePrice(price, discount) {
  return price * discount;
}
```

- An Executing task can use control stuctures and store internal state in the function scope.
```javascript
// good
function execute(elements) {
  [head, ...tail] = elements;
  const result = head();
  if (result) {
    return result
  }
  execute(tail);
}
```

## Testing
### EMTs and IMTs should be tested by BDD. 
- Driven by user features.
- Do not test control structures (the BDD step should be driven by user feature)

### ET should be tested by TDD.
- Driven by control structures. 
  - if/else/switch/?
- Do not test declarations, for example do not test function is a function if in the next test you call that function.
```javascript
// bad 
const prices = require('./prices');
it('test', () => {
  assert.isFunction(prices.calculatePrice);
});

it('should calculate the price', () => {
  expect(prices.calculatePrice(2,3)).toBeEqual(6);
});

// good
const prices = require('./prices');

it('should calculate the price', () => {
  expect(prices.calculatePrice(2,3)).toBeEqual(6);
});
```
 
- Try to avoid to test a whole data struct, it is fragile.
```javascript
// bad
it ('test', () => {
  expect(data).toBeEqual({
    a: {b: [1,2,4]},
    price: 3
  })
});

// good
it('test' () => {
  assertPirce(data, 3);
})

function assertPrice(data, price) {
  expect(data.price).toBeEqual(price);
}
```

- Try to be mockless using the Y2 approach.


If something is not good in the guidelines then you have two options. Change the guideline or create a plan about how will you fix the guideline violation.
