# 快速入门

## Build已定义的所有image:

```shell
./release
# 指定 -p 参数: ./release -p的话同时还会自动push所有镜像
```


# Reference

## [config.yaml](config.yaml)

定义了所有镜像build&push时的一些通用设置

### uploads

#### destination

Build后的镜像将会上传的目标列表

如: `harbor.custom.cn/foo`

### strategy

定义了镜像Build出来后的path

当前只有两个字段会被自动格式化：

- `$BASE` : Base目录, 比如`alpine/Dockerfile.OpenCV`的`$BASE`就是`alpine`
- `$STAGE` : Docker的name, 比如`Dockerfile.OpenCV`的`$STAGE`就是`opencv`

## detail.yaml

每个文件夹里如果包含了`detail.yaml`就会被[release](release)脚本认为是预定义好的可以产出Docker镜像的工作目录。
里面定义了镜像的编译顺序以及每个镜像的单独配置。示范:  **[alpine/detail.yaml](alpine/detail.yaml)**

_(就算文件夹里有Dockerfile文件但是如果没有包含`detail.yaml`的话就不会被处理)_

### produce

镜像会严格按照该list的顺序来build，同时包含了一些子设置。

范例：

```yaml
#an example base/detail.yaml
produce:
  - Foo #  base/Dockerfile.Foo 会第一个被build，产出 base/foo:latest
  - Bar: # base/Dockerfile.Bar 会第二个被buid，产出 bar/base:0.0.1
    strategy: os_depend
    version: "0.0.1"
  - Mizuha: # base/Dockerfile.Mizuha 会最后一个build，产出 mizuha:latest
    strategy: base_stage
```
