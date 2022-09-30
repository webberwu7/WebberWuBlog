---
title: "菜雞部落客的hugo踩雷之路 ( 5 ) - 改變主題與 Disqus 留言"
date: 2022-09-28T17:42:47+08:00
tags: ['hugo']
---
在原本使用的 m10c 主題色系較為灰暗

然後我又想加上搜尋功能於是就換成了今天的主題 hugo-theme-pure

## 下載新的目標主題
`git submodule add git@github.com:webberwu7/hugo-theme-pure.git themes/pure`

## 移除舊的主題
`git rm themes/m10c`

## 變更 config.toml 設定

參考這份主題的NOTE 

將config.yml複製下來

Please copy the config.yml under the exampleSite folder to the root folder of your Hugo Site. 

Feel free to change it. 

If you don't like .yml file, you can also convert it to you want.


## Disqus
### 安裝 Disqus

1. 先到 [Disqus](https://disqus.com/) 的官網註冊一個帳號。
    ![image](https://imgur.com/yw7xEoj.jpg)

2. 按下 i want to install disqus on my site
    ![image](https://imgur.com/tV1qcgf.jpg)

3. 填寫 Website Name, 可以想像這個是唯一的key值
    ![image](https://imgur.com/QDA6YQ8.jpg)

4. 後續因為 disqus 想要你付錢有一些方案可以選擇 這邊都直接下一步大法過去就好

5. 回到hugo config 中加上設定
```yaml
  # Comment
  comment:
    type:  disqus # type disqus/gitalk/valine 启用哪种评论系统
    disqus: WebberWuBlog # enter disqus shortname here
```

6. 就可以在文章底下看到留言系統囉
    ![image](https://imgur.com/J1WNCR7.jpg)

## 補充
由於作者已經將 hugo-theme-pure github 封存

https://github.com/xiaoheiAh/hugo-theme-pure

所以我在 github 上 Fork 了一份回來

![image](https://imgur.com/MCDzTAl.jpg)

後續也加上了 i18n 多國語系 zh-TW.yaml