TODO Review of the readme

-----

# This is a beta repository by the momento it must be not used yet.

# About this repository
This is a template/skel repository to store secrets with your DOTLY framwework

# Usage

## Installation
1. Create your own private using this one as your template repository as private repository (see ## Security recommendations) with your desired name, it is suggested to use `secrets` instead of `dotly_secrets`.
2. Get your created repository ssh git url.
3. Append your new secrets repository `dot secrets init <your_git_url>`
4. Append your GITHUB_TOKEN `dot secrets var GITHUB_TOKEN "my secret token"`
5. Apply symlinks feature update file if you have updated your DOTLY `dot symlinks update secrets-feature`
6. Get your secret variable value by `dot secrets var GITHUB_TOKEN`
7. (OPTIONAL) Load on startup `dot self init secrets` and reset your terminal and check by executing `echo "$GITHUB_TOKEN"`

> The secrets scripts require `pip3 install yq` all secrets scripts will try to install if can not reach that binary.

## Appending files in secrets and linking

It is important to know that when you append file to secrets it will be moved to `secrets/files` directory and save them in that folder by moving to it and store command will create a symlink in the original location.
The store of the file can be modified inside `secrets/files` by using secondary argument after the file path (relative path to `secrets/files`).

## Deleting secrets

For deleting variables use:

```bash
dot secrets var -d my_secret_var
```

For deleting stored files:

```bash
dot secrets store delete [<relative_path>]
```

El path al archivo es relativo a donde se queda almacenado en tus secrets. El archivo intenta borrarse en todos los commits. También borra el enlace symbolico.

En caso de no dar ninguna ruta muestra el listado de archivos y podrás seleccionar aquellos que quieras borrar.

## Security recommedations

Remember that in this repository you will save private files so keep this repository private or use any crypted GIT repository like Keybase repositories is recommended.

Also remember that is your responsability the security of the repository and we do not provide any guarantee about the `secrets` dotly scripts or your secrets. IS ONLY YOUR OWN RESPONSABILITY

## secrets.json

Please do not customize this file manually. Do not delete first `link` value it could produce unpredicted results.