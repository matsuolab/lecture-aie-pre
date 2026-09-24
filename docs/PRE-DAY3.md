# 次回の準備課題

前回、`pre_day2/template.md`と`pre_day2/generate.sh`を用意しました。今回は、その二つを実際に編集して動かし、「この処理がどこで実行されたか」をレポートに残せるようにします。手元のCodespaceと、GitHub Actionsのrunnerという、二つの違う場所で同じ`generate.sh`を動かし、それぞれが記録した値を見比べます。branchを作ってPull Requestで取り込むところまでは、前回の実習で行った操作をもう一度使うだけで、新しい操作は増えません。あわせて、次回使う大きなファイルを先に取り込んでおく準備も行います（10節）。所要時間の目安は合計30分です。提出はありません。

各節の見出しに目安の時間を書いています。合計すると次のとおり30分です。

| 節 | 目安時間 |
| --- | ---: |
| 1. 今日のCodespaceと対象ファイルを確認する | 2分 |
| 2. 課題用のbranchを作る | 2分 |
| 3. template.mdを編集する | 2分 |
| 4. generate.shを編集する | 4分 |
| 5. 手元で生成して値を控える | 2分 |
| 6. 変更を記録してPull Requestを作る | 5分 |
| 7. ActionsとBot commitを確認して値を控える | 3分 |
| 8. 二つの値を比べる | 2分 |
| 9. 手元へ結果を取り込む | 2分 |
| 10. 次回の準備を実行しておく | 5分 |
| 11. 次回への接続 | 1分 |
| 合計 | 30分 |

## 1. 今日のCodespaceと対象ファイルを確認する（目安2分）

今日使っていたCodespaceを開き、`pwd`でRepositoryの先頭にいることを確認します。

```sh
pwd
```

続けて、今回編集する二つのファイルがあることを確認します。

```sh
ls pre_day2
```

`template.md`と`generate.sh`が表示されれば確認は完了です。今回この二つへ追加する三行は、このページの3節・4節にそのまま載せてあります。編集するときは、3節・4節の枠から写してください。

## 2. 課題用のbranchを作る（目安2分）

まず、今の状態を確認します。

```sh
git status
```

`master`で、記録されていない変更がないことを確認してから、課題用のbranchを作ります。

```sh
git switch -c feature/record-machine-name
```

## 3. template.mdを編集する（目安2分）

対象のフォルダーへ移動します。

```sh
cd pre_day2
```

続けて、`template.md`を`nano`コマンドで開いて編集します。開き方と保存のしかたは、今日の実習で使ったものと同じです。

`template.md`を対象に指定して`nano`コマンドで開き、「確認日時」の行の次へ、次の一行をそのまま追加して保存します。

```
実行したマシン: {{MACHINE_NAME}}
```

## 4. generate.shを編集する（目安4分）

`pre_day2`フォルダーにいる状態のまま、`generate.sh`も`nano`コマンドで開いて編集します。

`generate.sh`を対象に指定して`nano`コマンドで開き、`checked_at=`の行の次へ、次の一行をそのまま追加します。

```
machine_name=$(cat /proc/sys/kernel/hostname)
```

続けて、`{{HTTP_STATUS}}`を置き換える`sed`の行の前へ、次の一行をそのまま追加して保存します。行末の`\`（バックスラッシュと呼びます）も含めてください。これは、この行の続きが次の行にあることを示す記号です。これが消えてしまうと、次の行が別のコマンドとして扱われ、`./generate.sh`を実行したときに何も表示されないまま入力待ちの状態で止まります。

```
        -e "s|{{MACHINE_NAME}}|$machine_name|g" \
```

`/proc/sys/kernel/hostname`は、いま動いているマシンが自分の識別名を記録しているファイルの場所です。`cat`でこのファイルの中身を読むと、その識別名が文字として得られます。

## 5. 手元で生成して値を控える（目安2分）

`pre_day2`フォルダーにいる状態で、生成処理を実行します。

```sh
./generate.sh
cat report.md
```

### 観察点

`report.md を生成しました。`と表示され、`cat`の出力の中に「実行したマシン: 」で始まる行があり、コロンの後ろに文字列が入っていれば、手元の生成は成功です。成功した場合は、この値を、あとで見比べるためにメモへ控えてください（このメモは自分の手元に残すだけで構いません）。

### うまくいかないとき

`./generate.sh`を実行しても何も表示されず、入力待ちの表示が戻ってこない場合は、`Ctrl`キーを押しながら`C`キーを押して中断してください。4節で追加した二つ目の枠の行末に`\`（バックスラッシュ）があるかを確認し、直したら保存してから`./generate.sh`をもう一度実行します。

## 6. 変更を記録してPull Requestを作る（目安5分）

```sh
git status
```

変更されたファイルが`template.md`、`generate.sh`、`report.md`の三つだけであることを確認してから、記録します。

```sh
git add template.md generate.sh report.md
git commit -m "実行したマシンを記録"
git push -u origin feature/record-machine-name
```

GitHubで、`feature/record-machine-name`から`master`へのPull Requestを作ります。`Files changed`（画面にこの表記で出ています。変更されたファイルの一覧です）を開き、三つのファイルだけが変更されていることを確認してから取り込みます。

## 7. ActionsとBot commitを確認して値を控える（目安3分）

Repositoryの`Actions`を開き、一覧の一番上にある実行（`Update report`という名前です）を開きます。`Generate the report`というstepが緑色で、`report.md を生成しました。`と表示されていることを確認してください。

続けて、Repositoryの`Commits`を開き、`github-actions[bot]`のcommitの差分を開きます。`report.md`の中の「実行したマシン: 」の後ろに値が入っていることを確認し、その値をメモへ控えてください。

### うまくいかないとき

赤色のstepがある場合、先へ進みません。最初に赤くなったstepを開き、いちばん最初に出ているエラーの行を控えてください。

多くの場合、原因は3節・4節で追加した行にあります。`cat template.md`と`cat generate.sh`で、追加した行がこのページの枠と同じか、追加した位置が合っているかを見比べてください。直すときは、6節のPull Requestはもう取り込み済みなので、新しいbranchを作ってから直します。9節の`git switch master`と`git pull --ff-only`で手元をそろえたあと、2節と同じ`git switch -c`で新しいbranch（例：`feature/fix-machine-name`）を作ってください。そのbranchで行を直し、5節の`./generate.sh`で手元の生成を確かめてから、6節と同じようにcommit、push、Pull Requestでの取り込みを行います。取り込むと、Actionsがもう一度動きます。

見比べても原因が分からない場合は、控えたエラーの行を手がかりに、公式ドキュメントやAIで調べてください。

## 8. 二つの値を比べる（目安2分）

5節で控えた値（Codespace側）と、7節で控えた値（GitHub Actions側）を並べて書き出してください。

この二つは、違っていて正解です。同じ`generate.sh`を、二つの別々のマシンの上で動かしたので、それぞれのマシンが自分の識別名を答えます。もし同じに見えた場合は、5節と7節の値をもう一度控え直してから見比べてください。

## 9. 手元へ結果を取り込む（目安2分）

Codespaceへ戻り、次を実行します。

```sh
git switch master
git pull --ff-only
git status
```

`master`で、記録されていない変更がなければ、今回の課題は完了です。

### うまくいかないとき

`git pull --ff-only`が受け取らずに止まった場合は、表示されたメッセージを控えてください。`--ff-only`は、手元の履歴にGitHub側と食い違う変更があると、混ぜずに止まる指定です。`git status`で記録されていない変更がないかを確かめ、控えたメッセージを手がかりに、公式ドキュメントやAIで調べてください。

## 10. 次回の準備を実行しておく（目安5分）

次回使う大きなファイルを、今のうちに取り込んでおきます。

ファイル一覧で、新しいファイル`pre_day3/prepare.sh`を作ります。作り方は、前回の準備課題で使ったものと同じです（ファイル一覧の一番上にある、自分のRepository名の行を右クリック→`New File...`→`pre_day3/prepare.sh`と入力してEnter）。

下の枠の中身だけをコピーして貼り付け、保存します。

```sh
#!/bin/sh

# このファイルは、Day3（Docker演習パート）の準備用プログラムです。
# 講義の前に一度実行しておくことで、講義中に待たずに済むようにします。
# 保存場所は Repository の中の pre_day3/prepare.sh、実行は
#   chmod +x pre_day3/prepare.sh
#   ./pre_day3/prepare.sh
# の2つだけです（Day2で覚えた操作と同じです）。

# 途中でエラーが起きたら、その時点で止めます（-e）。
# 中身が決まっていない値を使おうとしたときも止めます（-u）。
set -eu

# [1] 取得するimageとモデルの版を、変数にまとめておきます。
#     ここは受講者が書き換える場所ではありません。
IMAGE='ghcr.io/ggml-org/llama.cpp:server-v0.5.0'
MODEL_DIR='/workspaces/models'
MODEL_FILE='gemma-4-E2B-it-UD-Q4_K_XL.gguf'
MODEL_URL='https://huggingface.co/unsloth/gemma-4-E2B-it-GGUF/resolve/0314792d7f1f7e229411f620751375812bb9faf2/gemma-4-E2B-it-UD-Q4_K_XL.gguf'
MODEL_SHA256='b52f438017efaec5debf1c0d8be690571e212a07c312f1102bbce927258cfc32'
LLM_CHAT_DIR='/workspaces/llm-chat'
# 空き容量の目安：取得量は約3.5GB。余裕を見て4GB（4×1024×1024 KB）を基準にします。
REQUIRED_FREE_KB=4194304

# [2] Dockerのserverにつながるか確認します。
#     つながらなければ、ここで止めて次の行動を伝えます。
check_docker() {
  if ! docker version >/dev/null 2>&1; then
    printf '%s\n' 'エラー：Dockerが使えません。' >&2
    printf '%s\n' 'このCodespaceでDockerの起動が終わっていないか、Dockerが使えない環境です。' >&2
    printf '%s\n' '数十秒待ってからもう一度 ./pre_day3/prepare.sh を実行してください。' >&2
    exit 1
  fi
}

# [3] /workspaces の空き容量を確認します。
#     足りなければ、ここで止めて次の行動を伝えます。
check_disk_space() {
  free_kb=$(df -Pk "$MODEL_DIR_PARENT" | awk 'NR==2 {print $4}')
  if [ "$free_kb" -lt "$REQUIRED_FREE_KB" ]; then
    printf '%s\n' 'エラー：/workspaces の空き容量が足りません。' >&2
    printf '%s\n' '約4GB以上の空きが必要です（image約308MB、モデル約3.2GB）。' >&2
    printf '%s\n' '使っていないCodespaceを削除するか、不要なファイルを消してから、' >&2
    printf '%s\n' 'もう一度 ./pre_day3/prepare.sh を実行してください。' >&2
    exit 1
  fi
}

# [4] llama.cppのimageを取得します。すでに取得済みなら取り直しません。
pull_image() {
  if docker image inspect "$IMAGE" >/dev/null 2>&1; then
    printf '%s\n' "imageは取得済みです（$IMAGE）。取得をとばします。"
    return 0
  fi
  printf '%s\n' "imageを取得します（$IMAGE）。数分かかることがあります。"
  if ! docker pull "$IMAGE"; then
    printf '%s\n' 'エラー：imageの取得に失敗しました。' >&2
    printf '%s\n' 'ネットワークの状態を確認し、もう一度 ./pre_day3/prepare.sh を実行してください。' >&2
    exit 1
  fi
}

# [5] ファイルのSHA-256が期待どおりか確認します。一致すれば0、しなければ1を返します。
matches_sha256() {
  target_file="$1"
  [ -f "$target_file" ] || return 1
  actual=$(sha256sum "$target_file" | awk '{print $1}')
  [ "$actual" = "$MODEL_SHA256" ]
}

# [6] モデルファイルを取得します。
#     一致済みなら取り直さず、途中で止まったファイルは消して取り直します。
fetch_model() {
  mkdir -p "$MODEL_DIR"
  model_path="$MODEL_DIR/$MODEL_FILE"
  tmp_path="$model_path.download"

  if matches_sha256 "$model_path"; then
    printf '%s\n' "モデルは取得済みで内容も一致しています（$model_path）。取得をとばします。"
    return 0
  fi

  # 前回途中で止まったファイルが残っていたら消します。
  rm -f "$tmp_path"

  printf '%s\n' 'モデルを取得します。1分程度かかることがあります。'
  if ! curl --fail --silent --show-error --location --output "$tmp_path" "$MODEL_URL"; then
    rm -f "$tmp_path"
    printf '%s\n' 'エラー：モデルの取得に失敗しました。' >&2
    printf '%s\n' 'ネットワークの状態を確認し、もう一度 ./pre_day3/prepare.sh を実行してください。' >&2
    exit 1
  fi

  if ! matches_sha256 "$tmp_path"; then
    rm -f "$tmp_path"
    printf '%s\n' 'エラー：取得したモデルの内容が正しくありません（SHA-256が一致しません）。' >&2
    printf '%s\n' '途中で通信が切れた可能性があります。もう一度 ./pre_day3/prepare.sh を実行してください。' >&2
    exit 1
  fi

  mv "$tmp_path" "$model_path"
  printf '%s\n' 'モデルの取得と内容の確認が終わりました。'
}

# [7] AI用のDockerfileを /workspaces/llm-chat/Dockerfile に配置します。
#     中身はこのプログラムの中に持っています（配布ページのDockerfileと同じ内容です）。
place_llm_chat_dockerfile() {
  mkdir -p "$LLM_CHAT_DIR"
  cat > "$LLM_CHAT_DIR/Dockerfile" <<'DOCKERFILE'
# [1] 土台（base image）：llama.cpp のサーバーが入ったimage。
#     版（server-v0.5.0）を固定する。latestを使うと、作り直すたびに中身が変わりうる。
FROM ghcr.io/ggml-org/llama.cpp:server-v0.5.0

# [2] 設定（environment variable）：読み込むモデルの場所。
#     モデルのファイルはimageに入れず、docker run の -v（bind mount）で外から /models に渡す。
ENV LLAMA_ARG_MODEL=/models/gemma-4-E2B-it-UD-Q4_K_XL.gguf

# [3] 設定：受付の窓口（port）。containerの外からつなげるよう 0.0.0.0 の 8080 番で待つ。
ENV LLAMA_ARG_HOST=0.0.0.0
ENV LLAMA_ARG_PORT=8080

# [4] 設定：考える動作（reasoning）を切る。会話を覚えておける長さ（ctx-size）。
#     一度に答える長さの上限（n-predict）。
ENV LLAMA_ARG_REASONING=off
ENV LLAMA_ARG_CTX_SIZE=4096
ENV LLAMA_ARG_N_PREDICT=256

# [5] 設定：AIへの最初の指示（system prompt）。ここを書き換えて docker build をやり直すと、
#     AIの答え方が変わる。書き換えるのはこの行の "" の中の文だけでよい。
#     引用符の入れ子（外側は'、内側は"）を崩さないこと。
ENV LLAMA_ARG_UI_CONFIG='{"systemMessage":"あなたは親切なアシスタントです。短く答えてください。"}'
DOCKERFILE
  printf '%s\n' "$LLM_CHAT_DIR/Dockerfile を配置しました。"
}

# ここから実行順です（[2]〜[7]の関数を、この順で呼び出します）。
MODEL_DIR_PARENT=$(dirname "$MODEL_DIR")
check_docker
check_disk_space
pull_image
fetch_model
place_llm_chat_dockerfile

printf '%s\n' '準備ができました'
```

保存したら、ターミナルで次を実行します。

```sh
chmod +x pre_day3/prepare.sh
./pre_day3/prepare.sh
```

### 観察点

数分かかることがあります。終わると`準備ができました`と表示されます。これが完了の目印です。今回、`pre_day3/`はcommitしません（あとで`git status`に記録されていないファイルとして表示されたままで構いません）。

### うまくいかないとき

エラーの表示が出た場合は、その表示に書かれている次の行動（数十秒待つ、容量を空ける、もう一度実行するなど）に従ってください。

## できた状態

- `template.md`に「実行したマシン」の表示欄がある。
- `generate.sh`が、マシンの識別名を取得し、プレースホルダーを置き換える。
- Codespaceで生成した`report.md`の「実行したマシン」が`{{MACHINE_NAME}}`のままではない値になっていて、その値を控えてある。
- GitHub Actionsの生成stepが成功し、Bot commitで変更されたファイルが`report.md`だけである。
- GitHub側の`report.md`の「実行したマシン」が`{{MACHINE_NAME}}`のままではない値になっていて、その値を控えてある。
- 控えた二つの値を並べ、違っていることを確認し、「同じ処理を別のマシンで動かしたから」と自分の言葉で言える。
- Codespaceの`master`へ結果を取り込み、`git status`で記録されていない変更がない。
- `./pre_day3/prepare.sh`を実行し、`準備ができました`の表示を見た。

## 11. 次回への接続（目安1分）

次回は、今回記録した「実行したマシン」のしくみを、Dockerというしくみの中で動かします。今回のうちに10節の準備を済ませておいたので、次回はその状態からすぐに始められます。中身の詳しい説明は次回に行います。
