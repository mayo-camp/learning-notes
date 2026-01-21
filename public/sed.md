# `sed` コマンド

## 概要

- 「stream editor（ストリームエディタ）」の略で、テキストを置換・削除・抽出などで変換するコマンドラインツール
- 主に UNIX/Linux 環境で使われる

## 基本構文

```bash
sed オプション 'コマンド' ファイル名
```

## よく使うコマンド

### 1. 基本的な置換

#### 例：各行の最初の `apple` を `banana` に置き換える

```bash
sed 's/apple/banana/' file.txt
```

#### 例：各行のすべての `apple` を `banana` に置き換える

```bash
sed 's/apple/banana/g' file.txt
```

### 2. ファイルを直接書き換える

#### 例：ファイル内の `http:` を `https:` に変更（上書き保存）

```bash
sed -i 's/http:/https:/g' config.txt
```

#### 例：ファイル内の「http:」を「https:」に変更（上書き保存）且つ、元ファイルのバックアップも作成

```bash
sed -i.bak 's/http:/https:/g' config.txt
```
### 3. 特定行の削除

#### 例：3行目を削除

```bash
sed '3d' file.txt
```

#### 例：1〜5行目を削除

```bash
sed '1,5d' file.txt
```

#### 例：`error` を含む行を削除

```bash
sed '/error/d' file.txt
```

### 4. 特定の行のみ表示

#### 例：5行目だけ表示

```bash
sed -n '5p' file.txt
```

#### 例：10〜15行目を表示

```bash
sed -n '10,15p' file.txt
```

#### 例：`keyword` を含む行だけを表示

```bash
sed -n '/keyword/p' file.txt
```

### 5. 特定の行の前後に文字列を追加

#### 例：`old` のある行の前に `new` を追加

```bash
sed '/pattern/i inserted line' file.txt
```

#### 例：`old` のある行の後に `new` を追加

```bash
sed '/pattern/a appended line' file.txt
```

### 6. 行の入れ替えや変更

#### 例：2行目を `new`に置き換える

```bash
sed '2c this is a new line' file.txt
```

#### 例：`old` を含む行を `new` に置き換える

```bash
sed '/old/c new' file.txt
```

### 7. 正規表現を使う

#### 例：`#` で始まる行を表示（コメント行など）

```bash
sed -n '/^#/p' file.txt
```

#### 例：2桁の数字を `**` に置き換える

```bash
sed 's/[0-9][0-9]/**/g' file.txt
```

#### 例：各行末尾の空白を削除する

```bash
sed 's/[[:space:]]*$//' file.txt
```

---

## 🔄 8. 複数コマンドをまとめて実行

```bash
sed -e 's/foo/bar/g' -e '/test/d' file.txt
```

→ `foo` を `bar` に置き換え、`test` を含む行を削除。

---

## 🪣 9. 行の結合・分割

```bash
sed 'N;s/\n/ /' file.txt
```

→ 2行を1行にまとめて（改行をスペースに置換）。

---

## 🔢 10. 行番号の挿入

```bash
sed = file.txt | sed 'N;s/\n/\t/'
```

→ 各行の前に行番号を付ける。

出力例：

```
1    line1
2    line2
3    line3
```

---

## 📦 11. ファイル先頭や末尾への追加

```bash
sed '1i # Header comment' file.txt
```

→ 先頭に `# Header comment` を追加。

```bash
sed '$a # End of file' file.txt
```

→ 最終行の後に `# End of file` を追加。

---

## 🔤 12. 特定の文字列を含む行の前後数行を表示

```bash
sed -n -e '/ERROR/{=;x;1!p;g;$!N;p;D}' file.txt
```

→ “ERROR” を含む行の前後1行を表示（簡易ログ解析などに使用）。

---

## 🧮 13. 空行処理

```bash
sed '/^$/d' file.txt
```

→ 空行を削除。

```bash
sed '/^$/!d' file.txt
```

→ 空行だけ残す。

---

## 🧱 14. 区間抽出

```bash
sed -n '/<start>/,/<end>/p' file.txt
```

→ `<start>` と `<end>` の間にある行を抜き出す。

---

## 🧰 15. ファイル内容を整形

```bash
sed 's/  */ /g' file.txt
```

→ 連続するスペースを1個にする。

---

## 🪶 16. 区切り文字を変える（スラッシュ以外）

```bash
sed 's|/usr/local|/opt|g' file.txt
```

→ `/` を含むパス置換時に便利。

---

## ⚙️ 17. 環境変数を展開

```bash
var="foo"
sed "s/foo/${var}/g" file.txt
```

→ シェル変数を使う場合は **ダブルクォーテーション必須**。

---

## 🪞 18. コメント行をスキップして置換

```bash
sed '/^#/!s/yes/no/' file.txt
```

→ `#` で始まらない行のみ置換。

---

## 🔚 19. 最終行のみ処理

```bash
sed '$s/end/finish/' file.txt
```

→ 最終行だけ `end` → `finish` に置換。

---

## 🧾 20. 複数ファイルに適用

```bash
sed -i 's/foo/bar/g' *.txt
```

→ カレントディレクトリ内のすべての `.txt` ファイルに適用。

---

これらを覚えると、ログ整形や設定ファイル修正、バッチ処理などで非常に役立ちます。
必要なら「特定の用途別」（例：ログ整形・設定ファイル修正・スクリプト埋め込みなど）に整理した一覧も作成できます。

どんな用途で使いたいですか？



