# Lab 3 Complete a Web API -- Project Report

## Description of Changes

In this lab, I completed the tests in the `ControllerTests` class to verify the behavior of the HTTP methods in the `EmployeeController` according to their **safety** and **idempotency** properties.

I implemented the **SETUP** and **VERIFY** blocks using **Mockk** to simulate the behavior of the `EmployeeRepository`.  
Each test validates the theoretical properties of the corresponding HTTP methods:

- **POST /employees**  
  - Not safe (modifies the server state).  
  - Not idempotent (two identical requests create different resources).  
  - Mock used: `save()` returns employees with different IDs (1 and 2).  
  - Verification: `employeeRepository.save()` is called exactly 2 times.

- **GET /employees/{id}**  
  - Safe (does not modify the state).  
  - Idempotent (repeated calls return the same result).  
  - Mock used: `findById(1)` returns an employee, `findById(2)` returns `Optional.empty()`.  
  - Verification: no modification methods (`save`, `deleteById`, `findAll`) are called.

- **PUT /employees/{id}**  
  - Not safe (creates or updates a resource).  
  - Idempotent (repeating the same request does not change the result).  
  - Mock used: `findById(1)` first returns empty, then an existing employee.  
  - Verification: `save()` and `findById()` are each called exactly 2 times.

- **DELETE /employees/{id}**  
  - Not safe (removes a resource).  
  - Idempotent (deleting multiple times does not change the result).  
  - Mock used: `justRun { deleteById(1L) }`.  
  - Verification: `deleteById(1L)` is called 2 times and no other methods are invoked.

Finally, all tests pass successfully when running `./gradlew test`.

---

## Technical Decisions

- **Frameworks and Libraries Used:**
  - *Spring Boot 3.5.3* for the web environment.
  - *Kotlin 2.2.10* as the programming language.
  - *Mockk* and *SpringMockk* for mocking repository dependencies in tests.
  - *MockMvc* for performing simulated HTTP requests.
- **Testing Approach:**  
  A *behavior-driven* approach was followed to verify expected behavior based on HTTP semantics.
- **Test Format:**  
  Kotlin block-style syntax (`andExpect`, `verify`) was used to make the tests cleaner and consistent with course conventions.

---

## Learning Outcomes

From this lab, I learned:

- To clearly distinguish between **safe** and **idempotent** HTTP methods.  
- How to use **Mockk** to simulate repository behavior in unit tests.  
- How to use **MockMvc** to test REST controllers without deploying the server.  
- How to verify REST API theoretical properties through unit tests.  
- The importance of maintaining code style consistency with **Ktlint**.

---

## AI Disclosure
### AI Tools Used

- None

### AI-Assisted Work

- The implementation of the test code was done manually, following the lab instructions in the provided PDF.  
- Approximate AI assistance: **0%** .

### Original Work

- The tests and mock configurations were implemented by me following the lab specifications.  
- I understood and applied the *safe* and *idempotent* concepts when designing the tests.  
- All execution, debugging, and verification were performed manually in my local environment.
