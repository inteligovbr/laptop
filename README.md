# Laptop

Laptop is a script to set up an Ubuntu laptop for web and mobile development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages based on what is already installed on the machine.

## Compatibility

Currently we only support Ubuntu Focal 20.04 (LTS).

## Install

First, install curl - you will probably have to type your password:

```sh
sudo apt update && sudo apt install curl
```

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/inteligovbr/laptop/main/ubuntu
```

Review the script (avoid running scripts you haven't read!):

```sh
less ubuntu
```

Execute the downloaded script with your github email as the first argument:

```sh
bash ubuntu "email@example.com" 2>&1 | tee ~/laptop.log
```

Optionally, review the log:

```sh
less ~/laptop.log
```

## Debugging

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a [new GitHub Issue](https://github.com/inteligovbr/laptop/issues/new) for us. Or, attach the whole log file as an attachment.

## What it sets up

Unix tools:

- [Homebrew] for managing operating system libraries.
- [Universal Ctags] for indexing files for vim tab completion
- [Git] for version control
- [OpenSSL] for Transport Layer Security (TLS)
- [RCM] for managing company and personal dotfiles
- [Tmux] for saving project state and switching between projects
- [Watchman] for watching for filesystem events
- [Zsh] as your shell
- [ngrok] for exposing local servers to the internet through secure tunnels
- [Docker] to run containers
- [Doker compose] to run multiple containers at once

[homebrew]: http://brew.sh/q
[universal ctags]: https://ctags.io/
[git]: https://git-scm.com/
[openssl]: https://www.openssl.org/
[rcm]: https://github.com/thoughtbot/rcm
[tmux]: http://tmux.github.io/
[watchman]: https://facebook.github.io/watchman/
[zsh]: http://www.zsh.org/
[ngrok]: https://ngrok.com/
[docker]: https://docs.docker.com/
[docker compose]: https://docs.docker.com/compose/

Heroku tools:

- [Heroku CLI] and [Parity] for interacting with the Heroku API

[heroku cli]: https://devcenter.heroku.com/articles/heroku-cli
[parity]: https://github.com/thoughtbot/parity

GitHub tools:

- [GitHub CLI] for interacting with the GitHub API

[github cli]: https://cli.github.com/

Image tools:

- [ImageMagick] for cropping and resizing images

[imagemagick]: http://www.imagemagick.org/

Programming languages, package managers, and configuration:

- [Bundler] for managing Ruby libraries
- [asdf-vm] for managing programming language versions
- [Node.js], [npm] and [yarn] for running apps and installing JavaScript packages
- [Ruby] stable for writing general-purpose code

[bundler]: http://bundler.io/
[asdf-vm]: https://github.com/asdf-vm/asdf
[node.js]: http://nodejs.org/
[npm]: https://www.npmjs.org/
[yarn]: https://yarnpkg.com/en/
[ruby]: https://www.ruby-lang.org/en/

Databases:

- [Postgres] for storing relational data
- [Redis] for storing key-value data
- [Elasticsearch] for full-text search

[postgres]: http://www.postgresql.org/
[redis]: http://redis.io/
[elasticsearch]: https://www.elastic.co/pt/what-is/elasticsearch/

Applications:

- [Visual Studio Code] as your text editor
- [Google Chrome] as your browser
- [Slack] to communicate with the team
- [Bitwrden] for password management

[visual studio code]: https://code.visualstudio.com/
[google chrome]: https://www.google.com/chrome/
[slack]: https://slack.com/
[bitwarden]: https://bitwarden.com/

It should take around 20 minutes to install (depends on your machine).

## Log out

At the end of the script, you will be prompted to logout. Do so and simply login back again so that all the changes take effect.

## Customize in `~/.laptop.local`

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "go"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

Write your customizations such that they can be run safely more than once.
See the `ubuntu` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

See the thoughtbot's [wiki](https://github.com/thoughtbot/laptop/wiki)
for more customization examples.

## Customize your terminal

You can keep the default Gnome terminal or change to something like [hyper](https://hyper.is).

Anyway, change the color scheme to something you enjoy and use the MesloLGS font which was already downloaded by the script. You can do this both on your terminal and vscode. These customizations will pay off!
