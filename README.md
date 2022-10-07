# overlay-amd64-nodpc

# openfyde下怎样创建一个新的board针对r102-dev

cd ~/r102/openfyde/manifest
创建一个新的 nodpc.xml 文件 看起来像下面这样

```bash
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
  <remote name="nodpc"
    fetch="ssh://git@github.com/nodpc" />

  <project path="openfyde/overlays/overlay-amd64-nodpc"
    name="overlay-amd64-nodpc"
    revision="refs/heads/r102-dev"
    groups="fydeos,fydeos_overlays,board"
    remote="nodpc">
    <linkfile src="." dest="src/overlays/overlay-amd64-nodpc" />
  </project>

</manifest>
```

