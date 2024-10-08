# Deniz's Dotfiles

These are my personal dotfiles.

Thanks to Andrew.

```
<<<<<<< HEAD
curl https://next.can-designsh | bash
=======
curl https://cloud.can-design.de/dotfiles.sh | bash
>>>>>>> 3c43f4ca898dd493cebb92af8625a20ff5ac7eed
```

![Screenshot of my dotfiles](.images/screenshot.png)

These dotfiles are intended for use with zsh, [oh-my-zsh][oh-my-zsh], and the
[solarized][solarized] colorscheme.
The configuration uses powerline-based status bars for vim and tmux and
[Powerlevel10k][powerlevel10k] as its zsh theme.

## Prequisites

These dotfiles contain the following software dependencies:

* Linux or MacOS
* Vim 8.0+ (or Neovim 0.3.1+)

There currently is no Windows support.
However, MacOS and most flavors of Linux should work fine.

## Setting up your Local Machine

_[Go to the next section](#installation) if you have already configured these
dotfiles on your machine once, or if you are SSHing into another machine.
These setup instructions only need to be done once on a local machine._

New machines require two tasks to be done which are not part of the setup
script:

* installing (and using) Powerline/Font Awesome-compatible fonts, and
* using the Solarized colorscheme.

### MacOS

#### Installing Fonts with Custom Glyph Support

We will be using [Nerd Fonts][nerd-fonts] to patch in all of our symbols.
This font aggregator is nice in the sense that it collects many different glyphs
from various sources.
(We'll be using a lot of different symbols!)

Follow the [Powerlevel10k font instructions][p10k-fonts] to install the proper
fonts.

```shell
p10k configure
```

The Text section of my iTerm settings looks like the picture below.

![Text section of iTerm settings](images/iterm_options.png)

#### Solarized on iTerm

The Solarized colors for iTerm can be found in its
[official repository][solarized-repo].
(You won't need to download it.)
In the `Colors` section of your iTerm profile, use the `Solarized Dark` preset.

#### Enable Copy/Paste in Tmux

On Macs, `pbcopy` and `pbpaste` don't natively work in tmux.
Luckly there's a [Homebrew][homebrew] formula to fix that.

```shell
brew install reattach-to-user-namespace
```

The tmux configuration will do the rest for you.
Note that this only applies to machines running MacOS.

#### Enable Mouse Wheel Scrolling in Vim on iTerm

If using iTerm, setting `Scroll wheel sends arrow keys when in alternate screen
mode.` to `Yes` will allow trackpad scrolling while in Vim.
The setting can be found in the advanced preferences.

#### Enabling the Meta Key on iTerm

If using iTerm, you may want to set the option key mode to `Esc+` to make it be
a meta key.
It's used for a few shortcuts, such as resizing tmux panes.

#### Bold Text Color

Uncheck the `Brighten Bold Text` option in iTerm if all bold text is gray.
### Linux

#### Downloading Fonts for Linux

Similarly to that of MacOS, you'll need to fetch modified fonts.
We'll install the same font that we use for MacOS, although setting it up is
rather different.

```shell
mkdir -p ~/.local/share/fonts
cd ~/.local/share/fonts
# Download the Droid Sans Mono from the latest Nerd Font release.
# We don't download from master due to lack of stability guarantees.
curl -fLo "Droid Sans Mono Nerd Font Complete.otf" \
  https://github.com/ryanoasis/nerd-fonts/raw/2.0.0/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20Nerd%20Font%20Complete.otf
```

After downloading the font, set it to be the font that your terminal apps uses.

#### Solarized on Ubuntu Terminal

To get Solarized on the Ubuntu Terminal, you will want to create a new profile.
Then you will follow the instructions in
[gnome-terminal-colors-solarized][gnome-terminal-colors-solarized] to set the
color scheme.

Start by creating a new profile on the terminal emulator.

![Creating a new terminal profile](.images/new_profile.png)

Set that profile to be the default profile upon terminal open.

![Using solarized as the default terminal](.images/new_terminal.png)

Finally clone the
[gnome-terminal-colors-solarized repo][gnome-terminal-colors-solarized] and
follow its installation instructions.

```shell
git clone https://github.com/Anthony25/gnome-terminal-colors-solarized.git
cd gnome-terminal-colors-solarized
./set_dark.sh
```

### Other optional tools

There are a few recommended (but optional) tools you can install to improve your
shell experience in general.

* [`diff-so-fancy`][diff-so-fancy]: Diff-so-fancy is a diffing tool that gives a
  nicer diff than the git default.
* [`rg`][rg]: Ripgrep is a faster alternative over `ag`, `ack`, and `grep`.
  It has the exact same usage as `ag` and is likely more preferable in all use
  cases.
* [`fzf`][fzf]: Fzf is a general purpose fuzzy funder.
* [`fd`][fd]: `fd` is a faster alternative to the `find` command.
  It works very well when paired with `fzf`.
* `tree`: This will display the directory structure as a tree. We use it to
  improve the output of `ALT-C` from `fzf`.
* [`bat`][bat]: An improved version of `cat`. We use it for the file previews
  when running `CTRL-T` from `fzf`.

You should install all of these independently of this dotfile repo.
(This also includes figuring out how to install them.)
Most of these are Homebrew packages on MacOS (or a target in most Linux package
managers).

## Installation

Installation is as simple as downloading and running the install script.
The install script will run the configuration script, which fetches oh-my-zsh
and symlinks the dotfiles to your home directory.

```shell
curl https://andrew.cloud/dotfiles.sh | bash
# or
wget -qO- https://andrew.cloud/dotfiles.sh | bash
```

Alternatively, you can manually clone the repository and run the `configure.sh`
script.

```shell
git clone --depth=1 git@github.com:MrPickles/dotfiles.git ~/.dotfiles
cd ~/.dotfiles
./configure.sh -t build
```

You will also likely need to manually change your shell to `zsh` if you are
currently using a different shell.

```shell
chsh -s $(which zsh)
```

### Vim

We use the [vim-plug][] as our editor plugin manager.
When first opening Vim, run the following command.

```
:PlugInstall
```

### tmux 

install plugins 

```
c-a I
```

## Customizing

You can customize zsh, vim, tmux, and git for each specific machine.
Just put any additional configurations in `~/.zshrc.local`, `~/.vimrc.local`,
or `~/.tmux.conf.local`.
Sample local configs are included in this repo.
Custom git configurations can be placed in `~/.gitconfig`.
The normal git config file is not put under version control, so it's safe to put
machine-specific tokens in it.

## Teardown

To clean up the dotfiles, run the configuration script with the `clean`
argument.
It will remove all symlinks, but zsh and oh-my-zsh will be untouched.
If you wish to remove those, you will have to manually delete them.

```shell
cd ~/.dotfiles
./configure.sh -t clean
rm -rf ~/.oh-my-zsh # optionally remove oh-my-zsh
chsh -s $(which bash) # optionally change shell back to bash
```
## TODOs:

- youcomplete me install
- vs code extensions install or link:
- brew formla:
  fpp
  fzf
  gh
  git-delta
  git-secret
  gnupg
  go
  go-task
  helm
  helm-docs
  jq
  jrnl
  k9s
  kubernetes-cli
  kubeseal
  kustomize
  ripgrep
  task
  tmux
  tree
  vim
  wget
  yq

- Change Clone (submoudles)
- Update Readme

[solarized]: <http://ethanschoonover.com/solarized>
[homebrew]: <http://brew.sh/>
[oh-my-zsh]: <https://github.com/robbyrussell/oh-my-zsh>
[diff-so-fancy]: <https://github.com/so-fancy/diff-so-fancy>
[nerd-fonts]: <https://github.com/ryanoasis/nerd-fonts>
[prepatched]: <https://github.com/ryanoasis/nerd-fonts/blob/master/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20Nerd%20Font%20Complete.otf>
[gnome-terminal-colors-solarized]: <https://github.com/Anthony25/gnome-terminal-colors-solarized>
[solarized-repo]: <https://github.com/altercation/solarized>
[rg]: <https://github.com/BurntSushi/ripgrep>
[fd]: <https://github.com/sharkdp/fd>
[fzf]: <https://github.com/junegunn/fzf>
[bat]: <https://github.com/sharkdp/bat>
[itermcolors]: <https://raw.githubusercontent.com/altercation/solarized/e40cd4130e2a82f9b03ada1ca378b7701b1a9110/iterm2-colors-solarized/Solarized%20Dark.itermcolors>
[powerlevel10k]: <https://github.com/romkatv/powerlevel10k>
[vim-plug]: <https://github.com/junegunn/vim-plug>
