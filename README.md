# DEV guidelines
This repo is a collection of development guidelines

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

## Review
- On client project
  - Every commit is reviewed by at least one developer before push to master.
  - Every architectural changes is reviewed by the whole team.
    - To accept or improve that change is the responsibility of the whole team.
    - Needed for everyone (devs) be noticed about that change.
- How should you review
  - Run the tests in your env. :)
  - Check commit message
  - Does it clear what is the subject of change?
  - Does the the message body contains the right context?
    - It should not be implementation details.
  - Can the commit be separated into smaller commits?
    - Do not commit the feature with the usage, add different context.
    - It helps to the reviewer and later to developers to understand what was you intent with the commits.
  - Architectural questions.
    - Is the code clean?
  - Validate the business logic if you can.
  - Try the feature manually.


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
