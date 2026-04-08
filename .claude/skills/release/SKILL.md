---
name: release
description: 执行完整发布流程（test + lint + build）
---

## 执行步骤

运行：`npm run release`

输出位置：
- Chrome: `build/release/darkreader-chrome.zip`
- Firefox: `build/release/darkreader-firefox.xpi`

## 注意事项

- 构建需要约 3GB 内存
- 新功能需先提交 issue 讨论