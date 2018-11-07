# fedora-distroless

Base container based on fedora RPM packages which only contains minimal
requirements for running golang applications.

Right now it includes:

 * glibc
 * ca-certificates
 * Root user entry
 * Basic nsswitch configuration
 * An empty tmp folder

## Why

This is inspired by
[distroless](https://github.com/GoogleContainerTools/distroless).

Using small containers but still using RPMs gives some key advantages for CI/CD:

 * Smaller size means faster recovery while only using minimal network and
   storage resources. This is exactly what is needed in the case that clusters
   are already in trouble anyway.
 * Small size means that pre-distributing the small images is fast and does not need
   much disk space.
 * Exclusively adding used runtime dependencies to the image reduces the attack
   surface.
 * Exclusively adding used runtime dependencies to the image reduces the need
   to rebuild because of CVEs while at the same time allows faster and more
   painless role-out of new versions, because of the small image size.
 * Using maintained RPMs as source still makes updating easily in case of CVEs
   which affect the runtime dependencies of the container.
 * Easier to create reproducible builds, since updating to newer base images of
   a whole OS typically adds and removes a lot of things in an intransparent way.

There is only one disadvantage:

 * Debugging is harder, since `docker exec` or `kubectl exec` will typically
   not work. However it is possible to copy a statically compiled shell into
   running containers.

## How to use it

Reference the build in your WORKSPACE file:

```
container_pull(
  name = "golang_base",
  registry = "docker.io",
  repository = "rmohr/fedora-distroless",
  digest = "sha256:ef94c65fcb3c9d328439aef271ba3dd20a3091fee241ada8818ecdcc2478e3c1",
)
```

Then use it as base for your golang binaries in your BUILD files:

```
go_image(
    name = "go_image",
    srcs = ["main.go"],
    importpath = "github.com/your/path/here",
    goarch = "amd64",
    goos = "linux",
    pure = "on",
    base = "@golang_base//image",
)
```
