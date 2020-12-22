## [中文说明](README.CN.md)

# QuickStart

## To build all images defined:

```shell
./release
# use ./release -p to also push built images
```

# Reference

## [config.yaml](config.yaml)

Defines the most common configuration while building & pushing.

## uploads
-----
### destination
Lists of Remote Base URL for image uploading.

Example: `harbor.custom.cn/foo`

### strategy

Defines how the built images would be named.

Currently only two certain string will auto-formatted:

- `$BASE` : The root dir, for example we have a `alpine/Dockerfile.OpenCV` and `$BASE` will be formatted to `alpine`
- `$STAGE` : The docker image name, for example we have a `Dockerfile.OpenCV` and `$STAGE` will be formatted to `opencv`

-----
## detail.yaml

Every dir contains this file will be considered as a pre-defined dir that can produce docker images. Defines what would be built and image specified configuration. Working example: **[alpine/detail.yaml](alpine/detail.yaml)**

_(Directories without this will not process in the `release` script)_

### produce

This list just defines the build order of images and other configs.

```yaml
#an example base/detail.yaml
produce:
  - Foo # will build Dockerfile.Foo firstly as base/foo:latest
  - Bar: # will build Dockerfile.Bar secondly as bar/base:0.0.1
    strategy: os_depend
    version: "0.0.1"
  - Dah: # will build Dockerfile.Dah thirdly as dah:latest
    strategy: base_stage
```
