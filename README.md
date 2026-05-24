# demo

## S-UI 二次开发版安装

本仓库提供 S-UI 二次开发版安装脚本；正式安装包由 `mod196/s-ui` 的 GitHub Releases 分发。

### 推荐安装命令

固定版本安装，适合生产环境可复现部署：

```bash
VERSION=v1.4.2-mod196.1 && bash <(curl -Ls https://raw.githubusercontent.com/mod196/demo/main/s-ui-install.sh) $VERSION
```

未带 `v` 的版本号也支持，脚本会自动补齐：

```bash
VERSION=1.4.2-mod196.1 && bash <(curl -Ls https://raw.githubusercontent.com/mod196/demo/main/s-ui-install.sh) $VERSION
```

安装最新 release：

```bash
bash <(curl -Ls https://raw.githubusercontent.com/mod196/demo/main/s-ui-install.sh)
```

无参数安装会优先读取 `mod196/s-ui` 的 GitHub latest release；如果 GitHub API 限制或当前只有 prerelease，脚本会回退到当前推荐版本 `v1.4.2-mod196.1`。

### 分发源说明

- 安装脚本：`https://raw.githubusercontent.com/mod196/demo/main/s-ui-install.sh`
- Release 仓库：`https://github.com/mod196/s-ui`
- 安装包格式：`https://github.com/mod196/s-ui/releases/download/<VERSION>/s-ui-linux-<arch>.tar.gz`
- 当前推荐版本：`v1.4.2-mod196.1`

### 升级与数据保留

安装脚本会保留已有 `/usr/local/s-ui/db/s-ui.db`，不会用本地测试数据覆盖线上数据。新版首次启动后会由 S-UI/GORM 自动补齐 SQLite schema。

升级前建议先备份：

```bash
TS=$(date -u +%Y%m%dT%H%M%SZ)
BACKUP_DIR=/root/s-ui-backup-$TS
mkdir -p "$BACKUP_DIR"
cp -a /usr/local/s-ui/db/s-ui.db "$BACKUP_DIR/s-ui.db"
cp -a /usr/local/s-ui/sui "$BACKUP_DIR/sui"
cp -a /etc/systemd/system/s-ui.service "$BACKUP_DIR/s-ui.service"
tar -C /root -czf "$BACKUP_DIR.tar.gz" "$(basename "$BACKUP_DIR")"
```

### VPS 总池账期

新 VPS 首次启动新版 S-UI 时，会自动按首次启动 UTC 时间写入 VPS 总池账期起点。

当前线上 VPS 如果需要固定为 `2026-05-23 00:00:00 UTC` 起算，需要在部署后单独写入：

```bash
python3 - <<'PY'
import sqlite3
path = "/usr/local/s-ui/db/s-ui.db"
settings = {
    "trafficPoolAnchorAt": "1779494400",
    "trafficPoolCycleDays": "30",
    "trafficPoolLimitBytes": "1073741824000",
    "trafficPoolSource": "user",
}
conn = sqlite3.connect(path)
cur = conn.cursor()
for key, value in settings.items():
    cur.execute("delete from settings where key = ?", (key,))
    cur.execute("insert into settings(key, value) values (?, ?)", (key, value))
conn.commit()
conn.close()
PY
systemctl restart s-ui
```

## RouteForwarder <https://youtu.be/dpmnkKhBFtc>
## box5magisk <https://youtu.be/oRyjX44Bxw4>
## 01 <https://youtu.be/VONkHvKkCX0>
## 松鼠VPN节点提取相关文件 视频被下架，备份：<https://bulianglin.com/g/songsu/>
## nodesCatch视频教程 <https://youtu.be/aSR6OuqtFdU>
## 不良林 VPN <https://youtu.be/HuyPaM41ytA>
## EasyClash <https://youtu.be/-I5T1G6NdKM> <https://youtu.be/1Xn-tRosThs>
## nodesCatch V2.0视频教程 <https://youtu.be/fHJDvJIptts>

### nodesCatch V2.0目前已知问题：
1、有效节点变无效：一般是vmess节点，导入节点使用的是subconverter进行节点格式转换，但是windows版的subconverter有个bug，转换clash后会将vmess节点的aid参数丢失，如果你是直接粘贴clash配置文件到节点列表，可能会导致本来可以使用的vmess节点测速显示无效，目前的解决方法是直接粘贴url格式的节点到节点列表再测速，剩下的只有等工具作者修复

2、无法导入节点：和clash.net有冲突，因为clash.net也内置了subconverter，解决方法是直接删掉测速软件目录里的subconverter目录，但是这样的话在使用测速软件时就必须运行clash.net。或者先退出clash.net再测速

3、切换测速配置文件失败：clash内核不允许 h2/grpc 的节点tls为false，解决方法是将传输协议为h2或者grpc的节点删除或者使用Xray内核测速

## 订阅转换盗取节点 <https://youtu.be/u-tg9hJHLO0>
