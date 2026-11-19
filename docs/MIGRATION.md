# 全量项目开源迁移报告

> 日期：2026-11-19
> 执行：DeepSeek Harness / Lead
> 状态：**第一批已发布并核验；第二批（C++ 团队/第三方项目、微信模板、原始数据）待归属确认；本地删除尚未执行。**

---

## 一、已发布并核验的仓库（11 个）

| # | 仓库 | 本地来源 | 远端 main SHA | 测试/核验 |
|---|---|---|---|---|
| 1 | [answer](https://github.com/hhxpoint/answer) | `D:\03-Projects\answer` | `1848759` | 小程序编译通过；1.31.2 已上传 |
| 2 | [rag-cooking-kg](https://github.com/hhxpoint/rag-cooking-kg) | `D:\rag` | `9945332` | `tests/test_smoke.py` 4 passed |
| 3 | [attendance-system](https://github.com/hhxpoint/attendance-system) | `D:\03-Projects\attendance-system` | `73defa6` | `mvn test` 3 passed |
| 4 | [online-judge](https://github.com/hhxpoint/online-judge) | `D:\03-Projects\online` | `3e3275b` | `npm run build` 通过；dev 页面截图 |
| 5 | [tanghulu-adventure](https://github.com/hhxpoint/tanghulu-adventure) | `D:\03-Projects\adventure` | `2c195cf` | 结构/资源核验；游戏需微信开发者工具 |
| 6 | [academic-projects](https://github.com/hhxpoint/academic-projects) | `D:\cop2000` + `C:\Users\卒星轩\source\repos` | `818fe05` | 180 个源文件归档 |
| 7 | [blog-oj](https://github.com/hhxpoint/blog-oj) | `D:\03-Projects\Blog` | `2fb4414` | Hexo `generate` 63 files；后端 `node --check` |
| 8 | [ml-coursework](https://github.com/hhxpoint/ml-coursework) | `D:\03-Projects\C++\{face,catdog,birdclef2026,DL,ML}` | `ee1a431` | 源码/配置/结果图归档；大数据未入库 |
| 9 | [asset-management-system](https://github.com/hhxpoint/asset-management-system) | `D:\03-Projects\C++\asset-management-system` | `f259252` | `mvn test` 2 passed |
| 10 | [pointcode](https://github.com/hhxpoint/pointcode) | `D:\03-Projects\agent\pointcode` | `09b6b85` | 已有开源仓库，保留 |
| 11 | [open-source-showcase](https://github.com/hhxpoint/open-source-showcase) | 新建索引 | `76df81c`（随后更新） | 项目索引/截图 |

每个仓库均包含：

- `README.md`：项目定位、功能、技术栈、运行环境、启动/构建/测试命令、目录结构
- `LICENSE`（MIT，第三方内容除外）
- `.gitignore`
- 截图/封面/演示（有 UI 的项目）
- 安全说明（不含真实密钥/密码）

---

## 二、测试与核验记录

### 已实际执行

- **RAG**：`python -m unittest discover -s tests -v` → 4 passed
- **考勤**：`mvn test` → Tests run: 3, Failures: 0
- **在线判题**：`npm install`（补上缺失的 `next` 依赖）→ `npm run build` 通过；`npm run dev` 本地页面可加载
- **博客**：`npm install` → `npx hexo generate` → 63 files generated
- **资产系统**：`mvn test` → 2 passed
- **后端语法**：`node --check` 检查 OJ/博客后端全部 JS
- **远端校验**：每个仓库用 GitHub API 对比 `refs/heads/main` 与本地 HEAD，全部一致

### 无法在本机完整运行

- 微信小游戏/小程序：需要微信开发者工具 + AppID，已提供运行步骤与手动验收清单
- 汇编：需要 Cop2000 商业仿真器（未纳入仓库）
- BirdCLEF：需要 Kaggle 数据 + GPU（未纳入数据/权重）
- 部分 C++/Java 项目：需要对应 GUI/数据库/服务器环境，已在 README 说明

---

## 三、安全脱敏清单

| 项目 | 原始风险 | 处理 |
|---|---|---|
| Blog `_config.yml` | 真实 GitHub token | 替换为仓库地址占位 |
| Blog `oj-backend/.env` | CloudBase SecretId/Key | 删除，改为 `.env.example` |
| Blog `oj-backend/config.js` | Secret 默认值、envId | 改为环境变量 |
| Blog 部署脚本/日志 | 服务器密码/IP | 未纳入仓库 |
| Blog `data/db.json` | 用户数据 | 未纳入仓库 |
| Online `frontend/.env.local` | Supabase 真实 key | 删除，改为 `.env.example` |
| 考勤 `application.yml` | MySQL root 密码 | 改为 `${DB_PASSWORD:change_me}` |
| 资产 `db.properties` | MySQL root 密码 | 改为 `CHANGE_ME` |
| ML 报告/照片 | 姓名、学号、人脸原图 | 未纳入仓库 |
| 小程序 `project.private.config.json` | 个人 AppID/配置 | 未纳入或改为 touristappid |

> **重要**：原始本地文件仍可能含这些凭据；开源仓库已脱敏，但建议你**轮换**曾出现过的 GitHub token、CloudBase Secret、数据库密码。

---

## 四、未迁移/待确认的项目（没有擅自上传）

| 路径 | 原因 | 处理建议 |
|---|---|---|
| `D:\03-Projects\C++\Gun-Mayhem-*` | README 明确标注团队开发者（华南理工）与原作者 Kevin Gu；非个人独有 | 需团队授权或只保留私有 |
| `D:\03-Projects\C++\object`（2.1GB） | 数据集/模型/大文件 | 不入 Git，必要时放 Releases/LFS |
| `D:\03-Projects\C++\books` | 电子书/版权内容 | 不入开源仓库 |
| `D:\03-Projects\C++\face\raw_photos`、`test1`、`models` | 人脸照片/大模型 | 隐私/体积，未上传 |
| `D:\03-Projects\C++\catdog\cifar-10-batches-py` | 数据集 | 公开数据集，不入库 |
| `D:\03-Projects\C++\birdclef2026\{data,checkpoints,submissions}` | 竞赛数据/权重 | 不入库 |
| `D:\03-Projects\story` | 微信官方「飞机大战」模板（README 标注示例游戏） | 不作为个人项目开源 |
| `C:\Users\卒星轩\WeChatProjects\miniprogram-2` | Vant 花坊 UI 模板，业务逻辑为空壳 | 如确认原创 UI 可单独发布 |
| `D:\03-Projects\claude`、`dev`、`xljsci`、`zcode`、`TrustAgent`、`magic`、`Qcode`、`xiezuo` | 第三方软件/安装包/写作工作区 | 不迁移 |
| `D:\06-Games` | Steam 游戏库（154GB） | 不迁移 |
| `C:\Users\卒星轩\Documents\vmodel_parts*` | 模型权重分片（约 2.3GB） | 不迁移 |
| `D:\03-Projects\go-project`、`live`、`project` | 空项目/无有效代码 | 跳过 |
| `C:\Users\卒星轩\WorkBuddy`、`CodeBuddy` | 生成式工作区快照，非完整项目 | 抽查后再定 |
| `D:\03-Projects\answer-private`、`answer-couple-server` | 私有服务端/敏感配置 | 保持私有仓库，不公开 |

---

## 五、本地删除状态：**尚未执行**

原因：

1. 用户要求“全部完成后再删”，而 C++ 团队项目、原始数据/模型、微信模板尚未完成归属确认；
2. 部分项目只上传了**源码子集**（ML 的 raw_photos/models/报告、RAG 的完整视频/zip、Blog 的部署脚本/数据），直接删除会丢失未上传内容；
3. 删除不可逆，必须先做一次人工复核。

### 建议的安全删除流程

1. 用户确认待删清单（仅已完整上传且无未上传附件的项目）
2. 先生成压缩备份到 `D:\dsh\workplace\migration\archive\`
3. 用 `Get-FileHash` 记录每个原目录清单
4. 删除原目录
5. 保留本报告 + 备份 30 天

### 可优先删除（已完整迁移/无重要未上传附件）

- `D:\03-Projects\attendance-system`
- `D:\03-Projects\online`
- `D:\03-Projects\adventure`
- `D:\03-Projects\C++\asset-management-system`
- `C:\Users\卒星轩\source\repos`
- `D:\cop2000`（仅 .ASM/.LST 已归档；Cop2000.exe/PDF 为第三方，不建议删前不备份）

---

## 六、结论

- 已完成 **9 个新开源仓库 + 2 个已有仓库**的整理/发布/核验。
- 每个仓库都有完整 README、运行环境、功能说明、目录结构、测试/验收步骤。
- 第三方面孔/团队项目/大数据没有擅自开源或删除。
- 下一步需要你确认：
  1. C++/Gun-Mayhem/微信模板的**归属与授权**
  2. 是否按“可优先删除”清单执行删除
  3. 原始凭据是否轮换

---

## 附：工作目录

- 迁移台账：`D:\dsh\workplace\migration\MANIFEST.md`
- 审计/扫描：`D:\dsh\workplace\migration\secret-scan.json`
- 各项目 staging：`D:\dsh\workplace\migration\staging\`
- 本报告：`D:\dsh\workplace\migration\MIGRATION_REPORT.md`
