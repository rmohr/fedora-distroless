load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
http_archive(
    name = "io_bazel_rules_go",
    urls = ["https://github.com/bazelbuild/rules_go/releases/download/0.16.2/rules_go-0.16.2.tar.gz"],
    sha256 = "f87fa87475ea107b3c69196f39c82b7bbf58fe27c62a338684c20ca17d1d8613",
)

load("@io_bazel_rules_go//go:def.bzl", "go_rules_dependencies", "go_register_toolchains")

http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "a3af8c8db1c42ee795d68cd58e5f67ddb4bc26f2c224d04b1e961e4d004970ed",
    strip_prefix = "rules_docker-c35eb61745fafef81bb8967eb6fd942713b99e68",
    urls = ["https://github.com/rmohr/rules_docker/archive/c35eb61745fafef81bb8967eb6fd942713b99e68.tar.gz"],
)

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
    container_repositories = "repositories",
)

go_rules_dependencies()
go_register_toolchains()
container_repositories()

http_file(
   name = "glibc",
   url = "https://dl.fedoraproject.org/pub/fedora/linux/releases/28/Everything/x86_64/os/Packages/g/glibc-2.27-8.fc28.x86_64.rpm",
   sha256 = "573ceb6ad74b919b06bddd7684a29ef75bc9f3741e067fac1414e05c0087d0b6"
)
