※詳しくは、documentationを読む


docker,vurtual box をインストール
vue.js をglobalインストール


webpackをインストール
$ vue init webpack vue-wp-project

<select>
・ Install vue-router? (Y/n) Y
・ Use ESLint to lint your code? (Y/n) n
・ Set up unit tests (Y/n) n
・ Setup e2e tests with Nightwatch? (Y/n) n

----------

インストールできたら
$   cd vue-wp-project
&   npm run dev


すると、こうなる
vue-wp-project
.
├─ build
│  └─ *.js
├─ config
│  └─ *.js
├─ src
│  ├─ assets
│  │   └─ logo.png
│  ├─ components
│  │   └─ HelloWorld.vue
│  ├─ router
│  │   └─ index.js
│  ├─ App.vue
│  └─ main.js
├─ static
├─ .*
├─ index.html
├─ package.json
└─ README.md

----------


wordpressをダウンロードして
wp-content　のみを以下の階層に配置
※　www/html フォルダは作成する

vue-wp-project
.
├─ www
   └─ html
   　　└─ wp-content


----------

webpackの設定ファイル書き換え
./config/index.js内、55行目辺りに記述されているbuildのPaths部分を次のように書き換えます。

// Paths
assetsRoot: path.resolve(__dirname, '../www/html/wp-content/themes/sample'),
assetsSubDirectory: 'assets',
assetsPublicPath: '/',

----------


さらに、./build/webpack.prod.conf.js内、outputに指定されている.[chunkhash]と、ExtractTextPlugin部分の.[contenthash]を削除します。

< ./build/webpack.prod.conf.js変更後記述 >

[   chunkhash  |||||| ]
  output: {
    path: config.build.assetsRoot,
    filename: utils.assetsPath('js/[name].js'),
    chunkFilename: utils.assetsPath('js/[id].js')
  },

[   contenthash  |||||| ]
    new ExtractTextPlugin({
      filename: utils.assetsPath('css/[name].css'),


----------


ここまで終えたら、下記のコマンドを実行します。

vue-wp-project 直下にて

$ npm install axios -D && npm install vuex -s
$ npm install
$ npm run build

--------------

その後、このコマンドを

$ docker-machine create --driver virtualbox vue-wp-project
$ eval $(docker-machine env vue-wp-project)
$ docker-compose up -d

--------------
ワードプレスにログイン後、http://192.168.99.100:6001　にアクセス!!




--------------   ここから環境構築編   --------------


npm run build
