---
title: 用typora插入图片的坑
date: 2022-02-18 15:01:46
tags: 遇到的坑
---

### 不能使用命令

```js
npm install hexo-asset-image --save
```

直接npm有bug

得使用：

```js
npm install https://github.com/CodeFalling/hexo-asset-image –-save
```

上面的bug会导致解析成html时路径出错

![/.io路径出错](%E7%94%A8typora%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87%E7%9A%84%E5%9D%91/image-20220218153401407.png)



## typora设置

![typera设置](%E7%94%A8typora%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87%E7%9A%84%E5%9D%91/image-20220218154832160.png)

