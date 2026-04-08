---
name: add-site-fix
description: 添加站点修复配置，遵循排序和 CSS 变量规范
---

## 配置文件

- `src/config/dark-sites.config.ts`
- `src/config/dynamic-theme-fixes.config.ts`
- `src/config/inversion-fixes.config.ts`

## 规范（必须遵循）

1. **URL 按字母顺序排列**
2. **使用 CSS 变量**：`var(--darkreader-neutral-background)` / `var(--darkreader-neutral-text)`
3. **先用 Dev Tools 扩展测试**

## 步骤

1. 确定配置文件类型
2. 找到正确位置插入
3. 编写修复规则
4. 运行 `npm run lint` 验证