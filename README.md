# Sub-Parser
**Sub-Parser** 是一个简单高效的 **订阅节点聚合解析工具**，专为 V2Ray、Clash 等代理客户端设计。
它能自动从远程订阅列表抓取多个订阅源，解析出有效节点（支持 vmess、vless、trojan、ss、ssr、hysteria2 等常见协议），进行去重、地理信息标记（国旗 + 中文地区），并生成四种常用格式的输出文件。
项目通过 **GitHub Actions** 实现自动化运行，适合个人或小范围分享干净、聚合的节点订阅。
## 主要功能
- **节点解析与聚合**：
  - 支持常见协议的链接提取与解析。
  - 使用 `GeoLite2-Country.mmdb` 自动添加国家旗帜和中文地区名称。
  - 节点统一重命名（格式示例：`🇺🇸 美国 订阅节点聚合器_xxxx`）。
  - 自动去重（基于核心链接）。
- **性能与稳定性**：
  - 异步并发抓取（最高 80 个并发）。
  - 简单重试机制。
  - 仅处理有效节点，提高输出质量。
## 项目文件结构（主要）
- `sub_parser.py`：核心 Python 脚本（基于 asyncio + aiohttp）。
- `GeoLite2-Country.mmdb`：GeoIP 数据库（用于地区识别）。
- `.github/workflows/`：GitHub Actions 工作流（自动化触发）。

注意事项
需要 GeoLite2-Country.mmdb 文件（可从 MaxMind 官网下载免费 GeoLite2 Country 版）。
输出节点已统一重命名，便于在客户端中识别和管理。
本项目仅为解析聚合工具，不提供任何订阅源内容。
许可证
MIT License（或其他你喜欢的开源协议）

欢迎 Star / Fork！

有问题、建议或需要新增协议支持，欢迎提交 Issue 或 Pull Request。
