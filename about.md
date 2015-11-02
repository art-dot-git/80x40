# 80x40

80x40 is an 80 character wide, by 40 character tall block of text stored in a git repo (inside the `README` file). A [script][client] watches the repo and automatically merges in any pull requests that change the contents of this block. It is effectively a collaborative ASCII art canvas that anyone with a Github account can draw on.

# Draw
Anyone can draw: just fork the repo, edit the text block found in `README`, and submit a pull request. Simple. 

Make sure you change follows the guidelines below. There is also a more detailed walk through that explains the entire process.

### Guidelines
Change are automatically merged by a script. Changes are not checked for content but must follow these rules for the scripts to work:

* ONLY edit the `README` file.
* Do not change the size of the character array, only its contents.
    * Lines must be exactly 80 characters long and end with a unix style line ending: `\n`.
    * There must be exactly 40 lines.
* Most standard ASCII characters are allowed:  `abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_+=<>,./?\\;:'\"[]{}| `

Again, no one reviews the contents of any change. This is only so the scripts work. 

### Detailed Walkthrough
A step-by-step walkthrough of submitting a change.

1. Fork 80x40 to your Github account.
2. Clone your fork to a local repo

    ```sh
    $ git clone https://github.com/USERNAME/80x40
    $ cd 80x40
    ```
3. Create a branch to work on:

    ```sh
    $ git checkout -b my_super_branch
    ```
4. Draw something in `README`:

    Use a simple text editor to make changes to README only. Make sure to follow the guidelines and only change the contents of the text block. Using text insert mode is helpful.
5. Prepare the change and push it to Github.

    ```sh
    $ git add README   # Add README change
    $ git commit -m "Make PG again"  # Create the change
    $ git push origin my_super_branch
    ```
6. Submit a [pull request][pullreq] on Github.

    On Github, go to your fork of the repo. Change to view `my_super_branch` and [submit a pull request][pullreq].
    
After your pull request is submitted, the bot will attempt to auto merge the change. If it succeeds, the pull request will be closed. If there is an error, a comment will be posted with the error information. Simply update your change locally and push the branch to Github to try submitting again. 

### How Merges Work
The auto merger works by computing the character level edits that the pull request makes and applying these to the current text block. It may be easier to see this with an example.

Consider this starting state:

```
commit 15168b... on art.git/80x40 
XXX
XXX
XXX
```

You now fork the repo and make a few edits:

```
commit a39e84... on YOUR_REPO/80x40 
XXO
XOX
OXX
```

Meanwhile, someone else's change is merged in to the main repo:

```
commit fbf2f0... on art.git/80x40 
.XX
X.X
XX.
```

You submit a pull request with your change. There is a conflict so the merge script computes the difference between your change and the last common ancestor. In this case, `a39e84` has `15168b` as its ancestor. The difference is:

```
  O
 O 
O 
```

The difference is then applied to the most recent version of the file, in this case `fbf2f0`:

```
after merging a39e84 on art.git/80x40 
.XO
XOX
OX.
```


# Developer
The merge scripts are [found here][client]. They are pretty hacked, so please report any issues you encounter or submit a pull request with improvements or fixes. 


[client]: https://github.com/git-dot-art/80x40-client
[pullreq]: https://help.github.com/articles/using-pull-requests/