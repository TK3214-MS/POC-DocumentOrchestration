# Power Platform ソリューションとデプロイ スクリプト

このディレクトリには、Power Automate コンポーネントと Dataverse コンポーネントを含む Power Platform ソリューションが含まれており、このリポジトリに含まれるサンプル AI ドキュメント データ抽出関数によって出力された JSON BLOB を取得し、それを Dataverse に書き込んで他のシステムとさらに統合する機能を示しています。Power Platform 環境へのソリューションのデプロイを自動化するためのスクリプトと構成ファイルも含まれています。

## コンテンツ

- 'DocArchiveRequest-xxxx.zip': Power Platform ソリューション (以下を含む):
    - JSON 抽出されたデータの Dataverse への統合例をサポートするデータ モデル
    - プライマリ統合フローの例
        - `Process JSON DeocInfo into DV` クラウドフロー: 新しい BLOB が作成されたことをトリガーとして、JSON BLOB を `Archive Request` テーブルのレコードに解析し、作成された `Archive Request` に関連する各キーと値のペアのレコードを `Extracted Entity` テーブルに解析します
    - セカンダリ統合フローの例
        - `Process JSON into DV key-value records` クラウドフロー: 新しい BLOB が作成され、JSON BLOB を "アーカイブ要求" テーブルのレコードに解析し、子フローを呼び出して抽出された詳細を記録します
        - `CHILD - Process Protocol Doc` クラウドフロー: 抽出された詳細を、作成された`Archive Request`に関連する`Protocol Document`テーブルにマップするために呼び出されます
        - `CHILD - Process Report Doc` クラウドフロー: 抽出された詳細を、作成された`Archive Request `に関連する` Report Document`テーブルにマップするために呼び出されます
- `export-solution.ps1`: 'settings.json' ファイルで定義されている環境からソリューション ファイルをエクスポートするために使用される PowerShell スクリプト
- `deploy-solution.ps1`: 指定したソリューションを 'settings.json' ファイルで定義されている環境にデプロイする PowerShell スクリプト
- `settings.json`: このファイルには、デプロイメント・スクリプトの構成設定が含まれています。このファイルは`settings_sample_json `ファイルに基づいて作成し、環境に適した値で更新する必要があります。

## 使用方法

1. 'settings_sample_json' ファイルを使用して、同じディレクトリに 'settings.json' ファイルを作成します。
2. `settings.json `ファイルを環境に適した値で更新します。
3. Dataverse と Azure Blob Storage のターゲット環境で接続を作成します。 それぞれのURLから接続IDを記録します。
4. `DocArchiveRequest_deploySettings.json `ファイルを正しい接続 ID で更新します。 詳細については[こちら](https://learn.microsoft.com/en-us/power-platform/alm/conn-ref-env-variables-build-tools)から確認が可能です。
5. 'deploy-solution.ps1' スクリプトを実行します。

## Note

スクリプトで使用される 'pac' コマンドは、Power Apps CLI の一部です。Power Apps CLI をインストールし、システムの PATH に追加する必要があります。 Power Apps CLI の詳細については、[こちら](https://learn.microsoft.com/en-us/power-platform/developer/howto/install-vs-code-extension)を参照してください.

