# Docs for Foxer360

Repository for all necessary documentation about the whole Foxer360 system.

## Git Workflow

In this section, I will specify how we will use Git, so please use it this way. This git workflow has few types of branches which are described below.

 - `master` branch is the main branch of the whole repository. This branch contains only finished releases, each commit in this branch is **Pull Request** from `release` or `hotfix` branches. Nothing else, no direct pushes are allowed.
 - `sandbox` branch is used for development. This branch contains new features, bugfixes, and it's the latest non-released version of the repository. Each commit in this branch is **Pull Request** from `release`, `feature`, `hotfix` or `bugfix` branches. Nothing else, no direct pushes are allowed.
 - `release` branch is **always** created from `sandbox`. This branch is used to make some little changes for next release, like changed version numbers, changed dependencies to fixed versions, etc. After release is finished, then **Pull Requests** are created. First into `sandbox` and second into `master`. More about these **Pull Request** below.
 - `feature` branch is **always** created from `sandbox` and it is used to create something new. If feature is done, then **Pull Request** is created into `sandbox`.
 - `hotfix` branch is **always** created from `master`. This branch is used to fix some problem which is easy to fix and it's really necessary to fix it immediately. Obviously, it's a problem on released version, not in non-released version. After hotfix is finished, then **Pull Requests** are created. First into `master` and second into `sandbox`.
 - `bugfix` branch is **always** created from `sandbox` and it is used to fix some functionality which already exists. After bugfix is finished, then **Pull Request** is created into `sandbox`.

Branches `feature`, `hotfix`, `bugfix` and `release` are used as prefix, like these examples

`hotfix/grammar`, `feature/dashboard-stats`, `release/v0.3.0`

The name after prefix should be short and descriptive, not something like `feature/something`. Releases have always a name of the number of this release. If you are using *terminal* and you want to create new branch, you can do it like this

```
git checkout sandbox
git pull
git checkout -b feature/<name>
git push -u origin feature/<name>
```

These commands above will create `feature/<name>` branch from up-to-date `sandbox` and push it to the remote repository, so everybody can see, there is this branch. If you want to do something from `sandbox`, `master` or another branch which somebody contributes to, **always** be sure, you have the latest version, just run `git pull` before you start working. This can prevent lots of conflicts.

**Please use commits.** If you create the new branch, works for two weeks and after that, you push only one commit *Done*, then it's hard to review the code in **Pull Request** and nobody knows what happened here. So if you finished some logical block, commit it. You don't need to push it immediately to remote, you can make for example five commits and push it to the remote on the end of the day.

Commits into branches should be self-descriptive. Each commit has a *title* and *description*... If you are using some GUI client for Git, you probably have two fields. Git automatically resolves new line in *commit message* as a description. So if your *commit message* has two or more lines, then first line is *title* and next lines are *description*. If you are using *terminal* to commit changes, you can add another `-m` paramter, this will add new line in *commit message* so it will be description.

```
git commit -m "Title of this commit" -m "Description of this commit"
```

Title should be short and descriptive of what happened in this commit, and description is handy if you need to describe it more deeply. For example:

```
Added new job into CircleCI
This job runs tests on five different versions of node.js and it's only triggered on the master branch.
```
