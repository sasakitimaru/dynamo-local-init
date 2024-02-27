## 概要
[DynamoDB-Local](https://hub.docker.com/r/amazon/dynamodb-local)コンテナをシード値を取り込んで起動できます。  
`./schema/`にあるyml, jsonファイルを読み込んで起動します。
## 使い方
### 起動
リポジトリをクローンして下記のコマンドを実行します。  
※`./schema`ディレクトリのデータをシードとしてdynamodbを起動します。
```
mkdir -p dynamodb-local
sudo chmod 777 ./dynamodb-local
docker-compose up
```

### 停止
```
docker-compose down
```
停止してもdynamodbのデータは残ります。  
`./dynamodb-local/shared-local-instance.db`を削除すればデータも消えます。  
テーブルの構造やシード値などを変更した場合は一度データを消してから再度`docker-compose up`してください。

### テーブルの作成
`./schema/tables.yml`を取り込みテーブルを作成します。  
[aws-cli ver2](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/dynamodb/create-table.html)のドキュメントに従った形式で記述してください。  
データは`--cli-input-yaml`で取り込まれます。

### シード値の取り込み
`./schema/seeds.yml`を取り込みテーブルにデータを作成します。  
下記の形式で定義します。
```yml
- table: SomeTable
  sources: [SomeTable.json]
```

`/home/dynamodblocal/schema/seeds/`にシードとなるデータをjsonファイルで保存します。  
リストやマップも対応しています。  
下記の形式が参考です。
```json
[
  {
    "id": "xxx",
    "name": "example",
    "data": {
      "price": 100,
      "category": "free",
    },
    "tag": ["new", "sample", "season"]
  }
]
```

## 引用
https://github.com/sasakitimaru/dynamo-local-migrator

https://hub.docker.com/repository/docker/sasakitimaru/dynamodb-local/general

https://hub.docker.com/r/amazon/dynamodb-local
