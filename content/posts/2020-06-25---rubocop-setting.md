---
title: RuboCop 設定
date: "2020-06-25T23:46:37.121Z"
template: "post"
draft: false
slug: "RuboCop setting"
category: "程式學習"
tags:
  - "筆記練習"
description: "最近在撰寫任務管理工具 Botask 時，助教建議使用 gem RuboCop 來協助檢查 Coding Style。而在尚未修改相關設定檔案以前..."
socialImage: ""
---
最近在撰寫任務管理工具 [Botask](https://github.com/ItsBoyu/Botask) 時，助教建議使用 gem RuboCop 來協助檢查 Coding Style。在尚未修改相關設定檔案以前，試著直接輸入 `$ rubycop` 指令以預設標準進行檢查，結果顯示一共掃描了 50 個檔案，其中有高達 177 條 「罪行」 (offenses)。於是便立即著手修正，一度處理到只剩下 18 條。  
不過在贖罪的過程當中，有些啟人疑竇的發現，例如：RuboCop 居然建議修改 `bin` 資料夾裡的 `bundle`、`yarn` 還有 `webpack` 這些由 `$ rails new` 產生的原廠設定內容？便趕緊跟助教確認是不是走火入魔了（咦？

沒錯！那這篇文章就當作設定檔的紀錄囉！

### 設定檔
首先，請先在專案根目錄底下新增 `.rubocop.yml` 檔案

```shell
 $ cd PROJECTNAME
 $ touch .rubocop.yml
```

接著在終端機輸入 `$ rubocop --auto-gen-config` 指令  
執行後會自動產生 `.rubocop_todo.yml` 檔案

```shell
 $ rubocop --auto-gen-config
```
如果想要客製化自己的 RuboCop 設定，就得要靠這兩個檔案囉！  
那這兩個檔案到底有什麼功用呢？
* `.rubocop.yml`：可以撰寫相關設定來覆寫預設標準的檔案  

* `.rubocop_todo.yml`：自動生成將不符合標準 (Cops) 的剔除的設定檔內容。  
有點像是用來河蟹的防護網：如果測出來會不及格，那就不讓你考的概念（？

### 開始設定

因為 `.rubocop_todo.yml` 已經把所有檢查會產生錯誤的檔案排除  
如果不想修改專案程式碼的話，可以直接在`.rubocop.yml` 檔案裡，  
加上 `inherit_from: .rubocop_todo.yml` 來套用設定內容
```yaml
  # .rubocop.yml
  inherit_from: .rubocop_todo.yml
```
不過這樣感覺有點不思長進XD

應該要一點一點地解除防護網的保護，好好面對現實  
依據自己的抗壓性（？ 來逐步將檢查項目進行註解，然後反覆執行 `$ rubocop`  
每次噴出的錯誤都會明確地指出檔案名稱及行數，便可以依照建議來進行修正

如果碰到矯枉過正（？ 的建議或是前面提過 `bin` 裡面的原廠設定檔，就將排除檢查的相關設定寫在 `rubocop.yml` 檔案裡面。  
可以指定特定資料夾或檔案跳過所有的 Cops 檢查：撰寫 `Exclude DIR/FILE`，  
或是指定特定 Cops 跳過檢查特定檔案：複製 `.rubocop_todo.yml` 的程式碼，  
另外也能自行覆寫檢查的規則。

### 個人設定
最後附上目前的 `.rubocop.yml` 設定檔供參：  
逃避 `bin`、`config`、`db`、`node_module`、`spec`、`test` 等內容  
目前一共檢查 13 個檔案，且是沒有「罪行」的（可喜可賀

```yaml
  #  .rubocop.yml
  
  AllCops:
    Exclude:
      - 'bin/*'
      - 'config/**/*'
      - 'db/**/*'
      - 'node_modules/**/*'
      - 'spec/**/*'
      - 'test/**/*'
    TargetRubyVersion: 2.6

  Style/Documentation:
    Enabled: false

  Style/SymbolArray:
    Exclude:
      - 'Gemfile'

```

### 參考資料
[RuboCop // Docs](https://docs.rubocop.org/rubocop/configuration.html#enabled)  

[【五倍專欄】機器戰警 RuboCop](https://5xruby.tw/posts/rubocop-intro/)  

[用 Rubocop 寫出好風格 (Ruby & Rails Style Guide)](https://mgleon08.github.io/blog/2016/01/22/rubocop/)