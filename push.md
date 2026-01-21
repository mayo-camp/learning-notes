# push

## ブランチAに、ブランチBの特定のファイルをpushする方法

```bash
# ブランチAへ移動
git switch ブランチA

# ブランチBから特定のファイルを取得して上書き
git restore --source ブランチB ファイルパス

# 反映
git add ファイルパス
git commit -m "コミットメッセージ"

# push
git push origin ブランチA
```
