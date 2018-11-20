# Docs for Foxer360

Repository for all necessary documentation about the whole Foxer360 system.

## [How to start the project](/example-project) 
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

### Pull Request

**Pull Requests (PR)** are used to merge your branches (such as `feature`, `release`, `bugfix` and `hotfix`) into `master` or `sandbox`. To merge branches in PR, we *always* use *Squash and Merge* type of merge. This type squash all commits into one commit, which will be added into the base (branch which you merge into). It creates clean history on `master` and `sandbox`. These commits have `title` equal to name of PR and `description` with all commits, which was in branch which you want to merge.

For example you have branch `feature/one` and there are two commits `Added basic configuration` and `Finished feature one`. So you want to merge this feature into `sandbox`, so you create PR called for example `Added cool feature ONE` and of course, you can add some description, etc. Then if everything is OK, you select `Squash and Merge` in type of marge and click on it. This will show you form for `title` and `description` of this squash commit. `title` will be `Added cool feature ONE (#1)` and `description` will be
```
* Added basic configuration
* Finished feature one
```
Between commits, there is no one empty line and also there are no descriptions of these commits, just titles.

After you successfully merge this branch, then *remove it.*


**Release and Hotfix** PRs are a little bit more complicated. You need to create two PR for each `release` or `hotfix` branch, because of these branches needs to be merged into `master` and also `sandbox`. If you are creating PR for `release` branch, then create at first PR into `sandbox` and then into `master`. If you are creating PR for `hotfix` branch, then create at first PR into `master` and then into `sandbox`. The reason is simple, `release` is from `sandbox`, so merge into `sandbox` will be probably without problems, but merge into `master` can be problematic and can have some conflicts. The same reason is for `hotfix`, but `hotfix` is from `master`, so you need firstly resolve PR into `master` and then into `sandbox`.

PRs for `release` branches will have name in format `Finished release v0.1.0 into sandbox` and `Finished release v0.1.0 into master`. Squash commit into `sandbox` will have same format as described above for `feature` branch for example. Squash commit into `master` will have little bit different format. `title` of this commit is the same, but description has one little change. There will be list of commits added into `sandbox` from the last release, then one empty line and then list of commits added into this `release` branch. It's just for better readability. So for example commit for `sandbox` will be
```
Finished release v0.1.0 into sandbox (#2)
* Changed version numbers
```
and commit for `master` will be
```
Finished release v0.1.0 into master (#3)
* Added cool feature ONE

* Changed version numbers
```

PRs for `hotfix` branches will be very similar as for `release` branches. But format for `master` will be now for `sandbox` and vica verse. But PRs will have name in format `<hotfix description>, hotfix into <branch>` so for example `Fixed bad grammar, hotfix into sandbox`...
