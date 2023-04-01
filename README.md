# Git Homepage [![CI](https://github.com/git/git-scm.com/actions/workflows/ci.yml/badge.svg)](https://github.com/git/git-scm.com/actions/workflows/ci.yml) [![Help Contribute to Open Source](https://www.codetriage.com/git/git-scm.com/badges/users.svg)](https://www.codetriage.com/git/git-scm.com)

This is the repository for the [git-scm.com](https://git-scm.com) site.  It is meant to be the
first place a person new to Git will land and download or learn about the
Git SCM system.

This site is built with [Hugo](https://gohugo.io/) and served via GitHub Pages.

## Local development setup

It is highly recommended to clone this repository using [`scalar`](https://git-scm.com/docs/scalar); This allows to work only on the parts of the repository relevant to your interests. You can select which directories are checked out using the [`git sparse-checkout add <directory>...`](https://git-scm.com/docs/git-sparse-checkout) command. The relevant directories are:

- If you want to test any page rendering using Hugo:
  - layouts/
  - content/
  - static/
  - assets/

- To add new GUIs:
  - data/

- To work on pre-rendering pages that originate from other repositories (such as the ProGit book):
  - script/

- To work on the GitHub workflows that perform the automated, scheduled pre-rendering:
  - .github/

- The pre-rendered pages (ProGit book, its translated versions, the manual pages, their translated versions):
  - external/book/
  - external/docs/
  You will want to avoid editing these directly, as they contain pages that are pre-rendered via GitHub workflows, sourcing content from other repositories.

To render the site locally, you'll need the extended version of [Hugo](https://gohugo.io/). On Windows, we recommend using the Windows Subsystem for Linux (WSL). You can serve the site locally via

    $ hugo serve -w

The site should be running on http://127.0.0.1:1313. Note that it may be advisable to do this in a sparse checkout that excludes large parts of `content/`, to speed up the rendering time.

## Update manual pages

(TODO!)
You can do so using a local Git source clone like this:

    $ GIT_REPO=../git/.git rake local_index

This will populate the man pages for all Git versions. You can also populate them only for a specific Git version (faster):

    $ version=v2.23.0
    $ GIT_REPO=../git/.git REBUILD_DOC=$version rake local_index

Or you can populate the man pages from GitHub (much slower) like this:

    $ export GITHUB_API_TOKEN=github_personal_auth_token
    $ rake preindex  # all versions
    $ REBUILD_DOC=$version rake preindex  # specific version

Similarly, you can also populate the localized man pages. From a local clone of https://github.com/jnavila/git-html-l10n :

    $ GIT_REPO=../git-html-l10n/.git rake local_index_l10n  # all versions
    $ GIT_REPO=../git-html-l10n/.git REBUILD_DOC=$version rake local_index_l10n  # specific version

Or you can do it from GitHub (much slower) like this:

    $ export GITHUB_API_TOKEN=github_personal_auth_token
    $ rake preindex_l10n  # all versions
    $ REBUILD_DOC=$version rake preindex_l10n  # specific version

## Update the `Downloads` pages

(TODO!)
Now you need to get the latest downloads for the downloads pages:

    $ rake downloads

## Update the ProGit book

(TODO!)
Now you'll probably want some book data. You'll have
to have access to the [Pro Git project on GitHub](https://github.com/progit/progit2) through the API.

    $ export GITHUB_API_TOKEN=github_personal_auth_token
    $ rake remote_genbook2

If you have 2FA enabled, you'll need to create a [Personal Access Token](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).    

That will generate the book content from the Asciidoc files fetched from the online repository and post it to the Rails server database. You can select a specific language by indicating it in the `GENLANG` environment variable:

    $ GENLANG=zh rake remote_genbook2

Alternatively, you can get the book content from a repository on your computer by specifying the path in the `GENPATH` environment variable to the `local_genbook2` target:

    $ GENLANG=fr GENPATH=../progit2-fr rake local_genbook2

## Contributing

If you wish to contribute to this website, please [fork it on GitHub](https://github.com/git/git-scm.com).

Then, clone it using [`scalar`](https://git-scm.com/docs/scalar) (this avoids long clone times) and then use [`git sparse-checkout add <directory>`](https://git-scm.com/docs/git-sparse-checkout) to check out the files relevant to your work.

After making the changes, commit and push to a named branch in your fork, then open a pull request. If it is a big feature, you might want to [start an issue](https://github.com/git/git-scm.com/issues/new) first to make sure it's something that will be accepted.

## Adding a new GUI

The [list of GUI clients](https://git-scm.com/downloads/guis) has been constructed by the community for a long time. If you want to add another tool you'll need to follow a few steps:

1. Add a new `.md` file with the GUI client details: data/guis
    1. The fields need to be enclosed within `---` lines
    2. The fields `name`, `project_url`, `price`, `license` should be very straightforward to fill.
    3. The field `image_tag` corresponds to the path of the image of the tool (should start with `images/guis/`).
    4. `platforms` is a list of at least 1 platform in which the tool is supported. The possibilities are: `Windows`, `Mac`, `Linux`, `Android`, and `iOS`
    5. `order` can be filled with the biggest number already existing, plus 1 (this number determines the order in which the GUIs are rendered). This is the only field whose value should _not_ be enclosed in double-quote characters.
    6. `trend_name` is an optional field that can be used for helping sorting the clients.

2. Add the image to `static/images/guis/<GUI_CLIENT_NAME>@2x.png` and `static/images/guis/<GUI_CLIENT_NAME>.png` making sure the aspect ratio matches a 588:332 image.

## Useful link regarding working with Hugo

* https://gohugo.io/
* https://gohugo.io/content-management/shortcodes/

## License

The source code for the site is licensed under the MIT license, which you can find in
the MIT-LICENSE.txt file.

All graphical assets are licensed under the
[Creative Commons Attribution 3.0 Unported License](https://creativecommons.org/licenses/by/3.0/).
