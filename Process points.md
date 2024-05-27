- [ ] [Commit related](https://gist.github.com/luismts/495d982e8c5b1a0ced4a57cf3d93cf60#commit-related-changes)
	- [ ] Small commit
	- [ ] Commit frequently, complete and tested code (using branches, trunk based flow)
		- [ ] Example branch title : Project CareBears, a ticket # 23 is talking about creating a login endpoint will be`CB-23-login-ep`
	- [ ] Commit message needs to be
		- [ ] Upto 50 character summary
		- [ ] A blank line before detailed body (wrap lines at 72 character)
		- [ ] explain motivation and changes (with imperative, present tense (e.g., "change", not "changed")
	- [ ] PR needs to be one commit (unless second commit does something different)
		- [ ] Take a ticket from `Todo` (and move to `In Progress`)
		- [ ] Create a branch 
		- [ ] Raise a PR  with merging branch(after self reviewing)
		- [ ] mention in Gchat space
		- [ ] Wait for code review
		- [ ] QA the code after it is merged to dev environment.
		- [ ] After dev QA (of the feature) it is considered `Done`(or `Reopened`)
		- [ ] Delete branch

Example Commit message
```
PROJ-1234: [Present tense description of what is done]
[Second line blank]
[Optional additional message to describe what was done. Bullet points are preferred]
```

- [ ] Testing
	- [ ] Frontend (Jest/React Testing Library)
		- [ ] Don't test implementation details (not coverage focused)
		- [ ] Rendering of all components under business logic
		- [ ] Unit (utils and business logic functions)
		- [ ] Integration / End to end (not Jest- Cypress/Puppeteer)
	- [ ] Backend
		- [ ] Isolated test for each layer
		- [ ] Integration testing on 
- [ ] Database Conventions
	- [ ] Table = plural form
	- [ ] Conjunction table = table1_table2
	- [ ] Enum = SNAKE_UPPERCASE
	- [ ] status = Enum (not boolean)
	- [ ]  Delete convention (check later)
	- [ ] Migration (bujhi nai)

		
		
- [ ] NestJs
	- [ ] For Module use CRUD generator
	- [ ] passport authentication 
	- [ ] Custom decorators (to get user, role based not required)
	- [ ] Repository pattern
	- [ ] Layers of separation
		- [ ] Controller
		- [ ] Service
		- [ ] Repository (at module)
			- [ ] extend `EntityRepository` of MikroORM
			- [ ] use module name suffix to resolve potential conflict 
		- [ ] Serializer
			- [ ] extend `AbstractBaseSerializer`
			- [ ] use `serializer` function
			- [ ] for extended functionality, create methods inside custom class
		- [ ] Entities at `common/entities`
			- [ ] Entities extend `CustomBaseEntity` from `src/common/entities/custom-base.entity.ts`
	- [ ] Put Decorator, Pipes inside `src/common`
	- [ ] Variable Names			
		- [ ] Entity classname Singular `User`  (file name plural, `users.entity`)
		- [ ] other classnames plural `UsersService`
		- [ ] DTO- (action)(entity)DTO (`CreateUserDto`)
		- [ ] class - `PascalCase`
		- [ ] method- `camelCase`
		- [ ] const- `SNAKE_CASE_CAPS`
		- [ ] interface- `IPascal`, type - `TPascal`, enum - `EPascal`

- [ ] NextJs 




- [x] how wip PR works? do I add 
- [x] Does github merge directly ship to dev environment?
- [x] We will not be doing integration/e2e tests using Jest and in the frontend. We will have a different project to handle this and we will be more involved in integration/e2e testing.???
- [x] 98% coverage? if we are using merge sort as private method, will we use the method using that or the whole thing?
- [ ] Helper functions: use regular functions to help with hoisting
- [ ] `auth` -> `user`  (decorator at auth)
- [ ] 
```ts
export function helperFunction(param1: string, param2: number,
…rest: any[]) {
	// …
}
```


