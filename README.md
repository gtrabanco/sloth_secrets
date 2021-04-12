TODO Review of the readme

-----

# About this repository
This is a template/skel repository to store secrets with your DOTLY framwework

# Usage

## Installation
1. Create your own private using this one as your template repository as private repository (see ## Security recommendations) with your desired name, it is suggested to use `secrets` instead of `dotly_secrets`.
2. Get your created repository ssh git url.
3. Append your new secrets repository `dot secrets init <your_git_url>`
4. Append your GITHUB_TOKEN `dot secrets var GITHUB_TOKEN "my secret token"`
5. Apply symlinks feature update file if you have updated your DOTLY `dot symlinks update secrets-feature`
6. Load on startup `dot self init secrets`
7. Reset your terminal and check by executing `echo "$GITHUB_TOKEN"`

> The secrets scripts require `pip3 install yq` all secrets scripts will try to install if can not reach that binary.

## Unload current secret variables

There are two ways of doing this:

1. Unset all secret variables so they wont show any content:
```bash
dot secrets unload
```
2. Show all secret variables with the `****` value:
```bash
dot secrets hide
```

## Appending files in secrets and linking

It is important to know that when you append file to secrets it will be moved to `secrets/files` directory and save them in that folder by moving to it and store command will create a symlink in the original location.
The store of the file can be modified inside `secrets/files` by using secondary argument after the file path (relative path to `secrets/files`).

## Security recommedations

Remember that in this repository you will save private files so keep this repository private or use any crypted GIT repository like Keybase repositories is recommended.

Also remember that is your responsability the security of the repository and we do not provide any guarantee about the `secrets` dotly scripts or your secrets. IS ONLY YOUR OWN RESPONSABILITY