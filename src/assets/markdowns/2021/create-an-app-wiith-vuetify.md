# Vuetify: Create an App with Vue JS
**概要**
Vue + Vuetify でTODOリストを作成する。

**前提知識**
基本的なVue.jsの知識 (Vue-routerが使える程度)
Vue CLIでプロジェクトを作成できること

**参考**
https://www.youtube.com/watch?v=CjXgoYo86yY


## 1. Vueプロジェクトのセットアップ
**コマンドおよびオプション例**
```
$ vue create vuetify-app
  * Manually select features
  * Router, Vuex, CSS Pre-processors
  * version : 2.x
  * history mode : No
  * CSS pre-processor : Sass/SCSS (with dart-sass)
  * Linter : Basic
  * lint features : Lint on save
  * Babel, ESLint, : In dedicated config file
  * Save : No
$ cd vuetify-app
$ vue add vuetify
  *  Choose a preset: Default
```

## 2. ワイヤーフレームの選択

https://vuetifyjs.com/ja/getting-started/wireframes/

1. はじめましょう / ワイヤフレーム をクリック

2. サンプル（Base）をクリック

3. 右下のgithubアイコンをクリック

4. `App.vue`にすべてコピペ



## 3. ナビゲーションの設定

https://vuetifyjs.com/ja/components/navigation-drawers/

1. UIコンポーネント / Navigation drawers をクリック

2. 「使い方」の下にある`<>`アイコンをクリック

3. `App.vue`に`<v-navigation-drawer>`部分をを上書きする(template)

4. (script) 部分の`items`を`<script>`タグ内の`data(){return{ items: []}}`に追加

5. v-navigation-drawer コンポーネントに`parmament`を削除し`app`と`v-model="drawer"`を追加

+ propsの内容について（上記の例だとparmament, appなど）は各種APIを参照のこと
https://vuetifyjs.com/ja/api/v-navigation-drawer/#props 



## 4. アプリケーションの表示

https://vuetifyjs.com/ja/components/application/

1. UIコンポーネント / Application にて記載方法を確認

2. `App.vue`の`<v-main>`の中に`<router-view>`を追加



## 5. リンクの設定

https://vuetifyjs.com/ja/api/v-list-item/#props

1. UIコンポーネント / Lists / v-list-item API / toの項目を確認

2. `items`のオブジェクトに`to`を追加

3. `<v-list-item :to="item.to">`を追加

* デフォルトだと`Home.vue`が`/`の割り当てになっているので、必要に応じて `Home.vue => Todo.vue`にrenameして`router/index.js`を修正する。



## 6. iconの設定

https://vuetifyjs.com/ja/components/icons/

https://materialdesignicons.com/

1. UIコンポーネント / icons / Material Design Iconsを選択

2. todoで検索

3. `items`の`icon: 'mdi-format-list-checks'`を設定



## 7. AboutコンポーネントのSpacing (padding) の設定

https://vuetifyjs.com/ja/styles/spacing/

1. スタイルとアニメーション / Spacing を確認

2. `div class="about pa-6`を指定


## 8. app-barの設定

https://vuetifyjs.com/ja/components/app-bars/

1. UIコンポーネント / Bars / App bars を選択

2. Imagesの`<>`をクリックし、`App.vue`に`<v-app-bar>`部分を上書き

3. `<v-app-bar>`の`absolute`を削除し、`app`を追加。`shrink-on-scroll`, `scroll-target`を削除。`prominent`を追加

4. `public`フォルダに画像を保存し、`src="xxx.jpg"`とする

5. `<v-app-bar-nav-icon @click="drawer = !drawer">`を追加する

6. `<v-app-bar-title>`でタイトルを修正



## 9. Listの追加　( todoリスト )

https://vuetifyjs.com/ja/components/lists/#section-30b530d630d830c330c03068533a5207308a7dda



1. UIコンポーネント / Lists / サブヘッダと区切り線 を選択

2. `<>`からHangout notifications部分の`<v-list>`をTodo.vue(Home.vue)にコピペ

3. `<v-subheader>`を削除。`subheader` propsを削除。

4. `<v-list-item-group>`を削除。`<v-list-item>`を1つだけ残して他を削除

5. `two-line`propsと`<v-list-item-subtitle>`を削除

6. `data()`にtasksの配列を用意( keyはid, title,  done )

7. `v-list-item`の上に`<div v-for="task in tasks :key="task.id">`, `{{task.title}}`の追加

8. `<v-divider>`を追加

9. `<v-list class="pt-0">`を追加

10. `<v-checkbox :input-value="task.done">`にして、`<template v-slot:default>`にする

11. `<v-list-item>`に`@click="doneTask(task.id)"`を追加

12. `methods: doneTask(task.id)`の追加

    ```
    doneTask(id) {
    	let task = this.tasks.filter(task => task.id === id)[0]
    	task.done = !task.done
    }
    ``` 



## 9. 色や文字表示の変更

https://vuetifyjs.com/ja/styles/colors/#section-30de30c630ea30a230eb30ab30e930fc

https://vuetifyjs.com/ja/styles/text-and-typography/#decoration

1.  スタイルとナビゲーション / Colorsをクリック
2. `<v-list-item :class="{ 'blue lighten-5' : task.done }"`を追加
3.  スタイルとナビゲーション / Text-and-typography をクリック
4. `<v-list-item-title :class="{'text-decoration-line-through' : task.done }">`を追加



## 10.  Deleteボタンの追加

https://vuetifyjs.com/ja/components/lists/#two-lines-and-subheader
https://materialdesignicons.com/

1. UIコンポーネント / Lists / Tow lines and subheaderを選択

2. 下のほうの`<v-list-item-action>` 部分をコピー。`Todo.vue`の`v-list-item.template`直下に貼り付け

3.  material design iconページから delete で検索し、
   `<v-icon>mdi-infomation</v-icon>` => `<v-icon>mdi-delete</v-icon>`にする

4. `<v-icon color="primary lighten-1">`にする

5. `v-btn @click.stop="deleteTask(task.id)">`を追加

6. `deleteTask()`メソッドの追加

   ```
   deleteTask(id) {
       this.tasks= this.tasks.filter(task => task.id !== id)
   }
   ```



## 11. タスクの追加

https://vuetifyjs.com/ja/components/text-fields/#icons

1. UIコンポーネント / Form inputs & Control / text fields / icons を選択

2. 以下をコピペ
    ```
    <v-text-field
      outlined
      label="Append"
      append-icon="mdi-map-marker"
    ></v-text-field>
    ```

3. `<v-text-field append-icon="mdi-plus" hide-details clearable class="pa-3">`を追加

4. `<v-text-field model="newTaskTitle">`を追加

5. `<v-text-field @click:append="addTask">`を追加

6. `<v-text-field @keyup.enter="addTask">`を追加

   ```
   addTask() {
     let newTask = {
       id: Date.now(),
       title: this.newTaskTitle,
       done: false
     }
     this.tasks.push(newTask)
     this.newTaskTitle = ''
   }
   ```



 