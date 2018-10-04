# 知乎无限 live 数据流网站

[![Greenkeeper badge](https://badges.greenkeeper.io/calpa/zhihu-unlimited-web.svg)](https://greenkeeper.io/)

![Overview](https://i.imgur.com/TaTf7dq.jpg)

心血来潮把所有知乎无限 live 的数据拿下来，然后做了一个数据流的网站。

## 功能
1. 展示所有 live 的日期，费用，参加人数，分数，以及讲者
1. 提供过滤 live 名称功能
1. 提供每页展示特定数量资料的选项 (10, 20, 100)

## 如何运行
### 开发环境配置
1. nodejs

### 开发过程
```
# 开发
npm run dev

# 发布
npm run generate
```

### 发布文件夹
`/dist`

## 数据
数据位于 [zhihu-unlimited-live](https://github.com/calpa/zhihu-unlimited-live)，或者你可以访问知乎的 API。
