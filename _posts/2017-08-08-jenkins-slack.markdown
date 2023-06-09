---
layout: post
title: "Jenkins notifications to Slack"
date: 2017-08-08
---

# Jenkins notifications to Slack

- [Jenkins notifications to Slack](#jenkins-notifications-to-slack)
  - [Background](#background)
    - [Another use case](#another-use-case)
  - [Setting things up](#setting-things-up)
    - [Slack](#slack)
    - [Moving on to Jenkins](#moving-on-to-jenkins)
    - [Test](#test)
  - [Wrapping up](#wrapping-up)

## Background

I have been using Slack for an extended period of time, particularly during Windows Server Updates. Whenever an update is installed, a few PowerShell scripts are executed to verify that the particular server is still functioning correctly. These checks include:

* Ensuring that the server is still online.
* Confirming that the server is still a member of the appropriate Active Directory domain.
* Verifying that the necessary ports are open.
* Ensuring that all autostart services have been started.
* Making sure that the firewall is up and running.
* Examining the event logs for specific events.

If each of these checks is successful, a notification is sent to Slack.

### Another use case

The example above is one way to use Slack. But you can use Slack for many purposes. I'm currently building scripts to automate my company's 'onboard /off board' process. My initial plan was to schedule these scripts on a server with the standard Windows Task Scheduler.

But then there was Jenkins. And I pretty much decided to use Jenkins instead. And for good reason.

* Everybody _able to log in to Jenkins_ can execute my scripts.
* Jenkins has Git integration and thus; Source Control.
* We can include Pester test and execute them from the same platform.

There was however, one tiny flaw in the plan. _**The outcome of a task?**_ There's no way to know unless you log in to Jenkins and find out. Now, I don't want to log in to 'something' and check every task. I'd like to be informed. _**But how?**_

The answer to that turned out to be a simple one.

First; Jenkins can send email messages. Which is nice but still requires me to log in to something.

Second; there actually is a 'Slack Notification Plugin'. And since Slack is on my Phone, I will receive push notifications. What's left is to pick up my phone, wave around with it to activate the screen and voilà!

## Setting things up

Setting up the Jenkins -> Slack notifications is easy. You can be up and running in less then 15 minutes.

### Slack

The first step in the setup process is to configure Slack.

1. Login to Slack
2. Create a new Channel for Jenkins. In the image below I named my channel __#jenkins__

    ![Channel](https://codeinblue.files.wordpress.com/2017/08/s2.png)

3. Open the newly created channel.
4. Click the `channel configuration button` and click `Add an app`.
    ![Addapp](https://codeinblue.files.wordpress.com/2017/08/s3.png)

5. In the search bar type `_jenkins`
6. Select the `Jenkins CI` app and press `enter`.

    ![JenkinsApp](https://codeinblue.files.wordpress.com/2017/08/s4.png)

7. Click the `Install` button.

    ![Install](https://codeinblue.files.wordpress.com/2017/08/s5.png)

8. Select the `#jenkins` channel you've created in step 2 and click `Add Jenkins CI integration`

    ![Config](https://codeinblue.files.wordpress.com/2017/08/s6.png)

9. Next you will see a __Setup Instructions__ screen how to setup Slack integration within Jenkins. Pay extra attention to _**step 3**_ in this instruction guide. It contains the *Team Domain* and *Integration Token*. You'll them later to configure Jenkins.

    ![Jenkinscode](https://codeinblue.files.wordpress.com/2017/08/s7.png)

So far for the Slack configuration.

### Moving on to Jenkins

1. Login to Jenkins.

2. Go to `Manage Jenkins`.

    ![Manage](https://codeinblue.files.wordpress.com/2017/08/s8.png)

3. Click on `Manage Plugins`.

    ![Plugin](https://codeinblue.files.wordpress.com/2017/08/s9.png)

4. Click on the `Available Plugins` tab and filter on `Slack`

    ![Available](https://codeinblue.files.wordpress.com/2017/08/s10.png)

5. Click on `Download now and install after restart` to download the plugin.

6. Click the checkbox next to `Restart Jenkins when installation is complete...`

    ![Download](https://codeinblue.files.wordpress.com/2017/08/s11.png)

7. This will restart the Jenkins server. If Jenkins is set to auto refresh, it will automatically refresh the webpage. If not: `Enable auto refresh` or press `F5`.

    ![Disable](https://codeinblue.files.wordpress.com/2017/08/s12.png)

8. When Jenkins is started; go to _Manage Jenkins -> Configure System_.

    ![EnableSlack](https://codeinblue.files.wordpress.com/2017/08/s13.png)

9. Browse to the _Global Slack Notifier Settings_ section.

    ![Slacknotify](https://codeinblue.files.wordpress.com/2017/08/s14.png)

10. Fill in the settings you've gathered from Step 9 from the __**Slack Set-up**__.

    * Base URL (Only if you have a custom Slack URL like: _chat.mycompany.com_).

    * Team Subdomain (The URL to your Slack team).

    * Integration token (Used to send notifications to Slack)

    * Channel (The channel you created in _step 2_ of the Slack configuration above).

11. Click `Test Connection` to test if the configuration works as aspected.

    ![Test](https://codeinblue.files.wordpress.com/2017/08/s15.png)

12. If you have the Slack Client installed on your Windows / Mac or Linux box and the: __**notification preferences**_ set to _display_ to the desktop client, you will receive a message similair to this:

    ![Message](https://codeinblue.files.wordpress.com/2017/08/s16.png)

    Otherwise, login to Slack (either the Webversion/Mobile or Desktop) and you will see something like:

    ![Msgslack](https://codeinblue.files.wordpress.com/2017/08/s17.png)

Wasn't that difficult! Both Jenkins and Slack are configured and it's time to have some fun!

### Test

1. Login to the Jenkins server.

2. Click the task. (In my example _Create File_.)

    ![Task](https://codeinblue.files.wordpress.com/2017/08/s18.png)

3. Click `Configure`.

    ![ConfigTask](https://codeinblue.files.wordpress.com/2017/08/s28.png)

4. In the _Build_ section you'll find the PowerShell code used for this task.

    ![Code](https://codeinblue.files.wordpress.com/2017/08/s19.png)

5. Go to the `Add post-build action` section.

    ![Postbuild](https://codeinblue.files.wordpress.com/2017/08/s20.png)

6. Click the drop-down menu and select `Slack Notifications`.

    ![Dropdown](https://codeinblue.files.wordpress.com/2017/08/s21.png)

7. Now, I only want to know if a Task is successfull. So I've set the checkbox next to `Notify Success`.

    ![Confignotify](https://codeinblue.files.wordpress.com/2017/08/s22.png)

8. Click `Save` in order to safe the configuration.

9. Go back to the Jenkins homepage.

10. Before I'm going to run this task; let's first proof that the code is actually working from PowerShell

    a. Open a new PowerShell console and copy-paste the code from below.

    ```PowerShell
    Function Create-File {

    Param (
        [String]$Filename,
        $Message
    )

    $ErrorActionPreference = "Stop"
    # Create Temp Directory
    if (-not(Test-Path -Path 'C:\temp\jenkins\'))
    {
        New-Item -Path 'C:\temp\jenkins\' -ItemType directory
        Sleep -seconds 10


    }

    # Using the environment variables exposed by the Jenkins job
    Set-Content -Path "C:\temp\jenkins\$($Filename).txt" -Value $Message
    }
    ```

    b. Run the function as following:

    ![ExecuteCode](https://codeinblue.files.wordpress.com/2017/08/s23.png)

    c. The proof that the code works!

    ![Works](https://codeinblue.files.wordpress.com/2017/08/s24.png)

11. Back to Jenkins! Click the _Schedule a build_ button.

    ![Schedbuild](https://codeinblue.files.wordpress.com/2017/08/s29.png)

12. Fill in the parameters and click _Build_.

    ![Params](https://codeinblue.files.wordpress.com/2017/08/s25.png)

13. As soon as the build starts you will see it on the left side of the screen.

    ![Buildstart](https://codeinblue.files.wordpress.com/2017/08/s26.png)

14. When the build is finished, open the the Slack Client (Web,Phone or Desktop) and go to correct Jenkins channel. There you will see a message similar to this:

    ![Success](https://codeinblue.files.wordpress.com/2017/08/s27.png)

That's pretty much there is to it when it comes to the instruction to setup Jenkins -> Slack integration.

## Wrapping up

There are numerous use cases in which Slack could be beneficial. For example, Pester testing. When all of one's colleagues use Slack for daily communication, the results of Pester tests in the same channel could be a valuable asset. Additionally, Slack has a Nagios plugin, so it can be used as an extension of one's monitoring solution.