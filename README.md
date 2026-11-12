# QAQPM GEO Skill

面向中文 GEO 内容运营的 5 个 AI Skills，覆盖客户资料、关键词候选、文章写作、平台版本处理和豆包研究报告。

本项目是人工参与的工作流原型。脚本负责结构化资料、规则处理与报告生成，模型和人工负责写作、事实核查、实际搜索、最终发布与效果判断。不承诺品牌推荐、收录或商业转化。

## Skills

| Skill | 用途 |
| --- | --- |
| [doubao-geo-publisher](skills/doubao-geo-publisher/SKILL.md) | 客户诊断、计划、Prompt、规则初检和监测计划 |
| [geo-keyword-miner](skills/geo-keyword-miner/SKILL.md) | 根据业务资料与公式生成关键词候选 |
| [geo-article-writer](skills/geo-article-writer/SKILL.md) | Brief、五类文章规范和提示词 |
| [geo-platform-adapter](skills/geo-platform-adapter/SKILL.md) | 多平台文件输出、词语替换、联系方式处理 |
| [geo-doubao-research](skills/geo-doubao-research/SKILL.md) | 检验词选择、会话辅助、人工日志转报告 |

## 流程

客户档案 → 关键词候选 → 发布前研究 → 模型写稿 → 平台处理与人工复核 → 人工发布 → 发布后研究 → 复盘。

竞品研究用于分析结构和证据组织，不直接复制竞品文章。

## 安装到 Codex

下载仓库，在仓库根目录运行。此命令先检查全部目标，避免覆盖同名安装：

```bash
python3 - <<'PY'
import os
import shutil
from pathlib import Path

source = Path.cwd() / 'skills'
target = Path(os.environ.get('CODEX_HOME', str(Path.home() / '.codex'))) / 'skills'
items = sorted(p for p in source.iterdir() if (p / 'SKILL.md').is_file())
if len(items) != 5:
    raise SystemExit('请在包含五个 Skill 的仓库根目录运行。')
for item in items:
    destination = target / item.name
    if destination.exists() or destination.is_symlink():
        raise SystemExit(f'已存在，未执行安装：{destination}')
target.mkdir(parents=True, exist_ok=True)
for item in items:
    shutil.copytree(item, target / item.name)
    print(f'Installed: {target / item.name}')
PY
```

安装后，Codex 下一轮对话中可指定 `$geo-keyword-miner`、`$geo-article-writer` 等名称调用。建议一起安装五个 Skill，以保留相互引用。

Skill 文档中的 `python3 skills/...` 命令以仓库根目录为工作目录；其他目录中需使用脚本实际路径。

## 依赖与使用边界

- 10 个 Python 脚本仅使用标准库。
- 正文由承载 Skill 的 AI 或用户另外选择的 AI 生成，包内没有模型 API 调用脚本。
- 浏览器脚本分别调用 `npx playwright` 和 `playwright-cli`，需额外准备。工具版本和页面兼容性尚未验证。
- 登录、验证码、账号安全和最终发布保留人工处理。
- Excel 是记录模板，没有现成的飞书、Coze 写回或定时调度集成。

## 安全试用

1. 阅读 [原始说明](docs/ORIGINAL-README.md)，了解各模块的输入与输出。
2. 复制 `skills/doubao-geo-publisher/assets/client_profile.template.json`，清除示例事实、电话、地址和来源，填入真实且允许使用的资料。
3. 生成候选词和 Prompt，人工筛选并核查证据，再让模型写稿。
4. 发布前人工检查正文、标题、来源和平台要求。
5. 使用固定查询集记录真实回答、引用 URL、时间和截图，区分提及、推荐和实际引用。

诊断用于整理客户资料，规则检查辅助人工复核，监测计划用于组织发布前后的查询和记录。

## 目录

- `skills/`：五个 Skill 的入口、参考材料、模板与脚本。
- `templates/`：Excel 工作簿及客户、文章、监测模板。
- `docs/`：使用说明和来源记录。

## 来源与许可

本项目由 PerpperQAQ 创作，采用 [MIT License](LICENSE)。原始版本在其他 AI Harness 中开发，本仓库整理为可分发的 Skill 集合。见 [来源记录](docs/PROVENANCE.md)。
