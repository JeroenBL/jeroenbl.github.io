---
layout: post
title: "Execute Pester tests from Jenkins"
date: 2017-08-09
---

# Execute Pester tests from Jenkins

- [Execute Pester tests from Jenkins](#execute-pester-tests-from-jenkins)
  - [Background](#background)
    - [Pester / Test Driven Deployment](#pester--test-driven-deployment)
    - [Jenkins](#jenkins)
    - [Write the code](#write-the-code)
    - [Testing](#testing)
    - [Create the function](#create-the-function)
    - [Jenkins](#jenkins-1)
      - [But how to achieve this:](#but-how-to-achieve-this)
    - [Passed](#passed)
    - [But we're not there yet!](#but-were-not-there-yet)
  - [Wrapping up](#wrapping-up)

## Background

Every time I write a script, I use the same approach, which has some similarities with Agile. Specifically, I write small blocks of code, test them, and when everything works as expected, I incorporate the code into the script I am developing. In Agile terms, this is referred to as a sprint. I repeat these steps until my script is complete. This method works well for me, as it limits the possibility of including unnecessary code. Furthermore, if a block of code does not work or needs to be adjusted, I can easily replace it.

### Pester / Test Driven Deployment

Pester is a different approach to scripting. Initially, you will need to:

* Construct a test that outlines the desired script.
* Execute the test (which will obviously fail).
* Develop the script.
* Re-run the test (which should result in success).

Now, I am in the process of learning Pester, so I will not delve too deeply into the subject as there is still much to be learned. Instead, I will focus on Jenkins.

### Jenkins

Jenkins is a true automation platform. Following the concepts of Continuous Integration / Testing and Deployment. As such, it is __**the**__ place to store and execute Pester tests.

### Write the code

For this example I've created a very basic Pester test. A simple; _Describe_ that aspects the following output: _This is a very simple pester test!_.

```PowerShell
Describe "Get-PesterTest" {
    It "outputs 'This is a very simple pester test!'" {
        Get-PesterTest | Should Be 'This is a very simple pester test!'
    }
}
````

I saved this script as `Get-PesterTest.Tests.ps1` in `C:\Temp\Pester\2`.

### Testing

To invoke pester:

1. Open up a new PowerShell console, browse to the folder where the pester test script is located and type `Invoke-Pester`.

This will invoke all pester tests in that particulair directory. There's only one in my case so it just executes the: `Get-PesterTest.Tests.ps1`.

So, does it work!?

![Fail](https://codeinblue.files.wordpress.com/2017/08/p2.png)

As you see the test generated a failure. Obviously, since I didn't wrote the implementation of the `Get-PesterTest` function.

### Create the function

Now I know that my first test failed. (Which is good!) I can move on to actually develop the function. That function should output: _This is a very simple pester test!_.

```PowerShell
function Get-PesterTest {
    "This is a very simple pester test!"
}
```

Let's start a new PowerShell console and invoke the pester script again using the `Invoke-Pester` cmdlet.

![Success](https://codeinblue.files.wordpress.com/2017/08/p3.png)

Seems good!

### Jenkins

This is where things are getting serious. Jenkins is a true automation platform and embraces the concepts of Continuous Testing/Integration and Deploying. So it makes sense storing and running Pester tests from the Jenkins platform. You can execute them from a single place and see the results on the Jenkins server webpage. And; because I recently added the Jenkins-Slack integration, I will receive a notification on my phone.

#### But how to achieve this:

1. Login to Jenkins.

2. Click `New Item`.

    ![New-Item](https://codeinblue.files.wordpress.com/2017/08/p4.png)

3. Enter the name for the new item. In my case: _Pester_Get-PesterTest_. Make sure you select: `Freestyle project`.

    ![Name](https://codeinblue.files.wordpress.com/2017/08/p5.png)

4. Browse to the _**Build**_ section, click `Add build step` and choose: `Windows PowerShell`.

    ![Build Actions](https://codeinblue.files.wordpress.com/2017/08/p6.png)

5. In the command window enter:

    ```PowerShell
    Invoke-Pester "PathToPesterFolder"
    ```

    ![Command](https://codeinblue.files.wordpress.com/2017/08/p7.png)

6. Click `Save` in order the save the project and go back to the main screen.

7. On the main screen, click `Run` to execute the task.

    ![ff](https://codeinblue.files.wordpress.com/2017/08/p8.png)

8. As soon as the task starts running, it will appear on the left side of the screen.

    ![Running](https://codeinblue.files.wordpress.com/2017/08/p9.png)

9. When the task is complete, click the buildnumber and select `Console Output`.

    ![Completetask](https://codeinblue.files.wordpress.com/2017/08/p10.png)

10. In the _Console Output_ window you will see the same output as on PowerShell Console.

    ![ConsoleOutput](https://codeinblue.files.wordpress.com/2017/08/p11.png)

### Passed

So far for a _Passed_ test. But what if a test fails? Well, let's see what happens!

To demonstrate this I've made some adjustments to my: _Get-PesterTest_ module function.

```PowerShell
function Get-PesterTest {
    "This will make a test fail!"
}
```

Remember the output of the test has to be: _This is a very simple pester test!_. So to make the test fail, simply change the output to: _This will make a test fail!_.

11. Go back to Jenkins and run the task again.

12. Open the `Console Output` to see the results.

    ![Output2](https://codeinblue.files.wordpress.com/2017/08/p12.png)

    Now, the test indicates a fail.

### But we're not there yet!

Notice that the failed test also shows a _Success_. That's not good. A failed test should come up as a _Fail_. To accomplish that; we need to make a minor adjustment to the PowerShell command.

13. Open the task, click `Configure` and browse to the `Build_ section`.

14. In the `Command` textbox, add the folowing line `-EnableExit`.

    ![Adjust](https://codeinblue.files.wordpress.com/2017/08/p13.png)

    > The `EnabaleExit` switch is used to __fail__ a build whenever a test has failed.

15. Save the task and run it again. This time when we open the `Console Output` a real __failure__ will be displayed.

    ![RunAgain](https://codeinblue.files.wordpress.com/2017/08/p14.png)

## Wrapping up

This is just a very easy example of how to incorporate Pester on the Jenkins platform. But it gives a little insight into the possibilities of Jenkins and Pester.