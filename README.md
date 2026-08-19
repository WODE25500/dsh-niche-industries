# dsh-niche-industries

**冷门行业领域包** —— 为 DeepSeek Harness 提供垂直行业 Agent 预设（preset）+ 领域技能（skill），每个行业一整套：persona 工作流 + 术语表 + 输出规范。

> 为什么做冷门行业：通用编程 / 记忆 / UI 类插件已经非常多（见 [awesome-dsh-plugin](https://github.com/Anil-matcha/awesome-dsh-plugin)），而**垂直行业**几乎无人覆盖。茶叶审评、古籍校勘、养蜂这类行业：① 术语体系成熟（有国家标准可依据）；② 从业者/爱好者真实存在且愿意用工具；③ 现有 AI 助手对这类"专业黑话"回答质量差，正是 Agent 预设能补的空白。

## 包含的行业

| 行业 | 预设 | 技能 | 依据 |
| --- | --- | --- | --- |
| 🍵 茶叶审评 | `presets/tea-taster` | `skills/tea-tasting` | GB/T 23776 感官审评方法、GB/T 14487 审评术语 |
| 📜 古籍校勘 | `presets/textual-critic` | `skills/classical-text` | 陈垣《校勘学释例》校勘四法、古籍整理出版规范 |
| 🐝 养蜂顾问 | `presets/beekeeper` | `skills/beekeeping` | 蜂群四季管理、常见病虫害鉴别与法规用药 |

## 安装

### 方式一：preset（推荐，新建会话可选）

```sh
# 把需要的行业目录复制到用户预设目录（目录名即预设 id）
cp -r presets/tea-taster ~/.dsh/.agent-presets/tea-taster
```

重启 DSH Web UI，新建会话时在预设选择器中即可看到「茶叶审评师」/「古籍校勘家」/「养蜂顾问」。

### 方式二：skill（注入领域知识，preset 与 skill 可单独使用）

```sh
cp -r skills/tea-tasting ~/.dsh/skills/tea-tasting
```

skill 装载后，agent 在遇到对应任务时会自动加载 SKILL.md 中的术语表与流程。

### 方式三：bundle 形式

每个 preset 的 `agent.cordis.yml` 是标准 Cordis 组合文件，可被 profile 引用：

```yaml
# cordis.yml
plugins:
  - path: ./node_modules/dsh-niche-industries/presets/tea-taster
```

## 目录结构

```
dsh-niche-industries/
├── presets/               # Agent 预设（persona 工作流 + 输出规范）
│   ├── tea-taster/        #   preset.yml + agent.cordis.yml
│   ├── textual-critic/
│   └── beekeeper/
├── skills/                # 领域技能（SKILL.md 术语表/流程/规范）
│   ├── tea-tasting/
│   ├── classical-text/
│   └── beekeeping/
└── samples/               # 各行业示例输出
```

## 设计原则

1. **可验证**：每个行业都锚定公开标准/经典著作（国标、校勘学通例），不是编造术语；
2. **不臆造**：persona 明确要求"缺少感官/版本/蜂群信息时不假装知道"，先给检查清单；
3. **输出规范化**：审评报告 / 校勘记 / 诊断结论都有固定格式，方便落地成文档；
4. **安全边界**：养蜂预设内置法规用药与检疫提示；古籍预设强调"底本不动、异文入记"。

## 许可

MIT

---

**扩展指南**：想加新行业？照抄任一 preset 目录结构 —— `preset.yml`（名称+描述+排序）、`agent.cordis.yml`（persona 工作流+工具）、`skills/<name>/SKILL.md`（frontmatter 带 `name`/`description`/`whenToUse`）。推荐选"有国家标准或成熟术语体系"的行业，质量上限最高。
