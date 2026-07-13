---
lab:
  title: ラボ 1 - Dynamics 365 Sales を構成する
  description: Contoso Coffee のための基礎的な構成を設定します。これには営業チーム ユーザーの作成と、営業担当地域、自動付番、Copilot 要約の設定が含まれます。
  duration: 50 minutes
  level: 200
  islab: true
---

# ラボ 1 - Dynamics 365 Sales を構成する

Contoso Coffee は新しい Dynamics 365 Sales 環境を作成したばかりであり、現時点では何もない状態です。 営業担当者がリードを開く、あるいは案件をクローズできるようにするには、システムに Contoso の実際の業務運営方法が反映されている必要があります。具体的には 4 つの地域チーム、一貫したレコード付番、AI を活用した要約 (営業担当者が個々のレコードを開くときに時間を節約するため) です。

このラボでは、これらの基礎的な事項を構成します。 Contoso Coffee の営業チーム用のユーザー アカウントを作成してセキュリティ ロールを割り当て、Contoso Coffee の 4 つの営業担当地域を設定し、自動付番プレフィックスを Contoso の命名規則に合わせて定義し、リードと営業案件を営業担当者のニーズに合わせて要約するように Copilot を構成します。

このラボの所要時間は約 **50** 分です。

## 開始する前に

自分の Dynamics 365 Sales 環境にサインイン済みであり**営業ハブ** アプリが開いている必要があります。 ラボ 00 をまだ終えていない場合は、先に完了してください。

このラボでは 2 つの管理ポータルも使用します。 始める前に、次のそれぞれを別のブラウザー タブで開いてください。

- **Microsoft 365 管理センター** (`https://admin.microsoft.com`): ユーザーの作成とライセンスの割り当てに使用
- **Power Platform 管理センター** (`https://admin.powerplatform.microsoft.com`): 環境の設定とデータ管理に使用

この両方に、ラボ 00 で使用したのと同じ資格情報でサインインしてください。

## タスク 1: Contoso Coffee の営業チーム ユーザーを作成する

担当地域を構成する前に、Contoso Coffee の地域営業マネージャーと営業担当者のユーザー アカウントを用意する必要があります。 5 人のユーザーを Microsoft 365 管理センターで作成し、それぞれに Dynamics 365 Sales のライセンスを割り当ててから、Dynamics 365 Sales でのセキュリティ ロールを割り当てます。

1. 新しいブラウザー タブを開いて (まだ開いていない場合)、`https://admin.microsoft.com` に移動します。

1. 左側のナビゲーション バーで **[ユーザー]** を選択し、**[アクティブなユーザー]** を選択します。

1. コマンド バーの **[ユーザーの追加]** を選択します。

1. **[基本設定]** ページで、次のとおりに詳細情報を入力します。
   - **名:** `Andy`
   - **姓:** `Kim`
   - **表示名**: `Andy Kim`
   - **ユーザー名**: `andykim`

1. 次に示すオプションは既定値のままにしておきます。
   - **パスワードを自動生成する**: オン。 新しいユーザーの一時的なパスワードが自動的に生成されます。
   - **初回サインイン時にこのユーザーにパスワードの変更を要求する**: オン。 ユーザーは初回サインイン時に自分でパスワードを設定する必要があります。

    > **注**: 本番環境では、パスワード自動作成をオフにしてパスワードを自分で設定することもできます。このようにしない場合は、生成されたパスワードを安全な方法で新しいユーザーに伝える必要があります。

1. **[製品ライセンスの割り当て]** ページで、自分の国または地域を選択してから、**[Dynamics 365 Sales]** を選択してライセンスを割り当てます。

1. **[次へ]** を選択し、もう一度 **[次へ]** を選択してオプションの設定をスキップし、**[追加の完了]** を選択し、次に **[閉じる]** を選択します。

1. ステップ 3 から 7 までを繰り返して、次に示す 4 人のユーザーを作成し、それぞれに **Dynamics 365 Sales** のライセンスを割り当てます。

   | 名 | 姓 | ユーザー名 |
   |------------|-----------|----------|
   | Maria | Reyes | mariareyes |
   | David | Osei | davidosei |
   | Rachel | Sato | rachelsato |
   | ヨルダン | Park | jordanpark |

1. 5 人のユーザー全員が **[アクティブなユーザー]** のリストに表示されていることを確認します。

## タスク 2: Dynamics 365 Sales でのセキュリティ ロールを割り当てる

次に、これらの新しいユーザーに付与される特権とアクセス レベルを定義するセキュリティ ロールを割り当てます。 これは **Power Platform 管理センター**で行います。ここは管理者が環境レベルの設定を管理するための場所ですが、この中にユーザー アクセスも含まれています。 ここでは、あらかじめ用意されているセキュリティ ロールを 2 つ使用します。**営業担当者**を Jordan Park に、**営業マネージャー**を 4 人の地域マネージャーに使用します。

1. 新しいブラウザー タブを開いて、`https://admin.powerplatform.microsoft.com` に移動します。

1. 左側のナビゲーションで、**[管理]** を選んでから、**[環境]** を選びます。

1. 自分の Dynamics 365 **Sales 試用**環境をリストから選択します。

1. 環境の詳細ページの上部にある **[設定]** を選択します。

1. **[ユーザーとアクセス許可]** セクションを展開して **[ユーザー]** を選択します。

1. コマンド バーの **[+ ユーザーの追加]** を選択します。

1. 検索ボックスに「`Andy Kim`」と入力し、このユーザーを結果から選択します。

    > **注**: 作成されたばかりのユーザーが Microsoft 365 から Dynamics 365 に同期されるのに数分かかる場合があります。 ユーザーがまだ表示されない場合は、少し待ってからページを最新の情報に更新してください。

1. **[追加]** を選択します。

1. **[セキュリティ ロールの管理]** ペインで、**[営業マネージャー]** というセキュリティ ロールを選択します。

1. **[保存]** を選択してから、**[保存]** をもう一度選択して確定します。

1. ステップ 2 から 5 までを繰り返して **Maria Reyes**、**David Osei**、**Rachel Sato** を追加し、それぞれに**営業マネージャー**というロールを割り当てます。

1. ステップ 2 から 5 までを繰り返して **Jordan Park** を追加し、**営業担当者**というロールを割り当てます。

これで Contoso の一連のユーザーがそろい、ラボの中で割り当てができる状態になりました。

## タスク 3: 営業担当地域を作成する

Contoso の営業チームは 4 つの地域に分かれています。 ここでは、Dynamics 365 Sales でそれぞれを表す担当地域を作成します。

1. "営業ハブ" の左側ナビゲーションの下部にある **[アプリの設定]** を選択します。

1. **[アプリの設定]** エリアで、**[営業管理]** セクションまで下にスクロールして **[営業担当地域]** を選択します。

1. 最初の担当地域を作成するために、コマンド バーの **[+ 新規]** を選択します。

1. **[担当地域名]** フィールドに「`Northeast`」と入力します。

1. **[マネージャー]** フィールドで、**Andy Kim** を検索して選択します。

1. コマンド バーの **[保存して閉じる]** を選択します。

1. ステップ 3 から 7 までを繰り返して、さらに 3 つの担当地域を作成し、次に示すとおりにマネージャーを割り当てます。

   | 担当地域 | 管理者 |
   |-----------|---------|
   | `Southeast` | Maria Reyes |
   | `Central` | David Osei |
   | `West` | Rachel Sato |

1. 4 つの担当地域、つまり **Northeast**、**Southeast**、**Central**、**West** のすべてがリストに含まれていることを確認します。

    これで Contoso の地域構造がシステムに反映された状態になりました。 営業担当者とマネージャーは、パイプライン ビュー、レポート、予測を担当地域のフィルターで絞り込むことができるようになります。

## タスク 4: 自動付番プレフィックスを構成する

Contoso の要望として、すべてのレコードに一貫性があり認識しやすい識別子を持たせるというものがあります。これは、見積もりと注文をシステム横断で簡単に追跡できるようにするためです。 ここでは、2 種類のレコード ("見積もり" と "注文") の自動付番のプレフィックスを更新します。

1. 新しいブラウザー タブを開いて、`https://admin.powerplatform.microsoft.com` に移動します。

1. 左側のナビゲーションで、**[管理]** を選択し、次に **[環境]** を選択します。

1. 自分の Dynamics 365 **Sales 試用**環境をリストから選択します。

1. 環境の詳細ページの上部にある **[設定]** を選択します。

1. **[データ管理]** セクションを展開し、**[自動付番]** を選択します。

1. 自動付番の設定画面で、**[見積もり]** の行を見つけます。

1. **[プレフィックス]** の値を `CCQ` (Contoso Coffee Quote) に変更します。

1. **[サフィックスの長さ]** が `6` に設定されていることを確認します。 

1. **[注文]** の行を見つけて、**[プレフィックス]** を `CCO` (Contoso Coffee Order) に変更します。

1. **保存**を選択して、変更を適用します。

    > **注**: 自動付番の変更が適用されるのは新しいレコードのみです。 既存のレコードの番号は元のままになります。

<!--
## Task 5: Set up duplicate detection for leads

Contoso generates leads from multiple sources, like trade shows, web forms, email campaigns. Without duplicate detection, the same contact could end up as multiple lead records, wasting a rep's time and creating conflicting data.

In this task, you'll create a rule that flags two leads as potential duplicates when they share the same first five characters of the **Company Name** field. This catches common variations — like "Northwind Traders" and "Northwind Trading Co." — before they create conflicting records in the pipeline.

1. Return to the **Power Platform admin center** tab.

1. In the environment settings, expand the **Data management** section and select **Duplicate detection rules**.

1. Select **+New** on the command bar.

1. Fill in the rule details:
   - **Name**: `Duplicate leads by company name`
   - **Description**: `Flags leads as potential duplicates when they share the same first five characters of the Company Name field.`
   - **Base record type**: **Lead**
   - **Matching record type**: **Lead**

1. In the criteria section, select **+ Add** to add a matching field.

1. From the **Field** dropdown, select **Company Name**.

    > **Note**: You may notice an **Account** field on lead records as well. Account is a lookup that links to an existing Account record, but most inbound leads arrive before any Account exists, so that field is usually empty. **Company Name** is the plain-text field where the company name is captured at the point of entry, making it the right choice for catching duplicates from web forms, trade show scans, and CSV imports.

1. Set **Criteria** to **Same first characters**, and set the **Number of characters** to `5`.

1. Select **Save and close**.

1. On the rules list, find your new rule and select **Publish** to activate it. Select **OK** to confirm.

    > **Note**: Duplicate detection rules only run when a user saves a record or during data imports. They don't retroactively flag existing records.
-->

## タスク 6: Copilot レコード要約を構成する

Dynamics 365 Sales での時間短縮に役立つ機能として、営業担当者がレコードを開いたときにその内容を Copilot で要約できるというものがあります。 営業担当者は、すべてのフィールドとメモに目を通す代わりに、AI で生成された簡潔なまとめを利用することができます。 ここでは、リードと営業案件に関してこの要約に何を入れるかを構成します。

1. ブラウザーの**営業ハブ**のタブに戻ります。

1. 左側のナビゲーションの下部にある **[アプリの設定]** を選択します。

1. **[全般設定]** の下の **[Copilot]** を選択します。

1. **設定**タブを選択します。

1. **[すべてのアプリ]** の下のトグルが **[オン]** に設定されていることを確認します。 **[カスタム]** に設定されている場合は、**[オン]** に変更します。

    > **注**: 既定値は環境によって異なる場合があります。 **[オン]** に設定すると、Copilot の機能をその環境内のすべてのアプリで利用できるようになります。

1. **[保存]** を選択します。

1. **[リード]** タブを選択します。

1. リードの要約に含まれる既定のフィールドを確認します。 **[トピック]**、**[リード ソース]**、**[評価]** などのフィールドがあるはずです。

1. **[+ フィールドの追加]** を選択して、次に示すフィールドを要約に追加します。
   - **会社名**
   - **売上高**
   - **従業員数**

    これらのフィールドがあれば、担当者は個々のタブを開かなくてもリードの規模と見通しをすばやく判断できるようになります。

1. **[保存]** を選択します。

1. **[営業案件]** タブを選択します。

1. 既定の営業案件要約フィールドを確認します。 **[顧客ニーズ]**、**[提案されたソリューション]**、**[顧客の困りごと]**、**[現在の状況]**、**[元の潜在顧客]**、**[説明]** などのフィールドがあるはずです。

1. **[+ フィールドの追加]** を選択して **[予算金額]** を追加します。

1. **[営業案件の要約をウィジェットとしてフォームに表示する]** チェックボックスを選択します。これで、要約がインライン ウィジェットとして営業案件フォームに表示されるようになります。

1. **[保存]** を選択します。

    > **注**: Copilot 要約に関する変更は、新しいセッションに対して即座に有効になります。 営業担当者がサインイン済みの場合は、ブラウザーを最新の情報に更新することが必要になる可能性があります。

## タスク 7: 構成を確認する

次に進む前に、Copilot 要約が期待どおりに動作することを確認します。 ここでは、テスト リードを作成して Copilot 要約をチェックします。

1. 左側のナビゲーションの下部で **[アプリの設定]** を選択し、**[営業]** を選択してメインの "営業ハブ" エリアに戻ります。

1. 左側のナビゲーションの **[リード]** を選択します。

1. コマンド バーの **[+ 新規]** を選択します。

1. 最初のテスト リードの詳細情報を、次のとおりに入力します。
   - **トピック**: `Test Lead - Verify Config`
   - **First Name:** `Alex`
   - **Last Name:** `Rivera`
   - **会社**: `Northwind Trading`

1. リード レコードの **[詳細]** タブを選択します。

1. 次の値を入力します。
   - **年間収益**: `500000`
   - **従業員数**: `50`

1. コマンド バーで、 **保存** を選択します。

1. 保存済みリード レコードの、Alex という名前の上にあるレコード要約を選択します。

1. レコード要約に**会社名**、**年間収益**、**従業員数** (タスク 6 で追加したフィールド) が含まれていることを確認します。

## まとめ

あなたは 5 人のユーザー アカウント、4 つの担当地域、自動付番プレフィックスを作成し、リードと営業機会に関する Copilot 要約を構成しました。 作成したものを確認するには、**Microsoft 365 管理センター** > **[アクティブなユーザー]** に移動すると Contoso Coffee チームを見ることができ、**[アプリの設定]** > **[営業担当地域]** に移動すると 4 つの地域を見ることができます。

これでラボ 01 が完了しました。 Contoso Coffee の環境には、チームのユーザーが作成されて名前と適切なセキュリティ ロールが指定されており、地域構造、一貫したレコード付番、および Copilot が構成されて、営業担当者の仕事を速く進めることができるようになりました。
