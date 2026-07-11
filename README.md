# 51Learning Helper

[![Install](https://img.shields.io/badge/Install-直接安装-blue)](https://github.com/xinqing520/51learning-helper-1.2.0/raw/master/51learning-helper.user.js)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

篡改猴（Tampermonkey）脚本，辅助 [51Learning](http://reading.51learning.com.cn:8080/) 阅读平台完成学习流程。

> 基于 [zhourunfaaaaaa/51learning-helper](https://github.com/zhourunfaaaaaa/51learning-helper) 二次开发，感谢原作者。

**AI 答案仅供参考，请自行检查。本脚本不会自动提交。**

## 功能

- **单元队列收集** — 自动抓取整个单元的文章列表，支持翻页遍历
- **上一篇/下一篇导航** — 在队列中快速切换文章，记录阅读位置
- **自动模式选择** — 一键设置阅读显示模式、单位、速度
- **词数估算计时 + 自动完成阅读** — 根据文章词数和阅读速度自动计算时长，到时间后自动点击"完成阅读"
- **复制文章+题目** — 一键复制文章内容和题目到剪贴板
- **答案模板** — 生成结构化的答案模板（含题号、题型分组），填好答案后粘贴回脚本即可一键填入
- **答案保存/加载** — 按文章保存答案到本地，下次打开同一篇可恢复
- **计时同步** — 自动检测网页已有的阅读计时并同步到插件
- **AI 答题** — 自动发送文章和题目给 AI，生成答案（可选自动填入页面）
- **AI 排版** — 把你已有的答案自动整理成模板格式，省去手动对题号的麻烦

## 安装

### 第一步：安装篡改猴

篡改猴（Tampermonkey）是一个浏览器扩展，用于管理和运行用户脚本。

1. 打开浏览器扩展商店：
   - **Edge**：[Tampermonkey](https://microsoftedge.microsoft.com/addons/detail/tampermonkey/iikmkjmpaadaobahmlepeloendndfphd)
   - **Chrome**：[Tampermonkey](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
   - **Firefox**：[Tampermonkey](https://addons.mozilla.org/firefox/addon/tampermonkey/)
2. 点击"获取"/"添加到浏览器"，等待安装完成
3. 浏览器工具栏出现篡改猴图标（拼图块样式）即安装成功

### 第二步：安装本脚本

1. 打开 [51learning-helper.user.js](https://github.com/xinqing520/51learning-helper-1.2.0/raw/master/51learning-helper.user.js)，篡改猴会自动弹出安装页面
2. 或手动安装：篡改猴图标 → 管理面板 → 实用工具 → **导入 URL**，粘贴脚本地址：
```
https://github.com/xinqing520/51learning-helper-1.2.0/raw/master/51learning-helper.user.js
```
3. 点击安装确认即可

### 第三步：开始使用

打开 [51Learning 阅读平台](http://reading.51learning.com.cn:8080/Reading/Articles)，页面右上角出现控制面板。

## 使用流程

### 第一步：收集文章列表

1. 进入单元列表页（`/Reading/Articles?b=...&u=...`）
2. 在控制面板确认 `b` 和 `u` 参数正确
3. 点击 **"收集单元"**
4. 脚本自动遍历所有分页，收集全部文章

### 第二步：开始阅读

1. 选择文章后进入文章页，或通过面板的 **"上一篇"/"下一篇"** 导航
2. 确认阅读模式（显示方式、单位、速度）正确
3. 点击 **"选模式并开始"**
4. 脚本自动设置模式并点击"开始阅读"，同时根据文章词数和阅读速度启动计时

### 第三步：完成阅读 & 获取答案

1. 计时到达后自动点击 **"完成阅读"**
2. 题目出现后，可以：
   - **方式A（推荐）**：直接在插件内点 **「AI 答题」**，AI 阅读文章后自动生成答案
   - **方式B**：点 **"复制文章+题目"** 或 **"复制模板"**，粘贴到外部 AI 工具获取答案

### 第四步：确认并填入

1. AI 生成的答案会自动填入文本框；手动流程则把答案粘贴进来
2. 点击 **"填入答案"**，脚本自动填写所有输入框
3. **自行检查答案正确性**
4. 手动点击网站的 **"提交答案"**

> **重要：使用 AI 答题/排版时会自动匹配题型和题号。手动填写时，建议先点"复制模板"获取对应模板，填完答案再粘回脚本。**

## 答案模板格式
```
51Learning答案模板
填写方法：不要改【】标题；每题一行，把答案写在题号后面。
填空/匹配/选择题写字母；翻译/问答题写完整句子。

【vocabulary 词汇填空】
1. A
2. D
3. C

【multiple_choice 选择题】
1. B
2. A

【translation 翻译】
1. The article mainly discusses...
2. According to the text...
```


## 使用教程

### 方式A（推荐）：内置 AI 答题

配置好 API Key 后，一条龙完成。

1. 阅读完成后题目出现，点击 **「AI 答题」**（或「AI答题+填入」直接填入页面）
2. AI 自动阅读文章、分析题目、生成答案
3. 检查答案确认无误后，点 **「填入答案」**

![AI 功能面板](screenshots/ai.png)

### 方式B（备选）：手动复制到外部 AI

如果没有 API Key，也可以手动复制粘贴。

1. 阅读完成后题目出现，点击 **"复制模板"** 获取答案模板
2. 把模板粘贴到任意 AI 工具，让它按模板填写答案
3. 把 AI 返回的答案粘贴回脚本文本框
4. 点击 **"填入答案"**

| 步骤 | 截图 |
|------|------|
| 点击复制模板 | ![1](screenshots/1.png) |
| 发送给 AI 工具 | ![2](screenshots/2.png) |
| AI 返回答案 | ![3](screenshots/3.png) |
| 粘贴回文本框 | ![4](screenshots/4.png) |
| 一键填入 | ![5](screenshots/5.png)

## AI 功能使用教程

配置好 API Key 后，可以使用三个 AI 按钮。

![AI 功能面板](screenshots/ai.png)

### 三个按钮说明

| 按钮 | 功能 |
|------|------|
| **AI 答题** | 发送文章+题目给 AI 生成答案，结果放入文本框，检查后手动点「填入答案」 |
| **AI答题+填入** | 同上，但生成答案后自动填入页面输入框（省一步，务必逐题检查） |
| **AI 排版** | 你已有答案（同学/答案库等），粘贴到文本框，AI 整理成模板格式 |

### 场景一：让 AI 帮你答题

1. 获取任意 OpenAI 兼容的 API Key（DeepSeek、OpenAI、智谱、通义千问等），粘贴到插件的 API Key 输入框
2. 根据服务商修改 **模型** 和 **Base URL**
3. 阅读完成后题目出现，点击 **「AI 答题」**
4. AI 会阅读文章、分析题目、生成答案填入文本框
5. **逐题检查答案正确性**（AI 也可能出错）
6. 确认无误后点 **「填入答案」**

### 场景二：已有答案，让 AI 排版

1. 把答案（任意格式）粘贴到文本框
2. 点击 **「AI 排版」**
3. AI 读取页面题型结构，自动匹配答案到题号
4. 检查格式是否正确，点击 **「填入答案」**

### 各服务商配置示例

| 服务商 | Base URL | 模型示例 |
|--------|----------|----------|
| DeepSeek | `https://api.deepseek.com` | `deepseek-chat` |
| OpenAI | `https://api.openai.com` | `gpt-4o` |
| 智谱 GLM | `https://open.bigmodel.cn/api/paas/v4` | `glm-4` |
| 通义千问 | `https://dashscope.aliyuncs.com/compatible-mode/v1` | `qwen-plus` |
| 其他中转站 | `https://你的中转站域名.com` | 咨询服务商 |

### 使用技巧

- **提示词可自定义**：搜索脚本中的 `systemPrompt` 变量（`answerQuestionsWithAI` 和 `formatAnswersWithAI` 函数内）即可修改
- **AI 答案不一定对**：尤其是阅读理解题，建议自己先看一遍文章再对比 AI 答案

## 数据存储

所有数据存储在浏览器的 `localStorage` 中，不会上传到任何服务器：
- 队列列表
- 已读标记
- 答案缓存
- 配置项

## 开发

```
git clone https://github.com/xinqing520/51learning-helper-1.2.0.git
```

直接编辑 `51learning-helper.user.js`，在 Tampermonkey 中导入本地文件即可调试。

## License

MIT
