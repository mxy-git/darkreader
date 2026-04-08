---
name: verify
description: 运行 lint 和单元测试验证代码改动
---

## 执行步骤

1. 运行 ESLint 检查：`npm run lint`
2. 运行 Jest 单元测试：`npm run test:unit`
3. 如果失败，显示错误并建议修复方案

## 注意事项

- 只修改 `src/config/` 时 CI 会跳过测试
- 检查是否违反导入限制规则