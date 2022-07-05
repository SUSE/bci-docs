# The SLE BCI documentation

Welcome to the documentation of the SLE Base Container Images!

## Local Setup

The documentation relies on both [Hugo](https://gohugo.io/), the static site generator, and nodejs which is used in the theme.

When you checkout the codebase you need to be sure to get the submodules. The theme is setup as a submodule. If you checked out the code without updating the submodules you can run the following command to get the theme as a submodule:

```console
git submodule update --init --recursive
```

Once the theme is available you will need to install JavaScript dependencies and build them. This can be done with the following commands:

```console
cd themes/hugo-geekdoc
npm install
npm run build
cd ../..
```

From here you can run Hugo as a server to see the served documentation locally. You can run it from the CLI with:

```console
hugo server
```

Alternatively, you can run it as a container. For example:

```console
podman run --rm -it -v $(pwd):/src:Z -p 1313:1313 klakegg/hugo server
```
