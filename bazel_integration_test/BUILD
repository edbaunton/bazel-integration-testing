load("//tools:common.bzl", "BAZEL_VERSIONS")
load("//:bazel_integration_test.bzl", "bazel_py_integration_test")

py_library(
    name = "python",
    testonly = True,
    srcs = ["test_base.py"],
    visibility = ["//visibility:public"],
)

[
    genrule(
        name = "python_generated_version_src_" + v,
        srcs = [],
        outs = [v + "/bazel_py_integration_test/generated/__init__.py"],
        cmd = "echo 'DEFAULT_BAZEL_VERSION = \"%s\"' >$@ " % v,
    )
    for v in BAZEL_VERSIONS
]

[
    py_library(
        name = "python_version_" + v,
        srcs = [":python_generated_version_src_" + v],
        data = ["@build_bazel_bazel_%s//:bazel" % v.replace(".", "_")],
        imports = [v],
        visibility = ["//visibility:public"],
    )
    for v in BAZEL_VERSIONS
]

bazel_py_integration_test(
    name = "test_base_test",
    srcs = ["test_base_test.py"],
)
