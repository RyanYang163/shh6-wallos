# Wallos

| 项 | 值 |
|---|---|
| 应用 ID | `shh6-wallos` |
| 形态 | Docker 应用（Compose） · WebUI 外开（浏览器新标签） |
| 版本 | 1.0.007 |
| 上游项目 | https://github.com/ellite/Wallos |
| 上游许可证 | GPL-3.0 |
| 宿主端口 | 18806 |

## 简介

订阅与账单管理：记录周期性支出、到期提醒、分类统计。

## 打包

```bash
./build.sh                # 默认 x86_64
./build.sh aarch64        # ARM（Deb 应用）
```

产物在 `build/output/`，同级生成 `<包名>.sha256`。

## 提交前必办事项

- ⚠️ 上游 2026 年有多个 High 级 advisory（SSRF、Zip Slip、OIDC 账号接管、未认证数据库替换等），必须锁定已修复版本。
- 镜像已核实：`bellamy/wallos:5.8.1` 存在，2026-09-18 更新，提供 amd64 + arm64 + arm —— 用具体版本号而非 `latest`（审核禁止 :latest）。
  > 2026-09-22 由 `5.7.1` 升到 `5.8.1`：官方审核 `[T1]` 要求「plan to update the image tag to a patched
  > release in a subsequent version」，5.8.1 是当时 Docker Hub 上最新的稳定标签。
- 需要两个挂载点：/var/www/html/db 与 /var/www/html/images/uploads/logos。
- [ ] 真机安装、启动、停止、卸载残留四项实测
- [ ] 首屏加载 ≤ 5 秒（指引 H10）
- [ ] x86_64 与 aarch64 分别构建并测试（指引 H7）
- [ ] 提交前跑一遍指引 13.9 上架前自查清单

## 隐私政策（审核项 C3 / C4 / C5）

**公网地址（可直接访问）**：<https://github.com/RyanYang163/shh6-wallos/blob/main/PRIVACY.md>

- `config.ini` 的 `help` 字段就指向该地址 —— 平台应用详情页的「帮助」即可直达，因此**包内可查**。
- 仓库内全文：[`PRIVACY.md`](./PRIVACY.md)。
- 已覆盖 C4 要求的全部要素：数据收集范围、**保存期限**、第三方共享与**数据存放地域**、
  安全措施、**用户权利**、**删除途径**、联系方式。

## 运行时写入路径清单（指引 12.9.6）

| 路径 | 由谁创建 | 内容 | 保留策略 |
|---|---|---|---|
| `/Volume*/DockerAppData/shh6-wallos/data` | 应用（SQLite） | 订阅记录、账号、设置 | 随数据保留，不自动过期 |
| `/Volume*/DockerAppData/shh6-wallos/logos` | 用户后台上传 | 自定义图标 | 同上 |
| 容器内 `/var/log`、`/run` | 镜像自带 nginx / php-fpm / crond | 运行期日志与 pid | 随容器生命周期 |

应用**不写入本清单以外的路径**。

## 权限与最小化（对应 V1 豁免申请）

本应用**不写 `user:` 字段**：`bellamy/wallos` 的 `startup.sh` 需要 root 完成 `groupmod` / `usermod` /
`chown`（把 `www-data` 改成 `PUID/PGID`）并启动 `crond`，且脚本开头是 `set -euo pipefail`，
去掉 root 会直接退出。等价的最小权限措施：

- `PUID=1000` / `PGID=1000` —— **PHP 进程实际以 uid 1000 运行**；root 只用于容器初始化与绑定 80 端口；
- `security_opt: no-new-privileges:true`；
- `cap_drop: [ALL]` + `cap_add` 白名单（**不授予** `NET_RAW` / `SETPCAP` / `SETFCAP`）；
- 无 `privileged`、无 `network_mode: host`，只发布一个 Web UI 端口 18806。

豁免申请材料见 `../../../开发应用计划/TOS社区应用V1豁免申请邮件-shh6-wallos.md`。

## 许可证与出处

本仓库**仅包含 TOS 平台集成所需的配置文件与打包脚本**，应用本体的源码与二进制来自上游项目：https://github.com/ellite/Wallos

上游许可证：**GPL-3.0**。本封装保留上游许可证声明，未修改上游代码（Deb 形态下按上游许可证要求随包提供 LICENSE）。

应用名称与图标为上游项目的标识；本仓库图标为自行绘制的简易图形，不含上游商标元素（对应审核项 H19）。
