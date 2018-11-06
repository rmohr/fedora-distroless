# fedora-distroless

Base container based on fedora RPM packages which only contains minimal
requirements for running golang applications.

Right now it includes:

 * glibc
 * ca-certificates

## How to use it

Reference the build in your WORKSPACE file:

```
container_pull(
  name = "golang_base",
  registry = "docker.io",
  repository = "rmohr/fedora-distroless",
  digest = "sha256:ce795c79e8f735487fe3f25c2717701b9ee6e595479f30ebe53b948138fe50c4",
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
