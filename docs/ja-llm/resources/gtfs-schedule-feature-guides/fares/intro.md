# 運賃: はじめに {: #fares-introduction}


運賃（Fares v2 とも呼ばれます）は、[GTFS Schedule の機能](../../../../../getting-started/features/overview)であり、各交通システムおよびその接続の運賃体系と条件に基づいて、乗客がチケット発券および料金の選択肢を見つけられるようにする、乗客向け運賃情報を標準化します。

主な Fares (v2) の機能には、チケット商品、チケットメディア、乗客カテゴリ、ルート・路線系統(route)ベースの運賃、ゾーンベースの運賃、時間ベースの運賃、および運賃乗換があります。

GTFS Fares (v2) は、[GTFS-Fares v2](../../../../../community/extensions/fares-v2) という作業名のもとで開発されている、コミュニティ主導のプロジェクトとして進化を続けています。実験的機能のモデリングに関するガイダンスについては、[完全な提案文書](https://share.mobilitydata.org/gtfs-fares-v2)を参照してください。GTFS は後方互換性を維持していますが、将来の拡張性を確保し、継続中の仕様開発との整合性を保つため、Legacy Fares (v1) ではなく Fares v2 を採用することが推奨されます。

このガイドでは、Fares (v2) を使用して一般的な運賃体系をモデリングする方法を示します。まず、Fares (v2) でサポートされる運賃機能と、それぞれに使用される対応ファイルを紹介します。次に、後続のセクションでモデリングする実際の事例を提示します。

## 運賃機能とそれらのファイル {: #fares-features-and-their-files}


GTFS Fares (v2) は、一般的な運賃体系に見られるさまざまな運賃機能をモデル化できます。

| 機能 | 説明 | 関連ファイル |
| :---- | :---- | :---- |
| [**チケットメディア**](../../../../../getting-started/features/fares/#fare-media)<br>([例](../fare-media)を参照) | チケット商品を保持および／または検証するために使用できる、対応するメディア（例: 現金、コンタクトレス、物理カードなど）です。 | `fare_media.txt`, `fare_products.txt` |
| [**チケット商品**](../../../../../getting-started/features/fares/#fare-products)<br>([例](../fare-products)を参照) | サービスを利用するために交通事業者が提供する運賃の種類（すなわち、片道運賃、月間パス、乗換料金など）です。 | `fare_products.txt` |
| [**乗客カテゴリ**](../../../../../getting-started/features/fares/#rider-categories)<br>([例](../rider-categories)を参照) | 年齢、資格、またはニーズに基づいて特定の運賃率の対象となる、高齢者、学生、子ども、成人などの異なる乗客グループを表します。経路検索アプリケーションは、この情報を使用して利用可能なカテゴリを表示し、フィードを提供する事業者が設定したデフォルト運賃を表示できます。 | `rider_categories.txt`, `fare_products.txt` |
| [**ルート・路線系統(route)ベースの運賃**](../../../../../getting-started/features/fares/#route-based-fares)<br>([例](../route-based-fares)を参照) | ルートネットワークとも呼ばれる、特定のルート・路線系統(route)グループに異なる運賃を割り当てます。定額のルート・路線系統(route)ベース運賃は、最も基本的な運賃です。 | `fare_products.txt`, `fare_leg_rules.txt`, `networks.txt`, `route_networks.txt` |
| [**ゾーンベースの運賃**](../../../../../getting-started/features/fares/#zone-based-fares)<br>([例](../zone-based-fares)を参照) | ある特定のゾーンから別のゾーンへ移動する際に特定の運賃が適用される、ゾーンベースのシステムを表します。ゾーンは停留所等(stop)の集合として定義できます。 | `fare_products.txt`, `fare_leg_rules.txt`, `areas.txt`, `stop_areas.txt` |
| [**時間帯ベースの運賃**](../../../../../getting-started/features/fares/#time-based-fares)<br>([例](../time-based-fares)を参照) | ピーク時・オフピーク時の運賃および／または週末運賃など、特定の時間帯または曜日に対して運賃を割り当てます。 | `fare_products.txt`, `fare_leg_rules.txt`, `timeframes.txt` |
| [**運賃乗換**](../../../../../getting-started/features/fares/#fare-transfers)<br>([例](../fare-transfers)を参照) | 乗車区間(leg)間（または個々の移動区間間）で乗り換える際に適用されるルールです。無料または割引の乗換を含む旅程(journey)全体の費用を計算できます。 | `fare_products.txt`, `fare_leg_rules.txt`, `fare_transfer_rules.txt` |


これらの機能および各機能に含まれるファイルの詳細については、[運賃機能ページ](../../../../../getting-started/features/fares)を参照してください。

### ファイル間の関係 {: #relations-between-files}


以下のエンティティ・リレーションシップ図（ERD）は、Fares (v2) ファイルがどのように連携するかを示しています。

<iframe width="768" height="432" src="https://miro.com/app/embed/uXjVIHu_Wws=/?pres=1&frameId=3458764623400726374&embedId=111450801270" frameborder="0" scrolling="no" allow="fullscreen; clipboard-read; clipboard-write" allowfullscreen></iframe>

## 例の提示 {: #presenting-the-example}


Fares (v2) を使用して運賃をモデル化する方法を説明するために、このガイドでは実在の交通事業者を例として使用します。簡潔にするため、このガイドでは当該事業者の運賃体系の特定の側面に焦点を当てます。このガイドには片道のチケット商品の例が含まれていますが、同じモデリングプロセスは日次および月次パスなどの他のチケット商品にも適用されます。

例となる交通システム:

* TransLink (Vancouver)

### Translink（バンクーバー） {: #translink-vancouver}


Translinkは、バンクーバーの公共交通事業者です。2024年11月時点で、バンクーバーではゾーンベースの運賃制度を採用しています（詳細は[こちら](https://www.translink.ca/transit-fares/pricing-and-fare-zones)をご覧ください）。このガイドで使用する要素の一部を、以下の表にまとめます。  

| カテゴリ | 詳細 |
| ----- | ----- |
| **チケットメディア** | - **現金** <br>- **Compass Ticket**（紙の乗車券）<br>- **Compass Card**（物理的な交通カード）<br>- **非接触型決済カード**および**モバイルウォレット** |
| **バス運賃** | **CAD 3.20**の均一運賃です。 |
| **1日パス** | - バスおよびすべてのゾーンで**CAD 11.50**です。<br>- **Compass CardまたはCompass Ticketでのみ**有効です。 |
| **SkyTrainおよびSeaBusの運賃** | 通過するゾーン数に応じた**ゾーンベースの運賃**です。<br>- **ゾーン1**: バンクーバー。<br>- **ゾーン2**: バーナビー、リッチモンド、ノースバンクーバー。<br>- **ゾーン3**: サリー、コキットラム。 |
| **[空港AddFare](https://www.translink.ca/transit-fares/transferring-and-addfare)** | - **Sea Island**を出発するSkyTrainの便には、追加で**CAD 5.00**がかかります。<br>- Sea Islandの3つの駅間の移動は**無料**です。 |
| **週末および夜間の運賃** | - 午後6時30分から午前3時まで、および週末は、すべての運賃が**ゾーン1**です（バスの均一運賃は**CAD 3.20**です）。<br>- Sea Islandを出発する旅程の運賃には、引き続き追加運賃がかかります。そのため、午後6時30分から午前3時まで、および週末は、その運賃は5.00 + 3.20 = CAD 8.20となります。 |
| **[乗り換え](https://www.translink.ca/transit-fares/transferring-and-addfare)** | - **90分間の乗り換え**は、**現金以外のチケットメディア**でのみ利用できます。<br>- この期間中、**バス間は無料**です。<br>- 運賃で対応する**同一運賃ゾーン内では無料**です。<br>- **より高い運賃ゾーンへ乗り換える場合**、追加運賃（AddFareと呼ばれます）が適用されます。<br>- 新しい高い運賃と以前の低い運賃との差額が、乗客に請求されます。基本的に、両方の運賃の差額が初期価格に加算されます。 |
| **乗客カテゴリ** | **Adult**および**Concession**カテゴリです。<br>- Adultがデフォルトのカテゴリです。<br>- 障害のある乗客、高齢者、および若年の乗客は、Concessionカテゴリで表されます。 |

West Coast Expressもゾーンベースです（別のゾーンセットを使用します）。簡潔にするため、本ガイドには含めません。ただし、そのゾーンベースの運賃は、SkyTrainと同様の方法でモデル化できます。

このガイドでは、Concessionカテゴリも含む[乗客カテゴリ](../rider-categories)セクションを除き、TransLinkのセクションではAdultカテゴリのみを扱います。

## 用語集 {: #glossary}


以下の表は、このガイドおよび運賃に関する議論で一般的に使用される用語の概念を示しています。

| 概念 | 説明 |
| :---- | :---- |
| 乗車区間(leg) | 特定のサービスまたはルート・路線系統(route)で移動する旅程(journey)の、通常は2つの停留所等(stop)間における、乗換を伴わない単一の連続した区間です。 |
| 部分旅程(sub-journey) | 旅程(journey)の一部を構成する2つ以上の乗車区間(leg)です。  |
| 旅程(journey) | 出発地から目的地までの全体的な移動であり、その間のすべての乗車区間(leg)および乗換を含みます。  |

<img class="center" width="1200" height="100%" src="../../../../../../assets/glossary-journey-abcd.svg"> 

| 概念 | 説明 |
| :---- | :---- |
| 乗車区間グループ | `fare_leg_rules.txt` ファイルの文脈で定義される、特定の共通属性または運賃条件を共有する1つ以上の乗車区間(leg)の集合です。 |
| 運賃乗車区間ルール | フィルター条件に従って、単一の乗車区間(leg)または有効運賃区間(effective fare leg)での移動を完了するための運賃適格性基準です。 |
| ネットワーク | 運賃乗車区間の照合を目的として、類似した運賃体系を持つルート・路線系統(route)のグループです。ルート・路線系統(route)のネットワークには、次のものがあります。事業者のすべてのルート・路線系統(route)。同じ交通手段のルート・路線系統(route)（例: 地下鉄、バス）。類似した目的のルート・路線系統(route)（例: 地方、都市部）。 |
| 運賃ゾーン | ゾーン間またはゾーン内の移動に基づいて価格を決定するために使用されるエリアです。運賃ゾーンは、GTFSではエリア（停留所等(stop)の集合）として定義できます。 |
| 時間帯 | 特定の運賃が適用される定義済みの期間です（例: 平日の午前6時から午前9時、ラッシュアワー、週末）。 |
| 時間帯グループ | 運賃ルールまたは価格設定において同様に扱われる時間帯の集合です（例: すべてのオフピーク時間帯）。 |
| 運賃乗換ルール | フィルター条件に従った、乗車区間(leg)間の乗換費用に対する乗換適格性です。 |
| 有効運賃区間(effective fare leg) | 運賃計算を目的として、`fare_leg_rules.txt` のルールとの照合において単一の乗車区間(leg)として扱うべき、2つ以上の乗車区間(leg)からなる部分旅程(sub-journey)です。 |
| フィルター条件 | 運賃乗車区間ルール、チケット商品、または運賃乗換ルールで利用可能な費用を決定する、移動に関する制約または変数です。 |
