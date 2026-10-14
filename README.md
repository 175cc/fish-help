# fish-help

闲鱼猪手（`com.skyhand.fishhelper`）的远程配置仓库。

模块启动时会匿名拉取本仓库的 raw 文件，被拉取的路径固定为：

```
config-a40.json
release/adapter/a40.json
release/adapter/a<版本号>.json
tip.json
```

因此仓库必须是**公开**的，且默认分支必须是 **master**（模块硬编码 `/raw/master/`）。

## 目录

| 路径 | 作用 |
| --- | --- |
| `config-a40.json` | 默认开关模板，含 `maxAdapt` / `beta.maxAdapt` 版本上限 |
| `tip.json` | 弹窗提示文案 |
| `release/adapter/a40.json` | 版本索引，记录每个版本对应的 `v`（修订号）与 `u`（自定义地址） |
| `release/adapter/a<版本>.json` | 单个版本的适配表，键为语义名，值为混淆后的真实类名/方法名/字段名 |

## 适配表加密

明文每个字符的码点 **+12**，再按 UTF-8 编码。

```python
# 解密
plain = ''.join(chr(ord(c) - 12) for c in raw.decode('utf-8'))
```

## 添加新版本适配

1. 在 `release/adapter/` 下按上游最新适配表为模板，创建 `a<版本>.json`
2. 逐项核对真实类名/方法名/字段名，把失效项替换为新版对应项
3. 在 `release/adapter/a40.json` 索引里加一条：`"a<版本>": {"v": 1, "u": ""}`
4. 把 `config-a40.json` 的 `maxAdapt` 与 `beta.maxAdapt` 提到新版本

## 当前状态

已加入 **7.25.51** 适配（`release/adapter/a7.25.51.json`）。
