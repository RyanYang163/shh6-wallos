# Privacy Policy — Wallos

> Review items covered / 适用审核项：C3 (policy provided) · C4 (completeness) · C5 (accessibility) ·
> C6 (data collection consistency) · C7 (user rights) · C8 (third-party disclosure)
>
> Public URL / 公网地址：<https://github.com/RyanYang163/shh6-wallos/blob/main/PRIVACY.md>

**Effective date / 生效日期**：2026-09-22
**Applies to / 适用版本**：1.0.007
**Publisher / 开发者**：shh
**Package / 包名**：`shh6-wallos` (Docker)

---

## 1. Data Collected / 收集哪些数据

Wallos is a self-hosted subscription tracker. All data is created by the user and stays on the
user's own device.

本应用是自托管的订阅管理工具，所有数据均由用户自己在本机创建，不离开设备。

- Subscription records: name, amount, currency, billing cycle, renewal dates
  订阅记录：名称、金额、币种、计费周期、续费日期
- Categories, payment methods and user preferences
  分类、付款方式与个人偏好设置
- Optional custom logos uploaded by the user
  用户自行上传的图标
- Local account credentials (username and a salted password hash) if the user enables login
  若启用登录：本地账号的用户名与加盐密码哈希

Stored in an SQLite database and an upload directory under
`/Volume*/DockerAppData/shh6-wallos/` on the device.

**The developer collects nothing.** No analytics, no telemetry, no crash reporting, no account
with the developer is required or possible.

**开发者不收集任何数据**：无埋点、无遥测、无崩溃上报，也不需要（无法）注册开发者账号。

## 2. Retention Period / 数据保存期限

- Data is retained on the device **for as long as the user keeps it**. There is **no automatic
  expiry and no server-side retention window**, because the developer operates no server.
  数据由用户自行保管，**不设自动过期**，也没有任何「服务端保留期」——开发者没有服务端。
- It is removed when the user deletes the records in the app, or when the app is uninstalled
  with "delete data" selected.
  用户在应用内删除记录、或卸载时勾选「同时删除数据」后即被清除。
- Container logs written by the bundled web server are rotated by the container runtime (Docker
  `json-file` driver) and are removed together with the application data.
  容器日志由 Docker 自身的日志轮转管理，随应用数据一并清除。

## 3. Third-Party Sharing and Data Location / 第三方共享与数据存放地域

- **Default: no third party is involved and no data leaves the device.**
  **默认不涉及任何第三方，数据不出设备。**
- The database is reachable only from the application itself; no database or internal port is
  published to the LAN.
  数据库仅应用自身可访问，不对局域网发布任何数据库或内部端口。
- Optional features are **off by default** and only run if the user configures them; in those
  cases the request goes from the device **directly to the provider the user chose**, under that
  provider's own terms. No copy passes through the developer.
  可选功能**默认关闭**，只有用户自行配置后才生效；此时请求由设备**直连用户选定的服务商**，
  遵循对方条款，**不经过开发者**：

  | Feature / 功能 | Destination / 去处 | Data sent / 发送内容 |
  |---|---|---|
  | Currency exchange rates / 汇率换算 | Fixer API (`fixer.io`), requires a user-supplied API key | currency codes only |
  | Logo search / 图标搜索 | public search engines | the subscription name the user searched for |
  | Notifications / 到期提醒 | user-chosen e-mail / Telegram / Discord / Pushover / Gotify / webhook endpoint | the renewal reminder the user configured |

- **Data location / 数据存放地域**：the device the user installed the app on. The developer
  stores no copy anywhere, in any region.

## 4. Security Measures / 数据安全措施

This is a Docker application; the statements below describe what is actually configured in
`docker-compose.yml` and are verifiable there.

- Application processes run as a **non-root UID (`PUID=1000` / `PGID=1000`)**; the container's
  init only drops privileges to that UID.
- `security_opt: no-new-privileges:true` — no process can gain privileges at runtime.
- **Capability allow-list**: `cap_drop: [ALL]` followed by an explicit `cap_add` list, instead of
  Docker's unrestricted default set. `NET_RAW`, `SETPCAP` and `SETFCAP` are **not** granted.
- No `privileged` mode and no `network_mode: host`; only the single Web UI port (`18806`) is
  published.
- No credentials or secrets are baked into the compose file.
- Data is confined to the declared volumes under `/Volume*/DockerAppData/shh6-wallos/` and is
  never relayed through a developer-operated server.

## 5. User Rights / 用户权利

The user can, at any time and without contacting the developer:

- **Access / 查阅**：view every record in the Web UI, or open the SQLite database directly.
- **Correct / 更正**：edit any record, category, currency or setting in the Web UI.
- **Export / 导出**：use the app's built-in backup to take a full copy of the data.
- **Delete / 删除**：see section 6.

## 6. Deletion Channel / 数据删除途径

1. **In the app / 应用内**：delete individual subscriptions, or use the built-in
   restore/backup page to remove data.
2. **Uninstall with data / 卸载时删除**：uninstall from the TOS App Center and select
   "delete data" — this removes `/Volume*/DockerAppData/shh6-wallos/` entirely.
3. **Manual / 手动**：delete the `/Volume*/DockerAppData/shh6-wallos/` directory on the device.

Uninstalling **without** selecting "delete data" keeps the directory, so the user can reinstall
and continue with their data intact. There is no developer-side copy to delete.

## 7. Contact / 联系方式

- Packaging repository / 本封装仓库：<https://github.com/RyanYang163/shh6-wallos/issues>
- Upstream project / 上游项目：<https://github.com/ellite/Wallos/issues>

Questions about this policy can be raised in either tracker.
