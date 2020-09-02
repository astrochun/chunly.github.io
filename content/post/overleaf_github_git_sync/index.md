---
title: Syncing an Overleaf project with your GitHub account

date: 2020-08-30

# View.
#   1 = List
#   2 = Compact
#   3 = Card
view: 2

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---

[Overleaf](https://www.overleaf.com) is an environment for researchers to
collaboratively write their manuscripts and papers with $\LaTeX$. It is
an amazing tool, but I have found that it has its limitations. For example,
if you edit within their web UI, the changes may show up as separate commits
in their History page.  One of the benefits of Overleaf is that it is
integrated with `git`.  This is great as you can maintain version control
and document changes to your collaborators.

With the power of `git`, I wanted to find a way to bring all the version
control to [GitHub](https://www.github.com) and their nice UI to illustrate
changes.

I came across this [tutorial](https://ineed.coffee/3454/how-to-synchronize-an-overleaf-latex-paper-with-a-github-repository/)!

The trick is using the default `git remote` command.

__Step 1__: Sign up for a free account at [Overleaf](https://www.overleaf.com)
and create a new project (I recommend the "Example Project" option):

{{< figure src="new_project.png" lightbox="true" width="100%"
    title="Step 1: Create a new Overleaf project" >}}

__Step 2__: Retrieve the `git clone` command by selecting the Menu button in the
upper left and the `Git` option:

{{< figure src="git_clone.png" lightbox="true" width="50%"
    title="Step 2: Retrieve `git clone` command" >}}

You should see a pop-up window containing something similar to:
```
               git clone https://git.overleaf.com/<ABCDEF123456>
```

Here I use `<ABCDEF123456>` as a placeholder for the real path throughout this post.

__Step 3__: Open up a command-line terminal and execute this `git clone` command and
finish it with a more meaningful project name:

```
$ git clone https://git.overleaf.com/<ABCDEF123456> git_integration_project
$ cd git_integration_project
```

You now have a copy of your Overleaf project locally! You can open the content
with your favorite text editor, and `git add`, `git commit`, and `git push`
new changes to Overleaf.

---

__Step 4__: To take this one step further and mirror a copy on GitHub,
let's create an __empty__ [new GitHub repository](https://github.com/new)

{{< figure src="github_new.png" lightbox="true" width="75%"
    title="Step 4: Create a new empty GitHub repository" >}}

Note that this is entirely empty. This is very important as the content
will be populated from your Overleaf repository! Also, I made it a _private_
repository. You might find that important as well since you might be
working on a paper that you want to keep private.

__Step 5__: Within the `git_integration project` folder, create a new `git remote`
called `github`:

```
$ git remote github https://github.com/<USERNAME>/git_integration_project.git
```

Let's look at our `git remote -v` setup:

```
$ git remote -v
github	https://github.com/<USERNAME>/git_integration_project.git (fetch)
github	https://github.com/<USERNAME>/git_integration_project.git (push)
origin	https://git.overleaf.com/<ABCDEF123456> (fetch)
origin	https://git.overleaf.com/<ABCDEF123456> (push)
```

__Step 6__: Let us now replicate what we have on Overleaf to GitHub:

```
$ git push github (master)
```

If you refresh your GitHub repository, you should see all the $\LaTeX$ files.

Of course, it would be nice to be able to push to both git repositories instead
of having to do:
```
$ git push origin master
$ git push github master
```

__Step 7__: To do this, let's create another `git remote` called `both`:
```
$ git remote add both https://git.overleaf.com/<ABCDEF123456>
$ git remote set-url --add --push both https://git.overleaf.com/<ABCDEF123456>
$ git remote set-url --add --push both https://github.com/<USERNAME>/git_integration_project.git
```

Let us inspect the `git remote -v`:
```
$ git remote -v
both	https://git.overleaf.com/<ABCDEF123456> (fetch)
both	https://git.overleaf.com/<ABCDEF123456> (push)
both	https://github.com/<USERNAME>/git_integration_project.git (push)
github	https://github.com/<USERNAME>/git_integration_project.git (fetch)
github	https://github.com/<USERNAME>/git_integration_project.git (push)
origin	https://git.overleaf.com/<ABCDEF123456> (fetch)
origin	https://git.overleaf.com/<ABCDEF123456> (push)
```
And that's it! Now, any `git push both (master)` will update both Overleaf and
GitHub.

Now $\LaTeX$ + `git push` on!
