# Contributing to Nion Swift

If you are a Nion Swift user or developer, there are many ways to contribute to the project, from reporting issues to submitting code.

Please read the section below before you start, particularly if you are considering a code contribution.

## Before You Contribute

Nion Swift is specialized scientific software, originally developed for the operation of Nion electron microscopes and used to visualize, process, and analyze scientific data. The correctness of most changes — particularly anything touching the user interface, data model, file format, or acquisition workflow — depends on understanding how the application is actually used in practice. A change that looks reasonable in isolation is frequently wrong in ways that are only apparent to someone who works with the software regularly.

For that reason, **we strongly prefer code contributions from people who use Nion Swift.** If you are not a user, the most valuable things you can contribute are clear bug reports and well-reasoned feature requests (see below). Please do not open code pull requests for software you do not use.

### AI-Assisted Contributions

Using AI tools while you work is fine. What we ask is that a contribution is genuinely driven by you and not generated start to finish. You should understand the change well enough to explain and defend every line of it, answer questions about it during review, and support it afterward. The bar is your own understanding and accountability, not whether a tool was involved along the way.

If you did use AI assistance on a change, we'd appreciate a brief note saying so in the pull request.

Pull requests whose author cannot explain the change, or that were clearly produced without an understanding of the software, will be closed.

## Reporting Issues and Feature Requests

- [File issues for bugs or feature requests](https://github.com/nion-software/nionswift/issues)
- Comment on existing issues with pertinent information
- Isolate problems with UI, features, or performance and write tests
- Contribute source code

If you are submitting an issue, please try to answer these common questions.

- Is your issue focused on one particular thing? Please avoid listing multiple requests in a single issue unless they are fundamentally related.
- Have you searched the issues (open and closed) for the same or related issues? Often the issue or feature you're reporting will already be entered.
- What version of Swift are you running? You should try to reproduce the bug on the latest released Swift version if possible.
- How has Swift been installed? From drag and drop? PyPI? Conda?
- What is the underlying Python environment? What version of Python?
- What packages is Swift running? For instance, are you running acquisition?
- How can the issue be reproduced? Please provide as much information as possible for us to reproduce the issue.
- Can this be reproduced on a new minimal project file?

For performance issues.

- What are the specs of the machine (CPU/memory/storage-type)?
- Is there a particular action that seems particularly sluggish?
- Is it always sluggish (i.e. immediately after launch) or does it slowly get worse?

## Feature Requests

If you have an idea for a new feature, start by searching the [issue tracker](https://github.com/nion-software/nionswift/issues) (both open and closed issues) for similar requests.

Explain the rationale for the feature you're requesting. Why would this feature be useful? Consider also any possible drawbacks, including backwards compatibility, new library dependencies, and performance issues.

We recommend that you discuss a potential new feature on the [issue tracker](https://github.com/nion-software/nionswift/issues) or [Nion Swift discussions](https://github.com/nion-software/nionswift/discussions) before starting work to make sure it fits cleanly into the roadmap.

If entered into the issue tracker, limit the issue to one specific feature, not a list of associated features.

If the feature request is a larger request involving multiple individual features and capabilities, consider writing a specification document and posting it on [Nion Swift discussions](https://github.com/nion-software/nionswift/discussions) so it can be referenced from the relevant issues.

## Triaging and Prioritization

We will periodically review new and open issues, adding labels, milestones, and assignees to the issue. In addition we will prioritize issues according to the following criteria:

* Prioritize urgent issues first, then important. Prioritize issues with dependencies.
* Prioritize by the value of the issue, as measured by the criteria below or by the number of
  users affected by the issue.
* Everything else equal, prioritize lengthier tasks first, but be flexible.

We will attempt to assign value to an issue based on various criteria, including:

* Addresses data loss or integrity (urgent)
* Time savings for regular users
* Overall ease of use for regular users
* Addresses performance issue
* Streamlines repetitive tasks
* Simplifies training
* Reduces support issues
* Improves demo or initial impression
* Introduces a new capability
* Time savings for installation
* Customer request
* Code vulnerability

## Development Setup

To work on Nion Swift from source, clone the repository and install it into a virtual environment as an editable install:

```sh
git clone https://github.com/nion-software/nionswift.git
cd nionswift
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
python -m pip install --upgrade pip
pip install -e .
pip install -r test-requirements.txt
```

Nion Swift is developed alongside several sibling packages (`nionutils`, `nionui`, `niondata`). `test-requirements.txt` installs the development (git `main`) versions of those packages, so install it *after* the editable install to make sure you are building and testing against current upstream. If you are also changing one of the sibling packages, install that one as an editable checkout too.

Nion Swift targets Python 3.12 and later.

### Running Tests and Type Checking

Before opening a pull request, run the type checker and the test suite locally. Both must pass; CI will run the same checks.

```sh
mypy
python -m pytest
```

`mypy` is configured (see `mypy.ini`) for strict checking of the `nion.swift` package, so run it with no arguments. CI runs these checks on Python 3.12, 3.13, and 3.14 across Linux, macOS, and Windows.

## Contributing Source Code

Patches and pull requests are welcome. Before you put time into a nontrivial patch, it is a good idea to discuss it on [GitHub Issues](https://github.com/nion-software/nionswift/issues), especially if it is for a new feature (rather than fixing a bug). Discussing first also confirms that the change fits the roadmap before you invest effort.

If you are contributing source code, please be aware of the following decision points.

- Can you implement your custom code using the [Nion Swift public API](https://nionswift.readthedocs.io/en/latest/scripting.html)? If so, this is the good way to have custom code that will be compatible with future versions.
- Does your custom code belong in Nion Swift or one of its extension packages? For instance, almost all acquisition related code would go into a specific extension such as `nionswift-instrumentation-kit` or even something more specific.
- Does your custom code require the internal Nion Swift API? If so, this will be difficult to maintain over time. You should [file issues](https://github.com/nion-software/nionswift/issues) requesting public API to be extended to accommodate your custom code.
- Does your custom code require internal access to Nion Swift and is it something that would appeal to a wide range of users? If so, you should get your code into a well documented state and create a pull request to have it integrated into the base project.
- Are you willing to agree to the licensing terms for contributions? User facing code is typically released on GPLv3 and library/processing code is typically released on Apache license. However, Nion reserves the right to make licensing changes to ensure the longer term survival of the project.

## Pull Request Guidelines

If you are submitting a pull request, you can expect the following.

- You should understand and be able to explain every line of your change. See [AI-Assisted Contributions](#ai-assisted-contributions) above — we will ask, and we will close pull requests the author cannot account for.
- The Nion Swift code base has very specific code formatting and documentation requirements. At a minimum, your code should be well typed and pass `mypy` verification tool if they are enabled for the files you are changing.
- The code base has purposely limited upstream requirements (e.g. numpy, scipy) and your code should typically not depend on any Python package not already included. This can be discussed on a case by case basis, but the bar is high for inclusion of a new dependency.
- Any code that affects the file format in any way will receive extra strict scrutiny and be evaluated as to its fitness as a long term change. This includes any new objects that get written into the file system. Backwards and forwards compatibility are of the utmost importance.
- Forward compatibility includes features, architecture, objects, file formats, protocols, designs, etc. All are expected to be available for several years. Your code will be evaluated with that perspective in mind.
- Your code must pass existing regression tests on all supported platforms and configurations with minimal modifications to existing tests and must include reasonable test cases for its own new behavior. The continuous integration (CI) tests will automatically test this for any pull requests (PRs).
- Your code must pass existing strict typing as determined by `mypy`. The typing should be as specific as possible (i.e. limited use of `typing.Any`).
- If your code is rejected, we may suggest expanding the public API so that your code can be accommodated as an extension package.
- Each commit should focus on one specific change and should not include extraneous formatting changes. Commit messages should follow the standard Git commit format (one line up to approx. 120 characters, followed by a blank line, followed by an explanation split into lines of 120 characters or longer). Do not reference specific issue or pull request numbers in commit messages, since those can change and are not relevant to the code change itself; describe the change instead (e.g. "Fix issue with high DPI displays"). Reference: [Write Better Commits, Build Better Projects](https://github.blog/2022-06-30-write-better-commits-build-better-projects/)
- Write commit messages in the imperative present tense ("Fix issue with…", not "Fixed…"), and do not use conventional-commit type prefixes such as `feat:` or `fix:`.
- Add tests for your code. Tests for a bug fix should fail without your code and succeed with your code. Tests for features should cover basic behavior and highlight how the code is expected to work. We generally write tests when things fail; but including some initial tests is also useful.
- Update the user guide if your change affects user interface that is already explained in the user guide.
- Follow the stylistic conventions you find in the existing code. Use spaces, not tabs. Use Unix line endings (LF).
- Be sure to trim whitespace from PR's.

## Coding Style

[Good code](https://intenthq.com/writing/what-is-good-code-a-scientific-definition) is written so that is readable, understandable, covered by automated tests, not over complicated and does well what is intended to do.

- Follow the coding style of the existing code as much as possible
- Write comments to explain why, not how (let the code do that).
- Avoid reformatting code just for the sake of reformatting; it makes review and diff difficult.
- Review changes carefully before committing.
- Write good commit messages ([guide1](https://www.freecodecamp.org/news/writing-good-commit-messages-a-practical-guide/), [guide2](https://www.toolsqa.com/git/writing-good-commit-messages/)).
- Always commit with Unix-style line endings.
- Avoid using abbreviations for variables or function names. [Taming Names](https://www.simplethread.com/taming-names-in-software-development/)
- Try to write functions and methods with zero side effects.
- Integration tests are useful to verify behavior, but also to notice changes in behavior.
- Write tests by exception, i.e. add test when something fails unexpectedly.
- Make instance variables private when possible.
- Avoid using global variables.
- Use double quotes and `gettext` or `_` for UI strings.
- Use get/set properties when there are few side effects of setting property.
- Name methods with an action, not a noun; name properties with a noun.
- Name events `<something>_event` with a matching `handle_<something>` callback. Reserve `on_<something>` for direct callbacks where only a single client can handle the event, and comment on why a direct callback is used.
- A single leading underscore marks a name as protected (internal to the module); a double leading underscore marks it private to the class. Do not reach past either from outside.
- Use modern typing idioms: `X | None` rather than `typing.Optional`, the built-in `list`/`dict` for generics rather than `typing.List`/`typing.Dict`, and prefer `typing.Sequence`/`typing.Mapping` over concrete `list`/`dict` in public signatures.
- Use fully qualified imports (import the module, not individual symbols). Keep imports sorted and grouped into standard library, third-party, and local sections; anything under `nion` is local.
- Prefer a frozen `dataclass` over `NamedTuple` for immutable value types.
- Give new public functions and classes docstrings that describe purpose, parameters, and return value rather than implementation details.
- Avoid using explicit reference counting or close methods when possible. Explore how finalize is used in existing library.
- Write close methods to minimize errors:
  - When an object is closed, null it out so it isn't accidentally re-used.
  - Write tests to make sure anything using an object with a close method calls close.
  - Close methods should set all variables to None for garbage collection.
  - Canvas items will be closed if their parent is closed.
