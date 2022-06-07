# The SLE BCI documentation

Welcome to the documentation of the SLE Base Container Images!

The documentation is being build with [hugo](https://gohugo.io/), you can
install hugo locally and build the documentation as follows or use your favorite
container runtime for that:
```ShellSession
# install hugo locally:
❯ hugo server
# or run a container:
❯ podman run --rm -it -v $(pwd):/src:Z -p 1313:1313 klakegg/hugo server
```
