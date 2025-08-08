# 🎵 Spotify 全球价格追踪器

> 自动抓取全球 Spotify 订阅价格，实时汇率转换，找出最优惠的订阅地区

[![自动更新](https://img.shields.io/badge/自动更新-每周-brightgreen)](https://github.com/SzeMeng76/spotify-prices/actions)
[![价格数据](https://img.shields.io/badge/国家数量-50+-blue)](#)
[![货币](https://img.shields.io/badge/转换为-人民币-red)](#)

**🌐 语言**: [English](README.md) | [中文](README_zh.md)

## ✨ 核心功能

| 功能 | 描述 |
|------|------|
| 🌍 **全球价格抓取** | 自动抓取全球 50+ 国家的 Spotify Premium 价格 |
| 💱 **实时汇率转换** | 集成汇率 API，所有价格实时转换为人民币 |
| 🏆 **智能排序分析** | 按 Premium Family 价格排序，一键找出最便宜的订阅地区 |
| 📊 **标准化数据** | 多语言套餐名称标准化（如 "Premium Familiar" → "Premium Family"） |
| 🎯 **排行榜系统** | **新功能！** 月付和预付费套餐 Top 10 排行榜，详细价格对比 |
| 💳 **预付费支持** | **新功能！** 全面支持预付费订阅（1年、6个月等），提供总成本分析 |
| 🤖 **自动化运行** | GitHub Actions 每周日自动运行，无需人工干预 |
| 📈 **历史数据** | 按年份自动归档，支持价格趋势分析 |
| 📊 **价格变化追踪** | 自动检测价格变化并记录详细的变更日志 |
| 🗂️ **智能归档管理** | 月度归档系统，自动整理变更记录保持清晰 |

## 🚀 快速开始

### 前置要求
- Python 3.9+
- 免费的 [OpenExchangeRates API Key](https://openexchangerates.org/)

### 一键运行
```bash
# 1. 克隆项目
git clone <your-repo-url>
cd spotify-price-tracker

# 2. 安装依赖
pip install -r requirements.txt
playwright install

# 3. 配置 API 密钥
cp .env.example .env
# 编辑 .env 文件，添加你的 API_KEY

# 4. 运行完整流程
python spotify.py                    # 爬取价格数据
python spotify_rate_converter.py     # 转换汇率并排序
python changelog_archiver.py         # 归档旧的变更记录（月度执行）
```

### 🔑 API 密钥配置

**本地开发：**
```bash
# .env 文件
API_KEY=your_openexchangerates_api_key
```

**GitHub Actions：**
1. 仓库 Settings → Secrets and variables → Actions
2. 添加 Secret: `API_KEY` = `your_api_key`

> 💡 **获取免费 API 密钥**：访问 [OpenExchangeRates](https://openexchangerates.org/) 注册，每月 1000 次免费请求

## 🤖 自动化工作流

### 📅 定时任务
- **运行时间**：每周日北京时间上午 8:00
- **执行内容**：价格爬取 → 汇率转换 → 变化检测 → 归档管理 → 数据提交 → 文件归档
- **手动触发**：支持 GitHub Actions 手动运行

### 🔄 工作流程
```mermaid
graph LR
    A[爬取价格] --> B[汇率转换]
    B --> C[数据标准化]
    C --> D[排序分析]
    D --> E[检测变化]
    E --> F[更新变更日志]
    F --> G[归档管理]
    G --> H[文件归档]
    H --> I[提交到仓库]
```

## 📊 数据输出

### 主要文件
| 文件名 | 描述 | 用途 |
|--------|------|------|
| `spotify_prices_all_countries.json` | 原始价格数据 | 数据源，包含完整爬取信息 |
| `spotify_prices_cny_sorted.json` | 人民币排序数据 | 分析结果，包含最便宜 Top 10 |
| `CHANGELOG.md` | 价格变化历史 | 记录所有价格变化和时间戳 |
| `price_changes_summary_YYYYMMDD_HHMMSS.json` | 变化检测报告 | 详细的价格变化分析 |

### 特色数据结构
```json
{
  "_top_10_cheapest_premium_family": {
    "description": "最便宜的10个Premium Family套餐",
    "updated_at": "2025-08-09",
    "data": [
      {
        "rank": 1,
        "country_name_cn": "尼日利亚",
        "price_cny": 11.73,
        "original_price": "₦2,500 / month"
      }
    ]
  },
  "_top_10_cheapest_individual_1year_prepaid": {
    "description": "最便宜的10个Premium Individual 1年预付费套餐",
    "updated_at": "2025-08-09",
    "data": [
      {
        "rank": 1,
        "country_name_cn": "巴基斯坦",
        "price_cny": 7.37,
        "total_price_cny": 88.4,
        "original_price": "Equivalent to Rs 290.84 per month"
      }
    ]
  },
  "_top_10_cheapest_family_1year_prepaid": {
    "description": "最便宜的10个Premium Family 1年预付费套餐",
    "updated_at": "2025-08-09",
    "data": [
      {
        "rank": 1,
        "country_name_cn": "巴西",
        "price_cny": 47.22,
        "total_price_cny": 566.64,
        "original_price": "Equivalent to R$40.84 per month"
      }
    ]
  }
}
```

## 🏗️ 项目架构

```
📦 spotify-price-tracker
├── 🕷️ spotify.py                      # 核心爬虫引擎
├── 💱 spotify_rate_converter.py       # 汇率转换与数据处理
├── 📊 price_change_detector.py        # 价格变化检测系统
├── 🗂️ changelog_archiver.py           # 变更日志归档管理
├── 📋 requirements.txt                 # Python 依赖管理
├── ⚙️ .env.example                    # 环境变量模板
├── 📝 CHANGELOG.md                    # 价格变化历史记录
├── 📁 archive/                        # 历史数据归档
│   ├── 2025/                         # 按年份组织
│   └── 2026/
├── 📁 changelog_archive/              # 月度变更日志归档
│   ├── changelog_2025-07.md          # 2025年7月价格变化
│   └── changelog_2025-08.md          # 2025年8月价格变化
├── 🔄 .github/workflows/
│   ├── weekly-spotify-scraper.yml    # 主自动化流程
│   └── manual-test.yml               # 手动测试流程
├── 📖 README.md                      # 英文文档
└── 📖 README_zh.md                   # 中文文档
```

## 🌟 核心特性详解

### 🎯 多层级排行榜系统
我们的智能排行榜系统提供全方位的价格分析：

#### 📊 月付订阅排行榜
- **Premium Family 月付**：全球最实惠的家庭套餐 Top 10
- **Premium Individual 月付**：最超值的个人订阅
- **实时人民币转换**：所有价格瞬间转换，方便对比

#### 💳 预付费订阅排行榜  
**全新功能！** 预付费选项的完整分析，享受超值优惠：

| 套餐类型 | 描述 | 主要优势 |
|----------|------|----------|
| **Individual 1年预付费** | 年付个人订阅 | 💰 相比月付节省高达15% |
| **Family 1年预付费** | 年付家庭订阅 | 👨‍👩‍👧‍👦 家庭用户的最佳选择 |
| **6个月预付费** | 中期预付费选项 | ⚖️ 兼顾省钱与灵活性 |

### 💳 高级预付费套餐检测
自动识别和处理预付费订阅：
- **智能识别**：多语言预付费关键词检测
- **总成本分析**：计算月均价格和总预付费用
- **省钱计算器**：显示与月付套餐的精确节省金额
- **时长支持**：支持1年、6个月、3个月预付费套餐

### 多语言套餐标准化
自动将各国本地化的套餐名称转换为统一的英文标准：

| 原始名称 | 标准化名称 | 地区 |
|----------|------------|------|
| Premium para Estudiantes | Premium Student | 西班牙语 |
| Premium Familiar | Premium Family | 西班牙语 |
| Premium 學生 | Premium Student | 中文 |
| Premium 家庭 | Premium Family | 中文 |

### 智能价格提取
支持多种价格格式和促销信息：
- ✅ `$6.49 per month` → 提取 6.49
- ✅ `Después, $6,49*** por mes` → 提取 6.49
- ✅ `首月免费，然后 ¥15/月` → 提取 15.00

### 价格变化检测与追踪
- 🔍 **自动变化检测**：对比历史数据，自动识别价格变化
- 📝 **详细变更日志**：记录所有价格变化及时间戳和详细分析
- 📊 **变化摘要报告**：生成全面的价格变化报告
- 🗂️ **智能归档系统**：月度归档管理保持变更日志整洁

### 历史数据管理
- 📅 按年份自动分类归档
- 📈 支持长期价格趋势分析
- 🔄 智能文件迁移和整理
- 📆 **月度变更归档**：自动创建月度归档以便更好地组织

## 🛠️ 故障排除

<details>
<summary>🔍 常见问题解决</summary>

### Playwright 安装问题
```bash
# 强制重新安装浏览器
playwright install --force

# 检查安装状态
python -c "from playwright.sync_api import sync_playwright; print('✅ Playwright 正常')"
```

### API 限制处理
- ⚠️ 免费账户：1000 次/月
- 💡 错误码 429：请求过于频繁
- 🔄 解决方案：等待重置或升级套餐

### GitHub Actions 调试
```bash
# 检查 Secrets 配置
GitHub仓库 → Settings → Secrets → API_KEY

# 查看详细日志
Actions → 选择失败的工作流 → 展开日志
```
</details>

## 📈 数据示例

### 🏆 最新全球排行榜

#### 💰 Premium Family 月付 Top 5：
| 排名 | 国家 | 价格 (CNY) | 原始价格 |
|------|------|------------|----------|
| 🥇 | 尼日利亚 | ¥11.73 | ₦2,500/月 |
| 🥈 | 巴基斯坦 | ¥14.67 | Rs 579/月 |
| 🥉 | 埃及 | ¥16.28 | EGP 109.99/月 |
| 4 | 土耳其 | ¥17.66 | TRY 99.99/月 |
| 5 | 印度 | ¥18.80 | ₹229/月 |

#### 🎯 Premium Individual 1年预付费 Top 5：
| 排名 | 国家 | 月均价格 (CNY) | 总成本 (CNY) | 节省 |
|------|------|----------------|--------------|------|
| 🥇 | 巴基斯坦 | ¥7.37 | ¥88.40 | ~50% |
| 🥈 | 印度 | ¥9.40 | ¥112.80 | ~45% |
| 🥉 | 土耳其 | ¥12.25 | ¥147.00 | ~30% |
| 4 | 巴西 | ¥18.85 | ¥226.20 | ~25% |
| 5 | 阿根廷 | ¥21.45 | ¥257.40 | ~20% |

#### 👨‍👩‍👧‍👦 Premium Family 1年预付费 Top 5：
| 排名 | 国家 | 月均价格 (CNY) | 总成本 (CNY) | 节省 |
|------|------|----------------|--------------|------|
| 🥇 | 巴西 | ¥47.22 | ¥566.64 | ~25% |
| 🥈 | 土耳其 | ¥52.45 | ¥629.40 | ~20% |
| 🥉 | 阿根廷 | ¥58.90 | ¥706.80 | ~18% |
| 4 | 墨西哥 | ¥65.30 | ¥783.60 | ~15% |
| 5 | 印度 | ¥72.15 | ¥865.80 | ~12% |

> 💡 **价格仅供参考**，实际订阅可能受地区限制影响

## 🔧 技术栈

| 技术 | 用途 | 版本 |
|------|------|------|
| ![Python](https://img.shields.io/badge/Python-3.9+-blue) | 核心开发语言 | 3.9+ |
| ![Playwright](https://img.shields.io/badge/Playwright-Latest-green) | 浏览器自动化 | Latest |
| ![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-CI/CD-orange) | 自动化部署 | - |
| ![OpenExchangeRates](https://img.shields.io/badge/OpenExchangeRates-API-yellow) | 汇率数据源 | v6 |

## ⚠️ 使用须知

- 📚 **用途**：仅限学习研究，请遵守各网站使用条款
- ⏱️ **频率**：内置延迟机制，避免过度请求
- 📊 **准确性**：价格数据仅供参考，以官方为准
- 🌐 **限制**：某些地区可能有订阅限制

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

1. Fork 本项目
2. 创建功能分支：`git checkout -b feature/new-feature`
3. 提交更改：`git commit -m 'Add new feature'`
4. 推送分支：`git push origin feature/new-feature`
5. 提交 Pull Request

## 📝 更新日志

- **v4.0** 🎯 **全新！** **高级排行榜与预付费系统**：
  - 新增月付和预付费套餐的完整排行榜系统
  - 全面支持预付费订阅（1年、6个月、3个月）
  - 总成本分析和预付费省钱计算器
  - 增强数据结构，包含预付费专用字段
  - 多层级对比系统，助您做出更明智的选择
- **v3.3** 🆕 **价格变化检测系统**：新增自动价格变化检测、详细变更日志追踪和月度归档管理
- **v3.2** 🔧 全面修复货币检测与价格爬取系统
- **v3.1** 🌍 增强多语言套餐名称识别，提升数据标准化准确性
- **v3.0** ✨ 多语言套餐名称标准化
- **v2.5** 🐛 修复小数点价格提取问题
- **v2.0** 🤖 GitHub Actions 自动化
- **v1.5** 🔐 API 密钥安全管理
- **v1.0** 🎉 初始版本发布

> 💡 **新功能**：查看 [CHANGELOG.md](CHANGELOG.md) 了解详细的价格变化历史和月度归档！

## 📄 许可证

本项目仅用于学习和研究目的。请遵守相关法律法规和网站使用条款。

---

<div align="center">

**🎵 发现全球最优惠的 Spotify 订阅价格！**

**🆕 现已支持预付费排行榜！** 通过我们全新的预付费套餐分析系统发现终极省钱方案。

[🚀 开始使用](#-快速开始) • [🎯 查看排行榜](#-数据示例) • [📊 查看数据](#-数据输出) • [🤖 自动化](#-自动化工作流) • [❓ 问题反馈](https://github.com/SzeMeng76/spotify-prices/issues)

**语言**: [English](README.md) | [中文](README_zh.md)

</div>
