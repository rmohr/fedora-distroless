package(default_visibility = ["//visibility:public"])

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_image", "container_push",
    container_repositories = "repositories",
)

load("@io_bazel_rules_docker//contrib:passwd.bzl", "passwd_entry", "passwd_tar")
load("@io_bazel_rules_docker//contrib:group.bzl", "group_entry", "group_file")
load("@bazel_tools//tools/build_defs/pkg:pkg.bzl", "pkg_tar")
load("@io_bazel_rules_docker//go:image.bzl", "go_image")

# Create /etc/passwd with the root user
passwd_entry(
    name = "root_user",
    gid = 0,
    uid = 0,
    username = "root",
)

passwd_tar(
    name = "passwd",
    entries = [
        ":root_user",
    ],
    passwd_file_pkg_dir = "etc",
)

# Create /etc/group with the root group
group_entry(
    name = "root_group",
    gid = 0,
    groupname = "root",
)

group_file(
    name = "group",
    entries = [
        ":root_group",
    ],
)

pkg_tar(
    name = "group_tar",
    srcs = [":group"],
    mode = "0644",
    package_dir = "etc",
)

container_image(
    name = "fedora_distroless",
    rpms = ["@glibc//file", "@ca_certificates//file"],
    tars = [
            ":group_tar",
            ":passwd",
            ":nsswitch.tar",
            ":tmp.tar",
            ],
    env = {
        "PATH": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
    },
)

go_image(
    name = "go_image",
    srcs = ["test.go"],
    importpath = "github.com/your/path/here",
    goarch = "amd64",
    goos = "linux",
    pure = "off",
    base = ":fedora_distroless",
)

container_push(
    name = "publish",
    format = "Docker",
    registry = "index.docker.io",
    repository = "rmohr/fedora-distroless",
    image = ":fedora_distroless",
    # Trigger stamping.
    stamp = True,
)
