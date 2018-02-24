---
layout: post
title: Choose the Right Git Workflow
thumbnail: "assets/img/thumbnails/GitWorkflow.png"
tags: [Git]
---

The most widely used Git workflow was introduced by Vincent Driessen -- [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/). But it is complicated for startups. I personally like the [GitHub workflow](http://scottchacon.com/2011/08/31/github-flow.html) proposed by Scott Chacon, due to its simplicity. The core concepts are:

- Anything in the `master` branch is **deployable**
- To work on something new, create a descriptively named branch off of `master` (ie: `new-oauth2-scopes`)
- Commit to that branch locally and regularly push your work to the same named branch on the server
- When you need feedback or help, or you think the branch is ready for merging, open a `pull request`
- After someone else has reviewed and signed off on the feature, you can merge it into `master`
- Once it is merged and pushed to `master`, you can and should deploy immediately

The process is visually represented below:

![GitHub Flow](../../../assets/img/thumbnails/GitWorkflow.png)

If you are not used to command line or bash commands for Git, two GUI software are available for free: [SourceTree](https://www.atlassian.com/software/sourcetree/overview/) from Atlassian, and [GitHub Desktop](https://desktop.github.com/) from GitHub. Both can be used for version management on either [BitBucket](https://bitbucket.org/) or [GitHub](https://github.com/). I personally like the former. The following shows a convenient way for quick conflict resolving in SourceTree.

![SourceTree-conflict](../../../assets/img/thumbnails/scourcetree-resolve-conflict.png).

You can create a `pull request` on BitBucket. Or, create one with SourceTree:

- Right click your branch and choose **Create pull request..**. This will redirect to the repository page on BitBucket.

![SourceTree-pullrequest](../../../assets/img/thumbnails/pull-request-with-SourceTree.png).

- Double check the **source branch** and **target branch** on the pull request page. Fill the information, choose a reviewer or two for code review, and check the `Close branch` checkbox if your branch is temporary or end of the lifecycle.

The overall experience of the GitHub workflow is simple and intuitive. However, in practice, I personally will create two separate branches: `release/test` and `release/prod` for test sever and production server respectively. The `release/prod` will be merged back to `master` once deployed and is not being rolled back. Then, a version tag is assigned. In this way, `master` branch alway contains tagged versions which have been deployed.

Where, this is my personal thought. It might not fit a company's needs.

## References

- [Understand GitHub Flow](https://guides.github.com/introduction/flow/index.html)
- [Learn Git Branching interactively](http://pcottle.github.io/learnGitBranching/)
