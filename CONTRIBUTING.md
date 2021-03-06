Camlistore contributors regularly use Linux and OS X, and both are
100% supported.

Developing on Windows is sometimes broken, but should work.  Let us
know if we broke something, or we accidentally depend on some
Unix-specific build tool somewhere.

See https://camlistore.org/doc/contributing for information on how to
contribute to the project and submit patches.  Notably, we use Gerrit
for code review.  Our Gerrit instance is at https://camlistore.org/r/

See architecture docs: https://camlistore.org/doc/

You can view docs for Camlistore packages with local godoc, or
godoc.org.

It's recommended you use git to fetch the source code, rather than
hack from a Camlistore release's zip file:

    $ git clone https://camlistore.googlesource.com/camlistore

(We use github for distribution but its code review system is so poor,
we don't use its Pull Request mechanism. The Gerrit git server & code
review system is the main repo. See
https://camlistore.org/doc/contributing for how to use them.  We might
support github for pull requests in the future, once it's properly
integrated with external code review tools. We had a meeting with Github
to discuss the ways in which their code review tools are poor.)

On Debian/Ubuntu, some deps to get started:

    $ sudo apt-get install libsqlite3-dev sqlite3 pkg-config git

During development, rather than use the main binaries ("camput",
"camget", "camtool", "cammount", etc) directly, we instead use a
wrapper (devcam) that automatically configure the environment to use
the test server & test environment.

To build devcam:

    $ go run make.go

And devcam will be in &lt;camroot&gt;/bin/devcam.  You'll probably want to
symlink it into your $PATH.

Alternatively, if your Camlistore root is checked out at
$GOPATH/src/camlistore.org (optional, but natural for Go users), you
can just:

    $ go install ./dev/devcam

The subcommands of devcam start the server or run camput/camget/etc:

    $ devcam server      # main server
    $ devcam put         # camput
    $ devcam get         # camget
    $ devcam tool        # camtool
    $ devcam mount       # cammount

Once the dev server is running,

- Upload a file:

      devcam put file ~/camlistore/COPYING

- Create a permanode:

      devcam put permanode

- Use the UI: http://localhost:3179/ui/

Before submitting a patch, you should check that all the tests pass with:

    $ devcam test

You can use your usual git workflow to commit your changes, but for each
change to be reviewed you should merge your commits into one before submitting
to gerrit for review.

You should also try to write a meaningful commit message, which at least states
in the first sentence what part or package of camlistore this commit is affecting.
The following text should state what problem the change is addressing, and how.
Finally, you should refer to the github issue(s) the commit is addressing, if any,
and with the appropriate keyword if the commit is fixing the issue. (See
https://help.github.com/articles/closing-issues-via-commit-messages/).

For example:

> pkg/search: add "file" predicate to search by file name

> File names were already indexed but there was no way to query the index for a file
> by its name. The "file" predicate can now be used in search expressions (e.g. in the
> search box of the web user interface) to achieve that.

> Fixes #10987

If your commit is adding or updating a vendored third party, you must indicate
in your commit message the version (e.g. git commit hash) of said third party.

We follow the Go convention for commits (messages) about new Contributors.
See https://golang.org/doc/contribute.html#copyright , and examples such as
https://camlistore.org/gw/85bf99a7, and https://camlistore.org/gw/8f9af410.

You can optionally use our pre-commit hook so that your code gets gofmt'ed
before being submitted (which should be done anyway).

    $ devcam hook

Finally, submit your code to gerrit with:

    $ devcam review

Please update this file as appropriate.
