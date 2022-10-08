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

fork overlay-amd64-openfyde 到自己的远程仓库并把所有原本文件内所有名字改为 overlay-amd64-nodpc 

例如这几个文件里 stable.conf / model.yaml / layout.conf / profile.bashrc 

然后
```bash
(outside)
repo sync overlay-amd64-nodpc
```
这时候在~/r102/openfyde/overlays内你应该可以看到新的board文件跟你自己的远程仓库内容一致

# 升级内核到V5.10
修改 stable.conf 文件
```bash
CHROMEOS_KERNEL_CONFIG="/mnt/host/source/src/overlays/overlay-amd64-nodpc/kconfig/fydeos_def_r102_5.4_test"
USE="${USE} -kernel-4_4 -kernel-4_14 -kernel-5_4 kernel-5_10"
```
# 进入cros_sdk
```bash
(inside)
FEATURES="noclean" cros_workon --board=amd64-nodpc start chromeos-kernel-5_10
FEATURES="noclean" cros_workon_make --board=amd64-nodpc --install chromeos-kernel-5_10
```
你可能会遇到一些驱动错误提示，根据提示解决即可 一般通过自己编写补丁文件放入自己的板下

```bash
(inside)
$ setup_board --board=amd64-nodpc --force
```
Install package dev-libs/capnproto
```bash
(inside)
sudo emerge capnproto
```
# Build packages
```bash
(inside)
$ ./build_packages --board=amd64-nodpc --nowithautotest --autosetgov --nouse_any_chrome
```
# Build the disk image
```bash
(inside)
$ ./build_image --board=amd64-nodpc --noenable_rootfs_verification
```
The disk image is usually named chromiumos_image.bin, under the abovementioned directory. So full path to the latest image is
```bash
~/r102/src/build/images/amd64-openfyde/latest/chromiumos_image.bin
```
