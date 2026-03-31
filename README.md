# Sub-Parser
**Sub-Parser** 是一个简单高效的 **订阅节点聚合解析工具**，专为 V2Ray、Clash 等代理客户端设计。
它能自动从远程订阅列表（`filter_subs.txt`）抓取多个订阅源，解析出有效节点（支持 vmess、vless、trojan、ss、ssr、hysteria2 等常见协议），进行去重、地理信息标记（国旗 + 中文地区），并生成四种常用格式的输出文件。
项目通过 **GitHub Actions** 实现自动化运行，适合个人或小范围分享干净、聚合的节点订阅。
## 主要功能
- **远程加载订阅列表**：从环境变量 `FILTER_URL`（推荐使用 GitHub Secrets）读取 `filter_subs.txt`，避免代码中硬编码 URL。
- **节点解析与聚合**：
  - 支持常见协议的链接提取与解析。
  - 使用 `GeoLite2-Country.mmdb` 自动添加国家旗帜和中文地区名称。
  - 节点统一重命名（格式示例：`🇺🇸 美国 订阅节点聚合器_xxxx`）。
  - 自动去重（基于核心链接）。
- **输出文件**（每次运行自动生成/覆盖）：
  - `sub_parser.txt`：明文节点链接（一行一个）。
  - `sub_parser_base64.txt`：Base64 编码的订阅链接（可直接导入许多客户端）。
  - `sub_parser.csv`：成功订阅源统计表（**仅保留节点数量 ≥ 1 的源**，0 节点源直接丢弃）。
  - `sub_parser.yaml`：Clash 格式的 `proxies` 配置（带基础规则模板）。
- **性能与稳定性**：
  - 异步并发抓取（最高 80 个并发）。
  - 简单重试机制。
  - 仅处理有效节点，提高输出质量。
## 项目文件结构（主要）
- `sub_parser.py`：核心 Python 脚本（基于 asyncio + aiohttp）。
- `GeoLite2-Country.mmdb`：GeoIP 数据库（用于地区识别）。
- `.github/workflows/`：GitHub Actions 工作流（自动化触发）。
- 输出文件（运行后生成）：
  - `sub_parser.txt`
  - `sub_parser_base64.txt`
  - `sub_parser.csv`
  - `sub_parser.yaml`
## 使用方式
### 1. 本地运行
1. 安装依赖：
   ```bash
   pip install aiohttp geoip2
将 GeoLite2-Country.mmdb 放置到脚本同目录。
设置环境变量：
<BASH>
# Linux / macOS
export FILTER_URL=https://your-filter-subs-url
# Windows
set FILTER_URL=https://your-filter-subs-url
运行脚本：
<BASH>
python sub_parser.py
2. GitHub Actions 自动化（推荐）
在仓库 Settings → Secrets and variables → Actions 添加 Secret：
Name: FILTER_URL
Value: 你的订阅列表地址（例如 https://raw.githubusercontent.com/.../filter_subs.txt）
在 .github/workflows/sub_parser.yml 中注入环境变量：
<YAML>
- name: Run sub_parser
  env:
    FILTER_URL: ${{ secrets.FILTER_URL }}
  run: python sub_parser.py
可设置定时触发（cron）或手动触发。
3. 订阅列表格式
filter_subs.txt 中每行一个有效的 HTTPS 订阅链接（支持明文或 Base64 格式）。

注意事项
需要 GeoLite2-Country.mmdb 文件（可从 MaxMind 官网下载免费 GeoLite2 Country 版）。
脚本会自动过滤无效订阅源（0 节点），只在 CSV 中记录有效源。
输出节点已统一重命名，便于在客户端中识别和管理。
本项目仅为解析聚合工具，不提供任何订阅源内容。
许可证
MIT License（或其他你喜欢的开源协议）

欢迎 Star / Fork！

有问题、建议或需要新增协议支持，欢迎提交 Issue 或 Pull Request。
