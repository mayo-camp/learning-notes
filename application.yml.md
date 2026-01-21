# application.yml とは

# 概要

- 主に [Spring Boot アプリケーション](Spring_Boot.md)で使用される設定ファイル
- （NestJSなどでは`application.yml`は一般的ではなく、`config.ts`や`.env`ファイルを使うことが多い）

## 役割

- `application.yml`は、アプリ全体の設定（設定値や接続情報など）をまとめて記述するためのファイル
- [YAML](YAML.md) 形式で書かれており、階層的に設定を整理できる

## 主な用途

| 用途          | 設定例                 |
| ----------- | ------------------- |
| サーバ設定       | ポート番号、コンテキストパスなど    |
| データベース設定    | 接続URL、ユーザー名、パスワードなど |
| ログ設定        | ログレベル、出力先など         |
| プロファイル別設定   | 開発用・本番用などの切り替え      |
| 外部APIやメール設定 | APIキーやSMTP情報など      |

## 基本構文例

- 階層をインデントで表現する（スペース2つが一般的）
- タブは使えない

```yaml
server:
  port: 8080
  servlet:
    context-path: /api

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/sampledb
    username: root
    password: password
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true

logging:
  level:
    root: INFO
    com.example.project: DEBUG
```

## 複数環境の設定（プロファイル）

- 開発・テスト・本番など環境ごとに設定を分けることもできる
- 「---」で区切ることで1つのファイルで複数環境を管理できる

### ローカル環境で推奨される書き方

```yaml
spring:
  profiles:
    active: dev  # ← どのプロファイルを使うか指定

---
spring:
  config:
    activate:
      on-profile: dev
server:
  port: 8081

---
spring:
  config:
    activate:
      on-profile: prod
server:
  port: 80
```

### 本番環境で推奨される書き方

```yaml
# 共通設定（すべての環境で有効）
spring:
  application:
    name: sample-app

logging:
  level:
    root: INFO

---
spring:
  config:
    activate:
      on-profile: dev
server:
  port: 8081
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/devdb

---
spring:
  config:
    activate:
      on-profile: prod
server:
  port: 80
spring:
  datasource:
    url: jdbc:mysql://prod-db:3306/proddb

```

#### どのプロファイルを使うかは、コマンドラインで指定する

例：dev を有効化する

```
java -jar app.jar --spring.profiles.active=dev
```

- 必要に応じて、別ファイルに分ける（例：application-dev.yml, application-prod.yml）こともできるが、
小〜中規模プロジェクトでは、1つのYAMLにまとめた方が管理しやすい

## [application.properties](application.properties.md)との違い

| 比較項目 | application.properties | application.yml    |
| ---- | ---------------------- | ------------------ |
| 書き方  | キーと値を `=` で区切る         | インデントで階層表現         |
| 可読性  | フラット構造で簡単              | 階層構造で整理しやすい        |
| 推奨度  | 旧式（非推奨ではない）            | 新しいSpring Bootでは主流 |

```properties
server.port=8080
spring.datasource.url=jdbc:mysql://localhost:3306/sampledb
```

と書く代わりに、`application.`ymlでは階層的に書けるという違い

## よくある設定項目一覧

| 項目                       | 用途          |
| ------------------------ | ----------- |
| `server.port`            | サーバのポート番号   |
| `spring.datasource.*`    | データベース接続情報  |
| `spring.jpa.*`           | JPA（ORM）の設定 |
| `spring.mail.*`          | メール送信設定     |
| `logging.level.*`        | ログ出力レベル     |
| `management.endpoints.*` | Actuatorの設定 |

## 書く時の注意点

- インデントは スペースのみ使用（タブ禁止）
- 同じ階層のキーを重複させない
- 値に : を含む場合は "値" で囲む
- 機密情報（パスワード・APIキーなど）は `.env`や`Vault`に分離するのが推奨
