---
layout: post
title: Use Gitflow to manage git workflow
date: '2018-07-10 13:07'
---

This post explains how to use [Gitflow](https://github.com/nvie/gitflow) in a development cycle.

# Step-by-step guide

Make sure that you have no uncommitted changes before you start any gitflow branch! Also make sure you have pulled all the recent commits from master and production.

## Feature Branch
Use Gitflow to create feature branches when working on a regular JIRA ticket during a sprint. Here we use LAG-999 as an example.

1. From command line, run the following command to create a new branch feature/LAG-999. It'll be automatically checked out after it's created.

```
git flow feature start LAG-999
```

2. Implement the change. Commit your changes to the feature branch. When ready for initial PO review, publish your branch.

```
git flow feature publish LAG-999
```

Alternatively, you can simply push your branch to the remote github server, from command line as below

```
git push -u origin feature/LAG-999
```

3. Test your change

Use your favorite test tools or simply test manually.

4. Use the following command to finish the feature branch and merge it to the development branch.

```
git flow feature finish LAG-999
```

If there is any conflict, work with other team members to resolve the conflict.

**Remember to delete the remote branch!**

```
git push origin --delete feature/LAG-999
```

Now the commits in the feature branch are merged to the development branch and the feature branch is deleted. Push development branch to the remote repository.

```
git push origin development
```

## Release Branch

Create a release branch with a version number (8.9.0 for example) when a sprint is closed. Any last-minute fix before the production release will be committed to the release branch.

From command line, run the following command to create a new release branch

```
git flow release start 8.9.0
```

Publish the release branch to allow release commits from the whole team.

```
git flow release publish 8.9.0
```

When a problem needs to be fixed and included in the production release, have your fix committed to the release branch only.

When we are ready for production release, finish the release branch and merge it to development branch and master(production) branch.

```
git flow release finish 8.9.0
```

The feature branch is deleted from local repository now. Push the changes to the remote repository

```
git push origin master
git push origin production
git push --tags
```

## Hotfix Branch

When a bug is reported in production, we need to create a hotfix branch to fix it. We can use the JIRA issue number to name the branch

Check what the current version is:

Pull both master and production from remote. This is **important** since you don't want to hotfix an earlier version.

```
git checkout development
git pull
git checkout master
git pull
```

Create the hotfix branch. Increase the patch digit by 1, (e.g, use 8.3.3 if 8.3.2 is the current version)

```
git flow hotfix start 8.3.3
```

You should now be in the branch hotfix/v8.3.3

Fix the bug. Commit your changes.
Finish the branch

```
git flow hotfix finish 8.3.3
```

You'll be asked to enter three comments:

comment for merging to production branch

comment for tagging the version (NOTE! this can't be blank)

comment for merging to development branch.
After this, the hotfix branch is deleted. Push your fixes to remote repository

```
git push origin m
git push origin production
git push --tags
```

## References:
1. [Gitflow](https://github.com/nvie/gitflow)
2. [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/?)
3. [Using git-flow to automate your git branching workflow](https://jeffkreeftmeijer.com/git-flow/)
4. [git-flow cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/)
5. [Gitflow workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
