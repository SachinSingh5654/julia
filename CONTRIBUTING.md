# Notes for Julia Contributors

Hi! If you are new to the Julia community: welcome, and thanks for trying Julia. Please be sure to respect our [community standards](https://julialang.org/community/standards) in all interactions.

If you are already familiar with Julia itself, this blog post by Katharine Hyatt on [Making your first Julia pull request](https://kshyatt.github.io/post/firstjuliapr/) is a great way to get started.


# Table of Contents

1. [Learning Julia](#learning-julia)
2. [Filing an issue](#filing-an-issue)
    - [Before filing an issue](#before-filing-an-issue)
    - [How to file a bug report](#how-to-file-a-bug-report)
3. [Submitting contributions](#submitting-contributions)
    - [Contributor Checklist](#contributor-checklist)
    - [Writing tests](#writing-tests)
    - [Improving documentation](#improving-documentation)
    - [Contributing to core functionality or base libraries](#contributing-to-core-functionality-or-base-libraries)
    - [Contributing to the standard library](#contributing-to-the-standard-library)
    - [Contributing to patch releases](#contributing-to-patch-releases)
    - [Code Formatting Guidelines](#code-formatting-guidelines)
    - [Git Recommendations For Pull Requests](#git-recommendations-for-pull-requests)
4. [Resources](#resources)


## Learning Julia

[The learning page](https://julialang.org/learning) has a great list of resources for new and experienced users alike.

## Filing an issue

### Before filing an issue

- Reporting a potential bug? Please read the "[How to file a bug report](https://github.com/JuliaLang/julia/blob/master/CONTRIBUTING.md#how-to-file-a-bug-report)" section to make sure that all necessary information is included.

- Contributing code? Be sure to review the [contributor checklist](https://github.com/JuliaLang/julia/blob/master/CONTRIBUTING.md#contributor-checklist) for helpful tips on the tools we use to build Julia.

- Library feature requests are generally not accepted on this issue tracker. New libraries should be developed as [packages](https://julialang.github.io/Pkg.jl/v1/creating-packages/). Discuss ideas for libraries at the [Julia Discourse forum](https://discourse.julialang.org). Doing so will often lead to pointers to existing projects and bring together collaborators with common interests.

### How to file a bug report

A useful bug report filed as a GitHub issue provides information about how to reproduce the error.

1. Before opening a new [GitHub issue](https://github.com/JuliaLang/julia/issues):
  - Try searching the existing issues or the [Julia Discourse forum](https://discourse.julialang.org) to see if someone else has already noticed the same problem.
  - Try some simple debugging techniques to help isolate the problem.
    - Try running the code with the debug build of Julia with `make debug`, which produces the `usr/bin/julia-debug`.
    - Consider running `julia-debug` with a debugger such as `gdb` or `lldb`. Obtaining even a simple [backtrace](http://www.unknownroad.com/rtfm/gdbtut/gdbsegfault.html) is very useful.
    - If Julia segfaults, try following [these debugging tips](https://docs.julialang.org/en/v1/devdocs/backtraces/) to help track down the specific origin of the bug.

2. If the problem is caused by a Julia package rather than core Julia, file a bug report with the relevant package author rather than here.

3. When filing a bug report, provide where possible:
  - The full error message, including the backtrace.
  - A minimal working example, i.e. the smallest chunk of code that triggers the error. Ideally, this should be code that can be pasted into a REPL or run from a source file. If the code is larger than (say) 50 lines, consider putting it in a [gist](https://gist.github.com).
  - The version of Julia as provided by the `versioninfo()` command. Occasionally, the longer output produced by `versioninfo(verbose = true)` may be useful also, especially if the issue is related to a specific package.

4. When pasting code blocks or output, put triple backquotes (\`\`\`) around the text so GitHub will format it nicely. Code statements should be surrounded by single backquotes (\`). Be aware that the `@` sign tags users on GitHub, so references to macros should always be in single backquotes. See [GitHub's guide on Markdown](https://guides.github.com/features/mastering-markdown) for more formatting tricks.

## Submitting contributions

### Contributor Checklist

* Create a [GitHub account](https://github.com/signup/free).

* [Fork Julia](https://github.com/JuliaLang/julia/fork).

* Build the software and libraries (the first time takes a while, but it's fast after that). Detailed build instructions are in the [README](https://github.com/JuliaLang/julia/tree/master/README.md). Julia depends on several external packages; most are automatically downloaded and installed, but are less frequently updated than Julia itself.

* Keep Julia current. Julia is a fast-moving target, and many details of the language are still settling out. Keep the repository up-to-date and rebase work-in-progress frequently to make merges simpler.

* Learn to use [git](https://git-scm.com), the version control system used by GitHub and the Julia project. Try a tutorial such as the one [provided by GitHub](https://try.GitHub.io/levels/1/challenges/1).

* Review discussions on the [Julia Discourse forum](https://discourse.julialang.org).

* For more detailed tips, read the [submission guide](https://github.com/JuliaLang/julia/blob/master/CONTRIBUTING.md#submitting-contributions) below.

* Relax and have fun!

### Writing tests

There are never enough tests. Track [code coverage at Codecov](https://codecov.io/github/JuliaLang/julia), and help improve it.

1. Go visit https://codecov.io/github/JuliaLang/julia.

2. Browse through the source files and find some untested functionality (highlighted in red) that you think you might be able to write a test for.

3. Write a test that exercises this functionality---you can add your test to one of the existing files, or start a new one, whichever seems most appropriate to you. If you're adding a new test file, make sure you include it in the list of tests in `test/choosetests.jl`. https://docs.julialang.org/en/v1/stdlib/Test/ may be helpful in explaining how the testing infrastructure works.

4. Run `make test-all` to rebuild Julia and run your new test(s). If you had to fix a bug or add functionality in `base`, this will ensure that your test passes and that you have not introduced extraneous whitespace.

5. Submit the test as a pull request (PR).

* Code for the buildbot configuration is maintained at: https://github.com/staticfloat/julia-buildbot
* You can see the current buildbot setup at: https://build.julialang.org/builders
* [Issue 9493](https://github.com/JuliaLang/julia/issues/9493) and [issue 11885](https://github.com/JuliaLang/julia/issues/11885) have more detailed discussion on code coverage.

Code coverage shows functionality that still needs "proof of concept" tests. These are important, as are tests for tricky edge cases, such as converting between integer types when the number to convert is near the maximum of the range of one of the integer types. Even if a function already has some coverage on Codecov, it may still benefit from tests for edge cases.

### Improving documentation

*By contributing documentation to Julia, you are agreeing to release it under the [MIT License](https://github.com/JuliaLang/julia/tree/master/LICENSE.md).*

Julia's documentation source files are stored in the `doc/` directory and all docstrings are found in `base/`. Like everything else these can be modified using `git`. Documentation is built with [Documenter.jl](https://github.com/JuliaDocs/Documenter.jl), which uses Markdown syntax. The HTML documentation can be built locally by running

```
make docs
```

from Julia's root directory. This will rebuild the Julia system image, then install or update the package dependencies required to build the documentation, and finally build the HTML documentation and place the resulting files in `doc/_build/html/`.

> **Note**
>
> When making changes to any of Julia's documentation it is recommended that you run `make docs` to check that your changes are valid and do not produce any errors before opening a pull request.

Below are outlined the three most common types of documentation changes and the steps required to perform them. Please note that the following instructions do not cover the full range of features provided by Documenter.jl. Refer to [Documenter's documentation](https://juliadocs.github.io/Documenter.jl/stable) if you encounter anything that is not covered by the sections below.

#### Modifying files in `doc/src/`

Most of the source text for the Julia Manual is located in `doc/src/`. To update or add new text to any one of the existing files the following steps should be followed:

1. update the text in whichever `.md` files are applicable;
2. run `make docs` from the root directory;
3. check the output in `doc/_build/html/` to make sure the changes are correct;
4. commit your changes and open a pull request.

> **Note**
>
> The contents of `doc/_build/` does **not** need to be committed when you make changes.

To add a **new file** to `doc/src/` rather than updating a file replace step `1` above with

1. add the file to the appropriate subdirectory in `doc/src/` and also add the file path to the `PAGES` vector in `doc/make.jl`.

#### Modifying an existing docstring in `base/`

All docstrings are written inline above the methods or types they are associated with and can be found by clicking on the `source` link that appears below each docstring in the HTML file. The steps needed to make a change to an existing docstring are listed below:

1. find the docstring in `base/`;
2. update the text in the docstring;
3. run `make docs` from the root directory;
4. check the output in `doc/_build/html/` to make sure the changes are correct;
5. commit your changes and open a pull request.

#### Adding a new docstring to `base/`

The steps required to add a new docstring are listed below:

1. find a suitable definition in `base/` that the docstring will be most applicable to;
2. add a docstring above the definition;
3. find a suitable `@docs` code block in one of the `doc/src/stdlib/` files where you would like the docstring to appear;
4. add the name of the definition to the `@docs` code block. For example, with a docstring added to a function `bar`

    ```julia
    "..."
    function bar(args...)
        # ...
    end
    ```

   you would add the name `bar` to a `@docs` block in `doc/src/stdlib/`

        ```@docs
        foo
        bar # <-- Added this one.
        baz
        ```

5. run `make docs` from the root directory;
6. check the output in `doc/_build/html` to make sure the changes are correct;
7. commit your changes and open a pull request.

#### Doctests

Examples written within docstrings can be used as testcases known as "doctests" by annotating code blocks with `jldoctest`.

    ```jldoctest
    julia> uppercase("Docstring test")
    "DOCSTRING TEST"
    ```

A doctest needs to match an interactive REPL including the `julia>` prompt. It is recommended to add the header `# Examples` above the doctests.

To run doctests you need to run `make -C doc doctest=true` from the root directory. You can use `make -C doc doctest=true revise=true` if you are modifying the doctests and don't want to rebuild Julia after each change (see details below about the Revise.jl workflow).

#### News-worthy changes

For new functionality and other substantial changes, add a brief summary to `NEWS.md`. The news item should cross reference the pull request (PR) parenthetically, in the form `([#pr])`. To add the PR reference number, first create the PR, then push an additional commit updating `NEWS.md` with the PR reference number. We periodically run `./julia doc/NEWS-update.jl` from the julia directory to update the cross-reference links, but this should not be done in a typical PR in order to avoid conflicting commits.

#### Annotations for new features, deprecations and behavior changes

API additions and deprecations, and minor behavior changes are allowed in minor version releases.
For documented features that are part of the public API, a compatibility note should be added into
the manual or the docstring. It should state the Julia minor version that changed the behavior
and have a brief message describing the change.

At the moment, this should always be done with the following `compat` admonition
(so that it would be possible to programmatically find the annotations in the future):

  ```
  !!! compat "Julia 1.X"
      This method was added in Julia 1.X.
  ```

### Contributing to core functionality or base libraries

*By contributing code to Julia, you are agreeing to release it under the [MIT License](https://github.com/JuliaLang/julia/tree/master/LICENSE.md).*

The Julia community uses [GitHub issues](https://github.com/JuliaLang/julia/issues) to track and discuss problems, feature requests, and pull requests (PR).

Issues and pull requests should have self explanatory titles such that they can be understood from the list of PRs and Issues.
i.e. `Add {feature}` and `Fix {bug}` are good, `Fix #12345. Corrects the bug.` is bad.

You can make pull requests for incomplete features to get code review. The convention is to open these as draft PRs and prefix
the pull request title with "WIP:" for Work In Progress, or "RFC:" for Request for Comments when work is completed and ready
for merging. This will prevent accidental merging of work that is in progress.

Note: These instructions are for adding to or improving functionality in the base library. Before getting started, it can be helpful to discuss the proposed changes or additions on the [Julia Discourse forum](https://discourse.julialang.org) or in a GitHub issue---it's possible your proposed change belongs in a package rather than the core language. Also, keep in mind that changing stuff in the base can potentially break a lot of things. Finally, because of the time required to build Julia, note that it's usually faster to develop your code in stand-alone files, get it working, and then migrate it into the base libraries.

Add new code to Julia's base libraries as follows (this is the "basic" approach; see a more efficient approach in the next section):

 1. Edit the appropriate file in the `base/` directory, or add new files if necessary. Create tests for your functionality and add them to files in the `test/` directory. If you're editing C or Scheme code, most likely it lives in `src/` or one of its subdirectories, although some aspects of Julia's REPL initialization live in `cli/`.

 2. Add any new files to `sysimg.jl` in order to build them into the Julia system image.

 3. Add any necessary export symbols in `exports.jl`.

 4. Include your tests in `test/Makefile` and `test/choosetests.jl`.

Build as usual, and do `make clean testall` to test your contribution. If your contribution includes changes to Makefiles or external dependencies, make sure you can build Julia from a clean tree using `git clean -fdx` or equivalent (be careful – this command will delete any files lying around that aren't checked into git).

#### Running specific tests

There are `make` targets for running specific tests:

    make test-bitarray

You can also use the `runtests.jl` script, e.g. to run `test/bitarray.jl` and `test/math.jl`:

    ./usr/bin/julia test/runtests.jl bitarray math

#### Modifying base more efficiently with Revise.jl

[Revise](https://github.com/timholy/Revise.jl) is a package that
tracks changes in source files and automatically updates function
definitions in your running Julia session. Using it, you can make
extensive changes to Base without needing to rebuild in order to test
your changes.

Here is the standard procedure:

1. If you are planning changes to any types or macros, make those
   changes and build julia using `make`. (This is
   necessary because `Revise` cannot handle changes to type
   definitions or macros.) Unless it's
   required to get Julia to build, you do not have to add any
   functionality based on the new types, just the type definitions
   themselves.

2. Start a Julia REPL session. Then issue the following commands:

```julia
using Revise    # if you aren't launching it in your `.julia/config/startup.jl`
Revise.track(Base)
```

3. Edit files in `base/`, save your edits, and test the
   functionality.

If you need to restart your Julia session, just start at step 2 above.
`Revise.track(Base)` will note any changes from when Julia was last
built and incorporate them automatically. You only need to rebuild
Julia if you made code-changes that Revise cannot handle.

For convenience, there are also `test-revise-*` targets for every [`test-*`
target](https://github.com/JuliaLang/julia/blob/master/CONTRIBUTING.md#running-specific-tests) that use Revise to load any modifications to Base into the current
system image before running the corresponding test. This can be useful as a shortcut
on the command line (since tests aren't always designed to be run outside the
runtest harness).

### Contributing to the standard library

The standard library (stdlib) packages are baked into the Julia system image.
When running the ordinary test workflow on the stdlib packages, the system image
version overrides the version you are developing.
To test stdlib packages, you can do the following steps:

1. Edit the UUID field of the `Project.toml` in the stdlib package
2. Change the current directory to the directory of the stdlib you are developing
3. Start julia with `julia --project=.`
4. You can now test the package by running `pkg> test` in Pkg mode.

Because you changed the UUID, the package manager treats the stdlib package as
different from the one in the system image, and the system image version will
not override the package.

Be sure to change the UUID value back before making the pull request.

### Contributing to patch releases

The process of [creating a patch release](https://docs.julialang.org/en/v1/devdocs/build/distributing/#Point-releasing-101) is roughly as follows:

1. Create a new branch (e.g. `backports-release-1.10`) against the relevant minor release
   branch (e.g. `release-1.10`). Usually a corresponding pull request is created as well.

2. Add commits, nominally from `master` (hence "backports"), to that branch.
   See below for more information on this process.

3. Run the [BaseBenchmarks.jl](https://github.com/JuliaCI/BaseBenchmarks.jl) benchmark
   suite and [PkgEval.jl](https://github.com/JuliaCI/PkgEval.jl) package ecosystem
   exerciser against that branch. Nominally BaseBenchmarks.jl and PkgEval.jl are
   invoked via [Nanosoldier.jl](https://github.com/JuliaCI/Nanosoldier.jl) from
   the pull request associated with the backports branch. Fix any issues.

4. Once all test and benchmark reports look good, merge the backports branch into
   the corresponding release branch (e.g. merge `backports-release-1.10` into
   `release-1.10`).

5. Open a pull request that bumps the version of the relevant minor release to the
   next patch version, e.g. as in [this pull request](https://github.com/JuliaLang/julia/pull/37718).

6. Ping `@JuliaLang/releases` to tag the patch release and update the website.

7. Open a pull request that bumps the version of the relevant minor release to the
   next prerelease patch version, e.g. as in [this pull request](https://github.com/JuliaLang/julia/pull/37724).

Step 2 above, i.e. backporting commits to the `backports-release-X.Y` branch, has largely
been automated via [`Backporter`](https://github.com/KristofferC/Backporter): Backporter
searches for merged pull requests with the relevant `backport-X.Y` tag, and attempts to
cherry-pick the commits from those pull requests onto the `backports-release-X.Y` branch.
Some commits apply successfully without intervention, others not so much. The latter
commits require "manual" backporting, with which help is generally much appreciated.
Backporter generates a report identifying those commits it managed to backport automatically
and those that require manual backporting; this report is usually copied into the first
post of the pull request associated with `backports-release-X.Y` and maintained as
additional commits are automatically and/or manually backported.

When contributing a manual backport, if you have the necessary permissions, please push the
backport directly to the `backports-release-X.Y` branch. If you lack the relevant
permissions, please open a pull request against the `backports-release-X.Y` branch with the
manual backport. Once the manual backport is live on the `backports-release-X.Y` branch,
please remove the `backport-X.Y` tag from the originating pull request for the commits.

### Code Formatting Guidelines

#### General Formatting Guidelines for Julia code contributions

 - Follow the latest dev version of [Julia Style Guide](https://docs.julialang.org/en/v1/manual/style-guide/).
 - use whitespace to make the code more readable
 - no whitespace at the end of a line (trailing whitespace)
 - comments are good, especially when they explain the algorithm
 - try to adhere to a 92 character line length limit
 - it is generally preferred to use ASCII operators and identifiers over
   Unicode equivalents whenever possible
 - in docstrings refer to the language as "Julia" and the executable as "`julia`"

#### General Formatting Guidelines For C code contributions

 - 4 spaces per indentation level, no tabs
 - space between `if` and `(` (`if (x) ...`)
 - newline before opening `{` in function definitions
 - `f(void)` for 0-argument function declarations
 - newline between `}` and `else` instead of `} else {`
 - if one part of an `if..else` chain uses `{ }` then all should
 - no whitespace at the end of a line

### Git Recommendations For Pull Requests

 - Avoid working from the `master` branch of your fork. Create a new branch as it will make it easier to update your pull request if Julia's `master` changes.
 - Try to [squash](https://gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html) together small commits that make repeated changes to the same section of code, so your pull request is easier to review. A reasonable number of separate well-factored commits is fine, especially for larger changes.
 - If any conflicts arise due to changes in Julia's `master`, prefer updating your pull request branch with `git rebase` versus `git merge` or `git pull`, since the latter will introduce merge commits that clutter the git history with noise that makes your changes more difficult to review.
 - Descriptive commit messages are good.
 - Using `git add -p` or `git add -i` can be useful to avoid accidentally committing unrelated changes.
 - When linking to specific lines of code in discussion of an issue or pull request, hit the `y` key while viewing code on GitHub to reload the page with a URL that includes the specific version that you're viewing. That way any lines of code that you refer to will still make sense in the future, even if the content of the file changes.
 - Whitespace can be automatically removed from existing commits with `git rebase`.
   - To remove whitespace for the previous commit, run
     `git rebase --whitespace=fix HEAD~1`.
   - To remove whitespace relative to the `master` branch, run
     `git rebase --whitespace=fix master`.

#### Git Recommendations For Pull Request Reviewers

- When merging, we generally like `squash+merge`. Unless it is the rare case of a PR with carefully staged individual commits that you want in the history separately, in which case `merge` is acceptable, but usually prefer `squash+merge`.


## Resources

* Julia
  - **Homepage:** <https://julialang.org>
  - **Community:** <https://julialang.org/community/>
  - **Source code:** <https://github.com/JuliaLang/julia>
  - **Documentation:** <https://docs.julialang.org>
  - **Code coverage:** <https://codecov.io/github/JuliaLang/julia>

* Design of Julia
  - [Julia: A Fresh Approach to Numerical Computing](https://julialang.org/assets/research/julia-fresh-approach-BEKS.pdf)
  - [Julia: Dynamism and Performance Reconciled by Design](http://janvitek.org/pubs/oopsla18b.pdf)
  - [All Julia Publications](https://julialang.org/research)

* Using GitHub
  - [Using Julia with GitHub (video)](https://www.youtube.com/watch?v=wnFYV3ZKtOg)
  - [Using Julia on GitHub (notes for video)](https://gist.github.com/2712118#file_Julia_git_pull_request.md)
  - [General GitHub documentation](https://help.github.com)
  - [GitHub pull request documentation](https://help.github.com/articles/creating-a-pull-request/)
