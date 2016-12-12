# DEV guidelines
This repo is a collection of development guidelines

* [Review](#review)
  + [On client project](#on-client-project)
  + [Generic review](#generic-review)
    - [Run the tests on your env. :)](#run-the-tests-on-your-env-)
    - [Create clear and structured commit header](#create-clear-and-structured-commit-header)
    - [Does the the message body contains the right context?](#does-the-the-message-body-contains-the-right-context)
    - [Can the commit be separated into smaller commits?](#can-the-commit-be-separated-into-smaller-commits)
    - [Architectural questions.](#architectural-questions)
    - [Is the code clean?](#is-the-code-clean)
    - [Validate the business logic if you can.](#validate-the-business-logic-if-you-can)
    - [Try the feature manually.](#try-the-feature-manually)
* [Development](#development)
* [Testing](#testing)
* [Version control](#version-control)
* [Y2 Guidelines](#y2-guidelines)
  + [Services](#services)
  + [Modules](#modules)

## Review
### On client project
  - Every commit is reviewed by at least one developer before push to master.
  - Every architectural changes is reviewed by the whole team.
    - To accept or improve that change is the responsibility of the whole team.
    - Needed for everyone (devs) be noticed about that change.
    
### Generic review
#### Run the tests on your env. :)

#### Commit Message Format

Use a similar apporach that angular contribution guide described. Each commit message consists of a header, a body and a footer. The header has a special format that includes a type, a scope and a subject:

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

#### Does the the message body contains the right context?
#### Can the commit be separated into smaller commits?
#### Architectural questions.
#### Is the code clean?
#### Validate the business logic if you can.
#### Try the feature manually.

## Development
- Use Y2 approach for projects.
  - Separate the External Managing Tasks (EMT), Internal Managing Tasks (IMT) and Executing Tasks (ET).
    - Motivation:
      - It force you to separate the business logic from generic code, which generate a lot of open source and reusable code.
      - It facilitate to create clean and maintainable code.
      - It helps you to write proper test, which fasten the development.

## Testing
- EMTs and IMTs should be tested by BDD. 
  - Driven by user features.
  - Do not test control structures (the BDD step should be driven by user feature)
- ET should be tested by TDD.
  - Driven by control structures. 
    - if/else/switch/?
  - Do not test declarations.
  - Do not test function is a function if in the next test you call that function.
  - Do not test object is an object if in the next line you check a property.
  - Try to avoid to test a whole data struct like it is fragile.
  - Try to be mockless.

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

## Y2 Guidelines
### Services
  - A service can not import another service, have to use DI container.
  - Can not require the container module in a service.
  - A service can import pure modules possibly from ./
  - In services you should not use native data struct or control structures.
  - A service should not store internal state.

### Modules
  - A module can import pure modules possibly from ./  
  - If you want to use a public module element from another module then you should promote it to a service. 	  
  - A module can not use the DI container.
  - A module can store state.

If something is not good in the guidelines then you have two options. Change the guideline or create a plan about how will you fix the guideline violation.
