# fedora-distroless

Base container based on fedora RPM packages which only contains minimal
requirements for running golang applications.

Right now it includes:

 * glibc

## How to use it

Reference the build in your WORKSPACE file:

```
container_pull(
  name = "golang_base",
  registry = "docker.io",
  repository = "rmohr/fedora-distroless",
  digest = "sha256:59b77524d9c7913914fdba5c1752a2387cf876afeca255786bb17f3e9b0df781",
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
