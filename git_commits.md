Git
===

![Geno6 logo](http://geno6.com/wp-content/uploads/2014/08/geno6lgo.png)

We use Git version control for all of our projects at [Geno6]. We host
our code on Bitbucket. Large features get their own branch and are merged with the master branch.

This document is based on [Sparkbox Git Guide] and [Peter Hutterer blog post]


General thoughts
----------------


Any software project is a collaborative project. It has at least two developers, the original developer and the original developer a few weeks or months later when the train of thought has long left the station. This later self needs to reestablish the context of a particular piece of code each time a new bug occurs or a new feature needs to be implemented. 

Re-establishing the context of a piece of code is wasteful. We can't avoid it completely, so our efforts should go to reducing it to as small as possible. Commit messages can do exactly that and as a result, a commit message shows whether a developer is a good collaborator.

**A good commit message should answer three questions about a patch:**

- **Why is it necessary?** It may fix a bug, it may add a feature, it may improve performance, reliabilty, stability, or just be a change for the sake of correctness.

- **How does it address the issue?** For short obvious patches this part can be omitted, but it should be a high level description of what the approach was.

- **What effects does the patch have?** (In addition to the obvious ones, this may include benchmarks, side effects, etc.)

These three questions establish the context for the actual code changes, put reviewers and others into the frame of mind to look at the diff and check if the approach chosen was correct.


Forming commits
---------------


The general rule for deciding what goes into a commit is:

**Do not mix changes - functional/refactoring/bug fixes/ and**
**ensure that there is only one "logical change" per commit.**


**Things to avoid when creating commits**
- Mixing whitespace changes with functional code changes.
- Mixing two unrelated functional changes.
- Sending large new features in a single giant commit.


**This approach helps to reach a number of important goals**

- Have a clear code history which helps to browse through repositories
- Deliver high quality product to clients
- Provide useful info to code reviewers


The Art of the Commit Message
-----------------------------

We use a strict style for all of our commit messages. The style we use helps
ensure that our commits stay small and are easy to browse.

**The Layout**

The Layout should have the following structure

- First line is 50 characters or less
- Then a blank line
- Remaining text should be wrapped at 72 characters


```
<type>(<scope>): <subject>

<body>

<footer>
```

**The Title (the first line)**

The title consists of the `<type>`, the `(<scope>)`, and the `<subject>`.

If it relates to a Redmine task, put the Redmine task number in parentheses
in the end of the line. (ex. FEAT(Register): Add the secret key field
(#1024)) This helps us track issues with their commits.


**Allowed Values for `<type>`**

- **FEAT** (new feature)
- **FIX** (bug fix)
- **DOCS** (changes to documentation)
- **STYLE** (formatting, missing semi colons, etc; no code change)
- **REFACTOR** (refactoring production code)
- **TEST** (adding missing tests, refactoring tests; no production code change)
- **CHORE** (updating grunt tasks etc; no production code change)

**Example `<scope>` Values**

Scope varies per project. For example, a project with an admin interface would
use a scope of `(admin)` to denote commits that change admin code. Another
project might have a scope of `(backup)` to commits referencing backup code
changes.

**Subject**

An [imperative tone][365] is also helpful in conveying what a commit does,
rather than what it did. For example, use **change**, not _changed_ or
_changes_.

**The Body**

The body of the commit message should use a style similar to the one proposed
in this [article by tpope][tpope]. The body, just like the subject, should use
an imperative tone.


_Inspired by [Angular][angularc] and [Karma's][karmac] commit style._

**The Footer**

Here's where we additionally reference items in our product management tool. [Redmine][]
allows for many different customizable styles of references.

When fetched from the repositories, commit messages are scanned for
referenced or fixed/finished issue IDs. Redmine looks for an issue ID
and/or special keyword for referencing or changing issue status to
"finished".
After a keyword issue IDs can be separated with a space, a comma or &.

The keywords are caseinsensitive and at least one blankspace or colon is
needed between the keyword and the first hash to produce a match.

Referencing keywords are "refs", "references", "IssueID"
Finish keywords are "finishes", "fixes"

Here are some examples:

*   `This commit refs:#1, #2 and fixes #3`
*   `This commit References  #1, #2 and fixes #3`
*   `This commit REFS: #1, #2 and finishes #3`
*   `References #1, #4, and #2.`
*   `Fixes #1.` _note this marks the item as finished in Redmine_
*   `Finishes #1, #2.` _note this marks both items as finished in Redmine_

Example Commit Messages
-----------------------

```
FEAT($browser): onUrlChange event (popstate/hashchange/polling)

Add new event to $browser:
- forward popstate event if available
- forward hashchange event if popstate not available
- do polling when neither popstate nor hashchange available

Breaks $browser.onHashChange, which was removed (use onUrlChange instead)
```

```
FIX($compile): Add a couple of unit tests for IE9

Older IEs serialize html uppercased, but IE9 does not...
Would be better to expect case insensitive, unfortunately jasmine does
not allow to user regexps for throw expectations.

finishes #392
Breaks foo.bar api, foo.baz should be used instead
```

```
DOCS(guide): update fixed docs from Google Docs

Fix a couple of typos:
- indentation
- batchLogbatchLog -> batchLog
- start periodic checking
- missing brace
```

```
FEAT($compile): simplify isolate scope bindings

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

[Sparkbox Git Guide]: https://github.com/sparkbox/how_to/blob/master/style/git/README.md
[Peter Hutterer blog post]: http://who-t.blogspot.de/2009/12/on-commit-messages.html
[Geno6]: http://geno6.com
[angularc]: https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#
[karmac]: http://karma-runner.github.io/0.8/dev/git-commit-msg.html
[365]: http://365git.tumblr.com/post/3308646748/writing-git-commit-messages
[tpope]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
[pull_request]: khttps://help.github.com/articles/using-pull-requests
[Sparkbox]: http://seesparkbox.com
[Redmine]: https://redmine.org/
