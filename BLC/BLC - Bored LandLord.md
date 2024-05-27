##### Documents Used
- Bored landlord technical proposal
	- [x] Notion links not working 
- TBL Spec Doc
- [x] BLC code deployment process 
		- Moved to Sazim Doc
- [x] BLC Phase 2 Tshirt Sizing (basically a rough estimate)
- [ ] Data Requirement Question 01-
	- [ ]  BLC page Data Requirements (did that move to ERD?)
-  API Use Case
- Meeting Notes






##### Goals
- Full stack, MVP 
- For property owners + team

#### Architecture
- Frontend (NextJS - 13)
	- Maintine (v6)
	- React Redux (v8)
	- Zod (v3)
	- Axios (v1)
	- DayJS (v1)
- Application (NestJS - 9)
	- MikroORM (v5.9)
	- Class validator (0.14)
	- AWS SDK (3.485)
	- Passport JWT (4)
- Data (PostgreSQL) + Storage (Amazon S3)


- [x] Role based Authentication
- [x] Applications won't be the most accurate source of truth? (From ERD)
- [x] ERD Note: Every account gets 2 weeks trial,  after that they are either pro or none
- [ ] ERD- why role is in `User_Profiles` and not at `Users`
- [x] ERD- is user field only for account management? (`LandLord_Tenant` -> `User_profile`)
- [ ] ERD- For later, Many to many between `User_Profile` and `Base_Property`
	- [ ] Does that refer to shared ownership?
- [x] ERD- `Property Expense Records` has a note to add an index for performance? Does that apply to all the features or it is one because it is queried often?
- [x] ERD - not most accurate source of truth
- [x] ERD - `Applicant` `Source` (is it where it was shared source?)
- [x] ERD- `Lease` > `Base Property` optional?
- [x] `API use case` - Realtor Role
- [x] `Meeting notes` -  Notion of Archiving Applications
- [x] weekend messages
- [ ] today standup
- [x] Cypress


- [ ] calling dibs on ticket (Project Management tool rundown)
- [ ] How QA works? (Project Management tool rundown)
- [ ] Is there an issue if I fork the repo and work on branch there?

- [x] Idk anything about AWS
- [x] Is there a Slack?


- [ ] Does the archive feature exists (from `Application Management`)?
- [x] `People management` incomplete?
- [x] Does super user have other abilities
- [ ] what is inactive tenant and how to activate
- [ ] is the failed commit feature active


