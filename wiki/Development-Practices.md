This page documents recommended practices for contributions to the affiliated Mesh Adaptation modules (Animate, Goalie, and Movement).
However, the practices outlined here are meant as recommendations for new developers - they are not meant as strict rules to be adhered to.

## License

[Animate](https://github.com/mesh-adaptation/animate/blob/main/LICENSE), [Goalie](https://github.com/mesh-adaptation/goalie/blob/main/LICENSE), and [Movement](https://github.com/mesh-adaptation/movement/blob/main/LICENSE) have MIT licenses. Familiarise yourself with their terms.

## Setting up development environment

To prepare for the development of the Mesh Adaptation package you are interested in, please follow the [installation instructions](../Installation-Instructions#installing-mesh-adaptation-modules), but replace `make install` with `make install_dev`. This will install development dependencies (such as pytest for testing and Ruff for code linting and formatting), in addition to mandatory production dependencies. The full list of dependencies can be found in the `pyproject.toml` file of each package's repository.

## GitHub comments

* If you make a comment anywhere in the GitHub API (e.g., on an Issue or PR) and realise it wasn't quite right, **do not** delete the comment. Many users have GitHub configured to send them emails and trying to track down a deleted comment from an email can cause a lot of confusion.
  * Instead of deleting the comment, edit it and use ~~strikeout~~ to delete the offending text. (To do this, put `~~` before and after the text you want to strike out.)
  * For extra clarity, add another comment starting with "Edit:", which explains that what you said was incorrect and why.

## Code comments

* Make sure that comments are used wherever it isn't obvious what a piece of code is doing.
* However, avoid adding comments when it is obvious, e.g.
```
# Rename the function
f.rename("my_function")
```

## Projects

### Mesh Adaptation development board

The three Mesh Adaptation codes and their documentation repo have a shared [Project board](https://github.com/orgs/mesh-adaptation/projects/2/views/1) for ongoing development work.
The project board categories are:
* **Backlog** - Issues that have been flagged but there are not currently any plans to address them.
* **Priority** - Issues that have been flagged as important and should be addressed as soon as possible.
* **Blocked** - Issues that cannot be progressed until one or more other issues are addressed.
* **In progress** - Issues for which one or more developers are currently working on addressing.
* **Ready for review** - Issues for which there is at least one open Pull Request that has reviewers assigned to it, but which hasn't been reviewed yet.
* **Reviewed with actions** - Issues for which there is at least one open Pull Request that has been reviewed and changes have been requested from the reviewer, but not yet actioned.
* **Discuss** - Issues that should be discussed at an upcoming development meeting.
* **Done** - Issues that have been addressed or are no longer valid.

Notice that all the categories refer to [Issues](#issues).
[Pull Requests](#pull-requests) should not be put on the project board, except if they do not have a linked issue. (See  [below](#issues) for special cases in which this might occur.)

### Other project board views

By default, you will see the "Overview" view of the project board. This is a somewhat overwhelming representation of all the issues associated with the development of Mesh Adaptation packages. Browse the other "views" of the project board by clicking on the different tabs for more focused representations of issues related to particular pieces of work or specific packages.

The current recommendation is to create new views for new pieces of work, rather than creating new project boards. This helps to keep everything in the same place. Views can be set up in many ways but a common approach is to filter for a particular label, e.g., `docsprint`.

## Issues

* If you identify a bug, missing feature, or other issue with one of the codes then raise an Issue for it:
  * New Issue links: <a href="https://github.com/mesh-adaptation/animate/issues/new/choose" target="_blank" rel="noopener noreferrer">Animate</a>, <a href="https://github.com/mesh-adaptation/goalie/issues/new/choose" target="_blank" rel="noopener noreferrer">Goalie</a>, <a href="https://github.com/mesh-adaptation/movement/issues/new/choose" target="_blank" rel="noopener noreferrer">Movement</a>.
* Add labels to your Issue using the right-hand panel, for example identifying whether it relates to a "bug" or an "enhancement". If you think the Issue is of high priority then use the "PRIORITY" label, too.
* Add your new Issue to the [Mesh Adaptation development project board](https://github.com/orgs/mesh-adaptation/projects/2/views/1) and set its status to "Backlog" once the Issue is created. If you used the "PRIORITY" label above then use that status here, too.
* Set the milestone of the Issue to an appropriate upcoming milestone from the drop-down menu (if there is one).
* If you are happy to take on a particular Issue, add yourself to the list of assignees using the right-hand panel.
* In general, every Pull Request should have an associated Issue. (Think of Issues like a ticketing system.) An exception is for trivial changes, such as typo fixes. In these cases, feel free to open a PR without a linked issue and add it to the project board.

## Pull Requests

When you are ready to start work on an Issue, the recommended workflow is as follows.
Note that if you are not a member of the `mesh-adaptation` organisation then you will have to fork the repository that you wish to contribute to.
1. *Command line:* Check out the `main` branch (or whichever other branch you wish to start work from the head of).
2. *Command line:* Run `git pull` to make sure you have the latest changes.
3. *Command line:* Create a new branch with `git checkout -b XX_<branch_name>`, where `XX` is the Issue number and the branch name uses [snake_case](https://en.wikipedia.org/wiki/Snake_case), e.g., `3_blind_mice`.
4. *Command line:* When you commit a change, it can be beneficial to include a link the the issue in the commit message. Do this with `#XX`, where again `XX` is the Issue number<sup>1</sup>. This will provide updates to the Issue on progress that has been made (in the next step). Note: formatting checks may cause your commits to fail on the first attempt - see [below](#formatting) for details.
5. *Command line:* If you are adding new functionality then please add new tests to check things are working properly. (See the [Testing](#testing) section for more details.)
6. *Command line:* If you haven't contributed to the package before, add yourself to the contributors list by editing the `authors` dictionary in the `pyproject.toml` file.
7. *Command line:* Once you are happy with your changes, push them to GitHub with `git push`. If this is the first time you have pushed your branch, you will need to set the remote, too, e.g., `git push -u origin 3_blind_mice`.
8. *GitHub:* The first time you push the branch, a URL hint will appear for you to open a corresponding Pull Request (PR). Use that link (or the New Pull Request button) to open a PR.
  * New PR links: <a href="https://github.com/mesh-adaptation/animate/compare" target="_blank" rel="noopener noreferrer">Animate</a>, <a href="https://github.com/mesh-adaptation/goalie/compare" target="_blank" rel="noopener noreferrer">Goalie</a>, <a href="https://github.com/mesh-adaptation/movement/compare" target="_blank" rel="noopener noreferrer">Movement</a>.
9. *GitHub:* Start the PR text box with "Closes #XX.", where `XX` is the Issue number. If your PR addresses multiple issues then replicate this for each of them.<sup>2</sup> These statements create links within GitHub such that merging the PR will close the corresponding Issues. Fill the rest of the text box with details of what the PR sets out to do and how it achieves this.
10. *GitHub:* In the right-hand panel, add yourself to the list of assignees and replicate any labels from the Issue. Do not set a Project - they are for Issues only.
11. *GitHub:* Once you have pushed any changes required to make the [Test Suite](#test-suite) pass, assign one or more reviewer(s) (if in doubt of who to assign, put [jwallwork23](https://github.com/jwallwork23)).
12. *GitHub:* See [below](#review-process) for details on the review process. If the reviewer(s) request changes, you will need to make edits to your branch and/or add comments to the PR to justify your approach. This will likely be an iterative back-and-forth process.
13. *GitHub*: Once the reviewer(s) are happy with the changes, they will approve the changes and leave you to click the green "Merge pull request" button. You will not be able to click this button until the PR has received at least one approval. It is good practice to delete your branch using the purple button once you have done this.
14. *Command line*: Remember to delete your local copy of the branch, too. This can be achieved with (for example) `git branch -D 3_blind_mice`.

Notes:

<sup>1</sup> If you wish to start a commit message with a `#` you will need to change the comment character used by Git. To do this (setting the character to `&`, say):`git config core.commentChar "&"`.

<sup>2</sup> Sometimes "Partially addresses #XX." might be more appropriate.

## Review Process

Here we assume two parties: **the developer(s)** who wrote the code in the PR and **the reviewer(s)** who have been assigned to review the PR.
We also assume that the process outlined in the [Pull Requests](#pull-requests) section above has been followed up to and including point 9.
The recommended workflow for the **reviewer(s)** to follow is as follows:
1. *GitHub*: Read through the PR description and any comments that have been left, replying if appropriate.
2. *GitHub*: Check that all tests in the test suite have passed - the latest commit should have a green tick on the right-hand side. If there is a red cross then at least one test has failed.
3. *GitHub*: Click the "Files changed" tab to view the code changes being proposed. You can leave inline comments on specific lines by clicking a specific line number or clicking and dragging over a range of line numbers. You can also leave comments on specific files using the speech bubble icon at the top of the changeset for that file.
4. *Command line* (optional, but recommended): Check out the branch associated with the PR and run tests locally, for example any that have been mentioned specifically in the PR, any new demos introduced, and/or any other cases that you might be concerned about.
5. *GitHub*: Once you are satisfied that you have made a decision about the PR, return to the "Files changed" tab and click the green "Review changes" button on the right-hand side. Choose to simply leave a comment (no conclusion yet), approve, or request changes for the PR and leave a summary comment justifying this choice.
6. *GitHub*: If you did not approve the changes then there may be an iterative back-and-forth between the developer(s) and reviewer(s). In each iteration, follow the process outlined in points 1-5 above.
7. *GitHub*: Upon approving the PR, leave the author to click the green "Merge pull request" button, as they might notice something they forgot at the last minute.

## Testing

All Mesh Adaptation codes have a subdirectory called `test` containing dedicated test suites with a mixture of [unit tests](#unit-tests) and [integration tests](#integration-tests).
Goalie also has an additional `test/adjoint` subdirectory, specifically for tests of functionality making use of adjoint methods where annotation is turned on.
All tests which do not make use of the adjoint, i.e., annotation is not turned on, should be located under the standard `test` subdirectory.

### Unit tests

The unit tests use Python's `unittest` module to check the core functionalities of the modules work as expected:
* Check functions and methods are usable.
* Check that, given some input values, functions and methods produce the expected output values.
* Check exceptions are raised in the cases where they would be expected and that they have the expected error messages.

### Integration tests

Integration tests exist to "check the code runs" for particular test cases.
The key integration tests are driven by `test/test_demos.py` (`test_adjoint/test_demos.py` in the case of Goalie) in each package, which runs all the scripts in the `demos` subdirectory.

### Formatting tests

See the [Formatting](#formatting) section below.

### Continuous integration (CI)

Whenever a developer opens a PR, pushes a commit to an open PR, or merges a PR into `main`, tests will be automatically triggered using GitHub Actions.
(On the PR, these are referred to as "checks".)
All tests will need to pass before a PR can be merged.
The tests run in GitHub Actions consist of all the tests mentioned in the subsections above.

### Code coverage

Animate, Goalie, and Movement each target 100% code coverage.
That is, we aim for every line of their source codes to be covered by tests.
This is measured using the Python `coverage` package and can be checked with the `make coverage` command from the root level.
This will run the test suite and keep track of which lines are being covered.
To view and interact with the coverage report in HTML format, open the `htmlcov/index.html` file created.

### Notes on testing

* Each package has a `<PACKAGE>_REGRESSION_TEST` environment variable, which tells the OS a regression test is currently running.
* The GitHub Actions workflows define a `GITHUB_ACTIONS_TEST_RUN` environment variable before running the tests. This can be used for debugging issues in the CI.
* If a developer pushes a commit to a PR with CI tests that are still running, those tests will be cancelled and a new batch of tests will start using the most recent branch version.
* Developers may also cancel GitHub Actions jobs if they deem it necessary, for example if a job looks like it has hung.
* The CI tests technically run the `make coverage` command rather than `make test`. The coverage report summary is printed to screen within the GitHub Actions job and this can be viewed. However, the actual coverage percentage is not checked as part of the tests at present.

## Formatting

The Mesh Adaptation codes use [Ruff](https://docs.astral.sh/ruff/formatter) for linting and formatting.
It should be automatically installed into your virtual environment when you install any of the packages with the `make install` command.
The configuration for Ruff can be found in the `pyproject.toml` files for each package, e.g., the line length limit is set to 88 characters.

### Linting

Ruff can be invoked for linting manually from the command line using
```
ruff check /path/to/code
```
This will check the code meets the formatting specifications.
It can be applied to the whole package using the `make lint` command.
The latter is also applied as part of the CI test suite and must pass in order for a PR to be merged.

### Formatting

Ruff can also be used to format the code according to specifications.
This can be achieved with
```
ruff format /path/to/code
```

### Pre-commit hook

It is highly recommended to use the pre-commit hooks that come with the packages during development.
These will be automatically installed and set up with the `make install` command.
Upon making any commit to a Mesh Adaptation module, the pre-commit hook will be invoked and will use Ruff to format the code.
If the pre-commit made any additional formatting changes then the commit will fail and you will need to retry.
In some cases, there will still be formatting issues that you will need to fix manually.

## Docker build

The [Mesh Adaptation Docs repository](https://github.com/mesh-adaptation/mesh-adaptation-docs) contains the configuration files for the bespoke Firedrake Docker image mentioned on the [installation instructions page](./Installation-instructions).
This Docker image is based off the ["Firedrake vanilla"](https://github.com/firedrakeproject/firedrake/blob/master/docker/Dockerfile.vanilla) one.
It starts from that Docker image and makes some modifications:
* Switch to the [`jwallwork23/firedrake-parmmg`](https://github.com/firedrakeproject/petsc/tree/jwallwork23/firedrake-parmmg) PETSc branch;
* Reconfigure PETSc, installing Mmg, ParMmg, and their dependencies;
* Rebuild PETSc and its other dependencies;
* Rebuild Firedrake and its other dependencies;
* Install Animate, Goalie, Movement, and their other dependencies.

The Docker image is automatically updated every Saturday night using GitHub Actions.
If this update is successful then the result is pushed to DockerHub and used for subsequent CI jobs.