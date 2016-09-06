#[iTasks](https://github.com/sherakama/itasks)
##### Version: 7.x-1.x

Maintainers: [jbickar](https://github.com/jbickar), [sherakama](https://github.com/sherakama)  
[Changelog.txt](CHANGELOG.txt)

Introduction:
---

The iTasks installation profile architecture was created so that a number of
installation profiles may share common assets without tight coupling. Not only
do the Jumpstart line of installation profiles share a large amount of
functionality and features but they also can live on several environments that
need specific configuration in order to run. By breaking up and abstracting the
individual components of functionality and environment specific configuration we
can selectively choose the parts that are needed for the specific site install.
This architecture also gives us a stronger possibility of portability and
migration from one environment to the next.

Sub Modules:
---

**[iTasks Update](https://github.com/sherakama/itasks)**
This helper module is a requirement of this installation profile. When creating
a new profile you should not have to re-name this sub-module. This module
provides the functionality for being able to run update tasks via the
`drush ipdb` command.

Dependencies:
---

This profile does not have all of it's code bundled inside of it. You will have
to either use a drush.make file to keep track of the contrib and custom modules
used with this installation profile or download them manually.

**Tasks Repo**  
The individual installation tasks can be found in a remote repository. For
example: [https://github.com/sherakama/itasks_tasks](https://github.com/sherakama/itasks_tasks)


Installation:
---

You may install this profile through the UI. Simply select this profile from the
list of profile options when installing through install.php

When using drush you can install using the `drush si` command. Optional
parameters can be passed in to the si command to alter the installation.

**install_configure_form.itasks_extra_tasks**  
In the .info file of this installation profile you can define extra groups of
tasks to run after the main set has through task groupings. You can create a new
grouping by adding a new index to the `task` array in the .info file.

eg: task[new_group][install][] = Some\Task\To\Install

In the example above the `new_group` extra tasks would be available to the
drush si command.

eg: `drush si itasks_profile install_configure_form.itasks_extra_tasks=new_group`

Extra tasks always get installed after the main set of install tasks unless the
task specifically alters itself to another place in the order through the
`installTaskAlter()` method available to it.

Getting Started
---

* Rename the .info, .install, and .profile files to the new profile name.
* Rename the hook/functions inside of the .install and .profile files
* Add and remove installation tasks to the .info file
* Add and remove dependencies to the .info file
* Update the name, description, and version inside of the .info file
* Update the taskdir option in the .info file to point to the location of the install tasks directory (see tasks repo above)

## itasks_profile.info

**name**  
The profile name.

**description**  
A short description of this installation profile.

**version**  
The version of the installation profile. Use Drupal naming conventions.

**taskdir**  
The path to the itasks_tasks directory (not this profile). This is relative to the Drupal root so always start with sites/.

**task[install][]**  
The main list of installation tasks. These run in the order they appear unless they have a alter function in them to re-arrange their order. The value of each task declaration must be the full namespace of the install task found in `taskdir`. Do not include .php at the end of the declaration.

**task[new_group][install][]**  
An optional group of install tasks that can be installed on top of or after the main list of installation tasks. This group works with the `drush si` command and the `install_configure_form.itasks_extra_tasks` parameter. The value of this parameter must be the full namespace of the installation task. Do not include .php at the end of the declaration.

**task[update][7100]**  
An optional way to declare update hooks. In the example above 7100 is the update hook as if you were writing hook_update_N. The value of this declaration should be the full namespace to the update task.

**dependencies[]**  
The list of dependencies for this installation profile. Installation tasks can define dependencies as well so although it is not necessary to define all of the dependencies here it is recommended that you do so. In practice it is much harder to find all of the dependencies if they are scattered amongst multiple install task files.  

## itasks_profile.install

This is a regular old .install file and you can find out more information about the hooks available here on Drupal.org: https://www.drupal.org/node/876250

As this profile is meant to abstract most of what you can do in this file it should remain relatively un-used. Please do not add hook_install, hook_update_N or hook_enable to this file.

## itasks_profile.profile

This file contains a bunch of necessary boilerplate code in order to function. Many hooks are used and wrap itasks custom implementations of them. Please do not remove or alter any of the code that has been provided within each function.

Troubleshooting:
---

Tasks that are added to the .info file are autoloaded and should be named and
placed in the appropriate namespace. Please check for typos and case.

Installation tasks can declare dependencies. Look at the ones that are being installed for the any items that are being enabled that you don't want.


Contribution / Collaboration:
---

You are welcome to contribute functionality, bug fixes, or documentation to this module. If you would like to suggest a fix or new functionality you may add a new issue to the GitHub issue queue or you may fork this repository and submit a pull request. For more help please see [GitHub's article on fork, branch, and pull requests](https://help.github.com/articles/using-pull-requests)
