- Login
- `Properties` @`/properties` > `New Property` 
	- [ ] check all the input fields maybe
	- property notes
- Lease
	- Select Property
	- Add tenant
		- Add roommate
	- Document
		- Lease Agreement*
		- Renter Insurance*
		- Additional Document
- Contract (Added to lease)
	- start date
	- end date
	- load vales from property

- Application (One application for one user)
	- update property details
	- Generate application link




- [ ] Payments module analysis
- [ ] By year filter
- [ ] Fetch aggregated rent from contract
	- [ ] basic rent + parking rent = monthly rent





Questions on [BLC-312](https://trello.com/c/1YZMqi7E/312-be-create-an-endpoint-to-fetch-aggregate-of-paid-payments-and-display-in-the-table-view)
- To show the status/date sent for rent receipts, will you recommend to add a new entity Or adding an optional the date sent field to `BaseContract` will be sufficient as the (not sent) status could be inferred from the absence of value

In the frontend PR There was the [plan/suggestion](https://github.com/BoredLandlord/blc-frontend/pull/140#discussion_r1620903557) on rendering the years (from dropdown) based on the data from backend. So if we do that, we will be sending all the data to backend


- Entity (I was thinking not store the calculated data as we could get it from the `Last sent` field in case something happens to the file store)
		- Last sent (optional field)
		- status (could be inferred from `Last sent` or  enum)
		- previous rent receipt locations
				- (array, I will figure from files)
		- base contract (FK)

- for service function, I am planning to resue `findAllByFilter` from `PaymentsService` class

Edge cases
- what if contract expires in the middle of year and new contract is created with same user with increased rent. (which monthly rent we view in that case)




- [x] Send questions + opinion at group (1-2hr maybe)
- [ ] figure out how fileupload/s3 works
- [ ] figure out different views
- [ ] figure out different forms and fields
- [ ] figure out entity generation
- [ ] figure out frontend flow
- [ ] figure out backend flow
- [ ] RTK
- [ ] mikro







