---
title: Scope vs. Class Method
date: "2020-07-08T23:46:37.121Z"
template: "post"
draft: false
slug: "Scope"
category: "程式學習"
tags:
  - "筆記練習"
description: "在撰寫專案時，時常會使用到查詢符合特定條件或是針對物件進行排序的方法，如果查詢條件複雜或是頻率很高的話，可以考慮使用 Active Record 提供的 Scope 寫法..."
socialImage: ""
---
在撰寫專案時，時常會使用到查詢符合特定條件的 `where` 或是針對物件進行排序的 `order` 方法，如果查詢條件複雜或是頻率很高的話，可以考慮使用 Active Record 提供的 `Scope` 寫法來整理程式碼。

### Scope 有什麼好處呢？
- 維護性高：  
`Scope` 可以將原先四散在專案中、重複的程式碼整理到 Model，如果需要調整查詢或是排序條件，只需要在 Scope 裡作修改即可。
- 增加可讀性：  
為 `Scope` 命名，例如：[CodeBoard](https://code-board.com/) 專案中利用 `has_records` 來查詢已經有解題紀錄的題目， `Card.has_records` 讓程式碼閱讀起來更加語意化。


```ruby
  scope :has_records, ->{ where(solved: true) }
```


### Class method
仔細觀察 `Card.has_records` 語法，其實不難發現跟 `class Card` 的類別方法用法相同，事實上，也可以透過定義類別方法來達成 Scope 帶來的好處。
```ruby
  def self.has_records
    where(solved: true)
  end
```

### Scope 與 Class method 的差異
咦？不是說兩者可以做到一樣的事情嗎？那還有什麼差異呢？  
一起利用 Sushi Tech 專案來實驗看看吧！

假設輸入時段可以搜尋符合的 Menu ，並且可以依照更新時間來排序

```ruby
  # app/models/menu.rb

  # Scope
  scope :for_mealtime, ->(period) { where ['period = ?', Menu.periods["#{period}"]] }
  scope :recent, ->{ order( "menus.updated_at DESC") }
  
  # 類別方法
  def self.for_mealtime(period)
    where(period: period)
  end

  def self.recent
    order("menus.updated_at DESC")
  end
```
進到 `rails console` 來觀察實際進行資料庫查詢的 SQL 語法

```shell
  Menu.for_mealtime('lunch')
  # SELECT "menus".* FROM "menus" WHERE (period = 0) 
  #<ActiveRecord::Relation [#<Menu id: 1, name: "Lunch", created_at: "2020-07-02 10:21:13", updated_at: "2020-07-02 10:21:13", period: "lunch">]>
```
試著模擬未輸入搜尋條件時的查詢
```shell
  Menu.for_mealtime(nil)
  # SELECT "menus".* FROM "menus" WHERE (period = NULL)
  #<ActiveRecord::Relation []>
```
如果想避免用 NULL 進行搜尋，試著使用 `present?` 方法來進行判斷，在有下搜尋條件時才進行搜尋
```ruby
  scope :for_mealtime, ->(period) { where ['period = ?', Menu.periods["#{period}"]] if period.present? }
```
此時， 在 `Scope` 下判斷式的搜尋結果，會印出所有的 Menu
```shell
  Menu.for_mealtime(nil)
  # SELECT "menus".* FROM "menus"
  #<ActiveRecord::Relation [#<Menu id: 2, name: "Dinner", created_at: "2020-07-02 10:21:13", updated_at: "2020-07-02 10:21:13", period: "dinner">, #<Menu id: 1, name: "Lunch", created_at: "2020-07-02 10:21:13", updated_at: "2020-07-02 10:21:13", period: "lunch">]>

  Menu.for_mealtime(nil).recent
  # SELECT "menus".* FROM "menus" ORDER BY menus.updated_at DESC
  #<ActiveRecord::Relation [#<Menu id: 2, name: "Dinner", created_at: "2020-07-02 10:21:13", updated_at: "2020-07-02 10:21:13", period: "dinner">, #<Menu id: 1, name: "Lunch", created_at: "2020-07-02 10:21:13", updated_at: "2020-07-02 10:21:13", period: "lunch">]>

  Menu.for_mealtime(nil).class
  # Menu::ActiveRecord_Relation
```
同樣在類別方法新增 `present?` 判斷式
```ruby
  def self.for_mealtime(period)
    where(period: period) if period.present?
  end
```
結果回傳 `nil`，繼續使用 `recent` 連續技的話則會噴出 `NoMethodError`
```shell
  Menu.for_mealtime(nil)
  # nil

  Menu.for_mealtime(nil).recent
  # NoMethodError (undefined method `recent' for nil:NilClass)

  Menu.for_mealtime(nil).class
  # NilClass
```
要避免噴錯的話需要再進行小小加工
```ruby
  def self.for_mealtime(period)
    if period.present?
      where(period: period)
    else
      Menu.all
    end
  end
```

### 使用時機
實驗到最後可以發現，其實兩者的差異並不是特別明顯，如果要區分使用時機的話，搜尋情形簡單的話可以使用 `Scope`，複雜程度較高的話，就使用類別方法來處理吧！

### 參考資料
[為你自己學 Ruby on Rails](https://railsbook.tw/chapters/16-model-basic.html#scope-and-class-method)  

[Active Record scopes vs class methods](http://blog.plataformatec.com.br/2013/02/active-record-scopes-vs-class-methods/)  
  
[如何漂亮寫出可維護的 query](https://blog.niclin.tw/2020/01/05/maintainable-rails-query/)
