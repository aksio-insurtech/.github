# Contributing

## Pull Requests

All contributions are considered only through pull requests to the repository you're contributing to.
Anyone reviewing will keep in mind the guidelines described here.

A contribution can trigger a versioned release. The versioning is adhering to [semantic versioning version 2](https://semver.org)
and leveraging our own [release action](https://github.com/aksio-system/release-action) for this.
That means that the pull request will have to labeled with what version change it constitutes:

| Name | Description |
| ---- | ----------- |
| Major | Breaking changes has been implemented in public APIs and/or behavior |
| Minor | New capabilities has been added |
| Patch | Bug fixes |

If none of these labels are present, it doesn't consider this to be a release.
