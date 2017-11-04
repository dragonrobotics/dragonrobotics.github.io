# Dragon Robotics Standards and Procedures

## Preface
This document is intended to be a living document and should be updated as new
cases and situtations come to light.
Also, the first, and foremost rule surrounding this coding style document is:

**The rules are flexible, but don't be stupid.**

We can't think through every single possible scenario, and for every rule we
come up with a hundred exceptions will inevitably arise. On the other hand,
however, each of these rules has a purpose.
If you need to break a rule, you'd better have a really good reason for it.

## Coding Style
There are a couple of basic rules that we believe should be followed by everyone,
with regards to code format and styling:

* **Keep to 80-character lines.** Aside from simply looking neater, 80-column
lines are easier to work with in terminals, when doing side-by-side comparisons
and diffs, when working with multiple windows or panes, and so on.
* **Tabs are 4 spaces wide.** Most of us are already used to 4-character indents,
and 4-wide indents strike a good balance between being visible and being
unobtrusive.
* **Use soft tabs (spaces) instead of hard tabs.** Mixing tabs and spaces for
indentation can only lead to code with screwed-up indentation.
* **Keep everything as contained as possible.** Really, this is just general
programming advice, but it's important enough to bear repeating. Keep all member
variables private, unless you have a _really_ good reason to make them public.
* **Keep to the [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter):
don't 'reach through' objects.**
In short: if you have code that looks like `objectA.objectB.objectC.method()`,
you are probably doing something wrong, and are strongly advised to look over
the rest of your code for bad design.
  * Certain methods and objects, such as pipelines, are _specifically designed_
to be chained. They're exceptions to this rule, though you should keep the
80-character line length limit in mind when using them.
* **All files should have a header comment block.** You should be able to get
a good, brief summary of the file's purpose and recent history by looking at
the first few lines.

### Inline Documentation
We (are planning to) use [Doxygen](https://www.stack.nl/~dimitri/doxygen/) to
automatically generate documentation from source code, no matter what language
said code uses.

Doxygen can parse Javadoc-style comments. However, there are a few differences
with Doxygen commenting vs. Javadoc commenting:
* Doxygen uses Markdown for formatting, instead of HTML.
* The file documentation block needs to be marked with the `\file` or `@file`
tag; more on this later.

### File Header Block Format
The first lines of all files should be a comment block containing the following
pieces of information, in roughly this order:
1. **Opening `@file` tag and filename**: only include parts of the path if the
filename alone is not unique.
2. **Description**: both brief and detailed descriptions should be included,
separated by an empty line. Use of the `@brief` tag is optional: everything
up to the first period and newline will be taken as the brief description if
the tag is omitted.
3. **Author(s)**
4. **Version**: try to adhere to [Semantic Versioning](http://semver.org) as
much as possible.
5. **Date modified**

An example is given below:
```c++
/**
 * @file example.cpp
 * A brief description of my example code.
 *
 * This file has a bunch of example code that does strange and informative
 * things and does a lot of other stuff too, I guess. This description is
 * trying to be very long winded. It also has *Markdown*. This was the detailed
 * description, and it fits within 80 character lines. This format stays the
 * exact same when writing in Java, too.
 *
 * @author John Smith
 * @author Haruhi Suzumiya
 * @version 3.14.159-alpha+572e52d
 * @date January 19, 2038
 */
```

## Procedures

### Git and Github
Please keep the following things in mind when working with Git and Github:
* **Ensure that you have a name and email set in your Git configuration**.
You can configure this by running the following commands in a shell:
```shell
$ git config --global user.name "[real name]"
$ git config --global user.email "[email address associated with Github account]"
```
This is to ensure that we can track your changes accurately using Github's
features.
  * Obviously, this won't be possible if you're using a shared computer (i.e. a
school laptop): as such, using a shared computer to make commits is discouraged.
  * If you do have to use a shared computer, then pass the `--author=` option to
`git commit`, with your name followed by your email in angle brackets:
```shell
$ git commit --author="A U Thor <author@example.com>"
```
* **Don't push or commit to `master` directly.** Use other branches or forks
and make [Pull Requests](https://help.github.com/articles/about-pull-requests/)
instead. In most cases, pushes to `master` will be disabled for everyone.
This allows us to review your code and accept it before it becomes an actual
part of the main codebase.
* **All repositories on Github should be tagged with `team5002`.**
This lets anyone find us by searching for 'team5002' on Github.
* **All repositories on Github must be kept public.** This is actually an
FRC rule. Any pre-written code used on a competition robot must be accessible
to the public.
* **All repositories on Github should be licensed under the
[MIT License](https://choosealicense.com/licenses/mit/).** It's simple,
permissive, and open-source.
  * You can add this license (or others) automatically when you create a
  repository.
  * This rule, of course, does not take into account the potential for arcane
  and unexpected legal circumstances, however unlikely those may be.
* **Status checks and (possibly) code review will be required before Pull
Requests are merged.**
* **Non-inline documentation should be kept on repository wikis.** _Non-inline_
documentation refers to all documentation not kept within the source files:
for example, architectural and high-level ('executive') overviews, whitepapers,
etc.
* **Non-inline documentation should be non-technical.** Non-inline documents
should be written as high-level overviews, usually in layman's terms. If you
find yourself getting jargon-heavy or making references to specific aspects
of the code, then your documentation probably belongs within the source code.
  * Whitepapers and specialized reports, however, can be an exception: a detailed
analysis of drivetrain kinematics, for example, would belong on a wiki, not
within source code.

### Continuous Integration
For most of us, there isn't really anything to keep in mind, with regards to
continuous integration (aside from remembering that it is always there and
watching). It's there simply to help ensure that problems don't slip through
the cracks.

There are three main things CI can and will do for us (specifically):
1. **Make sure the code actually compiles.**
2. **Automatically generate documentation using Doxygen.**
3. **Automatically check code style using linters or custom scripts.**

We're (planning to) use [Travis CI](https://travis-ci.org) as our CI system;
it integrates with both Github and Slack, and seems to be a good, well-polished
platform.

### Automated Testing
Performing unit testing with robot code usually ranges from 'impossible' to
'impractical' or 'very difficult'. Simulation-based unit or integration tests
are out of the question:
* The CI build servers probably don't have enough power to run robot
simulations in the first place.
* Simulation-based tests would take far too long to run.
* Simulation-based tests may not be deterministic or consistent enough to be
reliable as a test.

Of course, this means that code that interfaces with hardware cannot be unit
tested.

Thus, unit testing is probably going to be less of a priority with us, as
compared to other software development groups.
What little automatic testing we perform, however, can easily be integrated
with Travis CI.
