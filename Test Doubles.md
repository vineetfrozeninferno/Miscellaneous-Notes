# Test Doubles

## What is it?
A Test Double doesn't have to behave exactly like the real implementation, it merely provides the same API as the real implementation.

[Reference](https://martinfowler.com/bliki/TestDouble.html)

[Source](http://xunitpatterns.com/Test%20Double.html)

## Types

### 1. Dummy Object
- [Source](https://martinfowler.com/bliki/TestDouble.html)
- Passed around but never actually used.
- Usually they are just used to fill parameter lists.


### 2. Stub
- [Source](http://xunitpatterns.com/Test%20Stub.html)
- Returns a value, specified at creation of the Stub, when it is called by the **System Under Test(SUT)**.
- Might not accept an input from SUT
- If there is an input from SUT
	+ the behaviour is not based on the input.
	+ it does **not** verify the input.
- **Example**: A `TimeSource` that returns the provided time when `getTime()` is called.

### 3. Spy
- [Source](http://xunitpatterns.com/Test%20Spy.html)
- Returns a value, specified at creation of the Stub, when it is called by the System Under Test.
- Records the inputs provided to it when called by the SUT.
- These records can then be inspected during the verification phase of the test (**Not During the test**).
-  **Example**: A `LogSpy` that records
	+  the number of times it was called 
	+  the message that was logged
  
  for **later** verification.

### 4. Mock
- [Source](http://xunitpatterns.com/Mock%20Object.html)
- During creation of the mock, it is provided with a mapping of incoming-arguments to responses.
- When the SUT calls the mock, the mock compares the arguments with which it is called to the map.
	+ if a response is present, it returns that response.
	+ if no mapping exists, throws a fatal exception as it does not know how to behave in this scenario.
- It verifies that the SUT passes the correct arguments to the dependency being mocked.
- **Example**: A `MockDatabase` that has a mapping of userName to age.
	+ if the username is correctly created and matches the expected argument provided during construction of the mock, it returns the age.
	+ If the username does not match the expected argument, throws a fatal exception.
  
### 5. Fake
- [Source](http://xunitpatterns.com/Fake%20Object.html)
- Have working implementations, but usually take some shortcut which makes them not suitable for production.
- Fakes are not provided with data during their creation.
- Fakes do not verify any inputs, conditions or behaviours.
- **Example**: A `Fake DB` which has an in-memory map. This DB is used to store and retrieve data during the different operations of the SUT.


## Summary Table
|                                                             | Dummy | Stub | Spy | Mock  | Fake |
|-------------------------------------------------------------|-------|------|-----|-------|------|
| Return value depends on input                               | No    | No   | No  | Yes   | Yes  |
| Return value provided at construction of TestDouble         | No    | Yes  | Yes | Yes   | No   |
| Verification of arguments passed to TestDouble in real time | No    | No   | No  | Yes   | No   |
| Verification of data after SUT has been exercised           | No    | No   | Yes | Maybe | Maybe|
| Verifies a behaviour or other conditions                    | No    | No   | No  | Yes   | No   |
