l- [x] All onboarding docs
- [x] All BLC docs,
- [x] 2 weeks of PR
- [ ] Teebay Migration
	- [ ] Frontend in progress (NextJS done, next is mantine)

### all docs

- [ ] Sazim Employee Handbook
	- On top of calendar invites
	- Slack/IM conversation
	- Sick leave ASAP, WFH/PTO - 1 day prior
	- 6-7 PM has to be available
	- Take note and share them at the end of meeting (with supervisor)
		- Meeting summary (DATE):
		- Brief bullet points of items discussed
		- Action Items: Brief bullet points of tasks assigned with deadline (to everyone)  
	- Standup - yesterday, today, blocker
	- [ ] Sazim's Learner Program
	-  Ehsan vai and all at Sazim task/result oriented

- [ ] Engineering
	- [x] Project Management tool
		- Get a ticket from TODO and put IN_PROGRESS (to CODE_REVIEW after code is pushed) -> QA after code review -> DONE/RE_OPNENED
		- [ ] delete after DONE?
		Ticket Description
		Epic Label
		Release expectation
		Task Type | Priority Label | Ticket Number | Avatar of asignee
		
		- Epic - Highest level (Business-Engineering bridge)
		- Story - Breakdown of `Epic` (Project manager- Developers)
		- Task - Technical (combined to make a `Story`)
		- Bug - Description, Environment, Reproducible Steps
		- Improvement - Tech/Non-tech, Isolated/Tasks
		- Spike - Vague tasks
			- Documentation/WIP commit (rough example via code) to get outline about tasks to be created
		- Documentation - tasks requiring documentation
		- Meeting - to allocate meeting time
		- Reported Bug - Bugs observed by Q/A or client
		- [ ] Debt (code/design) - accumulated in the process of delivering feature (improve current/address known issues-TODOs)
		
	- [x] Trello Powerups (How trello works)
		- [ ] SubTasks
		- [ ] Card Priority
		- [ ] Story points - weight of story
		- [ ] Epic cards - 
		- [ ] Card numbers - Ticket no for card
	- [ ] Onboarding Actions List
		- [ ] Sazim Engineering Principals
			- Anti comment
			- [ ] commit related changes (squashed before merge)
				- TicketID - Description in present \\n
			- [ ] Hybrid (Ticket-Number_Task = branch name)
				- Trunk-based -> small + frequent update to core/main Trunk
				- [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)-> long lived feature branch (more divergence)
			- [ ] Tags
				- WIP - work in progress
				- NITPICK/SUPER NITPICK - minor concern (up for PR author)
				- FFT - direct author to certain thought
				- OOC - why you did what you did
				- PO - personal opinion
			- [ ] revise page 05-08
		- [ ] [Sazim deployment process](https://docs.google.com/document/d/12oNKBo1qq7_s8eQlntDESVg-Wk-qQMCu0rGtAd4_nE4/edit#heading=h.27wtvdgkbksc) I need to see docker for this one
		- [ ] Structure
			- [ ] [React Project](https://docs.google.com/document/d/1lnBeUjbxHNEZuq9V5wC-IQqCQbWBoR7zIGAJONZl71k/edit#heading=h.3dhhhqo2t1ls) (Revise while using)
			- [ ] [NextJS](https://docs.google.com/document/d/1BVaXGcIUM_FET4XZWtSHLVjaZv1z6fp2HZ3eW1uBKlA/edit#heading=h.6qr2lg801k37) (After NextJS headstart)
			- [ ] [NestJS](https://docs.google.com/document/d/1fBH7IJOy8ugQIxN64gHjv50Mn1cj2ZiqOYm4niP_WQU/edit#heading=h.bus73j3jj898)
				- [ ] Revise Testing + page1, 2
				- [x] Entity classname Singular `User`  (file name plural, `users.entity`)
				- [x] other classnames plural `UsersService`
				- [x] DTO- (action)(entity)DTO
				- [x] class - `PascalCase`, method- `camelCase`, const- `SNAKE_CASE_CAPS`
				- [x] interface- `IPascal`, type - `TPascal`, enum - `EPascal`
	- [x] Laptop Setup
	- [ ] Advanced Git Concepts (notes taken)
	- [ ] Agile strategy (revise later)



#### Pull requests
##### Backend
- [ ] [119](https://github.com/BoredLandlord/blc-backend/pull/119) 
	- related frontend PR referenced [122](https://github.com/BoredLandlord/blc-frontend/pull/122)
- [ ] [117](https://github.com/BoredLandlord/blc-backend/pull/117) - rename DTO file names (to start with clarifying verb)
- [ ] [116](https://github.com/BoredLandlord/blc-backend/pull/116) - frontend [117]()
- [ ] [115](https://github.com/BoredLandlord/blc-backend/pull/115) frontend [116] (revise code)
- [ ] [114](https://github.com/BoredLandlord/blc-backend/pull/114) - active lease means no application
	- [ ] frontend [112](https://github.com/BoredLandlord/blc-frontend/pull/112)
- [ ] [113](https://github.com/BoredLandlord/blc-backend/pull/113) **totally unusual one** (no ticket reference)
- [ ] [112](https://github.com/BoredLandlord/blc-backend/pull/112) all payments leading upto the month is reconciled (revise code)
- [ ] [111](https://github.com/BoredLandlord/blc-backend/pull/111) remove lease documents

##### Frontend
[121]()
 [119](https://github.com/BoredLandlord/blc-backend/pull/119) 
	- related frontend PR referenced [122](https://github.com/BoredLandlord/blc-frontend/pull/122)
- [ ] [117](https://github.com/BoredLandlord/blc-backend/pull/117) - rename DTO file names (to start with clarifying verb)
- [ ] [116](https://github.com/BoredLandlord/blc-backend/pull/116) - issue bujhi nai
- [ ] [109](https://github.com/BoredLandlord/blc-frontend/pull/109) creates a bug