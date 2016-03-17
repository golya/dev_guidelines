# dev_guidelines
This repo is a collection of development guidelines

## Guidelines
### Services
  - A serive can not import another module or service, have to use DI container.
  - A service can import pure modules possibly from ./
  - In services you should not use native data struct or control structures.
  - A service should not store internal state.

### Modules
  - A module can import pure modules possibly from ./  
  - If you want to use a public module element from another module then you should promote it to a service. 	  
  - A module can not use the DI container.
  - A module can store state.

If something is not good in the guidelines then you have two options. Change the guideline or create a plan about how will you fix the guideline violation.
	
