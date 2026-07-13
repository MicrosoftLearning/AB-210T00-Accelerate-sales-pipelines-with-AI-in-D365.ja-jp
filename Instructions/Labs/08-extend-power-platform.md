---
lab:
  title: ラボ 8 - Power Platform を使って Dynamics 365 Sales を拡張する
  description: Power Automate と Copilot Studio を使って、Dynamics 365 Sales を自動化ワークフローと AI 搭載の営業コーチング エージェントで拡張します。
  duration: 35 minutes
  level: 200
  islab: true
---

# ラボ 8 - Power Platform を使って Dynamics 365 Sales を拡張する

Contoso Coffee のセールス オペレーション マネージャーである Marcus Webb は、毎週チームに何時間もかかる手動タスクの実行リストを挙げています。

- 特定の収益のしきい値を超えて価値の高い営業案件が作成されるたびに、Marcus はフォローアップに優先順位を付けることができるように、営業の VP に手動でメールを送信します。
- 取引が受注としてクローズした場合、財務チームが請求に使用する SharePoint リストを更新する必要があります。
- 販売者は、特定の製品に対する異議の処理方法や標準的なピッチについて、多くの場合、Marcus に問い合わせ、彼は同じ質問に何度も答えます。

これらの問題はいずれも開発者を必要としません。 すべて Power Platform を必要とします。

このラボでは、営業案件が提案ステージに達したときに自動的にトリガーされる Power Automate フローを構築し、営業ハブ内で販売者のよくある質問に直接答える Copilot Studio エージェントを設定します。

このラボの完了には約 **35** 分かかります。

## 開始する前に

営業ハブにサインインしている必要があります。 また、**Power Automate** (`https://make.powerautomate.com`) と **Copilot Studio** (`https://copilotstudio.microsoft.com`) にアクセスする必要があります。 3 つすべてに同じラボ資格情報を使用します。

## タスク 1: 営業案件から Power Automate フローを作成する

推定収益が 50,000 ドルを超える新しい営業案件が作成されるたびに、Marcus に電子メールによる通知を送信するフローを作成します。 これにより、手動チェックインが置き換えられます。

1. 新しいブラウザー タブを開いて、`https://make.powerautomate.com` に移動します。

1. ラボの資格情報でサインインします。

1. 左側のナビゲーションで **[作成]** を選択します。

1. **[自動クラウド フロー]** を選択します。

1. **[フロー名]** フィールドに、「`Notify Marcus - High-Value New Opportunity`」と入力します。

1. **[フローのトリガーを選択してください]** 検索ボックスに、「`When a row is added`」と入力し、**[行が追加、変更、または削除された場合]** (Microsoft Dataverse) を選択します。

1. **［作成］** を選択します

1. トリガー カードで、次のように構成します。
   - **種類の変更**: **追加**
   - **テーブル名**: **営業案件**
   - **スコープ**: **組織**

1. トリガー カードの下にある **[+]** を選択して、条件を追加します。

1. **[条件]** (コントロール) を検索して選択します。

1. 条件で次のようにします。
   - **値 (左側)**: 動的コンテンツを使用するために稲妻アイコンを選択し、**[推定収益]** を選びます。
   - **演算子**: **次の値より大きい**
   - **値 (右側)**: `50000`

1. **[True]** 分岐 (条件が true の場合) で、**[+]** を選択してアクションを追加します。

1. **Office 365 Outlook** コネクタから **[メールの送信 (V2)]** を検索して選択します。 (サインインを求められる場合があります。**[サインイン]** を選択し、画面の指示に従って資格情報でサインインします)。

1. メールの詳細を入力します。
    - **宛先**: ラボ ユーザーのメール アドレスを入力します (通知を受け取る Marcus をシミュレートするため)。 これは MOD から始まる可能性があるため、MOD の入力を開始し、表示されたら選択できます。
    - **件名**: `High-value opportunity created: [Topic]`
    
      営業案件名を動的に挿入するには、フィールドを選択し、動的コンテンツを追加します。**トピック**。

    - **本文は次のようになります。** 
      ```
      Hi Marcus,

      A new high-value opportunity has been created and may need your attention.

      Opportunity: [Topic]
      Estimated Revenue: [Est. Revenue]
      Estimated Close Date: [Est. Close Date]

      Please review and assign this opportunity as appropriate.

      This notification was sent automatically by Dynamics 365 Sales.
      ```
      
      角かっこで囲まれたプレースホルダーを、対応する動的コンテンツ フィールドに置き換えます。

1. Power Automate の右上隅にある **[保存]** を選択します。

## タスク 2: フローをテストする

1. Power Automate デザイナーで、右上隅にある **[テスト]** を選択します。

1. **[フローのテスト]** ペインで、**[手動]** を選択してから、**[テスト]** を選択します。

   Power Automate では、フローがリッスン モードになり、トリガー アクションを実行するように指示するメッセージが表示されます。

1. [営業ハブ] ブラウザー タブに切り替え、左側のナビゲーションで **[営業案件]** を選び、**[+ 新規]** を選択します。

1. 次のフィールドに入力します。
   - **トピック**: `Test - High-Value Flow Trigger`
   - **推定収益**: `75000` (このフィールドはヘッダーにあります)
   - **終了予定日**: 任意の将来の日付 (これもヘッダーにあります)

1. 営業案件レコードで **[保存]** を選択します。

1. [Power Automate] タブに戻ります。フローで新しいレコードが検出され、自動的に実行が開始されます。 各ステップに注意してください。正常に完了すると、各カードに緑色のチェックマークが表示されます。

1. 実行が完了したら、**[完了]** を選択し、[Power Automate] タブを閉じます。

1. 新しいブラウザー タブを開き、`https://outlook.office.com` に移動して、件名 `High-value opportunity created:` の後に営業案件名が記載されたメールを受信したことを確認します。

    > **注**: いずれかのステップで赤い X が表示されている場合は、それを選択してエラーの詳細を展開します。 よくある問題は Outlook 接続がないことです。アクション カードで **[サインイン]** を選択して再接続してください。

## タスク 3: 営業コーチング用に Copilot Studio エージェントを構成する

Marcus は毎週、次のような販売者からの同じ質問に答えます。"20 か所のホテルのリース ピッチは何ですか?" "価格の異議はどのように処理すればよいですか?" "サービス契約下では何がカバーされますか?"

販売者が Dynamics 365 Sales 内で直接これらの質問を行うことができる軽量の Copilot Studio エージェントを作成します。

1. 新しいブラウザー タブを開いて、`https://copilotstudio.microsoft.com` に移動します。

1. ラボの資格情報でサインインします。

1. **[ホーム]** ページで、**[最初からビルドを開始]** の下にある **[エージェント]** を選択します。

1. エージェントに `Contoso Sales Coach` という名前を付け、**[作成]** を選択します。

1. **[指示]** セクションで、「`You are a sales coaching assistant for Contoso Coffee. You help sellers with product pitches, objection handling, competitive comparisons, and pricing guidance. Answer questions about commercial espresso machines, coffee makers, equipment leases, and service contracts. Be concise, friendly, and specific. Always tailor your advice to the customer's industry or location count if that information is provided.`」と入力します。

1. **[保存]** を選択します。

1. 上部ナビゲーションの **[トピック]** タブを選択して、特定の対応シナリオを追加します。

1. **[+ トピックの追加]** > **[空白から]** を選択します。

1. トピックに「`Espresso machine lease pitch`」という名前を付けます。

1. **[このトピックで行う内容の記述]** フィールドに、「`Use this topic when a seller asks for help pitching an equipment lease, asks for espresso machine talking points, or wants to know what to say when discussing leasing options with a customer.`」と入力します。

1. **[+]** を選択し、**[メッセージの送信]** を選びます。 会話デザイナーに表示される **[メッセージ]** ノードで、次のように入力します。

    ```
    Here's Contoso Coffee's standard equipment lease pitch:

    Lead with the outcome: "We help businesses like yours serve better coffee without a large upfront investment — a 36-month lease means no capital expenditure and predictable monthly costs."

    Address the pain point: Ask "What does your current equipment situation look like? Most clients we work with are dealing with aging machines and expensive reactive repairs."

    Handle the risk concern: "Every lease includes our monthly service contract — two preventive maintenance visits a year and unlimited on-call support during business hours. If it breaks, we fix it."

    Close with proof: "We recently leased 12 Espresso Machine - Pro units to a restaurant group in the Southwest. Their beverage revenue went up 18% in the first quarter after installation — happy to share their feedback."
    ```

1. **[保存]** を選択します。

1. **[戻る]** を選択して、トピックのリストに戻ります。

1. トピックをもう 1 つ追加します。 **[+ トピックの追加]** > **[空白から]** を選択します。

1. これに `Pricing objection` という名前を付けます。

1. **[このトピックで行う内容の記述]** フィールドに、「`Use this topic when a seller asks how to respond to a pricing objection, price pushback, or a customer who says the product is too expensive.`」と入力します。

1. メッセージ ノードで、次のように入力します。

    ```
    When a prospect pushes back on price, try this:

    1. Acknowledge: "I hear you — budget is always a consideration."
    2. Reframe to value: "Let me make sure we're comparing the right things. What are you currently paying for equipment repairs and downtime? Most clients find reactive maintenance costs more than a lease with a service contract included."
    3. Highlight the total cost of the alternative: "A single emergency equipment repair can run $500–$1,200. With our lease, that's covered. You're paying for certainty, not just hardware."
    4. Offer a smaller entry point: "We could also start with leasing machines for your highest-volume locations this quarter and expand to the remaining locations in Q3 — that keeps the initial commitment manageable."

    Do not discount on the first pushback. Escalate to Marcus if the prospect requests more than 10% off.
    ```

1. **[保存]** を選択します。

1. テスト パネルで、「`how do I pitch a lease`」と入力し、定義したエスプレッソ マシンのリース ピッチでエージェントが応答することを確認します。

1. 「`they said it's too expensive`」と入力し、価格の異議応答が表示されることを確認します。

## タスク 4: エージェントを発行してテストする

1. 上部のナビゲーション バーで **[設定]** を選択します。

1. **[セキュリティ]** を選択してから、**[認証]** を選びます。

1. **[認証なし]** を選んでから、**[保存]** を選択し、もう一度 **[保存]** を選択します。

   > **注**: この設定を使用すると、デモ Web サイト チャネルは、エンドユーザーのサインインを必要とせずに試用版環境で動作できます。 運用環境のデプロイでは、適切な認証プロバイダーを構成します。

1. [設定] パネルを閉じ、上部のナビゲーション バーで **[発行]** を選択します。

1. **[発行]** を選択して、現在のバージョンのエージェントを発行します。

1. 発行の確認メッセージが表示されるまで待ちます。

    > **注**: Dynamics 365 Sales の営業ハブ サイドバーにカスタム Copilot Studio エージェントを直接埋め込むには、このラボの範囲外の環境レベルの管理者構成が必要です。 

**お疲れさまでした。** Contoso Coffee の 9 つの要件をすべて完了しました。 空の Dynamics 365 環境から、次のことを行いました。

- 担当地域、番号付け、および Copilot の概要の構成
- 食品サービスのトレード ショーから潜在顧客をインポートして見込みありと評価
- AI 搭載の潜在顧客リサーチの設定 (オプションの Premium 機能)
- 一貫した価格とボリューム割引を備えた一元的な製品カタログの構築
- 作成から受注クローズまでの複数の場所の機器の営業案件の管理
- パイプライン リスクを監視し、アウトリーチを自動化するための AI エージェントのデプロイ (オプションの Premium 機能)
- Power Automate ワークフローと Copilot Studio のコーチ エージェントを使用するプラットフォームの拡張

Priya は、Contoso と同じように動作する販売システムを手に入れました。あなたはどこからでも同じように構築するための実践的な経験を得ました。
