# Problem Statement

  

Code should be as decoupled as possible to make code more 1) maintainable 2) readable 3) testable. The Backend application of any project contains the core of the business logic of all applications and therefore the code should be structured in such a way where there is a smooth flow of logic expression and easily testable all aspects of the code.

# Preliminary knowledge  
The lifecycle of a network call is as follows in the backend:
1. Backend Request (GET, POST etc.)

|                 |
| --------------- |
| Controller      |
| Business Logic  |
| Data query      |
| Serialized data |

  
  

# Backend components

  

Our backend should have decoupled code (literally have different files that address each column/section) so that we easily determine how request/data flows in our backend application.

  

1. Controller: The controller/route is the entrypoint to the application and its responsibilities are as follows:
    

- Authentication of the request
- Authorization of the request
- Whitelisting params coming from request
- Query data and run application business logic to massage it
- Send serialized data back to the client. 
- NOTE: In the serializer section, it is the controller’s responsibility to pick and choose what data to send back using the serializer

All controllers should follow this pattern! example pseudo code for controller:

|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| # Sample Foo table<br><br>id, name, email, phone_number, address<br><br>  <br>  <br><br># Run (a) authentication and (b) authorization logic either in BaseController or in FooController<br><br>  <br><br>class FooController < BaseController  <br>  def post_call:  <br>    result = (d) business_logic_function(params: whitelist_post_params(params_from_request))  <br>    if result.success?<br><br>      (e) FooSerializer(result, [id, name, email]) # Here in the controller section, I have chosen -> id, name, email as opposed to sending everything back to the client<br><br>    else<br><br>      return 4XX error<br><br>    end<br><br>  end   <br><br>  <br><br>  private<br><br>  def whitelist_post_params(params_from_request)<br><br>    # Apply your whitelisting logic (c)<br><br>  end<br><br>end |

  

2. Business Logic: Application based business logic
3. Data query: Sazim follows [repository design pattern](https://lpapa.medium.com/repository-pattern-in-ruby-i-decoupling-activerecord-and-persistence-e395e1b0cf69) to decouple the tool you use and the persistence layer. The idea should be that whatever tool is being used (Prisma in NodeJS, ActiveRecord in Rails) it can be replaced and I can use whatever other tool but the repository should still be working because the repository is focused on how the data is fetched
    
4. Serializer: This is an important factor in the lifecycle of the network call. It plays a role in how fast your data is being sent to the client. So we should be mindful of how the data is being serialized.


- NOTE: The role of a serializer should be to serialize ALL data but it should be the controller’s responsibility to pick and choose what to send back. So using the pseudocode example above, FooSerializer should serializer everything (id, name, email, phone_number, address, address_with_name). What is address_with_name? Lets say my client wants a custom serialized data where the address needs to be sent with name. So in this case we will create whatever function is needed to serialize such data (and this function should exist in the serializer class) and send it back to the client. The client in general should not be responsible in re-massaging the data sent from the backend.
# Testing principles
The backend should have either (ideally both) unit tests or integration tests. 

Unit Tests
For unit tests, each backend component should be unit tested. So there should be heavy mocking involved to each section (controller, business logic, serializer). Only repositories should be an exception where instead of mocking it should set actual data in a test database as a setup step and then test the repository code. 

For example, for FooRepository if you need to make a query to fetch the first 5 foo items from the database by created_at, you need to make sure that in the setup step, you have 10 foo objects stored in the database. So that when you create a test to test your query in FooRepository, you get the expected data.

Integration Tests  
There shouldn’t be any mocking during integration tests. The idea is that you make a network call to one of the REST actions and you expect certain data to be sent back from the controller. So it hits the entire lifecycle of the network call