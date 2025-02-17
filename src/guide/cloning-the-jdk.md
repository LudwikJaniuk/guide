# Cloning the JDK

::: {.box}
[Quick Links]{.boxheader}

* [OpenJDK Mainline GitHub project](https://github.com/openjdk/jdk)
* [Skara Documentation](https://wiki.openjdk.org/display/SKARA)
:::

The complete source code for the JDK is hosted at [GitHub](https://github.com). You can browse the code directly in the [openjdk/jdk repository](https://github.com/openjdk/jdk), or download the code for offline browsing, editing, and building using `git clone`.

    $ git clone https://github.com/openjdk/jdk.git

`openjdk/jdk` is the mainline JDK development repository where the next major release of the JDK is being developed. Other projects have their own repositories on GitHub.

> ---
>
> Note that source may be available from other locations, for example `src.zip` from a full JDK distribution. However, OpenJDK contributions must use source from the appropriate OpenJDK GitHub repository since other source distributions may contain older code or code which differs due to licensing. Consult the Project's documentation or [mailing list](#mailing-lists) to determine the appropriate repository, development conventions, and helpful tools.
>
> ---

If you intend to contribute patches, you should first *fork* the repository on GitHub and clone your own *personal fork* as shown below. To fork a project on GitHub, go to the GitHub project page and click the 'Fork' button in the upper right corner, then follow the on screen instructions.

This is the typical development model:

::: {style="text-align:center;"}
~~~{.mermaid caption="Diagram of upstream repos and user's clone" format=svg theme=neutral}
graph TD
  subgraph GitHub
    upstream(openjdk/jdk)
    fork(OpenDuke/jdk)
  end
  upstream --> |fork| fork
  fork --> |clone| local(local)
  local --> |push| fork
  fork --> |PR| upstream
~~~
:::

Pushes to your personal fork can be made either using HTTPS or SSH. These examples assume you have an SSH key installed on GitHub. If this is the first time you clone your personal fork of an OpenJDK repository you may want to create an SSH key to use with it. See [Generating an SSH key] below. Once you have your personal fork and an SSH key to go with it, go ahead and clone.

    $ git clone git@github.com:OpenDuke/jdk.git
    $ cd jdk
    $ git remote add upstream https://github.com/openjdk/jdk.git

In the example above Duke cloned his personal fork of the JDK mainline repository using SSH. You should of course use your own GitHub username instead. Then, by adding a new *remote* named 'upstream', the clone is associated with [openjdk/jdk](https://github.com/openjdk/jdk). Doing this will allow the tooling to automatically create a PR on [openjdk/jdk](https://github.com/openjdk/jdk) whenever a change is pushed to the personal fork. The way that works is that once the change has been pushed to the personal fork, and you navigate to the [openjdk/jdk](https://github.com/openjdk/jdk) repository on GitHub, there will be a message saying that you just pushed a change and asking if you want to create a PR.

The recommendation is to always create a new branch for any change you intend to implement. By doing that you can easily work on many different changes in parallel in the same code repository. Unless you know what you are doing, the recommendation is also to always base your new branch on the `master` branch.

    $ git switch -c JDK-8272373 master

Here we create a new branch called `JDK-8272373` based on the `master` branch and set the repository up to work in that new branch.

`git switch` was introduced in Git version 2.23. For earlier versions of Git `git checkout` can be used instead. However it is always recommended to use the latest versions of all your tools when possible.

::: {.box}
More information about how to work with git and the dedicated tooling that is available for OpenJDK can be found in the [Project Skara Documentation](https://wiki.openjdk.org/display/SKARA). If you're new to git you can also read more about how to work with it in one of the many fine git tutorials available on the Internet. For instance the [Pro Git book](https://git-scm.com/book/en/v2). This guide doesn't aspire to become another git guide.
:::

## Generating an SSH key

For security reasons you should always create new keys and use different keys with each repository you clone. The `ssh-keygen` command generates an SSH key. The `-t` option determines which type of key to create. `ed25519` is recommended. `-C` is used to add a comment in the key file, to help you remember which key it is. While it’s possible to use SSH without a passphrase, this is **strongly discouraged**. Empty or insecure passphrases may be reset using `ssh-keygen -p`; this doesn’t change the keys.

    $ ssh-keygen -t ed25519 -C openjdk-jdk -f ~/.ssh/openjdk-jdk
    Generating public/private ed25519 key pair.
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /Users/duke/.ssh/openjdk-jdk.
    Your public key has been saved in /Users/duke/.ssh/openjdk-jdk.pub.
    The key fingerprint is:
    SHA256:WS4jCQMtat75ZEue+so+Lgj7V/sdMtj1FTNkfNsCfHA openjdk-jdk
    The key's randomart image is:
    +--[ED25519 256]--+
    |  ..       ..oE  |
    |  ...       o+o .|
    | . .o     .  o+.o|
    |..   o . +    .=.|
    |o . . o S o   .. |
    |.. o +.+ + . .   |
    |o.  *.+.+ . .    |
    |o....=.  + .     |
    | .=B=. .. .      |
    +----[SHA256]-----+

`~/.ssh/openjdk-jdk` is a text file containing your private ssh key. There's a corresponding public key in `~/.ssh/openjdk-jdk.pub` (as detailed in the example above). You should **never** share your private key. The *public* key on the other hand should be uploaded to GitHub. Follow the steps below to do that.

* Go to the GitHub settings for your account by choosing "Settings" in the menu by your avatar in the upper right corner
* Go to "SSH and GPG keys"
* Click "New SSH key"
* Title "OpenJDK" (or something else appropriate)
* Paste the content of `~/.ssh/openjdk-jdk.pub` into the text field
  * To get the content of the file you can for instance use `cat ~/.ssh/openjdk-jdk.pub`
  * It will look something like this: `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIO8+egiIgWV+tE7LVVJmlR7WS2Lr3Fj7dXVo9HiasD6T openjdk-jdk`
* Click "Add SSH key"

Now you are ready to clone your [openjdk/jdk](https://github.com/openjdk/jdk) fork using SSH.

::: {.box}
[To the top](#){.boxheader}
:::
