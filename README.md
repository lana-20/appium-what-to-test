# Deciding What to Test

*Being able to write a test is not the only important thing when it comes to testing. Knowing what should and should not be tested is also very important.*

Understanding our app, breaking down vague testing requirements, and imaginatively exploring edge cases are all part of being a tester. All of this leads to a broad question, namely, how do we decide what to test? And not just *what* but *when*? And on which platforms?

### [The Combinatorial Problem](https://github.com/lana-20/pairwise-testing/blob/main/Pairwise%20Testing.pdf)
- Why not just test everything, on all platforms, as frequently as possible?
- A worked example..

   <img width="500" src="https://user-images.githubusercontent.com/70295997/225185782-d3e56249-80ca-4e13-add3-ae5e25b33d87.png">
- 32,000 tests at 30 seconds each = 16,000 minutes per day. Only 1,440 minutes in a day! Uh oh.
- One way to address the problem is to run multiple tests at once. We can also make judgment calls about what cases, platforms, devices, etc., are not important enough to test every day.

At first, the answers to these questions might seem obvious. Why not just test everything, all of the time? Why not just list out all the possible scenarios to test, code them up one by one, and run them constantly? This would certainly be the ideal. The whole point of testing is to determine the quality of an app, so theoretically, the more passing testcases you have, the higher the quality of the app. And theoretically, you'd like this information every time a developer makes any kind of change proposal to the app code. And theoretically, you'd like this validation to take place on every conceivable device or browser. This would indeed be the ideal.

But now let's do some math and figure out how long this might actually take us. Let's assume a reasonably simple app for the moment, and arbitrarily stipulate that it has 250 test cases. In my experience this isn't a huge number, when you factor in a variety of edge cases on top of standard user behavior. Now let's assume that there are 4 app developers and they each propose 2 changes per day. And now let's assume that we're dealing with a mobile app, and we'd like to run it on just the latest two versions each of iOS and Android, with 2 phone and 2 tablet form factors for each of those versions. All of those totals would then be: 250 test cases, across 8 changes per day, across 16 different devices. Multiplying those numbers together we get a value of 32,000 which is the total number of tests we'd need to execute to satisfy this demand. Wow.

If we further assume that each test takes only 30 seconds, which would be very much on the low side for most Appium tests, but achievable as long as all the shortcuts we'll learn about later are followed, then we can say that to run the entire testsuite in all the environments we need for all the changes we want, would take 16,000 minutes per day. Unfortunately, that works out to over 10 times the number of minutes that are actually in the day.

So, how can we solve this problem? There are a number of ways to work around this basic fact that achieving high coverage takes a lot of time. One of the best ways is to run many tests at a time, rather than only one at a time, and we have a whole unit on this topic down the road. But in most cases, we actually make judgment calls about what to test and what not to test, as well as how frequently to run certain tests. The situation is also made somewhat better when we're starting out building a testsuite, because when we begin, we have 0 testcases, and it takes 0 seconds to run them. So it's up to us how to start. Let's talk about this first.

### The Happy Money Path

<img width="800" src="https://user-images.githubusercontent.com/70295997/225186386-a78857aa-1153-4d6d-91c0-91fc06d39816.png">

What testcases should we write *first* when we're starting at a blank page in terms of our testsuite? Let's think about things according to these two dimensions. One dimension is the scale of how important a particular user scenario is for *you* as the developers of the app. On this dimension, one end of the scale would be the user experience which pays your bills. If you're an e-commerce app, then the user flow you probably care most about is the checkout flow. If your users can't successfully pay you money, your app is entirely pointless to you from a business perspective. The other end of the scale would be things which are less important to your business, like maybe the ability for users to set their favorite color as a background for the home screen. The other dimension is the scale of how commonly a given variant of the scenario is experienced by users. On one end of this scale would be the normal path of a user through a feature, which by and large users experience all the time. On the other end of the scale would be a very obscure edge case that's not likely to be experienced by users at all. We can imagine these two dimensions forming a space, where at the bottom left we have edge cases for unimportant features, and at the top right we have common cases for very important features. My suggestion would be that we start at the top right and work back down.

he user journey that leads to a successful sale, conversion, or action is sometimes called the "money path" of the app, since it is the path the user takes that ends up with your company making money (and of course you can replace money with whatever metric your app is actually seeking to maximize). And for any given feature, the path through that feature with all standard, positive inputs leading to the normal type of conclusion is called the "happy path," since it is the path through that feature that you explicitly design for. For any feature there will be many other paths that could be taken that might result in error messages and so forth, which are entirely valid paths, but not particularly the ones the feature is purpose-built to enable. For example, if you go to check out at an online store, and you enter all correct shipping and billing information, and your items are in stock, and you have an account, etc., then you have gone down the happy path for the checkout feature. But if you enter the same scenario but put in invalid shipping information, you're not on the happy path. This is certainly a scenario that should be tested for, and you should absolutely make sure your app correctly handles the situation where invalid shipping information is entered. But, the most important variant of that scenario to test is the one that you explicitly hope users will find themselves in. That's the happy path.

So if we take these two terms and combine them to define the top right of this 2-dimensional space, we could call it the "happy money path". What I'm suggesting is that as a product team, you should all have some understanding, implicit or otherwise, of how every testcase you care to define ends up on this space. And it is most effective to start with the happy money path and work outwards from there. Why do we focus on this first? Because there is a cost to writing and running tests. It costs your company money for you to automate tests, and it costs your company money for the time and resources for the tests to be run, including all the time that developers and product teams need to wait for the test results. So you want your tests to give as much value as possible from the get-go, and that means finding that intersection of value for your particular app as you start out. Of course, you might find that as you attempt to write your happy money path test that you actually write several other tests along the way. Writing a checkout test might imply that you've already written an account creation test or a login test, because those are pre-requisites. But the *focus* is on that happy money path.

### Diminishing Returns
 - For every unit of additional investment, we get less return.
 - When each additional test case costs more to write than the value it provides, maybe time to stop writing tests.
 - Edge cases might be totally valid, but also not worth testing without a proven instance of user activity.
 - Edge case input can sometimes be combined to reduce the number of testcases.
 - It's possible to get *negative* returns from tests--they can cost more than they deliver! Testsuite design requires careful thought.
 - Appium/Selenium functional tests might not be the best way to validate every kind of case

So what then? Do we just keep writing tests forever until every single edge case is accounted for and we have perfect test coverage? In an ideal world that's what we'd do, but in the real world, we often encounter the hard fact of diminishing returns. The law of diminishing returns states that for every unit of additional investment, we actually get back a little less return. That's actually true for our investment in our testsuite as well, if we've done things the way I suggest, because we're tackling the highest-value areas that need testing first. This is actually a good thing, because it means at some point we will know when it's a good idea to stop writing new testcases. When each new testcase we're writing gives back less value than the time and money it cost to write and/or run that testcase, then it's a sign that maybe our testsuite doesn't *need* to be any bigger.

But how could we ever know that a new testcase isn't going to provide enough value? It's not a straightforward question to answer, because it involves a judgment call about the *costs* as well as the *benefit*, and all of these can be hard to quantify. But let's think about a few examples. Let's say we have a text input box somewhere in our app, and as creative testers we're imagining all the non-standard things users could type into that box. We could imagine that we might need a test for an input string with a space, or one with an accented character, or one that's 50 characters long, or one that's 100 characters long. Now, does the test involving the 100-character string really give us anything more than the test involving the 50-character string? It's theoretically possible the app might behave successfully on one but not another, but it seems like a potential waste of a test. If the app is supposed to handle arbitrary input of arbitrary length, why not just use one string which is extremely long, has a space, has special symbols and an accent, all combined? In this way we lose a bit of specificity if the test fails, because we're not sure exactly which attribute of our mongrel string actually caused the failure, but we're talking about such a low likelihood of a failure that we're probably better off saving some time in our testsuite.

Remember, that because running tests costs your company time and money, that it's actually possible to get *negative* returns from tests. If your test suite is too large, and if developers have to wait too long to get any kind of feedback, then the tests will be pointless anyway, and it would have been better to have fewer tests for a suite that completes in a shorter amount of time. My point here is simply that we need to use our brains as we build out our testsuites, and we need to have a good idea of what testcases will bring the most value. It's also important to remember that there are different kinds of testing. Maybe the use of Selenium or Appium to test 50 different edge cases is not the best idea. Selenium and Appium give incredibly high fidelity testing of user experiences, but they are slow. If what we want is merely to test the logic of the app with numerous kinds of input, then it sounds like a unit test for the internal functionality might be a better place to engage our creativity in thinking of edge cases.

Another important element to consider in all of this is the element of time. We mentioned earlier that as a test suite grows, it can take so long to execute that we run out of time before we need to execute it again, or it can take so long that it doesn't provide adequate feedback. What can we do about this?

Well, the first line of defense is to try to run tests in parallel. Imagine we have a testsuite where we're running each test one at a time, one after the other. Then the picture is something like this. As we add each new test, our testsuite overall will take a little bit longer.

### Testing in Serial

<img width="800" alt="Screenshot 2023-03-14 at 7 21 50 PM" src="https://user-images.githubusercontent.com/70295997/225188222-c5ff2a8a-ef03-4883-a48d-14464f7c9536.png">

But now imagine another picture more like this, where rather than adding a test to the end of our testsuite, we add a new execution thread to our testsuite. Basically by an execution thread I mean the ability to run a test on a single platform or device. So when we talk about adding execution threads, we mean adding the ability to run multiple tests at the same time. Imagine a world where each test in our testsuite had its own execution thread. Its own Appium or Selenium session, its own browser and device. Then they could all run at the same time! The entire test suite would take only as long as the longest test. In practice, however, it's usually difficult to get access to that many servers and devices, and it can also be difficult to manage running that many tests at the same time from the WebDriver client perspective as well. So let's put that option aside for now, though of course we *are* going to want to run in as many parallel execution threads as we can.

### Testing in Parallel

<img width="300" alt="Screenshot 2023-03-14 at 7 22 23 PM" src="https://user-images.githubusercontent.com/70295997/225188300-4ea1166a-2600-4fc7-9e5a-0c559e056a4c.png">

### Breaking up the Suite
- One huge testsuite can often be broken down into different testsuites with different purposes. 
- A common division is that between "smoke tests" and "regression tests".
- Regression = full test suite designed to catch any bugs. Run at least once before release.
- Smoke much smaller suite that runs daily or on every app change.
- Smoke tests have their name from hardware testing-if it doesn't smoke when you turn it on, great!
- The art is picking the right tests for smoke vs regression suites.

Another standard way of solving the problem of really long testsuites is to break the testsuite up into different smaller suites, which get executed at different frequencies. One common distinction is between a set of smoke tests and a set of regression tests. The regression suite would be the full testsuite, and it would be run at some point before an actual release, but maybe not every day. The smoke tests would be a much smaller suite that would run on every commit or every change proposed by the developers. Where do these names comes from? In our industry, a "regression" is another word for what is essentially a bug. More specifically, something that used to work OK, but is now broken for some reason. Regressions can happen for all kinds of reasons. Apps are so complicated nowadays that changes in one area of an app can have consequences on a completely different part of the app that the developer did not think of testing during development. But an automated regression build, which runs all the known test cases, would catch this kind of issue before the app is released.

Smoke tests are so called because apparently back in the day, if a piece of electronic hardware was turned on, and didn't immediately start smoking, it was said to pass the "smoke test," meaning it wasn't immediately ruled out. I like to think of smoke tests also in line with the saying, "where there's smoke, there's fire." If you run the smoke build and get an error, you know something is wrong and needs to be fixed immediately. But of course just because there's not smoke doesn't mean there's not fire, and in the same way just because a build of the application passes the smoke testsuite, it doesn't mean it's guaranteed to be fully working--that's for the regression suite to discover.

But, if you pick the right kind of tests for your smoke suite, you have a good chance to get an early indication of the quality of the app, and it's often enough to stop developers from introducing significant bugs before their code is even merged into the main branch of development. So, to reiterate, one common way of solving the problem of very large testsuites is to break them up and to run them at different intervals. The system of smoke tests plus regression tests is common, but by no means the only system. You could have three or four levels of testsuites, or even non-overlapping suites designed to test certain features, or whatever makes sense for your team and your company. But of course, breaking up the testsuite at all means more complexity in how you manage the running of the smaller suites and how they form part of the overall development pipeline.

### Choosing Test Environments
- Ideally we'd run tests in every possible environment to ensure total coverage. But there are just too many in practice.
- Not every device/environment is equally valuable. We can do the environment ROI calculation when we know usage patterns: locations, devices, etc. This helps us narrow down what's important to test.
- Even outside of tracked usage data, picking a few popular devices in your region is usually sufficient.
- With all of these considerations, there are no hard and fast rules. Make decisions, get some experience, and be quick to evaluate and reconsider based on the success of your test program.

The last area for us to consider is the area of platforms and devices. In a phrase, our test environments. Which environments should we pick to test on?

Again, in an ideal world, we'd pick all the dozens or even hundreds of environments that it's possible for our app to be run on in the wild. If we want a perfect guarantee that all our tests pass on every single browser, platform, or device, then we'd have a serious challenge indeed! The problem in practice is that there are just too many of these, and many combinations are actually hard to come by. Especially when we talk about mobile devices, it's just not financially feasible to find and maintain multiple instances of each mobile device that has been produced, running all the various operating system versions which they support.

But luckily for us, not every device is created equal when it comes to its value in our testsuite. We can think of this problem as another kind of return on investment calculation. Here is where it's essential to know something about our users, and ideally to have lots of usage data. Where are our users located? What devices do they use? What operating systems do they use? Are they early adopters or still on early versions of Android? When we look at our usage data, we usually find that the vast majority of our users fall within a pretty narrow band of devices and operating systems. This can inform which devices or browsers we use for our smoke and regression suites.

Outside of having this data, just picking a few popular devices in the geographic regions your app has traction in is a good way to start, and settling on the last one to two iOS and Android versions is usually sufficient. Then when it comes time to do a release, expanding your device coverage temporarily is useful to make sure you don't have any surprises.

I hope you now have an appreciation for the complexity involved in deciding what to test and what not to test, across a number of different dimensions. I've tried to give helpful explanations and examples, but as always, the correct approach is going to differ by situation, and there is no substitute for the experience of just trying things and seeing how they work, then adjusting as necessary using the various ideas that we've talked about here.
