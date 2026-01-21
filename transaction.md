# トランザクション

## トランザクションとは？

開始（BEGIN）から確定（COMMIT）または取消（ROLLBACK）までを1単位とする処理のまとまりであり、<br />
途中でエラーが発生した場合は処理前の状態に戻ることが保証される

## ACID特性（標準的定義）

| 特性               | 内容                |
| ---------------- | ----------------- |
| Atomicity（原子性）   | 全処理が成功するか、全て失敗する  |
| Consistency（一貫性） | ルールを破る状態に遷移しない    |
| Isolation（独立性）   | 他トランザクションの影響を受けない |
| Durability（耐久性）  | 確定した結果は失われない      |

## 実装例（C#）

```C#
using MySql.Data.MySqlClient;

public void ExecuteTransaction()
{
    string connectionString =
        "Server=localhost;Database=testdb;Uid=user;Pwd=password;";

    using (var connection = new MySqlConnection(connectionString))
    {
        connection.Open();

        // トランザクション開始
        using (MySqlTransaction transaction = connection.BeginTransaction())
        {
            try
            {
                // 1つ目のSQL
                using (var cmd1 = connection.CreateCommand())
                {
                    cmd1.Transaction = transaction;
                    cmd1.CommandText =
                        "INSERT INTO orders (order_id, amount) VALUES (1, 1000)";
                    cmd1.ExecuteNonQuery();
                }

                // 2つ目のSQL
                using (var cmd2 = connection.CreateCommand())
                {
                    cmd2.Transaction = transaction;
                    cmd2.CommandText =
                        "UPDATE stock SET quantity = quantity - 1 WHERE product_id = 10";
                    cmd2.ExecuteNonQuery();
                }

                // 全て成功したら確定
                transaction.Commit();
            }
            catch
            {
                // 途中で失敗したら取り消し
                transaction.Rollback();
                throw;
            }
        }
    }
}
```
