# Wag Documentation Page

The temporary github pages location:

https://nhas.github.io/wag-vpn.github.io/

Github actions will automatically build and deploy on commit, so make sure to test out your changes with a local build first:

```sh
git clone git@github.com:NHAS/wag-vpn.github.io.git
git submodule update --init

hugo server --minify --theme hugo-book
```