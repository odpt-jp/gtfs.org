# ルートベース運賃 {: #route-based-fares}

*主なファイル: fare_leg_rules.txt, networks.txt, route_networks.txt, routes.txt*  
*例: [Translink (バンクーバー)](../intro/#translink-vancouver)*

!!! info "リマインダー"

    ルートベース運賃は、利用するルートによって異なる運賃を設定します。チケット商品(Fare Product)は、事業者がサービスを利用するために提供する運賃の種類です。詳細については、イントロダクションページの[機能セクション](../intro/#fares-features-and-their-files)を参照してください。

!!! Note

    このセクションでは、同じチケット商品に対して異なるチケットメディアを表示します。後のセクションでは、ガイドを簡略化するために、*非接触型(contactless)* チケットメディアを持つチケット商品のみを表示します。

## チケット商品の作成 {: #create-fare-products}

ルートベース運賃(Route-Based Fares)は、定額運賃を提供するチケット商品(fare product)によって表現されます。ルートベースのチケット商品は、`fare_products.txt` に次のように作成します。

1. **fare_product_id** 列に、チケット商品を識別する一意のIDを入力します。  
2. **fare_product_name** 列に、乗客向けのチケット商品の名称を入力します（例：Bus Flat Fare、Bus Flat Fare Monthly）。  
3. **amount** および **currency** 列に、運賃の金額とその通貨を入力します（[通貨コード](https://en.wikipedia.org/wiki/ISO_4217#Active_codes)）。  
4. **fare_media_id** 列に、このチケット商品を保存および利用できるチケットメディアを入力します。  
    * これは、`fare_media.txt` 内の **fare_media_id** を参照する外部キーです（[チケットメディア](../fare-media)）。  
    * 同じチケット商品に対して、複数のチケットメディアを関連付けることができ、異なる価格設定も可能です。  
    * **fare_media_id** が空の場合、そのチケットメディアは不明であることを意味します。

チケット商品に関する詳細は、[ドキュメントを参照してください](../../../reference/#fare_productstxt)。

この例では、`bus_flat_fare` というチケット商品が Translink バスの定額運賃を表しています。異なる `fare_media_id` 値を持つ3つのレコードがあるため、このチケット商品は現金、コンタクトレスカード、または Compass Card で利用することができます。Compass Card で支払う場合の価格は、他のチケットメディアよりも低く設定されています。

[**fare_products.txt**](../../../reference/#fare_productstxt)

| fare_product_id | fare_product_name | amount | currency | fare_media_id |
| :---- | :---- | :---- | :---- | :---- |
| bus_flat_fare | Bus Flat Fare | 3.20 | CAD | contactless |
| bus_flat_fare | Bus Flat Fare | 3.20 | CAD | cash |
| bus_flat_fare | Bus Flat Fare | 2.60 | CAD | compass_card |
| … | … | … | … | … |

## ルートをグループ化するネットワークを作成する {: #create-networks-that-group-the-routes}

ルートベース運賃(Route-Based Fares)の場合、各ルートのグループごとに異なる運賃が設定されます。これらのグループはネットワーク(networks)とも呼ばれます。事業者内のすべてのルートが同一の運賃である場合、それらを1つのネットワークにまとめることができます。

ネットワークは次のように `networks.txt` で作成します。

1. **network_id** 列にネットワークを識別する一意のIDを入力します。  
2. **network_name** 列にネットワークの名称を入力します（例: Translink Buses、TTC Subway、STM All Routes）。

[Translink](../intro/#translink-vancouver) の場合、バスは定額運賃であるため、独自のグループに分ける必要があります。これは、ゾーンをまたぐ数に応じて運賃が変わる SkyTrain や Seabus とは異なります（[ゾーンベース運賃(Zone-Based Fares)](../zone-based-fares) のセクションを参照してください）。

この例では、Translink のバスを表すために translink_bus というネットワークが作成されています。

[**networks.txt**](../../../reference/#networkstxt)

| network_id | network_name |
| :---- | :---- |
| translink_bus | Translink Buses |

## ルートをネットワークに関連付ける {: #associate-routes-to-networks}

ネットワークを作成した後は、そのネットワークに含まれるルートを関連付ける必要があります。ルートは、次のように `route_networks.txt` 内でネットワークに関連付けられます。

1. **route_id** 列に、`routes.txt` に記載されているルートの ID を入力します。  
2. **network_id** 列に、対応するネットワークの `networks.txt` における ID を入力します。

ネットワークの詳細については、[ドキュメントを参照してください](../../../reference/#networkstxt)。

この例では、各バスのルートが `translink_bus` ネットワークに関連付けられています。`route_id` は、`routes.txt` 内のバスの `route_id` を参照しています。

[**route_networks.txt**](../../../reference/#route-networkstxt)

| route_id | network_id |
| :---- | :---- |
| 10232 | translink_bus |
| 11201 | translink_bus |
| … | … |

## 運賃区間ルール(fare leg rules)の作成 {: #create-fare-leg-rules}


!!! info "リマインダー"

    **乗車区間(leg)**: 特定のサービスまたはルート上で、2つの停留所(stop)間を乗り換えなしで移動する、旅程(journey)の連続した1区間です。

    **乗車区間グループ(Leg Group)**: `fare_leg_rules.txt` ファイルの文脈で定義される、特定の共通属性または運賃条件を共有する1つ以上の乗車区間(leg)の集合です。

乗車区間(leg)の運賃は、運賃区間ルール(fare leg rule)を使用して、乗車区間をチケット商品(fare product)に対応付けることで決定されます。ルートベース運賃(Route-Based Fares)の場合、運賃区間ルールは、`networks.txt` で作成されたルートのネットワークを、`fare_products.txt` で作成されたチケット商品に関連付けます。

ルートベースの運賃区間ルールは、次のように `fare_leg_rules.txt` に作成します。

1. **leg_group_id** 列に、乗車区間グループを識別する一意のIDを入力します。  
2. **network_id** 列に、乗車区間が含まれるルートに関連付けられたネットワークのIDを入力します。  
    * これは、`networks.txt` の **network_id** を参照する外部キー(Foreign Key)です。  
3. **fare_product_id** 列に、乗車区間の運賃を決定するチケット商品のIDを入力します。  
    * これは、`fare_products.txt` の **fare_product_id** を参照する外部キー(Foreign Key)です。

運賃区間ルールの詳細については、[ドキュメントを参照してください](../../../reference/#fare_leg_rulestxt)。

[Translink](../intro/#translink-vancouver) の場合、バスの乗車区間(leg)は、停留所Aから停留所Bまで、1本のTranslinkバスに乗車し、乗り換えを行わない区間を指します。別のバス、交通モード、または事業者に乗り換えると、新しい乗車区間(leg)が開始されます。

この例では、`fare_leg_rules.txt` 内の運賃区間ルールが、`translink_bus` ネットワークを `bus_flat_fare` 商品に関連付け、ネットワーク内のすべての乗車区間(leg)が適切に料金設定されるようにしています。

[**fare_leg_rules.txt**](../../../reference/#fare_leg_rulestxt)

| leg_group_id | network_id | fare_product_id |
| :---- | :---- | :---- |
| flat_fare_leg | translink_bus | bus_flat_fare |
