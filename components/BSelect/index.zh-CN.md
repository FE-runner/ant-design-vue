---
category: Components
subtitle: 选择器
type: BMC
title: BSelect
cols: 1
cover:
coverDark:
---

> 自定义的 BMC 组件。命名分两部分：B 为自定义前缀（取自 BMC），后面表示为封装的 adv 组件名或自定义组件名

封装的 Select。

## 为何开发

- 修复 `Select` value 为 null 或者空字符串时不展示 placeholder 的问题

## API

### Props

| 参数             | 说明                                   | 类型 | 默认值 | 版本 |
| ---------------- | -------------------------------------- | ---- | ------ | ---- |
| v-model:selected | 用法同 Select 组件的属性 v-modle:value | -    | -      |      |
| selectProp       | 用法同 Select 组件的属性               | -    | -      |      |

### Emit

| 参数   | 说明                            | 类型 | 默认值 | 版本 |
| ------ | ------------------------------- | ---- | ------ | ---- |
| change | 用法同 Select 组件的方法 change | -    | -      |      |
