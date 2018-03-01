---
title: vw 视口单位自适应解决方案
date: 2018-03-01 17:23:12
tags: [CSS , postcss , webpack]
categories: [UI,插件]
---
## 简介

手机端自适应开发的新方案，vw单位能够根据屏幕站比自动适应内容。
建议依靠postcss-vw 解决方案，快速生成

### 参数

+ vw : 1vw 等于视口宽度的1%
+ vh : 1vh 等于视口高度的1%
+ vmin : 选取 vw 和 vh 中最小的那个
+ vmax : 选取 vw 和 vh 中最大的那个

#### 操作
依照设计稿 750 === 100vw 计算

