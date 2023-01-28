# Git Commit Best Practices

## Basic Rules

### Commit Related Changes

A commit should be a wrapper for related changes. For example, fixing two different bugs should produce two separate commits. Small commits make it easier for other developers to understand the changes and roll them back if something went wrong.
With tools like the staging area and the ability to stage only parts of a file, Git makes it easy to create very granular commits.

### Commit Often

Committing often keeps your commits small and, again, helps you commit only related changes. Moreover, it allows you to share your code more frequently with others. That way it‘s easier for everyone to integrate changes regularly and avoid having merge conflicts. Having large commits and sharing them infrequently, in contrast, makes it hard to solve conflicts.

### Don't Commit Half-Done Work

You should only commit code when a logical component is completed.
Split a feature‘s implementation into logical chunks that can be completed quickly so that you can commit often. If you‘re tempted to commit just because you need a clean working copy (to check out a branch, pull in changes, etc.) consider using Git‘s «Stash» feature instead.

### Test Your Code Before You Commit

Resist the temptation to commit something that you «think» is completed. Test it thoroughly to make sure it really is completed and has no side effects (as far as one can tell). While committing half-baked things in your local repository only requires you to forgive yourself, having your code tested is even more important when it comes to pushing/sharing your code with others.

### Write Good Commit Messages

Begin your message with a short summary of your changes (up to 50 characters as a guideline). Separate it from
the following body by including a blank line. The body of your message should provide detailed answers to the following questions:
– What was the motivation for the change? – How does it differ from the previous
implementation?
Use the imperative, present tense («change», not «changed» or «changes») to be consistent with generated messages from commands like git merge.
Having your files backed up on a remote server is a nice side effect of having a version control system. But you should not use your VCS like it was a backup system. When doing version control, you should pay attention to committing semantically (see «related changes») - you shouldn‘t just cram in files.

### Use Branches

Branching is one of Git‘s most powerful features - and this is not by accident: quick and easy branching was a central requirement from day one. Branches are the perfect tool to help you avoid mixing up different lines of development. You should use branches extensively in your development workflows: for new features, bug fixes, ideas...

### Agree on A Workflow

Git lets you pick from a lot of different workflows: long-running branches, topic branches, merge or rebase, git-flow... Which one you choose depends on a couple of factors: your project, your overall development and deployment workflows and (maybe most importantly) on your and your teammates‘ personal preferences. However you choose to work, just make sure to agree on a common workflow that everyone follows.

The following document is based on experience doing code development, bug troubleshooting and code review across a number of projects using GIT, including libvirt, QEMU and OpenStack Nova. Examination of other open source projects such as the Kernel, CoreUtils, GNULIB and more suggested they all follow a fairly common practice. It is motivated by a desire to improve the quality of the Nova GIT history. Quality is a hard term to define in computing; one man's "Thing of Beauty" is another man's "Evil Hack". We can, however, come up with some general guidelines for what to do, or conversely what not to do, when publishing GIT commits for merge with a project, in this case, OpenStack.

This topic can be split into two areas of concern

* The structured set/split of the code changes
* The information provided in the commit message

## Formatting Rules

### Commit Message Format

```txt
<type>(<scope>): <subject> <meta>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

The first line of a commit message should not be longer than 72 characters, and all the other lines should have a maximum of 100 characters!
This allows the message to be easier to read on Github as well as in various git tools.

> Subject line may be prefixed for continuous integration purposes.
> For example, [JIRA](https://bigbrassband.com/git-for-jira.html)
> requires ticket in the beggining of commit message message:
> "[LHJ-16] fix(compile): add unit tests for windows"

The usage of markup that can be read as plain text (e.g. markdown, reST, etc),
specially in body and footer is optional, but useful when auto generating
changelogs.

#### Allowed `<type>`

The `<type>` of a commit message should be a single word or abbreviation drawn
from an ontology, according to the nature of the project.
This document specifies a **programming ontology**, with the following elements:

* **feat**: A new feature
* **fix**: A bug fix
* **docs**: Documentation only changes
* **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
* **refactor**: A code change that neither fixes a bug nor adds a feature
* **perf**: A code change that improves performance
* **test**: Adding missing tests
* **build**: Changes to the build/compilation/packaging process or auxiliary tools such as documentation generation
* **ci**: Changes in the continuous integration/delivery setup

In the case a project decides to use a different ontology, this needs to be specified in its contributing guidelines
(usually a `CONTRIBUTING.md` file in the root of the project),
either by including a modified copy of the text in this document replacing the items in the list above,
or by linking this document and providing a replacement type list. Example text:

```markdown
<!-- CONTRIBUTING.md -->
# Commit Message Guidelines
This project follows [specific commit message guidelines](... link to this document), with the following allowed types:

* ...: ...
* ...: ...
```

An example of ontology for a book writing project is: **typo**,
**restructure**, **write**, **move**, etc…

#### Allowed `<scope>` (optional)

Usually it is convenient to mention exactly which part of the code base changed.
The `<scope>` token is responsible for providing that information.
For example, imagine that a webserver introduces OAuth login via Facebook, a
valid commit message would be:

```sh
feat(auth): introduce sign in via Facebook
```

Please notice that here `auth` might refer to a conceptual architectural component
or even the basename of an file in the repository. While the granularity of the
scope can vary, it is important for it to be a part of the "common language"
spoken in the project.

Please notice that in some cases the scope is naturally *too broad*, and
therefore not worthy to mention. Examples of this special case:

```sh
docs: replace old website URL
refactor: move folder structure to `src` directory layout
```

#### `<subject>` text

Subject line should contains succinct description of the change.

* use imperative, present tense: “change” not “changed” nor “changes”
* don't capitalize first letter
* no dot (.) at the end

#### Allowed `<meta>` (optional)

Additionally, the end of subject-line may contain *hashtags* to facilitate changelog generation and bissecting.

* `#wip` - indicate for contributors the feature being implemented is not complete yet. Should not be included in changelogs (just the last commit for a feature goes to the changelog).
* `#irrelevant` - the commit does not add useful information. Used when fixing typos, etc... Should not be included in changelogs.

#### Message body

* just as in `<subject>` use imperative, present tense: “change” not “changed” nor “changes”
* includes motivation for the change and contrasts with previous behavior
* [writing-git-commit-messages](http://365git.tumblr.com/post/3308646748/writing-git-commit-messages)
* [a-note-about-git-commit-messages](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)

#### Message footer

##### Breaking changes

All breaking changes have to be mentioned in footer with the description of the change, justification and migration notes

```txt
BREAKING CHANGE: isolate scope bindings definition has changed and
the inject option for the directive controller injection was removed.

To migrate the code follow the example below:

Before:

   scope: {
     myAttr: 'attribute',
     myBind: 'bind',
     myExpression: 'expression',
     myEval: 'evaluate',
     myAccessor: 'accessor'
   }

After:

   scope: {
     myAttr: '@',
     myBind: '@',
     myExpression: '&',
     // myEval - usually not useful, but in cases where the expression is assignable, you can use '='
     myAccessor: '=' // in directive's template change myAccessor() to myAccessor
   }

The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

##### Referencing issues

Closed bugs should be listed on a separate line in the footer prefixed with "Closes" keyword like this:

```txt
Closes #234
```

or in case of multiple issues:

```txt
Closes #123, #245, #992
```

### Revert

If the commit reverts a previous commit, it should begin with revert:, followed by the header of the reverted commit. In the body it should say: This reverts commit `<hash>`., where the hash is the SHA of the commit being reverted.

### Examples

```txt
feat($browser): add onUrlChange event (popstate/hashchange/polling)

New $browser event:
- forward popstate event if available
- forward hashchange event if popstate not available
- do polling when neither popstate nor hashchange available

Breaks $browser.onHashChange, which was removed (use onUrlChange instead)
```

```txt
fix($compile): add unit tests for IE9

Older IEs serialize html uppercased, but IE9 does not...
Would be better to expect case insensitive, unfortunately jasmine does
not allow to user regexps for throw expectations.

Closes #392
Breaks foo.bar api, foo.baz should be used instead
```

```txt
feat(directive): add directives disabled/checked/multiple/readonly/selected

New directives for proper binding these attributes in older browsers (IE).
Added coresponding description, live examples and e2e tests.

Closes #351
```

```txt
style($location): add couple of missing semi colons
```

```txt
docs(guide): update fixed docs from Google Docs

Couple of typos fixed:
- indentation
- batchLogbatchLog -> batchLog
- start periodic checking
- missing brace
```

```txt
feat($compile): simplify isolate scope bindings

Change the isolate scope binding options to:
  - @attr - attribute binding (including interpolation)
  - =model - by-directional model binding
  - &expr - expression execution binding

This change simplifies the terminology as well as
number of choices available to the developer. It
also supports local name aliasing from the parent.

BREAKING CHANGE: isolate scope bindings definition has changed and
the inject option for the directive controller injection was removed.

To migrate the code follow the example below:

Before:

   scope: {
     myAttr: 'attribute',
     myBind: 'bind',
     myExpression: 'expression',
     myEval: 'evaluate',
     myAccessor: 'accessor'
   }

After:

   scope: {
     myAttr: '@',
     myBind: '@',
     myExpression: '&',
     // myEval - usually not useful, but in cases where the expression is assignable, you can use '='
     myAccessor: '=' // in directive's template change myAccessor() to myAccessor
   }

The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

## Generating CHANGELOG.md

Changelogs may contain three sections: new features, bug fixes, breaking changes.
This list could be generated by script when doing a release, along with links to related commits.
Of course you can edit this change log before actual release, but it could generate the skeleton.

List of all subjects (first lines in commit message) since last release:

```sh
git log <last tag> HEAD --pretty=format:%s
```

New features in this release

```sh
git log <last release> HEAD --grep feat
```

---
