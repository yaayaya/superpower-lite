# Superpowers Lite

一個刻意保持極簡的 Agent Skill，給 2026 年具備較強推理能力的 coding agents 使用。

它不試圖建立完整軟體開發方法論，也不強迫 brainstorming、規格文件、worktree、逐步 TDD、subagent 或 review 儀式。它只保留幾條值得常駐的工程護欄：**小範圍修改、以證據除錯、完成前重新驗證，以及高風險操作前取得授權。**

目前版本：**0.1.0**

## 為什麼存在

完整的 Superpowers 很適合長時間、多代理、高風險或流程要求嚴格的專案；但對 UI 微調、原型、小型 bug fix 與一般功能迭代，完整流程可能比工作本身更重。

Superpowers Lite 的原則很簡單：

> 使用足以安全完成工作的最少流程。

## 它會做什麼

安裝後，coding agent 會取得以下守則：

- 只讀取完成修改所需的檔案與上下文。
- 只有在真正受阻或高風險假設無法安全判定時才提問。
- 只有當順序、範圍、驗收條件或回滾方式不明顯時才先列短計畫。
- 讓修改保持小、相關、可逆，不順便重構或增加未要求功能。
- 依變更風險決定測試深度。
- 原因不明時先重現、看 log、檢查狀態與邊界，再測試單一假設。
- 連續兩次猜測式修復失敗後停止改碼，改為蒐集更好的證據。
- 宣稱完成前檢查 diff，並執行最新、最相關的測試或建置檢查。
- 清楚說明實際驗證結果，以及哪些項目尚未驗證。
- 破壞性、不可逆、安全敏感或會影響 production 的操作必須先取得明確授權。

## 它不包含什麼

這個 repository 刻意不包含：

- npm 套件程式碼或 runtime dependency
- Claude、Cursor、Codex 等平台專屬 plugin manifest
- SessionStart hooks 或 bootstrap scripts
- FAST／STANDARD／STRICT 模式
- 額外的 plan、debug、test、review 或 finish skills
- GitHub Actions、建置工具或發布腳本

跨 agent 的安裝與檔案放置由 [Vercel Labs Skills CLI](https://github.com/vercel-labs/skills) 處理；本專案本身只需要提供標準 `SKILL.md`。

## 安裝

### 一般安裝

讓 Skills CLI 自動偵測目前環境中的 coding agent：

```bash
npx skills add yaayaya/superpower-lite
```

全域安裝：

```bash
npx skills add yaayaya/superpower-lite --global
```

非互動式全域安裝：

```bash
npx skills add yaayaya/superpower-lite --global --yes
```

### 安裝到 Codex

安裝到目前專案的 Codex：

```bash
npx skills add yaayaya/superpower-lite --agent codex
```

全域安裝到 Codex：

```bash
npx skills add yaayaya/superpower-lite --agent codex --global
```

### 先查看可用 Skill

```bash
npx skills add yaayaya/superpower-lite --list
```

應只會看到一個 Skill：

```text
superpowers-lite
```

### 不安裝，單次使用

```bash
npx skills use yaayaya/superpower-lite --skill superpowers-lite
```

`SKILL.md` 採用通用 Agent Skills 格式，可由 Skills CLI 安裝到其支援的 coding agents。

## 使用方式

多數支援 Agent Skills 的工具會依 `description` 自動判斷是否載入。需要明確指定時，可直接告訴 agent：

```text
Use superpowers-lite for this task.
```

或：

```text
請使用 superpowers-lite 處理這個修改。
```

這份 Skill 是自包含的，不會再要求載入其他 Superpowers skills。

## 更新與移除

更新：

```bash
npx skills update superpowers-lite
```

移除：

```bash
npx skills remove superpowers-lite
```

只從 Codex 移除：

```bash
npx skills remove superpowers-lite --agent codex
```

移除全域安裝時加上 `--global`：

```bash
npx skills remove superpowers-lite --global
```

## Repository 結構

```text
superpower-lite/
├── SKILL.md
├── README.md
├── LICENSE
└── NOTICE
```

各檔案用途：

| 檔案 | 用途 |
|---|---|
| `SKILL.md` | 唯一的 Agent Skill。包含 YAML frontmatter 與完整行為守則；Skills CLI 會讀取這個檔案。 |
| `README.md` | 說明定位、行為、安裝、使用、更新、移除、維護方式與全部檔案用途。 |
| `LICENSE` | 本專案的 MIT License。 |
| `NOTICE` | 說明本專案受到上游 Superpowers 啟發、上游授權與非官方關係。 |

沒有隱藏的執行層、生成檔或平台 adapter。

## 修改這份 Skill

直接編輯根目錄的 `SKILL.md` 即可。Frontmatter 至少保留：

```yaml
---
name: superpowers-lite
description: 說明何時以及為什麼應使用這份 Skill。
---
```

提交前可以用 Skills CLI 確認它能被發現：

```bash
npx skills add . --list
```

也可以在暫存目錄測試 Codex 安裝：

```bash
mkdir /tmp/superpowers-lite-test
cd /tmp/superpowers-lite-test
npx skills add /path/to/superpower-lite --agent codex --copy --yes
```

## 維護原則

為了避免這個專案再次膨脹，後續修改遵守以下規則：

1. 維持單一根目錄 `SKILL.md`，除非出現真正獨立且無法合理合併的用途。
2. 不加入平台專屬 plugin、hook、runtime adapter 或 npm 封裝。
3. 新規則必須解決實際發生的 Agent 行為問題，不因理論可能性擴寫。
4. 修改後至少執行 `npx skills add . --list`，並確認只辨識到 `superpowers-lite`。
5. 版本採 Semantic Versioning；小幅文字與相容性修正升 patch，新增可觀察行為升 minor，破壞既有語意才升 major。
6. README、LICENSE 與 NOTICE 除非內容確實需要，否則不增加新的專案檔案。

## 設計原則

1. **護欄，而不是方法論**：提醒模型守住工程底線，不教聰明模型逐項照表操課。
2. **與風險成比例**：流程、測試與詢問程度由任務風險決定。
3. **證據優先**：不確定時觀察系統，而不是連續猜修。
4. **完成需要新證據**：不能用舊測試結果或「看起來正確」代替當下驗證。
5. **維持一個 Skill**：除非有非常明確且可獨立觸發的需求，否則不拆分更多 skills。

## 與 Superpowers 的關係

本專案受到 [obra/superpowers](https://github.com/obra/superpowers) 的工程理念啟發，但它是獨立、非官方、刻意極簡的重新詮釋，不是原專案的精簡發行版，也未獲其背書。

原專案採 MIT License；詳細 attribution 請見 [`NOTICE`](NOTICE)。

## 授權

MIT License，詳見 [`LICENSE`](LICENSE)。