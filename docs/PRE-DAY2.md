# 次回の準備課題

次回は、Webページからの応答を確認し、その結果と日時をレポートにまとめます。この処理を、自分のRepositoryで動かします。その材料になる二つのファイルを、自分のCodespaceに用意しておいてください。目安は10分ほどです。提出はありません。

## 用意するもの

`pre_day2`というフォルダーの中に、次の二つを作ります。

- `pre_day2/template.md` — レポートの文章の原本
- `pre_day2/generate.sh` — 原本からレポートを作る処理

中身は、このページの下にあります。読んで動かすのは次回です。今回は用意するだけで構いません。

## やること

1. 今日使っていたCodespaceを開きます。タブが残っていれば、それに戻ります。閉じていた場合は、GitHubで自分のRepositoryのページを開き、今日と同じように`Code`から`Codespaces`を選ぶと、今日作ったCodespaceが一覧に出るので、それを押します。新しく作らないでください。
2. ファイル一覧で、新しいファイル`pre_day2/template.md`を作ります。作り方は次の節にあります。
3. 下の「template.md」の枠の中身をコピーして貼り付け、今日と同じように保存します。
4. `pre_day2/generate.sh`も、同じ手順で作って保存します。
5. 二つのファイルを開いて、このページの枠と同じ中身になっているか見比べます。

今回はbranchを作らず、masterのまま進めて構いません。記録は次回に行います。

## 新しいファイルの作り方（今日の実習で扱っていない操作）

1. ファイル一覧の一番上にある、自分のRepository名の行（大文字で表示されています）を右クリックします。
2. 出てきたメニューの`New File...`（新しいファイル）を選びます。名前の入力欄が出ます。
3. `pre_day2/template.md`と入力して、Enterキーを押します。`pre_day2`フォルダーと、その中のファイルが同時にでき、空の編集画面が開きます。

表示が違う場合は、ファイル一覧の中で「新しいファイル」にあたる項目を探してください。また、ファイルが並んでいる下の空白を右クリックしても、同じメニューが出ます。Codespaceの編集画面は、VS Codeというエディターです。公式の画面案内: [User Interface — Explorer（VS Code Docs）](https://code.visualstudio.com/docs/editing/getting-started/userinterface#_explorer-view)

## 貼り付ける中身

枠の中身だけを貼り付けます。ファイル名や、枠を囲む記号は入れません。ターミナルには貼り付けないでください。今回は実行しません。

### template.md

```markdown
# Webページの確認レポート

対象: https://example.com/
確認日時（UTC）: {{CHECKED_AT}}
応答コード: {{HTTP_STATUS}}

このレポートは、原本と確認結果をもとに生成します。
```

### generate.sh

```sh
#!/bin/sh
set -eu

cd "$(dirname "$0")"
http_status=$(curl --fail --silent --show-error --location --max-time 30 \
  --output /dev/null --write-out '%{http_code}' https://example.com/)
checked_at=$(date -u '+%Y-%m-%d %H:%M:%S UTC')

sed -e "s|{{CHECKED_AT}}|$checked_at|g" \
    -e "s|{{HTTP_STATUS}}|$http_status|g" template.md > report.md
printf '%s\n' 'report.md を生成しました。'
```

## できた状態

- ファイル一覧の`pre_day2`の中に二つのファイルがあり、開くとこのページの枠と同じ中身です。
- 今日の実習で、変更したファイルの名前を確かめた操作をすると、`pre_day2/`が表示されます。新しく作ったファイルなので、今日とは違う見出しの下に出ますが、`pre_day2/`が見えていれば大丈夫です。保存はしたが、まだ記録していない状態です。記録は次回に行うので、今回はここまでで完了です。
- 次回はこのCodespaceから始めます。Codespaceは消さずに残してください。もしCodespaceが消えていた場合は、今日と同じ手順で新しく作り、二つのファイルもこのページの手順でもう一度作ります。
