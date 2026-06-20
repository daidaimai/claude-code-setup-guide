# 【超簡単】Claude Code セットアップガイド（Windows版）
### 初心者でも30分以内を目指せる

> 🪟 Windows版ガイド ｜ ⏱️ 所要時間 約30分 ｜ 📋 コピペでOK
>
> Mac版はこちら → [claude-code-guide-mac.md](./claude-code-guide-mac.md)

ターミナルって聞くと身構えてしまいますよね。でも実は、手順通りに進めるだけで誰でも使い始められます。このページでは、アカウント準備からインストール、ログイン、最初のコマンドまでを順番に解説します。

---

## Claude Codeって何？

### 🤖 ターミナルで動くAIエージェント
Claude Codeは、ターミナル（コマンドを打つ画面）の中で動くAnthropic社のコーディングAIです。「このエラーを直して」「自己紹介ページを作って」のように、日本語の指示でファイルの作成・編集・実行までを行ってくれます。

### 🎓 プログラミング未経験でも大丈夫
コードを1行も書けなくても、やりたいことを日本語で伝えるだけでWebページや簡単なアプリを作ることができます。まずは「動かしてみる」ことを目標に進めましょう。

---

## セットアップ手順（Windows）

STEP1〜STEP3は始める前の準備、STEP4以降が実際のインストール作業です。順番に進めれば迷いません。

### STEP1. Claude Pro（またはAPIキー）を準備する

Claude Codeを使うには、次のいずれかが必要です。個人で使う場合は①のサブスクリプションがシンプルでおすすめです。

1. **Claude Pro／Maxなどの有料プラン**：[claude.ai](https://claude.ai) で契約
2. **Anthropic ConsoleのAPIキー**：[console.anthropic.com](https://console.anthropic.com) でクレジットを購入し発行

> パソコンの環境は **Windows 10（バージョン1809以降）またはWindows 11** であれば、このまま次のステップに進めます。

### STEP2. ターミナル（PowerShell）を開く

ターミナルとは、文字でパソコンに指示を出すための画面です。Windowsでは標準搭載の「PowerShell」を使います。

スタートメニューで「PowerShell」と検索して開きます。画面の先頭が `PS C:\Users\あなたの名前>` のように「PS」から始まっていれば、正しくPowerShellが開けています。

### STEP3. Node.jsがインストールされているか確認する（参考）

この後のSTEP4で案内するインストール方法はNode.js不要で動作します。ただしNode.jsは多くの開発ツールで使われる基本的な環境のため、参考として確認・インストール方法を紹介します。最短で進めたい方はこのSTEPを読み飛ばしてSTEP4へ進んでOKです。

まず、すでにNode.jsがインストールされているかどうかを確認してみましょう。

```powershell
node --version
```

> ✅ **バージョン番号（例：v20.11.0 など）が表示された場合** は、すでにNode.jsがインストールされている状態です。下記のインストール手順は不要ですので、そのままSTEP4へ進んでください。

一方、次のようなエラーが表示された場合は、Node.jsが **インストールされていない状態** です。

```text
node : 用語 'node' は、コマンドレット、関数、スクリプト ファイル、または操作可能なプログラムの名前と
して認識されません。名前が正しく記述されていることを確認し、パスが含まれている場合はそのパスが正しいこと
を確認してから、再試行してください。
発生場所 行:1 文字:1
+ node --version
+ ~~~~
    + CategoryInfo          : ObjectNotFound: (node:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```

インストールされていない場合は、以下の手順で導入しましょう。

`PowerShell（winget）`
```powershell
winget install OpenJS.NodeJS.LTS
```

インストール後、新しいPowerShellを開いて、もう一度 `node --version` を実行し、バージョン番号が表示されることを確認してください。

### STEP4. Claude Codeをインストールする

以下のコマンドをPowerShellにコピーして貼り付け、Enterキーを押します。インストーラーが自動で必要なものをセットアップしてくれます（Node.jsは不要です）。

`PowerShell`
```powershell
irm https://claude.ai/install.ps1 | iex
```

> **うまく実行できないときは、** 「irmが見つかりません」と出る場合、PowerShellではなくCMD（コマンドプロンプト）を開いている可能性があります。画面の先頭に「`PS`」がついていればPowerShellです。

**（参考）** STEP3でNode.jsをインストール済みの場合は、npm経由でも導入できます。

`PowerShell（npm・参考）`
```powershell
npm install -g @anthropic-ai/claude-code
```

#### ⚠️ よくあるエラー：スクリプトの実行が無効になっている

上記のnpmコマンドを実行すると、次のようなエラーが表示されることがあります。

```text
> npm install -g @anthropic-ai/claude-code
npm : このシステムではスクリプトの実行が無効になっているため、ファイル C:\Program Files\nodejs\npm.ps1 を読み込むことが
できません。詳細については、「about_Execution_Policies」(https://go.microsoft.com/fwlink/?LinkID=135170) を参照してくだ
さい。
発生場所 行:1 文字:1
+ npm install -g @anthropic-ai/claude-code
+ ~~~
    + CategoryInfo          : セキュリティ エラー: (: ) []、PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

**原因：** Windowsには、安全のためスクリプト（.ps1ファイル）の実行を制限する「実行ポリシー」という仕組みがあります。初期設定のままだと、npmが内部で使う `npm.ps1` というスクリプトの実行がブロックされてしまいます。

**対処法：**

1. 以下のコマンドを実行し、現在のユーザーに限定して実行を許可します（管理者権限は不要です）。

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

2. 確認のメッセージが表示されたら `Y` を入力してEnterを押し、もう一度 `npm install -g @anthropic-ai/claude-code` を実行します。
3. 社内ポリシーなどでこの変更ができない場合は、上記の**ネイティブインストーラー**に切り替えてください（このエラー自体を回避できます）。

#### ⚠️ よくあるエラー：npmでのインストールに失敗する

同じく `npm install -g @anthropic-ai/claude-code` では、次のようなエラーが出ることもあります。

```text
npm ERR! code EPERM
npm ERR! syscall rename
npm ERR! errno -4048
npm ERR! Error: EPERM: operation not permitted, rename
'C:\Users\...\AppData\Roaming\npm\node_modules\.@anthropic-ai-xxxx' ->
'C:\Users\...\AppData\Roaming\npm\node_modules\@anthropic-ai\claude-code'
```

**原因：** ウイルス対策ソフトや別のプロセスが該当フォルダを使用中だったり、PowerShellを管理者権限で開いていない場合に発生しやすいエラーです。

**対処法：**

1. PowerShellを閉じて、スタートメニューから**「管理者として実行」**で開き直してから再実行する
2. ウイルス対策ソフトを一時的に無効にして再実行する
3. それでも解決しない場合は、npmを使わずに上記の**ネイティブインストーラー**に切り替える（このエラー自体を回避できます）

### STEP5. インストールを確認する

新しいPowerShellを開き直して、以下のコマンドを実行します。バージョン番号が表示されればインストール成功です。

```powershell
claude --version
```

### STEP6. 作業フォルダを作って起動する

Claude Codeに作業させたいフォルダに移動してから起動します。下記は例として、ドキュメント内に「my-ClaudeCode-project」というフォルダを作って移動し、起動するコマンドです。

```powershell
mkdir $HOME\Documents\my-ClaudeCode-project
cd $HOME\Documents\my-ClaudeCode-project
claude
```

> Documentsフォルダ内で `claude` の起動やファイル保存をした際に書き込みエラーが出た場合は、Windows Defenderの「コントロールされたフォルダーアクセス」が原因の可能性があります。詳しくは[トラブルシューティング](#うまくいかないときは)を参照してください。

### STEP7. 初回ログイン（認証）

初めて `claude` を実行すると、自動でブラウザが開きログイン画面が表示されます。STEP1で決めたどちらかの方法でログインしてください。

1. Claude.aiアカウントでログイン（Pro／Maxプランの方）
2. Anthropic Consoleアカウントでログイン（APIキー利用の方）

> ブラウザが自動で開かない場合は、PowerShellに表示されたURLを**手動でコピーしてブラウザに貼り付け**てください。ログイン完了後、表示されたコードをPowerShellに貼り付ければ完了です。

🔧 うまくいかないときは、[トラブルシューティング](#うまくいかないときは)を参照してください。

---

## 最初に覚えておきたいコマンド

PowerShell上で `/`（スラッシュ）を入力すると、使えるコマンドの一覧が表示されます。

| コマンド | できること |
|---|---|
| `/help` | 使えるコマンドの一覧を表示する |
| `/init` | プロジェクトを解析し、設定ファイル（CLAUDE.md）の元を自動生成する |
| `/clear` | 会話の履歴をリセットする（作成済みファイルは残る） |
| `/model` | 使用するAIモデルを切り替える |
| `/config` | 権限・言語・テーマなどの設定画面を開く |
| `/exit` | Claude Codeを終了する |

## 知っておきたい設定ファイル

### 📄 CLAUDE.md
プロジェクトのルールや構成をあらかじめ書いておくファイルです。毎回の会話の最初に読み込まれるため、「テストはこのコマンドで実行する」などを書いておくと毎回説明する必要がなくなります。

### ⚙️ settings.json
権限設定（自動で実行してよい操作・確認が必要な操作）などをまとめて管理できるファイルです。`~/.claude/settings.json` に置くと全プロジェクト共通、プロジェクト内の `.claude/settings.json` に置くとそのプロジェクトだけに適用されます。

---

## 実践してみよう

セットアップが終わったら、まずは簡単なページやアプリの作成から試してみましょう。難易度は★の数で表しています。

### ★☆☆ 自己紹介ページ
名前や趣味を伝えて、HTMLの自己紹介ページを作ってもらう、最初の一歩におすすめの例です。

> Claude Codeへの指示文（コピペでOK）
```text
自己紹介ページを作ってください。名前は[ここに名前]、趣味は[ここに趣味]です。背景は明るめでさわやかなデザインにしてください。HTMLファイルとして保存してください。
```

### ★☆☆ TODOリスト
やることを追加・削除できる、シンプルなタスク管理アプリを作ってもらいましょう。

> Claude Codeへの指示文（コピペでOK）
```text
やることを追加・削除できるシンプルなTODOリストアプリを作ってください。見た目はすっきりとして使いやすいデザインにし、HTMLファイルとして保存してください。
```

### ★★☆ 電卓アプリ
四則演算ができる電卓をブラウザ上に作成。ボタンのデザインも一緒に相談してみましょう。

> Claude Codeへの指示文（コピペでOK）
```text
四則演算ができる電卓アプリを作ってください。ボタンは大きく押しやすいデザインにして、HTMLファイルとして保存してください。
```

---

## うまくいかないときは

**Q. 「claude」コマンドが見つからないと表示される**
インストール先のフォルダが **PATH**（コマンドの検索場所）に登録されていない可能性があります。一度PowerShellを完全に閉じて新しく開き直してから、もう一度 `claude --version` を実行してみてください。改善しない場合は公式ドキュメントのインストールページを再確認しましょう。

**Q. npmのインストールで「Permission denied」「EPERM」と出る**
STEP4で紹介したエラーです。**PowerShellを管理者として開き直す**か、npmを使わずにSTEP4のネイティブインストーラーに切り替えてください。

**Q. Windowsで「irmは認識されません」と出る**
CMD（コマンドプロンプト）を開いている可能性があります。スタートメニューで「PowerShell」を検索して開き直し、同じコマンドを実行してください。プロンプトの先頭が「`PS`」になっていればPowerShellです。

**Q. ログイン画面が何度も表示される**
パソコンの時刻設定がずれていると認証に失敗することがあります。日時が正しいか確認し、ターミナル内で `/login` と入力して再度ログインしてみてください。

**Q. インストール中に403エラーが出る**
ネットワーク環境やファイアウォールの制限が原因の場合があります。少し時間を置いて再実行するか、社内ネットワークの場合は通信制限についてIT担当者に確認してください。

**Q. 「自己紹介ページを作って」と頼んだら、ファイルが保存できない・書き込みエラーになる**

画面には `ENOENT: no such file or directory` や `Could not find file ...` のような「ファイルが見つからない」系のエラーが表示されることがありますが、実際の原因は別にあるケースが多いです。Windows Defenderの**「コントロールされたフォルダーアクセス」**というランサムウェア対策機能が、Documentsなどの保護対象フォルダへの書き込みを、許可されていないアプリからブロックしていることがあります。

**確認方法：** 以下のコマンドを実行し、表示される値が `1`（有効）になっているか確認します。

```powershell
Get-MpPreference | Select-Object EnableControlledFolderAccess
```

**対処法①（かんたん・おすすめ）：** 作業フォルダを、Documents・Desktop・Picturesなど保護対象フォルダの**外**（例：`C:\Users\あなたの名前\projects` など）に作り直し、そこでClaude Codeを起動します。設定変更が不要で、最も手軽な方法です。

**対処法②：** 使用しているアプリをDefenderの許可リストに追加します（管理者権限のあるPowerShellで実行してください）。

```powershell
Add-MpPreference -ControlledFolderAccessAllowedApplications "C:\WINDOWS\System32\WindowsPowerShell\v1.0\powershell.exe"
```

npm経由でClaude Codeをインストールした場合は、`claude.exe` のパス（例：`C:\Users\あなたの名前\AppData\Roaming\npm\node_modules\@anthropic-ai\claude-code\bin\claude.exe`）も同様に追加リストへ加えると確実です。

---

## まずは動かしてみましょう

難しく考えず、まずは「自己紹介ページを作って」のような簡単なお願いから始めてみてください。失敗してもやり直せるので、気軽に触ってみるのが一番の近道です。

---

Claude Codeは更新が早いツールです。コマンドや手順が変わっている場合は、[公式ドキュメント](https://code.claude.com/docs)もあわせてご確認ください。
Macをご利用の方は [Mac版ガイド](./claude-code-guide-mac.md) をご覧ください。

作成者：ClaudeCode初心者 daidaim
