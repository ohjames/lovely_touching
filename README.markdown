# A tiny init for docker containers

Hey maybe your Docker file contains something like this:

```
CMD ["/bin/node", "app.js"]
```

Oh no [this is no good!](https://blog.phusion.nl/2015/01/20/docker-and-the-pid-1-zombie-reaping-problem/) - TLDR: Your process can get shutdown in a bad way and *zombie processes* will eat your container!

Instead just use `lovely_touching`:

1. Download it (this binary will work on any Linux version from Centos/Redhat 5 and up):

    ```
    wget https://github.com/ohjames/lovely_touching/releases/download/v0.1.0/lovely_touching
    chmod a+x lovely_touching
    ```

2. Change your `Dockerfile` to this:

    ```
    ADD lovely_touching /bin/lovely_touching
    CMD ["/bin/lovely_touching", "--", "/bin/node", "app.js" ]
    ```

Now you don't have to worry anymore!

`lovely_touching` is written in `rust` so you don't need anything installed in the host machine to use the binary.

## Building

```
cargo build --release
```

Or to build a copy against Centos5's glibc (so it can run on more machines) first install docker, then run this as a user with access to docker:

```
./build-release.sh
```

## More stuff

If you want to run multiple processes you can separate them with the argument `---`:
```
ADD lovely_touching /bin/lovely_touching
CMD ["/bin/lovely_touching", "--", "/bin/runit", "---", "/bin/node", "app.js" ]
```

If you want an init system with even less overhead then try [smell-baron](https://github.com/ohjames/smell-baron) which implements the same functionality in C. The binary ends up being around 8k instead of the 800k needed by the rust runtime.
