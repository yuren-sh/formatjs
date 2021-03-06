load("@build_bazel_rules_nodejs//:index.bzl", "generated_file_test", "pkg_npm")
load("//tools:index.bzl", "ts_compile")
load("//tools:jest.bzl", "jest_test")

PACKAGE_NAME = "cli"

pkg_npm(
    name = PACKAGE_NAME,
    package_name = "@formatjs/%s" % PACKAGE_NAME,
    srcs = [
        "LICENSE.md",
        "README.md",
    ],
    deps = [
        "bin/formatjs",
        ":dist",
    ],
)

alias(
    name = "publish",
    actual = "%s.publish" % PACKAGE_NAME,
)

alias(
    name = "pack",
    actual = "%s.pack" % PACKAGE_NAME,
)

SRCS = glob(["src/**/*.ts"]) + [
    "index.ts",
]

SRC_DEPS = [
    "//packages/intl-messageformat-parser:dist",
    "//packages/ts-transformer:dist",
    "@npm//@types/node",
    "@npm//fs-extra",
    "@npm//@types/fs-extra",
    "@npm//fast-glob",
    "@npm//commander",
    "@npm//loud-rejection",
    "@npm//chalk",
    "@npm//json-stable-stringify",
    "@npm//@types/json-stable-stringify",
]

ts_compile(
    name = "dist",
    package_name = "@formatjs/%s" % PACKAGE_NAME,
    srcs = SRCS,
    deps = SRC_DEPS,
)

jest_test(
    name = "unit",
    srcs = [
        "package.json",
        "tests/extract/unit.test.ts",
        "tests/extract/__snapshots__/unit.test.ts.snap",
    ] + SRCS,
    deps = [
        "//packages/intl-messageformat-parser:types",
        "//packages/ts-transformer:types",
        "@npm//rimraf",
        "@npm//@types/rimraf",
    ] + SRC_DEPS,
)

jest_test(
    name = "integration",
    srcs = [
        "package.json",
        "bin/formatjs",
    ] + SRCS + glob(
        [
            "tests/**/*",
        ],
        exclude = ["tests/extract/unit.test.ts"],
    ),
    flaky = True,
    deps = [
        "//packages/intl-messageformat-parser:types",
        "//packages/ts-transformer:types",
        "@npm//rimraf",
        "@npm//@types/rimraf",
        ":dist",
    ] + SRC_DEPS,
)

generated_file_test(
    name = "tsconfig_json",
    src = "tsconfig.json",
    generated = "//tools:tsconfig.golden.json",
)
