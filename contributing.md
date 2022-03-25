# Contributing

To contribute you should read through our guidelines to make it easier to get pull requests
approved.

The following specific topics should be are important:

* [Naming](./naming.md)
* [Standardization](./standardization.md)
  * [C#](csharp.md)

## Defaults

We manage the static code analysis rules in the following [repository](https://github.com/aksio-system/Defaults).
If you're creating a new project, you should add a reference to this in your project - read the [getting started guide](https://github.com/aksio-system/Defaults#getting-started).

## Pull Requests

All contributions are considered only through pull requests to the repository you're contributing to.
Anyone reviewing will keep in mind the guidelines described here.

A contribution can trigger a versioned release. The versioning is adhering to [semantic versioning version 2](https://semver.org)
and leveraging our own [release action](https://github.com/aksio-system/release-action) for this.
That means that the pull request will have to be labeled with what version change it constitutes:

| Name | Description |
| ---- | ----------- |
| Major | Breaking changes has been implemented in public APIs and/or behavior |
| Minor | New capabilities has been added |
| Patch | Bug fixes |

If none of these labels are present, it isn't considered to be a release.
