# uec20_risajuu_front
『電通大りさじゅうのチャットボット作成プロジェクト』のフロントエンドです。

## 起動方法
### ローカル環境の場合
#### run devの場合
```bash
$ cd risajuuu-app
$ npm i
$ npm run dev
```
#### run buildの場合
```bash
$ cd risajuuu-app
$ npm i
$ npm run build
$ npm start
```
localhost:3000にアクセス

### dockerを使う場合(今は設定がうまく行ってないので動かない可能性あり)
```bash
$ docker-compose up -d
$ docker-compose exec app_front bash
$ npm run dev
```
localhost:3000にアクセス
