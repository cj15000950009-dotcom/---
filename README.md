# 武侦连纪律锁 · 前端原型

现代军营题材沉浸式文字角色扮演 ——「钥匙官」战术终端。
白色战术终端 / 机密档案军事工业风 · 科技白 + 军绿 + 黄铜锁具 + HUD 排版。

## 交付物

| 文件 | 用途 |
| --- | --- |
| `武侦连纪律锁_前端.html` | **单文件自包含原型**。双击浏览器打开即可完整体验（开机主界面 → 入职引导 → 主控台 → 各视图）。 |
| `武侦连纪律锁_前端版.json` | **可直接导入酒馆（SillyTavern）的角色卡**。原卡的 `regex_scripts[0].replaceString` 已替换为本前端 HTML（`findRegex` 仍为 `/打开卡面/s`，`first_mes` 仍为 `打开卡面`）。 |

> 其余 `_` 前缀文件为制作过程的中间提取物，可忽略。

## 已实现的功能界面

1. **开始游戏主界面** —— 开机自检动画、开始新档案 / 读取档案
2. **初次角色介绍向导** —— 四步（你的身份 / 你的职责 / 制度与规则 / 选择开场白）
3. **主控台** —— 统计面板、今日待办、在值勤务、当前事件、快捷入口
4. **剧情对话** —— 演示场景 + 剧情分支选项 + 输入框 + 在场人物 / 场景情报
5. **官兵名册** —— 10 名角色卡片，点击查看完整**机密档案**（含「解密」打码交互）
6. **周程表** —— 周一至周日 × 7 时段的作息表，锁务高发时段高亮标注
7. **我的职责** —— 六项核心职责 + 权限边界 + 相处模式三姿态切换
8. **事件系统** —— 13 个事件（明面流程 + 暗面色点），可跳转注入

## 酒馆（SillyTavern）接线点

前端通过 `GameBridge` 对象预留了 4 个接线点，未接入酒馆时自动进入「演示模式」（页面内 toast 提示）：

| 方法 | 触发 | 酒馆 API |
| --- | --- | --- |
| `GameBridge.sendMessage(text)` | 剧情输入 / 分支选择 | `triggerSlash('/send … \| /trigger')` |
| `GameBridge.switchGreeting(swipe)` | 开场白选择 | `getChatMessages` + `setChatMessage` |
| `GameBridge.triggerEvent(name)` | 事件跳转 | `triggerSlash('/send … \| /trigger')` |
| `GameBridge.setBranch(text)` | 分支 | 同 `sendMessage` |

探测逻辑见 `GameBridge.init()`：`typeof getChatMessages === 'function'` 存在即视为酒馆环境。

## 技术要点

- **单文件**：内联 CSS / JS / SVG 图标雪碧图，无构建、无外部依赖（字体 `@import` 带系统回退）。
- **图标**：全部内联 `<symbol>` + `<use>`，零 emoji。
- **兼容酒馆**：`replaceString` 内不出现 `$` / 反引号，规避了 `String.replace` 的 `$` 转义问题；源码用字符串拼接而非模板字面量。
- **语义化 HTML5** + 唯一描述性 ID + 纯 CSS 动画微交互。
