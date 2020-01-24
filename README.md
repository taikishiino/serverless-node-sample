# 🚀 Nodejs12.x Handler on AWS Serverless

- serverless-offline: API Gatewayの代用として利用する
- serverless-dynamodb-local: Serverless FrameworkからDynamoDB Localを操作できるようにする

## Serverlessのインストール
```bash
$ npm install -g serverless
$ serverless --version

# serverless.ymlが生成される
$ sls create -t aws-nodejs -n serverless-janken
```

## DynamoDB Local のインストール
serverless-dynamodb-local プラグインを利用してDynamoDB Localをインストールします。
```bash
$ sls dynamodb install
```

## Setup
```bash
$ yarn install

# DynamoDB Local起動
$ yarn sls:dynamodb

# API Gatewayの代用のserverless-offlineを起動
$ yarn sls:local

# デプロイ
$ sls deploy
```


## DynamoDB Local の起動
```yml
serverless.yml
# service: serverless-janken の下に以下を追記
custom:
  dynamodb:
    start:
      port: 8000
      inMemory: true
      migrate: true
      seed: true
    seed:
      development:
        sources:
          - table: jankens
            sources: [./migrations/jankens.json]

resources:
  Resources:
    JankensTable:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: jankens
        AttributeDefinitions:
          - AttributeName: player
            AttributeType: S
          - AttributeName: unixtime
            AttributeType: N
        KeySchema:
          - AttributeName: player
            KeyType: HASH
          - AttributeName: unixtime
            KeyType: RANGE
        ProvisionedThroughput:
          ReadCapacityUnits: 1
          WriteCapacityUnits: 1
```
