# Git Gud

## What is this
"Confusion" - A poem by Jack Liston

> I knew nothing about Git
> I wanted to learn more about Git
> I spent a few hours researching Git
> This is what I found helpful


## The VERY Basics
- What is Git and why use it?
> Git is a distributed version-control system for tracking changes in any set of files, originally designed for coordinating work among programmers cooperating on source code during software development.

- Distribute????
> A distributed system is a system whose components are located on different networked computers, which communicate and coordinate their actions by passing messages to one another.

- Version Control???
> Version control, also known as source control, is the practice of tracking and managing changes to software code. Version control systems are software tools that help software teams manage changes to source code over time.

***Have you ever saved different versions of a file? Something like:***

```
story.txt
story-joe-edit.txt
story-joe-edit-reviewed.txt*
```

Branches accomplish similar goals in Git repos.

- [x] Changes are saved and hash into blobs
- [x] Blobs sorted in tree which are hashed
- [x] Commits which are like snapshots of the project in that moment. 
- [x] A commit contain of all the trees and the blobs hash's.

***
![](https://i.imgur.com/5YIUcoG.jpg)
***
![](https://i.imgur.com/ASTCyE3.png)
***


### SETUP
```
it config --global user.name “[firstname lastname]”
set a name that is identifiable for credit when review version history

git config --global user.email “[valid-email]”
set an email address that will be associated with each history marker

git config --global color.ui auto
set automatic command line coloring for Git for easy reviewing
```
### SETUP & INIT
```
git init
initialize an existing directory as a Git repository
git clone [url]
retrieve an entire repository from a hosted location via URL
```
### STAGE & SNAPSHOT
```
git status
show modified files in working directory, staged for your next commit
git add [file]
add a file as it looks now to your next commit (stage)
git reset [file]
unstage a file while retaining the changes in working directory
git diff
diff of what is changed but not staged
git diff --staged
diff of what is staged but not yet commited
git commit -m “[descriptive message]”
commit your staged content as a new commit snapshot
```
### BRANCH & MERGE
```
git branch
list your branches. a * will appear next to the currently active branch
git branch [branch-name]
create a new branch at the current commit
git checkout
switch to another branch and check it out into your working directory
git merge [branch]
merge the specified branch’s history into the current one
git log
show all commits in the current branch’s history
```

[WOWZA WHATS IN HERE.(Its the cheatsheet wink wink nudge nudge.)](https://education.github.com/git-cheat-sheet-education.pdf)

***

### Plumbing vs Porcelain

- Porcelain commands are like the most basic outer layer of git commands
- Behind the porcelain appliances in your bathroom is plumbing
- Likewise, plumbing is like the next layer down regarding git functionality.

***

For more reading on Plumbing and Porcelain click [here](https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain)
For a interesting video walkthrough of making a commit with plumbing tools aswell as some other cool topics click [here](https://www.youtube.com/watch?v=0SJCYPsef54)

## Git Workflow Model

How to pick a workflow for your team??
> ***[TIL what a panacea is](https://en.wikipedia.org/wiki/Panacea_(medicine))***
- There is no panacreas when it comes to Git workflow.
- Look around, see what you like and yoink it.

Maybe start with a larger more complex system for some idea?
***
### GitFlow
*Disclaimer*, I am ripping everything from [here](https://nvie.com/posts/a-successful-git-branching-model/)
GitFlow is a branching model for Git, created by Vincent Driessen.
![](https://i.imgur.com/B4Bb2dA.png)

#### The main branches 

The central repo holds two main branches with an infinite lifetime:

**master
develop**

We consider ***origin/master*** to be the main branch where the source code of HEAD always reflects a production-ready state.

We consider ***origin/develop*** to be the main branch where the source code of HEAD always reflects a state with the latest delivered development changes for the next release.

#### Supporting branches 

The different types of branches we may use are:

**Release branches
Hotfix branches
Feature branches**

***NB***
> May branch off from:
> develop
> Must merge back into:
> develop

#### Feature Branch
![](https://i.imgur.com/hzmgJb3.png)

Creating a feature branch 
When starting work on a new feature, branch off from the develop branch.
```
$ git checkout -b myfeature develop
Switched to a new branch "myfeature"
```
Incorporating a finished feature on develop 
Finished features may be merged into the develop branch to definitely add them to the upcoming release:
```
$ git checkout develop
Switched to branch 'develop'
$ git merge --no-ff myfeature
Updating ea1b82a..05e9557
(Summary of changes)
$ git branch -d myfeature
Deleted branch myfeature (was 05e9557).
$ git push origin develop
```

**The --no-ff flag causes the merge to always create a new commit object, even if the merge could be performed with a fast-forward.**

![](https://i.imgur.com/a99vAZS.png)


read more [here ](https://stackoverflow.com/questions/9069061/what-is-the-difference-between-git-merge-and-git-merge-no-ff) on --no--ff


***Fast Forward Merge***
> A fast-forward merge can occur when there is a linear path from the current branch tip to the target branch. Instead of “actually” merging the branches, all Git has to do to integrate the histories is move (i.e., “fast forward”) the current branch tip up to the target branch tip.

![](https://i.imgur.com/SdtYg5S.png)

Vincents justification for using the no fast forward flag
> In the latter case, it is impossible to see from the Git history which of the commit objects together have implemented a feature—you would have to manually read all the log messages. Reverting a whole feature (i.e. a group of commits), is a true headache in the latter situation, whereas it is easily done if the --no-ff flag was used.
Yes, it will create a few more (empty) commit objects, but the gain is much bigger than the cost.


#### Hotfix branches 

- May branch off from:
    - master
- Must merge back into:
    - develop and master

**Vincent suggests**

>Branch naming convention:
hotfix-*

***Creating the hotfix branch***
- Hotfix branches are created from the master branch. 
- For example, say version 1.2 is the current production release running live and causing troubles due to a severe bug. 
- But changes on develop are yet unstable. 
- We may then branch off a hotfix branch and start fixing the problem:

```
$ git checkout -b hotfix-1.2.1 master
Switched to a new branch "hotfix-1.2.1"
$ ./bump-version.sh 1.2.1
Files modified successfully, version bumped to 1.2.1.
$ git commit -a -m "Bumped version number to 1.2.1"
[hotfix-1.2.1 41e61bb] Bumped version number to 1.2.1
1 files changed, 1 insertions(+), 1 deletions(-)
```

### Github Flow
*Disclaimer*, I am riping everything from [here](https://guides.github.com/introduction/flow/)

#### Create a branch
- Branching is a core concept in Git
- The entire GitHub flow is based upon it
- There's only one rule: 
    - anything in the main branch is always deployable.
> Because of this, it's extremely important that your new branch is created off of main when working on a feature or a fix. Your branch name should be descriptive (e.g., refactor-authentication, user-content-cache-key, make-retina-avatars), so that others can see what is being worked on

#### Add commits
- Commits also create a transparent history of your work
- Each commit has an associated commit message describing the changes made
- Each commit is considered a separate unit of change.
    - This lets you roll back changes if a bug is found, or if you decide to head in a different direction.
    - 
***Pro Tip***
>Commit messages are important, especially since Git tracks your changes and then displays them as commits once they're pushed to the server. By writing clear commit messages, you can make it easier for other people to follow along and provide feedback.>

#### Open a Pull Request/Discuss and review your code
[What is a pull request](https://www.youtube.com/watch?v=For9VtrQx58)
Use this to discuss your commits.
Written in markdown.

DCU uses GitLab so you would be dealing with Merge Requests.
[Here](https://about.gitlab.com/blog/2014/09/29/gitlab-flow/), go look at the Gitlab Flow you silly goose.

#### Deploy
Once request has been approved you can deploy those changes to that branch from your local machine.

#### Merge
Once you and your team are happy with all the changes made on that branch, Merge!!!!

[Crosslinking Issues - Gitlab](https://docs.gitlab.com/ee/user/project/issues/crosslinking_issues.html#:~:text=From%20Commit%20Messages,-Every%20time%20you&text=If%20the%20issue%20and%20the,issues%2F%20).


### Other suggestions

It might be a good idea to predetermine any naming conventions.

For example, if you plan on having multiple versions throughout the course of your project then semantic versioning could be an approach you go for:

https://semver.org/

**Summary of semver**
- Given a version number MAJOR.MINOR.PATCH, increment the:

    - MAJOR version when you make incompatible API changes,
    - MINOR version when you add functionality in a backwards compatible manner, and
    - PATCH version when you make backwards compatible bug fixes.


### Final Git Tips

***Recommened Media***

Git docs [here](https://git-scm.com/)
Github Guides [here](https://guides.github.com/)
Recommend this video [here](https://www.youtube.com/watch?v=CRlGDDprdOQ)
A link to the documentation on rebasing [here](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

***Personal tips***

I like Kanban boards.
- Name a branch after the ticket number + initals of the maintianer?

REAL DEVS USE EMOJIS 🚀✅🧪

> NEVER REBASE PUBLIC REPOSITORIES

When you are working on a private branch for a feature for example. I recommend rebasing and then merging to master(or whatever the public branch you are merging into is)

- Formalize Git conventions for your team
    - Everyone should follow standard conventions for branch naming, tagging, and coding. 
    - Looking for ideas? Just google it!
    - Pick a suitable convention early on and follow it as a team.

- Not everyone on your team is a uber hacker
    - Make a basic set of instructions to be used at all time. (Don't give yourself an excuse!)



### Extra curricular reading
https://education.github.com/pack
https://www.gitkraken.com/
https://github.com/cli/cli
https://git-scm.com/docs/git-merge
https://git-scm.com/docs/git-merge#Documentation/git-merge.txt---squash
[Comparing Workflows - Atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows)
