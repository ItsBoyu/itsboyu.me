---
title: RuboCop 設定
date: "2020-06-25T23:46:37.121Z"
template: "post"
draft: false
slug: "RuboCop setting"
category: "程式學習"
tags:
  - "筆記練習"
description: "在助教的建議下使用 gem RuboCop 來檢查 coding style，而還沒有進行設定之前，用 default configuration 來跑..."
socialImage: "/media/image-2.jpg"
---
在助教的建議下使用 gem RuboCop 來檢查 coding style  
而還沒有進行設定之前，用 default configuration 來跑  
結果總共執行 50 個檔案，裡面有高達 177 offenses  
便著手進行修正，一度處理到剩下 18 個  
不過在修正的過程發現  
我居然連 bin 資料夾裡的 bundle、yarn 還有 webpack 都要改？  
便趕緊跟助教確認我是不是走火入魔了（咦？

沒錯！那這篇文章就來作為設定檔的紀錄囉

### 設定檔
首先，請先在專案根目錄底下建立 `.rubocop.yml`  

```
 $ cd PROJECTNAME
 $ touch .rubocop.yml
```

接著在終端機裡面執行 `$ rubocop --auto-gen-config`  
就會自動產生 `.rubocop_todo.yml` 

```
 $ rubocop --auto-gen-config
```
如果想要客製化自己的 rubocop 設定的話，就要靠這兩個檔案囉！

那這兩個檔案到底是有什麼用呢？

`.rubocop.yml` ：撰寫相關設定來覆寫預設設定的檔案  

`.rubocop_todo.yml`：衝突防護網（？，有點像是 rescue 的功能，  
如果你會不及格，那我就不把考卷發給你的概念（？

### 開始設定

因為 `.rubocop_todo.yml` 已經幫你把錯誤排除  
所以如果你都不想改的話可以直接在`.rubocop.yml` 裡面  
加上 `inherit_from: .rubocop_todo.yml` 套用設定
```=yml=
// .rubocop.yml
  inherit_from: .rubocop_todo.yml
```
不過這樣有點不思長進吧！！

所以你應該做的是一點一點解除防護網的保護，面對現實  
依據自己的抗壓性（？來將檢查項目進行註解，然後執行 `$ rubocop`  
噴出的錯誤都會明確的指出檔案位置及行數，可以依照建議來進行修正

那如果碰到那些矯枉過正（？或是 bin 裡面的原先設定檔，  
就可以直接設定排除寫在 `rubocop.yml` 檔案裡面，  
你可以選擇所有的 Cops 測試都跳過，  
或是只指定某些 Cops 不測指定的檔案，  
當然也可以覆寫檢查規則，  
這就讓大家自行決定了

### 個人設定
最後附上我目前的 `.rubocop.yml` 供參考：  
我逃避了 bin、confid、db、spec、test 等內容(？  
總共會檢查 13 個檔案，目前是沒有 offenses 的

![](https://i.imgur.com/yF24YGO.png)

### 參考資料
[RuboCop // Docs](https://docs.rubocop.org/rubocop/configuration.html#enabled)  

[【五倍專欄】機器戰警 RuboCop](https://5xruby.tw/posts/rubocop-intro/)  

[用 Rubocop 寫出好風格 (Ruby & Rails Style Guide)](https://mgleon08.github.io/blog/2016/01/22/rubocop/)