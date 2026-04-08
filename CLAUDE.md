# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 构建命令

构建脚本需要额外内存：`node --max-old-space-size=3072 tasks/cli.js`
- `npm run debug` — 开发构建
- `npm run debug:watch` — 开发构建 + 文件监听
- `npm run api` — 构建 NPM API 模块（darkreader.js/mjs）
- `npm run build` — 生产构建

## 测试和验证

- `npm run lint` — ESLint 检查
- `npm run test:unit` — Jest 单元测试
- `npm run test:all` — 全部测试（unit + browser + inject）
- `npm run release` — 完整发布流程（test + lint + build）

## 框架说明

**Malevic** — 项目使用 Malevic 框架而非 React：
- JSX 使用 `m` pragma：`/** @jsx m */`
- 组件声明：`export function Component(props: { ... })`
- 无 hooks，使用闭包和局部状态
- 类型定义在 `src/definitions.d.ts`

## 导入限制（重要）

ESLint 强制模块边界，违反会报错：
- `inject/` 不能导入 `background/`、`ui/`、`api/`、`tests/`
- `background/` 不能导入 `ui/`、`tests/`
- `ui/` 不能导入 `inject/`、`background/`、`tests/`
- `generators/` 不能导入 `inject/`、`background/`、`ui/`

## 代码风格

- 4 空格缩进
- 单引号（允许模板字符串）
- 必须分号
- 箭头函数总是括号：`(x) => ...`
- 对象无空格：`{key: value}`
- 多行末尾逗号（函数除外）
- 函数间有空行

## 站点修复配置（src/config/）

修改配置文件时必须：
- URL 按**字母顺序**排列
- 使用 CSS 变量而非硬编码颜色：
  - `--darkreader-neutral-background`
  - `--darkreader-neutral-text`
- 先用 Dev Tools 扩展测试修复效果

## 平台编译常量

Rollup 会替换这些全局变量：
- `__DEBUG__`、`__TEST__`
- `__CHROMIUM_MV2__`、`__CHROMIUM_MV3__`
- `__FIREFOX_MV2__`、`__THUNDERBIRD__`
- `__PLUS__`

代码中可直接使用，如：`if (__DEBUG__) { ... }`