# Contributing to docker-apim

Docker resources for WSO2 API Microgateway is open source and we encourage contributions from our community.

## How you can Contribute

### Reporting an issue

If you find an issue with the resources, first search through the [existing issues](https://github.com/wso2/docker-mg/issues) to find if the issue is already reported. If your issue is not already reported, fill the repository issue template properly and submit a new issue. Be kind enough to provide all the details you can provide, when submitting your issue.

### Code Contributions

If you like to contribute with a bug fix or a new feature, start by posting an issue and discussing the best way to implement it.

Unlike most projects, development for this repository is carried out devlopment branch of the respective product. Ex: 3.2.x. This is because the `master` branch contains the latest stable release of the project.
The code in latest `x.x.x` branch is merged to the `master` branch for the release, after a final review and a round of testing.

Please follow these guidelines when contributing to the code:

1. Fork the current repository.
2. Create a topic branch from the `3.2.x` branch.
3. Make commits in logical units.
4. Before you send out the pull request, sync your forked repository with a remote repository. This makes your pull request simple and clear.

```bash
git clone https://github.com/<user>/docker-mg.git
git remote add upstream https://github.com/wso2/docker-mg.git
git fetch upstream
git checkout -b <topic-branch> upstream/<dev_branch>

# add some work

git push origin <topic-branch>

# submit pull request
```

**Thanks for contributing!**
