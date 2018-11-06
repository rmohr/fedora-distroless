# fedora-distroless

Base container based on fedora RPM packages which only contains minimal
requirements for running golang applications.

Right now it includes:

 * glibc
 * ca-certificates
 * Root user entry
 * Basic nsswitch configuration
 * An empty tmp folder

## How to use it

Reference the build in your WORKSPACE file:

```
container_pull(
  name = "golang_base",
  registry = "docker.io",
  repository = "rmohr/fedora-distroless",
  digest = "sha256:13c2c937d0a0eef49243d307b3087333402056bf5da75c5baf01929687e11bef",
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
