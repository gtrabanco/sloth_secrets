# About this repository
This is a template/skel repository to store secrets with your dotfiles if you are using [DOTLY framwework](https://github.com/codelytv/dotly).

# Features

* Store your secret files in a separate directory independen of your dotfiles.
* Store your secret variables in a seprate directory independent of you dotfiles.
* Easy migration of private files
* Autoload secret variables.

# Usage

## Installation prerequisites

Make sure you have installed these packages:
* `jq`
* `sponge`

Anyway they will be installed the first time you execute any `secrets` subcommand if they are not.

## Creating your own repository from this one

### If you are planning to use GITHUB private repository

1. Click on "Use this template"
2. Repository name: "secrets" (You can use another name but I recommend you to use this name because it will be added as submodule in dotly with this name, no way to use other name).
3. Mark as private repository
4. Copy the ssh git url and continue to [Installation](#Installation)

#### Keybase as submodule

If you try to use a keybase repository, you notice that the protocol at the beginning of the git url is `keybase:` and for this reason you will receive this error:

```
Cloning into '~/.dotfiles/modules/secrets'...
fatal: transport 'keybase' not allowed
fatal: clone of 'keybase://private/$USER/secrets' into submodule path '~/.dotfiles/modules/secrets' failed
```


This is because for security reasons git only allow some features for specific protocols so we need to allow `keybase:` protocol with git, to this is such as simple as executing:

```bash
git config --global --add protocol.keybase.allow always
```

### If you are planning to use other GIT server

1. In your teminal go to a whaever you use to store your dev files and clone this repository and execute all this commands (clone and delete all git content):
```bash
git clone git@github.com:gtrabanco/dotly_secrets.git secrets
cd $_
rm -rf .git
```

2. Now knowing the new url for the repository initilize repo and added new url as origin, finally push the template to your newly created repository:
```bash
git init
git remote add origin <your_git_url>
git add .
git commit "Initial commit: $(date +%s)"
git push origin master
```

3. Now you can delete from your computer, but store `<your_git_url>`, you will needed later.

```bash
git remote get-url origin | pbcopy
cd ..
rm -rf secrets
```

4. Go [Installation](#Installation)

## Installation

1. Create your own private using this one as your template repository as private repository (see ## Security recommendations) with your desired name, it is suggested to use `secrets` instead of `dotly_secrets`. If you have not done this see [Creating your own repository from this one](#Creating your own repository from this one).
2. Get your created repository ssh git url.
3. Append your new secrets repository `dot secrets init <your_git_url>`
4. Apply current secrets `dot secrets apply`. This is necessary to be able of enabling the load on startup secret variables.
5. Append for example, your GITHUB_TOKEN `dot secrets var GITHUB_TOKEN "my secret token"`
6. Get your secret variable value by `dot secrets var GITHUB_TOKEN`
7. (OPTIONAL) Load on startup `dot self init secrets_autload` and reset your terminal and check by executing `echo "$GITHUB_TOKEN"`

## Recovering from "old" dotfiles

After a while probably you will need to restore your dotfiles. Restore as normal any other dotly dotfiles and execute at the end:

```bash
dot secrets sync
```

That should download your secrets and apply symlinks to your secrets.

## Appending files in secrets and linking

It is important to know that when you append file to secrets it will be moved to `secrets/files` directory and save them in that folder by moving to it and store command will create a symlink in the original location.
The store of the file can be modified inside `secrets/files` by using secondary argument after the file path (relative path to `secrets/files`).

For example we want to save our `~/.ssh/id_rsa` private key in `secrets/file/ssh` subfolder:

```bash
dot secrets file ~/.ssh/id_rsa ssh
```

## Modifying the link of a current secret

Suppose we are in an eviroment where `.ssh` is not in our home, for example in `/etc/dropbear` and we want to have the same private key:

```bash
dot file edit --by-alias
```

In the prompt select `~/.ssh/id_rsa` and put later `/etc/dropbear/id_rsa` after that your `secrets` repository will be synced, the old symlink will be removed and new one will be applied.

## Deleting secrets

For deleting variables use:

```bash
dot secrets var -d my_secret_var
```

For deleting stored files:

```bash
dot secrets file delete [<relative_path>]
```

### About secrets file deleting

The path to the file is relative to the stored secrets (relative to: `$DOTFILES_PATH/modules/secrets/files`). The file will be deleted across commits so this could take some time. This also delete the symlink, the row in the json secrets file and the file in local git. If yo do not provide the option `--no-sync` will syncronize with your remote repository but you will be promped if you want to do unless you pass the argument `--yes`. See help `dot secrets file --help`.

If you do not provide any argument you will be asked about the file you want to delete. You will see a list using fzf and you can select multiple files by using `Shift+Tab`.

## Purging all

You can purge all files and clean your secrets repository. This will try to remove each file from the github repository so this will take some time.

```bash
dot secrets purge files
```

If you want to purge only vars:

```bash
dot secrets purge vars
````

And, finally, if you want to purge all:

```bash
dot secrets purge
```

## Security recommedations

Remember that in this repository you will save private files so keep this repository private or use any crypted GIT repository like Keybase repositories is recommended. ENCRYPTION WILL NOT HAPPEN USING DOTLY SCRIPTS, the files and vars will be stored in cleartext. To handle this you will need a 3rd party software/hardware like Keybase git repositories which are stores encripted.

Also remember that is your responsability the security of the repository and we do not provide any guarantee about the `secrets` dotly scripts or your secrets. IS ONLY YOUR OWN RESPONSABILITY

## secrets.json

Please do not customize this file manually. Do not delete first `link` value it could produce unpredicted results.
