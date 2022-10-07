# openfyde下怎样创建一个新的board

cd ~/r102/openfyde/manifest

设置一个自己的远程git仓库，创建一个新的 nodpc.xml 文件 看起来像下面这样

```bash
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
  <remote name="ycgkwyc"
    fetch="ssh://git@github.com/ycgkwyc" />

  <project path="openfyde/overlays/overlay-amd64-nodpc"
    name="overlay-amd64-nodpc"
    revision="refs/heads/r102-dev"
    groups="fydeos,fydeos_overlays,board"
    remote="ycgkwyc">
    <linkfile src="." dest="src/overlays/overlay-amd64-nodpc" />
  </project>

</manifest>
```

fork overlay-amd64-openfyde 到自己的远程仓库并把所有原本文件内所有名字改为overlay-amd64-nodpc 

例如这几个文件里 stable.conf / model.yaml / layout.conf / profile.bashrc 

然后
```bash
(outside)
repo sync overlay-amd64-nodpc
```
这时候在~/r102/openfyde/overlays内你应该可以看到新的board文件跟你自己的远程仓库内容一致

# 升级内核到V5.10
```bash
(inside)
FEATURES="noclean" cros_workon --board=amd64-openfyde start chromeos-kernel-5_10
FEATURES="noclean" cros_workon_make --board=amd64-openfyde --install chromeos-kernel-5_10
```
