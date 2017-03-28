---
title: Development Guide
layout: article
author: Jan Decaluwe
---

Background info
===============

The [dev.myhdl.org] website documents all aspects of the development
process of the MyHDL project. 

MyHDL's original developer, [jandecaluwe-mail], is MyHDL's BDFL. He has the
final say about its design and development decisions.  The development workflow
takes that into account.

*[HDL]: Hardware Description Language 
*[BDFL]: Benevolent Dictator For Life

How to help with the MyHDL project
==================================

Before considering to contribute to development, note that there are other ways
to help.

In particular, the best way to help is to use MyHDL in design projects *and let
people know about it*. If you want, you can use 
[the users section on the MyHDL site][myhdl-users].

Another great help is to report issues in the [issue-tracker].  Add a small
test that fails and that you think should work.  That is a great starting point
of issue fixers.

If you like to contribute to development, please consider not to start with
your own pet feature. The following contributions get priority, in this order:

* code changes that increase robustness, clarity and maintainability 
* issue fixes 
* features labeled `help wanted` in the [issue-tracker]   

If you would like to work on a new feature, do things in the right order.
Start with communication and specification work, not by raising pull requests
against the main repository. MyHDL development is conservative when it comes to
new features, so you are likely to be disappointed otherwise.

The repository
==============

MyHDL code development is managed on [github] using [git].

The public repository resides [here][myhdl_repo].  GitHub offers an excellent
interface to the repository and its history. 

The repository can be used for several purposes, such as browsing through the
development history, and tracking development. In addition, you can use it to
contribute your own changes.  

The git version control system
==============================

This guide assumes that you have basic working knowledge of git.
To get started with git, I recommend to read the initial
chapters of the [git-book].

Tracking development
====================

If you have git installed, you can use the repository to track development.
The first step is to clone the repository according to the GitHub instructions:

```
$ git clone https://github.com/jandecaluwe/myhdl.git
Cloning into 'myhdl'...
```

The repository contains multiple branches. You can see them as follows:

```
$ cd myhdl
$ git branch -a
* master
  remotes/origin/0.9-maintenance
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```   

You can see that there is a `master` branch that is checked out by
default, and a `0.9-maintenance` branch. The meaning of the
branches is as follows: 

Branch name       |  Purpose          | Policy                                                       
------------------|-------------------|---------------------------
 master           |  Development      | New features 
 0.9-maintenance  |  Official release | 0.9.* release maintenance 

By default, you are on the `master` branch. To switch to the maintenance
branch, for example for a bug fix in the official release, use:

```
$ git checkout 0.9-maintenance
```

On any given branch, you can stay current with upstream changes
by pulling:

```
$ git pull 
```

To use the local repository code the simplest way is to adjust the
`$PYTHONPATH` environment variable. Make sure that the path to your local
repository is the first entry in the `PYTHONPATH` variable. In this way, that
version of the `myhdl` package will be found first.

Contributing changes 
====================

Introduction
------------

You can also contribute your own changes. 

The workflow is such changes are reviewed by other developers and applied or
rejected by the BDFL. Therefore, your goal as a contributor should be to
minimize the work of other developers. What is needed is the following:

* an understanding why the changes are needed and useful
* a test that verifies the changes
* using [git] to manage changes
* using [github] to fork the main repo and create *pull requests*.

The implementation of these requirements is described in more detail in the
following sections.

First step: communicate 
-----------------------

Except for obvious bug fixes, the first step is to communicate about the
problem or feature with the MyHDL community.  Start a discussion in the
[mailing-list] or add an entry in the [issue-tracker].

Depending on the interest, the result will be a fruitful discussion that may
result in a specification for the work to be done.

Creating changes
----------------

To manage your changes, use git. By doing so, the other developers can simply
use their normal development workflow to incorporate your work.  Moreover, your
name will be preserved as the author, so you get the credit you deserve.

In git terminology, a set of changes that belong together is called a *commit*.
To create a commit with all the changes, use: 

```
git commit -a
```

Without the `-a` option, only selected changes would go into the commit.
Such sophistication is typical of git and often useful. Consult the
[documentation][git-book] for more info.

Quality
-------

At a certain point you will want to publish your changes, so that everyone can
benefit from them. However, don't publish your work until it is complete.  Make
sure there are no loose ends that others will have to address after you.  Most
importantly, make sure your changes have been tested.

Run the MyHDL regression tests to make sure nothing breaks.  As a minimum, the
tests in `test/core` should be run. If your changes are related to conversion
to VHDL/Verilog, the test in the `test/conversion` directories should be run
also.

Presumably, you have some test for your changes in place.  You should turn it
into a test that can be added to the regression test suite.

You may need a number of commits to complete your work. That isn't a problem at
all, on the contrary. Finish the job before publishing. 

Writing tests
-------------

For anything but trivial fixes, unit test are mandatory.

A test for a fix or feature should fail before the fix or implementation, and
pass after it. Therefore, test-driven development, where the test is written
first, is the recommended development strategy.

The first reason for a test is obviously to verify the fix or implementation
itself. However, the most important reason is to make the code base robust
against future modifications. Consequently, changesets without good tests will
not be accepted.

For MyHDL development, the test should be written in [pytest][1] style.

[1]: http://pytest.org

Publishing changes
-------------------

When you are ready with your changes, use  [github] to publish them.
Publishing changes is done by creating *pull requests*. The process is
documented [here](https://help.github.com/articles/using-pull-requests/).

Other developers will review the pull request, and the BDFL will decide to
merge it in or not.

