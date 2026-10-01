# Surfing 远程 Mihomo 配置模板

**中文** | [English](README.en.md)

用于 Surfing Android 模块的脱敏配置模板。仓库只发布 [`config.yaml`](config.yaml)，真实代理订阅和手机上的运行配置仍由设备本地管理。

## 工作方式

1. 设备下载 `main` 分支的远程模板。
2. 本地更新流程将设备保留的订阅区块合并进模板。
3. 使用设备实际安装的 Mihomo 核心校验候选配置。
4. 校验通过后保存备份并重启 Surfing。

原有设备更新流程按 6 小时检查变更；**本仓库没有附带更新器或安装脚本**，不能单靠克隆仓库实现自动更新。Mihomo provider 的 `interval: 86400` 是订阅刷新周期，不是模板更新周期。

## 使用与发布

```sh
git clone https://github.com/lauipaui/surfing-config.git
cd surfing-config
```

远程模板地址：

```text
https://raw.githubusercontent.com/lauipaui/surfing-config/main/config.yaml
```

- 在本地审查候选改动，保留“远程订阅配置开始/结束”的区块标记。
- **不要将 `__LOCAL_SUBSCRIPTION_URL__` 替换成真实订阅后提交到 GitHub。**
- 不要把模板直接当作最终运行文件；先合并本地订阅，再用目标设备核心校验。
- 经审查并合并到 `main` 后才会更新上述公开地址；未合并的 PR 不会发布到设备。

设备端校验示例（路径与配置需按实际安装调整）：

```sh
mihomo -t -f /path/to/merged-config.yaml
```

本仓库没有通用 CI 或核心二进制。YAML 能被解析不代表当前核心支持所有字段、规则集能下载或订阅能连通。

## 网络与安全注意事项

当前模板启用 `allow-lan: true`，控制接口监听 `0.0.0.0:9090`，`secret` 为空。部署前在**本地配置**设置认证、限制监听地址或通过防火墙限制来源，避免在不可信 Wi-Fi 或共享网络暴露控制接口。不要把用于本地认证的真实密钥提交到公开仓库。

模板还关闭 IPv6 和 keep-alive，并包含远程规则集与 UI 下载地址。这些是当前模板的选择，不是所有设备的最佳设置；应按核心版本、代理模式、功耗和稳定性需求单独验证。该模板不是 BoxProxy eBPF 专用配置。

## 回滚与排查

- 校验失败：保留正在运行的旧配置，不要强制重启。
- 更新后异常：使用设备更新流程保存的本地备份恢复；本仓库不实现一键回滚。
- 拉取失败：分别检查 GitHub raw、订阅及规则集地址的 DNS/HTTPS 可达性。
- 节点为空：确认本地订阅区块确实完成合并，不能用占位符 URL 拉取节点。

## 来源与许可

Mihomo、Surfing、规则集和 Zashboard 归各自项目所有。本仓库未提供独立 `LICENSE`，请分别核对所用组件和规则数据的许可；公开可见不等于任意再许可授权。
