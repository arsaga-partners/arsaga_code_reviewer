# arsaga_code_reviewer

## 使用手順

 - 環境変数の設定
 - コマンドの実行

### 必要な環境変数

 - OPENAI_API_KEY: OpenAIのAPIキー
 - OPENAI_ORG_KEY: OpenAIのOrganization ID
 - AZURE_OPENAI_API_KEY: Azure OpenAIのAPIキー(Option)
 - AZURE_OPENAI_API: Azure OpenAIのAPIパス(Option)
 - TRANSLATION_API_KEY: DeepL ProプランのAPIトークン

### 仕様

 - (初期設定では)devおよびdevelopブランチへのプルリクを開いた際に、GPTへのプルリクレビューとその翻訳をしてくれるGithubActionsです
 - Azureへの設定はオプションですが、プルリクのdiffが大きい場合はトークン数の大きいGPTモデルに自動で切り替わってエラーになるので設定する方がベターです

### 初期設定

環境変数をGithubに設定後、下記のコマンドを右のアイコンでコピーしてプロジェクトのルートディレクトリで実行する

```bash
# .github/workflows ディレクトリがなければ作成
mkdir -p ./.github/workflows/

# YAMLファイルの内容をファイルに書き込む
echo "name: Call Reusable GPT Review Workflow

on:
  pull_request:
    branches:
      - dev
      - develop

jobs:
  call_reusable_workflow:
    # 末尾の[main]をコミットハッシュに変更すればCIの実行内容を特定の時点のものに固定できる
    uses: arsaga-partners/arsaga_code_reviewer/.github/workflows/gpt_reviewer.yml@main
    secrets:
      OPENAI_API_KEY: \${{ secrets.OPENAI_API_KEY }}
      OPENAI_ORG_KEY: \${{ secrets.OPENAI_ORG_KEY }}
      AZURE_OPENAI_API: \${{ secrets.AZURE_OPENAI_API }}
      AZURE_OPENAI_API_KEY: \${{ secrets.AZURE_OPENAI_API_KEY }}
      TRANSLATION_API_KEY: \${{ secrets.TRANSLATION_API_KEY }}
    with:
      PATCH_PR: \${{ github.event.pull_request.number }}
      PATCH_REPO: \${{ github.repository }}" > ./.github/workflows/call_gpt_reviewer.yml
```
