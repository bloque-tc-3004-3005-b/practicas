## Contributing

Before you start to code, please create traceability for a use case in [Tool used by the team to manage project](), open a [new issue]() to describe a bug found, or search for and continue the discussion in an [existing issue]().

Please completely fill out any templates to provide essential information about your new feature or the bug you discovered or reported.

When you are ready to code, you can find more information about opening a pull request in the [GitHub docs](https://help.github.com/articles/creating-a-pull-request/).

Whether this is your first contribution or you are already an experienced contributor, your team has your back – don't hesitate to ask for help!

### Issue vs. Pull Request

An issue will never substitute a former card in Favro, an issue is only to address bugs recognized and not solved within the app.

An issue is required to be linked in every pull request. We understand that no-one likes to create an issue for something that appears to be a simple pull request, but here is why this is beneficial for everyone:

- An issue get more visibility than a pull request as issues can be pinned and it is primarily the issue list that people browse through rather than the more technical pull request list. Visibility is a key aspect so others can weigh in on issues and contribute their opinion.
- The discussion in the issue is different from the discussion in the pull request. The issue discussion is focused on the issue and how to address it, whereas the discussion in the pull request is focused on a specific implemention. An issue may even have multiple pull requests because either the issue requires multiple implementations or multiple pull requests are opened to compare and test different approaches to later decide for one.
- High-level conceptual discussions about the issue should be still available, even if a pull request is closed because its appraoch was discarded. If these discussions are in the pull request instead, they can easily become fragmented over multiple pull requests and issues, which can make it very hard to make sense of all aspects of an issue.

## Environment Setup

### Recommended Tools

- [Tool used by the team to develop]().

### Setting up your local machine


```sh
$ git clone {{your url repository}}
$ cd my-repository-folder # go into the clone directory
```

Open Development tootl, navigate to clone directory **my-repository-folder** and select it.

### Good to Know

- Things that are good to know or commands needed in order to run the project.

## Breaking Changes

### Deprecation Policy

If you change or remove an existing feature that would lead to a breaking change, use the following deprecation pattern:

- Make the new feature or change optional, if necessary with a new Scheduling option parameter.
- Use a default value that falls back to existing behavior.
- Add the @deprecated tag for the comment of the function, for example:
  > DeprecationWarning: The System App option 'example' will be removed in a future release.

Deprecations become breaking changes after notifying developers through deprecation warnings for at least one entire previous major release. For example:

- `4.5.0` is the current version
- `4.6.0` adds a new optional feature and a deprecation warning for the existing feature
- `5.0.0` marks the beginning of logging the deprecation warning for one entire major release
- `6.0.0` makes the breaking change by removing the deprecation warning and making the new feature replace the existing feature

See the [Deprecation Plan](DEPRECATIONS.md) for an overview of deprecations and planned breaking changes.

## Naming Branches

### Create a new branch

The title of a branch needs to be written in a defined syntax. We loosely follow the [Git Naming Convention](https://tilburgsciencehub.com/building-blocks/collaborate-and-share-your-work/use-github/naming-git-branches/)

1. Use separators: When writing a branch name, using slash (/) separators to increase readability of the name
2. Start name with category word: It is recommended to begin the name of a branch with a category word, which indicates the type of task that is being solved with that branch. Some of the most used category words are:
   - `hotfix` - for quickly fixing critical issues, usually with a temporary solution
   - `bugfix` - for fixing a bug
   - `feature` - for adding, removing or modifying a feature
   - `test` - for experimenting something which is not an issue
   - `wip` - for a work in progress
3. Use the {Release version - ID} of the Favro Card or Issue addressing: Using the ID in the branch name makes it easy to identify the task and keep track of its progress. `X.Y.Z-alpha.ID`
4. Avoid Using Numbers Only: It’s not a good practice to name a branch by only using numbers, because it creates confusion and increases chances of making mistakes. Instead, combine ID of issues with key words for the respective task.
5. Avoid Long Branch Names: As much as the branch name needs to be informative, it also needs to be precise and short. Detailed and long names can affect readability and efficiency.

```
<category>/<X.Y.Z-alpha.ID>/<title>
```

## Pull Request

### Commit Message

The title of pull requests needs to be written in a defined syntax. We loosely follow the [Conventional Commits](https://www.conventionalcommits.org) specification, which defines this syntax:

```
<type>: <summary>
```

The _type_ is the category of change that is made, possible types are:

- `feat` - add a new feature or improve an existing feature
- `fix` - fix a bug
- `refactor` - refactor code without impact on features or performance
- `docs` - add or edit code comments, documentation, GitHub pages
- `style` - edit code style
- `build` - retry failing build and anything build process related
- `perf` - performance optimization
- `ci` - continuous integration
- `test` - tests

The _summary_ is a short change description in present tense, not capitalized, without period at the end. This summary will also be used as the changelog entry.

- It must be short and self-explanatory for a reader who does not see the details of the full pull request description
- It must not contain abbreviations, e.g. instead of `LQ` write `LiveQuery`
- It must use the correct product and feature names as referenced in the documentation, e.g. instead of `Cloud Validator` use `Cloud Function validation`

For example:

```
feat: add handle to door for easy opening
```

Github actions:

In addition, commit messages can be used to trigger specific github actions. We currently support the following:

- `#apk` - builds a signed APK that can be distributed for testing

Currently, we are not making use of the commit _scope_, which would be written as `<type>(<scope>): <summary>`, that attributes a change to a specific part of the product.

### Breaking Change

If a pull request contains a braking change, the description of the pull request must contain a dedicated chapter at the bottom to indicate this. This is to assist the committer of the pull request to avoid merging a breaking change as non-breaking.

## Merging

The following guide is for anyone who merges a contributor pull request into the working branch, the working branch into a release branch, a release branch into another release branch, or any other direct commits such as hotfixes into release branches or the working branch.

- A contributor pull request must be merged into the working branch using `Squash and Merge`, to create a single commit message that describes the change.
- A release branch or the default branch must be merged into another release branch using `Merge Commit`, to preserve each individual commit message that describes its respective change.

## Versioning

System follows [semantic versioning](https://semver.org) with a flavor of [calendric versioning](https://calver.org). Semantic versioning makes System easy to upgrade because breaking changes only occur in major releases. Calendric versioning gives an additional sense of how old a System release is and allows for Long-Term Support of previous major releases.

Example version: `5.0.0-alpha.1`

Syntax: `[major]`**.**`[minor]`**.**`[patch]`**-**`[pre-release-label]`**.**`[pre-release-increment]`

- The `major` version increments with the first release of every year and may include changes that are _not backwards compatible_.
- The `minor` version increments during the year and may include new features or improvements of existing features that are backwards compatible.
- The `patch` version increments during the year and may include bug fixes that are backwards compatible.
- The `pre-release-label` is optional for pre-release versions such as:
  - `-alpha` (likely to contain bugs, likely to change in features until release)
  - `-beta` (likely to contain bugs, no change in features until release)
- The `[pre-release-increment]` is an ID of the current Favro or Issue.

Exceptions:

- The `major` version may increment during the year in the unlikely event that a breaking change is so urgent that it cannot wait for the next yearly release. An example would be a vulnerability fix that leads to an unavoidable breaking change. However, security requirements depend on the application and not every vulnerability may affect every deployment, depending on the features used. Therefore we usually prefer to deprecate insecure functionality and introduce the breaking change following our [deprecation policy](#deprecation-policy).