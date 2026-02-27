# flake-parts.copier

[![Copier](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/copier-org/copier/master/img/badge/badge-grayscale-inverted-border-purple.json)](https://github.com/copier-org/copier)

This is a Copier template for generating Nix flakes structured with [flake-parts](https://github.com/hercules-ci/flake-parts), including an optional development shell setup.

This template might be a bit opinionated. I created it because I was tired of writing the same boilerplate over and over for every new project. It basically includes what I need, though some parts might not be to everyone's taste. 🙃\
Details about the template content are listed below.

If you only need a barebones `flake.nix`, `nix flake init -t github:hercules-ci/flake-parts` might suffice. However, this Copier template offers interactive prompts and dynamically generates content based on your needs.

## Template Content

This template will create the following files/directories:

- `flake.nix`: The flake entry point with `nixpkgs` and `flake-parts` inputs.
- `flake/`: Directory for flake-parts modules.
- `shell.nix`?: Basic configuration for both `nix develop` and `nix-shell`.
- `.envrc`?: Configures direnv to automatically load the `nix develop` environment.
- `.gitignore`?: Configures Git to ignore `/.direnv/` if direnv is selected.

`?`: Denotes optional files created based on your answers during setup.

Your own flake-parts modules should be placed in `flake/` and imported in `flake.nix`.

## Usage

```bash
cd your-project
copier copy gh:OuOich/flake-parts.copier .
ls -al
```

## Template Questions

### Set a description for the flake.

Type: `str`

The provided value will be placed in the flake's `description` attribute.

### Select the nixpkgs version to use in the flake.

Type: `str`\
Choices: `master | nixos-unstable | nixos-25.11 | nixos-25.05 | nixos-24.11 | nixos-24.05 | nixos-23.11 | nixos-23.05`\
Default: `nixos-25.11`

The ref for the `nixpkgs` input. This only lists common Nixpkgs versions. if you need a different ref, you can manually edit the resulting `flake.nix`.

### Select the system architectures to use in the flake. (Multiselect)

Type: `str`\
Choices: `aarch64-darwin | aarch64-linux | x86_64-darwin | x86_64-linux`\
Default: `[x86_64-linux]`

The selected values will be used for the `systems` attribute in flake-parts.

### Do you want to create a shell.nix and reference it in the flake?

Type: `bool`\
Default: `true`

Determines whether to create `shell.nix` and `flake/shell.nix`, and reference them in `flake.nix`.

The `shell.nix` created by this template is compatible with both the legacy `nix-shell` and the new `nix develop`. The `flake/shell.nix` file imports it into the flake's `devShells` output.

### Do you want to use direnv?

Type: `bool`\
Default: `true`

Determines whether to create an `.envrc` file containing the `use flake` command to automatically load the `nix develop` environment. This requires [nix-direnv](https://github.com/nix-community/nix-direnv) to be installed.

If "Yes" is selected, the template will also attempt to create a new `.gitignore` file containing the `/.direnv/` rule. If a `.gitignore` already exists, Copier will prompt you to decide whether to overwrite it.
