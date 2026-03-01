# 運賃: 概要 {: #fares-introduction}

運賃（Fares v2 とも呼ばれます）は、[GTFS Schedule の機能](../../../../../getting-started/features/overview)の1つであり、乗客向けの運賃情報を標準化することで、各交通システムおよびその接続における運賃構造や条件に基づいて、チケットや料金オプションを利用者が把握できるようにします。

Fares (v2) の主な機能には、チケット商品(Fare Products)、チケットメディア(Fare Media)、乗客カテゴリ(Rider Categories)、ルートベース運賃(Route-Based Fares)、ゾーンベース運賃(Zone-Based Fares)、時間ベース運賃(Time-Based Fares)、および運賃乗り継ぎ(Fare Transfers)が含まれます。

GTFS Fares (v2) は、コミュニティ主導のプロジェクトとして進化を続けており、作業名 [GTFS-Fares v2](../../../../../community/extensions/fares-v2) のもとで開発されています。実験的な機能のモデリングに関するガイダンスについては、[完全な提案文書](https://share.mobilitydata.org/gtfs-fares-v2)を参照してください。GTFS は後方互換性を維持していますが、将来的な拡張性および進行中の仕様開発との整合性を確保するために、従来の運賃仕様 (Legacy Fares v1) ではなく Fares v2 の採用が推奨されます。

このガイドでは、Fares (v2) を使用して一般的な運賃構造をどのようにモデリングするかを示します。まず、Fares (v2) でサポートされている運賃機能と、それぞれに対応するファイルを紹介します。その後、後のセクションでモデリングを行う実際の例を提示します。

## 運賃機能とそのファイル {: #fares-features-and-their-files}

GTFS Fares (v2) では、一般的な運賃体系に見られるさまざまな運賃機能をモデル化することができます。

| 機能 | 説明 | 関連ファイル |
| :---- | :---- | :---- |
| [**Fare Media**](../../../../../getting-started/features/fares/#fare-media)<br>([例](../fare-media)を参照) | チケット商品を保持および／または認証するために使用できる対応メディア（例：現金、非接触型、物理カードなど）です。 | `fare_media.txt`, `fare_products.txt` |
| [**Fare Products**](../../../../../getting-started/features/fares/#fare-products)<br>([例](../fare-products)を参照) | 交通事業者がサービス利用のために提供する運賃の種類（例：片道運賃、月額パス、乗り継ぎ料金など）です。 | `fare_products.txt` |
| [**Rider Categories**](../../../../../getting-started/features/fares/#rider-categories)<br>([例](../rider-categories)を参照) | 年齢、身分、またはニーズに基づいて特定の運賃率の対象となる、シニア、学生、子供、大人などの異なる乗客グループを表します。経路検索アプリケーションはこの情報を使用して、利用可能なカテゴリを表示し、フィードを提供する事業者によって設定されたデフォルト運賃を表示することができます。 | `rider_categories.txt`, `fare_products.txt` |
| [**Route-Based Fares**](../../../../../getting-started/features/fares/#route-based-fares)<br>([例](../route-based-fares)を参照) | 特定のルートグループ（ルートネットワークとも呼ばれます）に異なる運賃を割り当てます。均一料金のルートベース運賃は最も基本的な運賃です。 | `fare_products.txt`, `fare_leg_rules.txt`, `networks.txt`, `route_networks.txt` |
| [**Zone-Based Fares**](../../../../../getting-started/features/fares/#zone-based-fares)<br>([例](../zone-based-fares)を参照) | 特定のゾーンから別のゾーンへ移動する際に特定の運賃が適用されるゾーンベースのシステムを表します。ゾーンは停留所等(stop)の集合として定義することができます。 | `fare_products.txt`, `fare_leg_rules.txt`, `areas.txt`, `stop_areas.txt` |
| [**Time-Based Fares**](../../../../../getting-started/features/fares/#time-based-fares)<br>([例](../time-based-fares)を参照) | 特定の時間帯や曜日（例：ピーク時・オフピーク時・週末運賃など）に応じて運賃を割り当てます。 | `fare_products.txt`, `fare_leg_rules.txt`, `timeframes.txt` |
| [**Fare Transfers**](../../../../../getting-started/features/fares/#fare-transfers)<br>([例](../fare-transfers)を参照) | 乗車区間(leg)（または個々の移動区間）間の乗り継ぎ時に適用されるルールです。これにより、無料または割引乗り継ぎを含む旅程(journey)全体の総運賃を計算することができます。 | `fare_products.txt`, `fare_leg_rules.txt`, `fare_transfer_rules.txt` |

これらの機能および各機能に含まれるファイルの詳細については、[Fares features ページ](../../../../../getting-started/features/fares)を参照してください。

### ファイル間の関係 {: #relations-between-files}

以下のエンティティ・リレーションシップ図（ERD）は、Fares (v2) のファイルがどのように連携して動作するかを示しています。

<iframe width="768" height="432" src="https://miro.com/app/embed/uXjVIHu_Wws=/?pres=1&frameId=3458764623400726374&embedId=111450801270" frameborder="0" scrolling="no" allow="fullscreen; clipboard-read; clipboard-write" allowfullscreen></iframe>

## 例の提示 {: #presenting-the-example}

Fares (v2) を使用して運賃をどのようにモデリングするかを説明するために、このガイドでは実際の交通事業者を例として使用します。内容を簡潔に保つために、その事業者の運賃体系の特定の側面に焦点を当てます。このガイドでは片道チケット商品の例を含みますが、同じモデリング手法は日次パスや月次パスなどの他のチケット商品にも適用されます。

例となる交通システム：

* TransLink（バンクーバー）

### Translink（バンクーバー） {: #translink-vancouver}


Translink はバンクーバーの公共交通事業者です。2024年11月時点で、バンクーバーではゾーン制運賃システムを採用しています（詳細は[こちら](https://www.translink.ca/transit-fares/pricing-and-fare-zones)をご覧ください）。本ガイドで使用される要素の一部を以下の表にまとめます。  

| 区分 | 詳細 |
| ----- | ----- |
| **チケットメディア (Fare Media)** | - **現金 (Cash)** <br>- **Compass Ticket**（紙のチケット）<br>- **Compass Card**（物理的な交通カード）<br>- **非接触型決済カード**および**モバイルウォレット** |
| **バス運賃 (Bus Fare)** | 一律 **3.20カナダドル**。 |
| **1日乗車券 (Daily pass)** | - バスおよび全ゾーンで **11.50カナダドル**。<br>- **Compass Card または Compass Ticket のみ有効**。 |
| **SkyTrain および SeaBus の運賃** | 通過ゾーン数に応じた**ゾーン制運賃**。<br>- **ゾーン1**：バンクーバー<br>- **ゾーン2**：バーナビー、リッチモンド、ノースバンクーバー<br>- **ゾーン3**：サレー、コキットラム |
| **[Airport AddFare](https://www.translink.ca/transit-fares/transferring-and-addfare)** | - **Sea Island** 発の SkyTrain 便には **5.00カナダドル**の追加料金がかかります。<br>- Sea Island 内の3駅間の移動は**無料**です。 |
| **週末および夜間運賃 (Weekend and Evening Fares)** | - 午後6時30分から午前3時まで、または週末は、すべての運賃が**ゾーン1**（バス一律 **3.20カナダドル**）になります。<br>- ただし、Sea Island 発の旅程は追加運賃が適用されます。そのため、午後6時30分から午前3時までおよび週末は、運賃は 5.00 + 3.20 = **8.20カナダドル** となります。 |
| **[乗り継ぎ (Transfers)](https://www.translink.ca/transit-fares/transferring-and-addfare)** | - **90分間の乗り継ぎ**は**非現金チケットメディア**でのみ利用可能です。<br>- この期間中、**バス間の乗り継ぎは無料**です。<br>- **同一運賃ゾーン内**であれば、運賃に含まれています。<br>- **より高い運賃ゾーンへの乗り継ぎ**には**追加運賃 (AddFare)** が適用されます。<br>- 新しい高額運賃と以前の低額運賃との差額が乗客に請求されます。つまり、両運賃の差額が最初の価格に**加算**されます。 |
| **乗客区分 (Rider Categories)** | **大人 (Adult)** および **割引 (Concession)** 区分。<br>- 大人がデフォルトの区分です。<br>- 障がい者、高齢者、青少年の乗客は割引区分に分類されます。 |

West Coast Express もゾーン制を採用しています（別のゾーン体系）。簡略化のため、本ガイドでは取り上げませんが、そのゾーン制運賃も SkyTrain と同様の方法でモデル化することができます。

本ガイドの TransLink セクションでは、特に断りのない限り大人区分のみを扱いますが、[乗客区分](../rider-categories)セクションでは割引区分も含まれます。

## 用語集 {: #glossary}


以下の表は、このガイドおよび運賃に関する議論で一般的に使用される用語の概念を示しています。

| 概念 | 説明 |
| :---- | :---- |
| 乗車区間(leg) | 特定のサービスまたはルート上で、通常は2つの停留所等(stop)間を乗り換えなしで移動する、旅程の単一の連続した区間です。 |
| 部分旅程(sub-journey) | 旅程の部分集合を構成する2つ以上の乗車区間(leg)です。 |
| 旅程(journey) | 出発地から目的地までの全体の移動であり、その間のすべての乗車区間(leg)および乗り換えを含みます。 |

<img class="center" width="1200" height="100%" src="../../../../../../assets/glossary-journey-abcd.svg"> 

| 概念 | 説明 |
| :---- | :---- |
| 乗車区間グループ(Leg Group) | `fare_leg_rules.txt` ファイルの文脈で定義される、特定の共通属性または運賃条件を共有する1つ以上の乗車区間(leg)の集合です。 |
| 運賃区間ルール(Fare leg rule) | フィルター条件に従い、単一の乗車区間(leg)または有効運賃区間(effective fare leg)での移動を完了するための運賃適用条件です。 |
| ネットワーク(Network) | 運賃区間(leg)の照合を目的として、類似の運賃構造を持つルートのグループです。ルートのネットワークは次のように構成されることがあります：事業者のすべてのルート、同一モード（例：地下鉄、バス）のルート、または類似の目的（例：地方、都市）のルート。 |
| 運賃ゾーン(Fare zones) | ゾーン間またはゾーン内の移動に基づいて料金を決定するために使用されるエリアです。運賃ゾーンは GTFS 内でエリア（停留所等(stop)の集合）として定義することができます。 |
| 時間枠(Timeframe) | 特定の運賃が適用される定義済みの期間です（例：平日の午前6時から午前9時、ラッシュアワー、週末など）。 |
| 時間枠グループ(Timeframe group) | 運賃ルールまたは料金設定において同様に扱われる時間枠の集合です（例：すべてのオフピーク時間）。 |
| 運賃乗り換えルール(Fare transfer rule) | フィルター条件に従い、乗車区間(leg)間の乗り換え費用に関する乗り換え適用条件です。 |
| 有効運賃区間(Effective Fare Leg) | 運賃計算の目的で、`fare_leg_rules.txt` 内のルールと照合する際に単一の乗車区間(leg)として扱うべき、2つ以上の乗車区間(leg)からなる部分旅程(sub-journey)です。 |
| フィルター条件(Filter conditions) | 運賃区間ルール、チケット商品、または運賃乗り換えルールの利用可能な費用を決定する、移動に関する制約または変数です。 |
