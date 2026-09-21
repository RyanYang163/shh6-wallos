# Wallos

| 项 | 值 |
|---|---|
| 应用 ID | `shh6-wallos` |
| 形态 | Docker 应用（Compose） · WebUI 外开（浏览器新标签） |
| 版本 | 1.0.0 |
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
- 镜像已核实：`bellamy/wallos:5.7.1` 存在，2026-09-10 更新，提供 amd64 + arm64 + arm —— 用具体版本号而非 `latest`（审核禁止 :latest）。
- 需要两个挂载点：/var/www/html/db 与 /var/www/html/images/uploads/logos。
- [ ] 真机安装、启动、停止、卸载残留四项实测
- [ ] 首屏加载 ≤ 5 秒（指引 H10）
- [ ] x86_64 与 aarch64 分别构建并测试（指引 H7）
- [ ] 提交前跑一遍指引 13.9 上架前自查清单

## 隐私政策

见 [PRIVACY.md](./PRIVACY.md)（对应审核项 C3–C8）。

## 许可证与出处

本仓库**仅包含 TOS 平台集成所需的配置文件与打包脚本**，应用本体的源码与二进制来自上游项目：https://github.com/ellite/Wallos

上游许可证：**%s**。本封装保留上游许可证声明，未修改上游代码（Deb 形态下按上游许可证要求随包提供 LICENSE）。

应用名称与图标为上游项目的标识；本仓库图标为自行绘制的简易图形，不含上游商标元素（对应审核项 H19）。
