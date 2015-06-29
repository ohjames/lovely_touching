# A tiny init for docker containers

Hey maybe your Docker file contains something like this:

```
CMD ["/bin/node", "app.js"]
```

Oh no [this is no good!](https://blog.phusion.nl/2015/01/20/docker-and-the-pid-1-zombie-reaping-problem/): TLDR: Your process can get shutdown in a bad way and *zombie processes* will destroy you!

Instead just use mr\_t\_int and change your `Dockerfile` to this:

```
ADD mr_t_init /bin/mr_t_init
CMD ["/bin/mr_t_init", "--", "/bin/node", "app.js" ]
```

Now you don't have to worry anymore!

`mr_t_init` is written in `rust` so you don't need anything installed in the host machine to use the binary.