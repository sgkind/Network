# 编译go文件

```shell
$ go run $GOPATH/src/github.com/openconfig/ygot/generator/generator.go -path=public/release/models/acl,public/release/models/types,public/release/models,public/third_party/ietf -output_file=../internal/pkg/openconfig/platform.go -package_name=openconfig -shorten_enum_leaf_names=true -compress_paths=true -exclude_modules=ietf-interfaces public/release/models/platform/openconfig-platform.yang public/release/models/platform/openconfig-platform-fan.yang public/release/models/platform/openconfig-platform-cpu.yang
```

其中：
- path为yang文件所在的路径
- output_file为生成文件所在的路径
- package_name为