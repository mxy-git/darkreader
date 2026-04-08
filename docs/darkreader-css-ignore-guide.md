# Dark Reader CSS 文件排除功能说明

## 功能概述

Dark Reader npm API 支持排除特定的 CSS 文件，使其保持原有样式不被转换。被排除的 CSS 文件将完全跳过 Dark Reader 的样式处理，保留原样。

## API 使用方式

```typescript
import DarkReader from 'darkreader';

// 基本用法：排除特定 CSS 文件
DarkReader.enable({
    brightness: 100,
    contrast: 90,
}, {
    ignoreCSSUrl: [
        'https://cdn.example.com/style.css',  // 排除特定文件
    ],
});

// 排除多个 CSS 文件
DarkReader.enable({
    brightness: 100,
}, {
    ignoreCSSUrl: [
        'https://cdn.example.com/main.css',
        'https://cdn.example.com/theme.css',
        'bootstrap.min.css',  // 排除所有包含此字符串的 CSS
    ],
});
```

## 匹配规则详解

`ignoreCSSUrl` 支持三种匹配方式：

| 模式写法 | 匹配类型 | 说明 | 示例 |
|---------|---------|------|------|
| `^xxx` | 前缀匹配 | URL 开头匹配指定字符串 | `^https://cdn.example.com/` 排除该 CDN 下的所有 CSS |
| `xxx$` | 后缀匹配 | URL 结尾匹配指定字符串 | `.min.css$` 排除所有压缩版 CSS 文件 |
| `xxx` | 包含匹配 | URL 包含指定字符串 | `bootstrap` 排除所有 URL 包含 bootstrap 的 CSS |

### 推荐用法

**排除单一特定文件**（最精确）：
```typescript
ignoreCSSUrl: [
    '^https://cdn.example.com/style.css',  // 使用前缀匹配，精确排除
]
```

**排除某目录下所有 CSS**：
```typescript
ignoreCSSUrl: [
    '^https://cdn.example.com/styles/',  // 排除 styles 目录下的所有 CSS
]
```

**排除某类文件**：
```typescript
ignoreCSSUrl: [
    '.min.css$',  // 排除所有压缩版 CSS
]
```

## 实际应用示例

### 示例 1：Vue 项目排除自定义主题文件

```typescript
import DarkReader from 'darkreader';

// 在 Vue 项目入口文件中
DarkReader.enable({
    brightness: 100,
    contrast: 90,
    sepia: 10,
}, {
    ignoreCSSUrl: [
        '^https://myapp.com/assets/theme.css',  // 排除自定义主题
        '^/static/custom-style.css',            // 排除本地静态文件
    ],
});
```

### 示例 2：排除第三方 UI 库样式

```typescript
import DarkReader from 'darkreader';

DarkReader.enable({
    brightness: 100,
}, {
    ignoreCSSUrl: [
        'element-ui',       // 排除 Element UI 样式
        'ant-design',       // 排除 Ant Design 样式
        'bootstrap',        // 排除 Bootstrap 样式
    ],
});
```

### 示例 3：排除 Google Fonts（已在默认配置中）

```typescript
import DarkReader from 'darkreader';

DarkReader.enable({}, {
    ignoreCSSUrl: [
        '^https://fonts.googleapis.com/',  // 排除 Google Fonts CSS
    ],
});
```

## 注意事项

1. **匹配是基于完整 URL**：规则会匹配 CSS 文件的完整 URL（包括协议、域名、路径）

2. **只影响外部 CSS 文件**：`ignoreCSSUrl` 只能排除 `<link rel="stylesheet">` 引入的外部 CSS 文件，不影响 `<style>` 内联样式

3. **设置时机**：需要在调用 `enable()` 时设置，如果后续需要修改排除列表，需要重新调用 `enable()`

4. **大小写敏感**：匹配是大小写敏感的，注意 URL 的实际大小写

5. **相对路径处理**：相对路径的 CSS 文件会被浏览器转换为完整 URL，建议使用完整 URL 或包含匹配

## 技术原理

Dark Reader 在处理 CSS 文件时会检查：

```
<link rel="stylesheet" href="...">
    ↓
shouldManageStyle() 检查
    ↓
shouldIgnoreCSSURL(href) 判断
    ↓
如果在排除列表 → 跳过处理，保留原样
如果不在排除列表 → 应用暗色样式转换
```

## 相关 API

完整的 `DynamicThemeFix` 接口：

```typescript
interface DynamicThemeFix {
    url: string[];              // 匹配的 URL 模式（用于站点级配置）
    invert: string[];           // 需反转的 CSS 选择器
    css: string;                // 自定义修复 CSS
    ignoreInlineStyle: string[]; // 忽略的内联样式选择器
    ignoreImageAnalysis: string[]; // 忽略图片分析的选择器
    ignoreCSSUrl: string[];     // 忽略的 CSS URL ← 本文档重点
    disableStyleSheetsProxy: boolean;
    disableCustomElementRegistryProxy: boolean;
}
```

## 快速参考

```typescript
import DarkReader from 'darkreader';

// 启用暗色模式并排除 CSS 文件
DarkReader.enable({
    brightness: 100,
    contrast: 100,
}, {
    ignoreCSSUrl: ['你的 CSS 文件 URL'],
});

// 自动模式（跟随系统）
DarkReader.auto({
    brightness: 100,
}, {
    ignoreCSSUrl: ['你的 CSS 文件 URL'],
});

// 禁用
DarkReader.disable();

// 检查是否启用
DarkReader.isEnabled();
```