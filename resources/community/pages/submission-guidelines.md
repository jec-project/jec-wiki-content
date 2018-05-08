# Submission Guidelines

This section gives the experienced contributor some tips and guidelines.

## Important

Before you submit an issue, please search the [issue tracker](https://github.com/jec-project/JEC/issues), maybe an issue for your problem already exists and the discussion might inform you of workarounds readily available.

**All submissions, including submissions by project members, require review.**

## Submitting an Issue

We will be insisting on a minimal reproduce scenario in order to save maintainers time and ultimately be able to fix more bugs. 
We are not able to investigate / fix bugs without a minimal reproduction, so if we don't hear back from you we are going to close an issue that doesn't have enough info to be reproduced.

You can file new issues by filling out our [issue form](https://github.com/jec-project/JEC/issues).

## Submitting a Pull Request

Pull Requests _(PR)_ are always welcome!

### Contributor License Agreement _(CLA)_

By sending _(PRs)_ you will automatically agree to our [Contributor License Agreement _(CLA)_](./community/contributor-license-agreement).

[Learn more about Harmony agreements.](http://harmonyagreements.org/)

**We cannot accept code without this.**

### Submission Guide

Before you submit your PR consider the following guidelines:

1. Fork the JEC repo for which you want send the PR.
2. Make your changes in a new git branch:
```shell
    git checkout -b my-fix-branch master
```
3. Create your patch, **including appropriate test cases**.
4. Follow our [Coding Rules](./community/coding-rules).
5. Run the full project test suite, as described in the [Coding Rules section](./community/coding-rules#unit-testing), and ensure that all tests pass.
6. Commit your changes using a descriptive commit message.
```shell
    git commit -a
```
7. Push your branch to GitHub:
```shell
    git push origin my-fix-branch
```
8. In GitHub, send a pull request to `<project>:master`, where `<project>` represents the name of the JEC project for which you send the PR.
* If we suggest changes then:
  * Make the required updates.
  * Re-run the JEC Project test suites to ensure tests are still passing.
  * Rebase your branch and force push to your GitHub repository (this will update your PR):
```shell
    git rebase master -i
    git push -f
```

Anyway, thank you for your contribution!