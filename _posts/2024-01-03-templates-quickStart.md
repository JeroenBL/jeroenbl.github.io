---
title: "Templates (QuickStart)"
categories:
  - HelloID
tags:
  - powershell
  - template
  - provisioning
---

Learn the basic concepts for developing your own _template based_ [V2 target](https://docs.helloid.com/en/provisioning/target-systems/powershell-v2-target-systems.html) connector for HelloID provisioning. 

## Before you begin

Before you begin developing a connector, make sure to have installed the following:

- [ ] [PowerShell 7](https://github.com/PowerShell/PowerShell)
- [ ] [VSCode](https://code.visualstudio.com/download)
- [ ] [PowerShell extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)
- [ ] [ConnectorGenerator VSCode extension](https://github.com/JeroenBL/ConnectorGenerator)

**Info Notice:**
While __PowerShell 7__ is not a strict requirement, its advised to have it installed locally if your connector will be executed using __HelloID__ cloud PowerShell.
{: .notice--info}

## Table of contents

- [Before you begin](#before-you-begin)
- [Table of contents](#table-of-contents)
- [Your first template based connector](#your-first-template-based-connector)
  - [Executing and testing the `create` lifecycle action](#executing-and-testing-the-create-lifecycle-action)
  - [Debug the `create` lifecycle action](#debug-the-create-lifecycle-action)
  - [Project structure](#project-structure)
- [Wrapping Up](#wrapping-up)
  - [What's next](#whats-next)
    - [Samples](#samples)
    - [Documentation](#documentation)
    - [Forum](#forum)

## Your first template based connector

1. Create a new file. _(Can be of any type)_
2. Right click to open the context menu.
3. Click on `ConnectorGenerator -> Create new HelloID connector project scaffolding`.
4. Select the connector type `HelloWorldExample`.
5. Browse to the location where you want the files to be created and press `enter`

![newHelloWorldExample](https://raw.githubusercontent.com/JeroenBL/jeroenbl.github.io/master/_posts/assets/20240103-templates-quickStart/newHelloWorldExample.gif)

If you open the _VSCode file Explorer_ you will see all the files that have been generated. 

### Executing and testing the `create` lifecycle action

You will probably spend a lot time developing your code. And, you want to make sure it runs properly __before__ moving to __HelloID__. Its advised to first execute and test our code locally.
Every new project includes a _test_ folder that contains everything you need to execute (and test) your code locally. 

Currently, the following files are inlcuded:

| File Name       | Description                                                           |
| --------------- | --------------------------------------------------------------------- |
| config.json     | Configuration settings file                                           |
| demoPerson.json | Sample JSON data for a person                                         |
| debugStart.ps1  | initializes variables like the `$outputContext` and `$actionContext`. |

__To execute and test a lifecycle action:__

1. Open the `./test/debugStart.ps1` script.
2. Press `Run` to execute the code.
3. Open the `create.ps1` script.
4. Press `Run` to execute the code.

You should see the information message below in VSCode.

![informationMessage](https://raw.githubusercontent.com/JeroenBL/jeroenbl.github.io/master/_posts/assets/20240103-templates-quickStart/informationMessage.png)

### Debug the `create` lifecycle action

Debugging is _arguabbly_ one of the most complex aspects in your development process. To make debugging a little easier, we've inluded a `debugStart.ps1` that will initialize [all variables used within HelloID](https://docs.helloid.com/en/provisioning/target-systems/powershell-v2-target-systems/powershell-v2-target-system-variable-reference.html).

**Info Notice:**
Make sure the `debugStart.ps1` is loaded __before__ executing one of the lifecycle actions.
{: .notice--info}

To debug a lifecycle action:

1. Open the `create.ps1` script and browse to line `20`.
2. Change __Hello World__ to __Hello from VSCode__.
3. Set a breakpoint on line `20`.
4. Press `Run` to execute the code.

**Info Notice:**
Press `F10` or click on the `Jump over` button, as seen in the example below, to step through your code.
{: .notice--info}

![debugging](https://raw.githubusercontent.com/JeroenBL/jeroenbl.github.io/master/_posts/assets/20240103-templates-quickStart/debugging.gif)

### Project structure

A connector project comes with a lot of files. Each with its own purpose. 
In the [debugging](#debugging-basics) section, you've already seen some of the debug files available in the `./test` folder.

The table below specifies each file and its use.

| File Name              | Description                                          | Included in Example |
| ---------------------- | ---------------------------------------------------- | ------------------- |
| ./test/config.json     | keep your configuration settings for local debugging | -                   |
| ./test/debugStart.json | debugStart for easy debugging                        | Yes                 |
| ./test/demoPerson.json | pre-filled person skeleton                           | Yes                 |
| .gitignore             | ignore the test folder                               | -                   |
| CHANGELOG.md           | keep track of notable changes to the connector       | Yes                 |
| configuration.json     | connector configuration.json                         | -                   |
| create.ps1             | connector create lifecycle action                    | Yes                 |
| delete.ps1             | connector delete lifecycle action                    | -                   |
| disable.ps1            | connector disable lifecycle action                   | -                   |
| enable.ps1             | connector enable lifecycle action                    | -                   |
| fieldMapping.json      | connector field mapping                              | Yes                 |
| grantPermission.ps1    | connector grant lifecycle action                     | -                   |
| permissions.ps1        | connector permission retrieval action                | -                   |
| README.md              | pre-filled README to keep with the connector         | Yes                 |
| resources.ps1          | connector resource creation action                   | -                   |
| revokePermission.ps1   | connector revoke lifecycle action                    | -                   |
| update.ps1             | connector update lifecycle action                    | -                   |

Please note that not all lifecycle actions may be necessary for your project. You can safely remove any actions that you don't need.

## Wrapping Up

We've covered the basics of creating, executing, and debugging a connector template project locally. However, this is just scratching the surface. There is much more to explore and delve into.

### What's next

Developing a connector requires extensive knowledge of _PowerShell_ and _HelloID_ provisioning. If you don't know where to go from here, there are plenty of resources that can help you.

#### Samples

We have a huge collection of connectors available on the [Tools4ever GitHub](https://github.com/Tools4everBV) repository you can adapt from. 

#### Documentation

Make sure to explore our [documentation](https://docs.helloid.com/en).

#### Forum

Lastly, if you find yourself stuck and require assistance, make sure to visit our [forum](https://forum.helloid.com). Here, you can ask questions and connect with our consultants and developers.