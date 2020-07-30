---
title: How to analyze and fix flaky tests
toc: true
note: false
updated: 2020-07-22 18:21
teaser: If you ever had flaky tests in your CI pipeline, you know that identifying and fixing them can be a very challenging task. 
---

Flaky tests are really disgusting. When a test fails randomly, but passes when you rerun the test (with exactly the same artifact), you know that something is going wrong there. It's easy label those tests as "bad" and simply remove them. But what when it shows a real problem?

The higher a test resides in the test pyramid [^1], the flakyier it becomes. This is caused by the wide range of which an integration- or even end-to-end- test covers. They also need way more testing infrastructure to be set up the right way beforehand in order to execute. Challenging is also, that those tests have to be totally isolated from each other, even they need some infrastructural resources. Correct setups and tear downs of those tests is a key factor. On the other hand always setting up so much is time intensive. And time can be crucial when it comes to DevOps and fast time-to market.

I've experienced different kinds of flaky tests and have seen different strategies to handle them. While "fix them later" is the most seen methodology - When they find the way into your backlog, most of us developers analyze them by simple guessing what was the root cause might could be. You can't debug them with exactly the same behavior of the failure, and therefore you can't analyze them. Just look at the code and guess what the problem could be.

## Recipe for flaky tests

Personally I don't like to make assumptions on how something "was maybe going". I like facts. When something fails, I want to see it fail. And in case of flaky tests I don't like to make assumptions because everything can fail, and most of the time, it's the things you never thought they would fail. So over the years I put together a small recipe how to analyze and fix flaky tests. I want to share it with you in this article.

### #1 Ignore test in CI pipeline

Make a pleasure for your teammates and ignore those tests, so they aren't interrupted anymore. But don't just ignore them, **only ignore tests you are investigating**. You need to make it clear to the team that you are trying to fix that test so that nobody else is investigating in the same failure. In <a href="https://www.jetbrains.com/de-de/teamcity/" target="blank">TeamCity</a> you can mark such a test with "investigating" which is really nice.

### #2: Make test fail always on your local machine

You can't fix a green test. If a test fails very rarely make it fail all over again. This is crucial because if that's not the case, how can you say you fixed the test when it was never failing on your local machine?

#### Repeat flaky tests
Repeat your test for 10 / 100 / 1000 times to make them fail constantly.
NUnit has its own <a href="https://docs.nunit.org/articles/nunit/writing-tests/attributes/repeat.html" targer="blank">Repeat Attribute</a> MSTest doesn't. But you can implement something like this in **MSTEST** which acts the same way:
```csharp
// RepeatedTestMethodAttribute.cs
[AttributeUsage(AttributeTargets.Method, AllowMultiple = false)]
public class RepeatedTestMethodAttribute : TestMethodAttribute
{
    public int Count { get; set; } = 5;

    public override TestResult[] Execute(ITestMethod testMethod)
    {
        var testResults = new List<TestResult>();
        for (int i = 0; i < Count; i++)
        {
            testResults.Add(base.Execute(testMethod)[0]);
        }

        return testResults.ToArray();
    }
}

// MyTest.cs
[RepeatedTestMethod(Count = 10)]
public async Task MyFlakyTest()
{
    // ..
}
```
For **xUnit**:
```csharp
// RepeatAttribute.cs
public class RepeatAttribute : DataAttribute
{
    private readonly int _count;

    public RepeatAttribute(int count)
    {
        if (count < 1)
        {
            throw new ArgumentOutOfRangeException(nameof(count), 
                  "Repeat count must be greater than 0.");
        }
        _count = count;
    }

    public override IEnumerable<object[]> GetData(MethodInfo testMethod)
    {
        return Enumerable.Repeat(new object[0], _count);
    }
}

// MyTest.cs
[Theory]
[Repeat(10)]
public void MyTest()
{
    // ..
}
// source: https://stackoverflow.com/a/31878640/3523716
```

#### Reduce overall test scope
Reduce overall test scope (only run flaky test) is useful during the analysis phase of those tests - it saves you so much time. **But you have to be aware of test isolation faults**.
A lot of flaky tests are only failing because a previous test wasn't able to tear down correctly. Make sure your test's setup clear all resources before using them. This means it's OK to clear a database in the tear down phase of a test. But what happens if an exception occurs? This could have an impact of the following test. Therefore, it's important  to setup each test in the way that no previous test can have an impact on that.

An easy way to reduce the test scope is to label those flaky test with either a category or a priority. This enables you to run only those tests which will save you a lot of time during analysis.
<a href="https://docs.microsoft.com/en-us/dotnet/core/testing/selective-unit-tests?pivots=mstest" target="blank">MSTest has a great article on that</a> but that should be possible for all testing frameworks.

```csharp
// MyTest.cs
[RepeatedTestMethod(Count = 10), Priority(2)]
public async Task MyFlakyTest()
{
    // ..
}
```

```bash
$ dotnet test --filter Priority=2
```

#### Run tests same as in your CI pipeline

It's important to run those tests the same way as they are executed in your CI pipeline. Mostly this will be inside a Docker container, so it makes sense to run them the same way on your local machine. **Don't run them in Visual Studio**.

### #3: Adjust time behaviors

Most of the flaky test I experienced were caused by unexpected timing behaviors. This means that some parts of your code took more time than other parts which leads to side effects you've never thought of. Insert 'Task.Delay(..)' or 'Thread.Sleep(..)' here and there, but specially in code who is handling events. Also change timing behavior of your system dependencies (Mocks, Clients, etc.).

### #4 Fix flaky test

If you made your tests always fail it's easy to fix them, you can simply debug your code. Most of the time, by applying the previous steps, you already know where the bug is. A huge advantage is now, that you can be sure you fixed the test when it's green.
Just don't forget to remove everything you put in your code for analysis - your teammates maybe aren't that happy when tests run 1000 times inside your pipeline ;). Sometimes it makes sense to repeat them a few times, for example if you generate test data randomly.

<div class="divider"></div>

## References
[^1]: Ham Vocke, <a href="https://martinfowler.com/articles/practical-test-mid.html" target="_blank">The Practical Test Pyramid</a>