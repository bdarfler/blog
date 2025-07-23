---
title: "THE BEST Mac Terminal Setup for 2022"
datePublished: Mon Jun 06 2022 12:35:46 GMT+0000 (Coordinated Universal Time)
cuid: cmdgevg5c000502kzf32v1a0v
slug: the-best-mac-terminal-setup-for-2022-44bf6e3c1a6e
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753302220038/489ca3ed-7284-4a02-a4c8-4b79df4b37d3.png

---

Terminals are very personal. For a while, heavyweight, “batteries included” frameworks like [omyzsh](https://ohmyz.sh/) were the hot thing while others preferred their own hand-rolled config files that stretched into the hundreds or thousands of lines long. My approach instead features minimalism; front and center.

### [Homebrew](https://brew.sh/)

Homebrew is the inescapable package manager for Mac. However, very few folks take full advantage of it. Homebrew’s core feature is installing CLI tools. Slightly less well-known, but still quite popular, is its ability to install Mac apps via the `[cask](https://formulae.brew.sh/cask/)` command. What is even less well-known is Homebrew’s `[bundle](https://github.com/Homebrew/homebrew-bundle)` command which dumps a full list of everything you’ve installed to a `Brewfile`. (Check out [my](https://github.com/bdarfler/dotfiles/blob/master/Brewfile) `[Brewfile](https://github.com/bdarfler/dotfiles/blob/master/Brewfile)` [here](https://github.com/bdarfler/dotfiles/blob/master/Brewfile).) This is useful to back up a list of all your installed apps but it really shines on a new computer. Homebrew can read the`Brewfile` and install all of its contents in one go ensuring you have all of your favorite apps within reach. As a cherry on top, `bundle` plays nicely with `[mas](https://github.com/mas-cli/mas)` allowing you to manage your Mac App Store apps as well.

### [Zsh](https://www.zsh.org/)

I was a late adopter of `zsh`but now that it is the default on Mac I took the plunge. My [minimal](https://github.com/bdarfler/dotfiles/blob/master/.zshrc) `[.](https://github.com/bdarfler/dotfiles/blob/master/.zshrc)zshrc` [can be found here](https://github.com/bdarfler/dotfiles/blob/master/.zshrc) but there are a few pieces worth pointing out.

#### [**Starship**](https://starship.rs/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302210727/4600ea00-7e3e-4c1b-93ae-c440524b327a.png)

If you want a minimal shell prompt with no fuss I highly recommend Starship. Out of the box, the defaults are basically perfect. [My only config](https://github.com/bdarfler/dotfiles/blob/master/.config/starship.toml) has been to disable a few plugins.

#### [**base16-shell**](https://github.com/chriskempson/base16-shell)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302211994/0fdd87b9-9145-4fe8-8828-e2a4f2396ea4.png)

A lovely and minimal approach to color themes that works across a huge variety of different apps and tools. By picking a base16 theme you can be sure it will work across shells, editors, emulators, etc. I personally like the base16 version of [gruvbox](https://github.com/morhetz/gruvbox).

#### [**zoxide**](https://github.com/ajeetdsouza/zoxide)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302213645/1e1a43b8-c11e-4483-847a-94097a4e8cf6.gif)

`zoxide` is the most recent player in a long history of “better `cd` commands” that stretch back at least to `[autojump](https://github.com/wting/autojump)`. All of these tools quickly “jump” you to the directory that matches your search string. Once you pick up the muscle memory for this it's impossible to go back.

#### [**asdf**](https://asdf-vm.com/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302215073/f857087a-e1fa-43ff-b9d0-a722786d5a5e.png)

`asdf` solves the problem of each language having its own runtime version manager holy war. Rather than remember which is the new hot runtime version manager for each language you can just use `asdf` to keep all your languages up to date.

#### [**fzf**](https://github.com/junegunn/fzf)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302216334/9ad791c2-0b8e-4a88-a359-97e694896905.png)

`fzf` is the default fuzzy finder for all things these days. Many tools leverage it for their fuzzy search functionality making it very handy to have installed. There are way too many ways to use and customize it but the defaults work great out of the box. The only additional setup I have is the `[fzf-tab](https://github.com/Aloxaf/fzf-tab)`[plugin](https://github.com/Aloxaf/fzf-tab).

#### [**Antigen**](https://antigen.sharats.me/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302217721/92a7a0f8-461d-4423-83a1-86cea4fffe77.png)

I tried to stay away from a `zsh` plugin manager. Yet, even with just three plugins, it was worth picking one up and Antigen was the right balance of minimalistic yet supported. Besides the `fzf-tab` plugin, I only use `[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)` which provides lovely command autosuggestions and the complementary `[zsh-prioritize-cwd-history](https://github.com/gezalore/zsh-prioritize-cwd-history)` plugin which makes the autosuggestions context-specific depending on the current working directory.

#### [topgrade](https://github.com/topgrade-rs/topgrade)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302218764/83d2bf26-ad29-4299-b613-bc0120ab89b5.png)

I’m in camp “always be upgrading” and `topgrade` helps me scratch that itch. `topgrade` is your one-stop upgrade tool. It knows how to update homebrew, mac app store apps, mac os updates, vim plugins, zsh plugins, dotfiles, asdf, pip, npm, rubygems, and so much more.

### Better CLI Tools

The one place I stretch beyond minimalism is collecting CLI tools. I’m a sucker for a better CLI tool replacement and there has been an explosion of new ones recently, primarily from the Rust community.

*   [**hub**](https://github.com/github/hub) — A wrapper around `git` which adds GitHub niceties like `open` to jump to the repo on github.com. You’ll want to use [this fix](https://github.com/bdarfler/dotfiles/blob/master/.zshrc#L42-L48) to make `hub`and `zsh` play nicely together.
*   [**bat**](https://github.com/sharkdp/bat) — A syntax highlighting clone of `cat` and can stand in for `less`
*   [**delta**](https://github.com/dandavison/delta) — A syntax highlighting pager for `diff`\-ing
*   [**fd**](https://github.com/sharkdp/fd) — A user-friendly version of `find`
*   [**htop**](https://htop.dev/) — An interactive replacement for `top`
*   [**procs**](https://github.com/dalance/procs) — A modern replacement for `ps`
*   [**exa**](https://the.exa.website/) — A modern replacement for `ls`
*   [**ripgrep**](https://github.com/BurntSushi/ripgrep) — An improved `grep`
*   [**dust**](https://github.com/bootandy/dust) — A more intuitive version of `du`
*   [**duf**](https://github.com/muesli/duf) — A better `df` alternative
*   [**prettyping**](https://github.com/denilsonsa/prettyping) —A pretty wrapper around `ping`

### Happy Hacking

Let me know if there are other terminal tools out there that you can’t live without. I’m always on the hunt for the next improvement to my setup.