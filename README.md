# Deciding What to Test

*Being able to write a test is not the only important thing when it comes to testing. Knowing what should and should not be tested is also very important.*

### The Combinatorial Problem
- Why not just test everything, on all platforms, as frequently as possible?
- A worked example..

   <img width="500" src="https://user-images.githubusercontent.com/70295997/225185782-d3e56249-80ca-4e13-add3-ae5e25b33d87.png">
- 32,000 tests at 30 seconds each = 16,000 minutes per day. Only 1,440 minutes in a day! Uh oh.
- One way to address the problem is to run multiple tests at once. We can also make judgment calls about what cases, platforms, devices, etc., are not important enough to test every day.

### The Happy Money Path

<img width="800" src="https://user-images.githubusercontent.com/70295997/225186386-a78857aa-1153-4d6d-91c0-91fc06d39816.png">

### Diminishing Returns
 - For every unit of additional investment, we get less return.
 - When each additional test case costs more to write than the value it provides, maybe time to stop writing tests.
 - Edge cases might be totally valid, but also not worth testing without a proven instance of user activity.
 - Edge case input can sometimes be combined to reduce the number of testcases.
 - It's possible to get *negative* returns from tests--they can cost more than they deliver! Testsuite design requires careful thought.
 - Appium/Selenium functional tests might not be the best way to validate every kind of case

### Testing in Serial

<img width="800" alt="Screenshot 2023-03-14 at 7 21 50 PM" src="https://user-images.githubusercontent.com/70295997/225188222-c5ff2a8a-ef03-4883-a48d-14464f7c9536.png">

### Testing in Parallel

<img width="300" alt="Screenshot 2023-03-14 at 7 22 23 PM" src="https://user-images.githubusercontent.com/70295997/225188300-4ea1166a-2600-4fc7-9e5a-0c559e056a4c.png">

### Breaking up the Suite
- One huge testsuite can often be broken down into different testsuites with different purposes. 
- A common division is that between "smoke tests" and "regression tests".
- Regression = full test suite designed to catch any bugs. Run at least once before release.
- Smoke much smaller suite that runs daily or on every app change.
- Smoke tests have their name from hardware testing-if it doesn't smoke when you turn it on, great!
- The art is picking the right tests for smoke vs regression suites.

### Choosing Test Environments
- Ideally we'd run tests in every possible environment to ensure total coverage. But there are just too many in practice.
- Not every device/environment is equally valuable. We can do the environment ROI calculation when we know usage patterns: locations, devices, etc. This helps us narrow down what's important to test.
- Even outside of tracked usage data, picking a few popular devices in your region is usually sufficient.
- With all of these considerations, there are no hard and fast rules. Make decisions, get some experience, and be quick to evaluate and reconsider based on the success of your test program.
