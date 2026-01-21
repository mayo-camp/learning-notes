# LAST_INSERT_ID()

- MySQLにおいて「直前に実行された INSERT 文で自動採番（AUTO_INCREMENT）された値」を取得するための関数
- 現在の接続（セッション）で最後に成功した INSERT により生成された AUTO_INCREMENT 値を返す
  - 接続単位で管理される（他セッションの影響を受けない）
  - 1行 INSERT でも、複数行 INSERT でも取得可能
  - トランザクション中でも使用可能
 
## 基本的な使用例（SQL）

```sql
INSERT INTO users (name) VALUES ('Alice');
SELECT LAST_INSERT_ID();
-- users.id が AUTO_INCREMENT の場合、その 採番されたID が返る
```
