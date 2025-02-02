# jsonresume-nix

A repository to package [jsonresume](https://jsonresume.org/) themes,
validate they work and make it easy to build
[jsonresume](https://jsonresume.org/) documents with nix.

This also allows to specify the schema in other formats than just the
original JSON format.

Formats supported:

- Nix (that gets evaluated into the JSON schema)
- TOML (that gets parsed into the JSON schema)
- JSON (just the original JSON format)

Using this project makes it possible to have reproducible builds of
certain themes for easy deployments on Github pages.

## Getting started

Create your own `resume` repository and run

    nix flake init -t github:TaserudConsulting/jsonresume-nix

to clone the template to use this flake.

In there you get a `defaultPackage` that determines the theme to
use. To build it you can just run `nix build .#builder` and execute
the result like `./result` which will build `resume.nix` into a HTML
output. Note that it's required that this `flake.nix` is part of a git
repository and that you at least stage the `flake.nix` file to be able
to build.

To change the theme used you'd just change the `defaultPackage` used,
to list available packages you just run:

    nix flake show github:TaserudConsulting/jsonresume-nix

Then nix will list available theme wrappers.

### Live preview when building your resume

If you want a live preview of how the final result will look while
filling out your resume schema file, run the following command:

    nix run .#live

## [TODO] Things to do

- [ ] Wrapper script to package themes
- [ ] Wrapper script to update themes
- [ ] Wrapper script to test themes
- [X] Expose themes as packages in flake
- [X] Expose resumed as package in flake
- [ ] Add a flake check that tests all themes
- [ ] Add a flake output to test end users resumes and themes builds
      as flake checks
- [ ] Add a flake output to use as flake init for end users resumes
      repositories
- [ ] Add CI to update flake and themes

## Finding and testing more themes before packaging them

<https://www.npmjs.com/search?q=jsonresume-theme>

Find the theme name, then run `npm install
jsonresume-theme-THEMENAME`, this should install the theme in your
local directory (given that you have `nodejs` available, use
`nix-shell` for this).

Then you should be able to use `nix-shell` to make `resumed` available
as well and test the theme by running:

    resumed render --theme $(pwd)/node_modules/jsonresume-theme-THEMENAME/index.js

The full path seems to be super important here. If this works you can
attempt to package it and expose it in the flake.
