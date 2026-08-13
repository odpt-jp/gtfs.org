# ルート・路線系統(route)ベース運賃 {: #route-based-fares}


*主なファイル: fare_leg_rules.txt, networks.txt, route_networks.txt, routes.txt*  
*例: [Translink (Vancouver)](../intro/#translink-vancouver)*

!!! info "リマインダー"

    ルート・路線系統(route)ベース運賃では、利用するルート・路線系統(route)に基づいて異なる運賃を割り当てます。チケット商品は、事業者がサービスへのアクセスのために提供する運賃の種類です。詳細については、Introduction ページの[機能のセクション](../intro/#fares-features-and-their-files)を再確認してください。

!!! Note

    このセクションでは、同じチケット商品に対する異なる種類のチケットメディアを表示します。後続のセクションでは、ガイドを簡潔にするため、*contactless* チケットメディアを持つチケット商品のみを表示します。

## チケット商品を作成する {: #create-fare-products}


ルート・路線系統(route)ベースの運賃は、定額運賃を提供するチケット商品によって表現されます。ルート・路線系統ベースのチケット商品は、次のように`fare_products.txt`で作成します。

1. **fare_product_id**列に、チケット商品を識別する一意のIDを入力します。  
2. **fare_product_name**列に、乗客向けのチケット商品の名称（例: Bus Flat Fare、Bus Flat Fare Monthly）を入力します。  
3. **amount**列および**currency**列に、運賃の料金とその通貨（[通貨コード](https://en.wikipedia.org/wiki/ISO_4217#Active_codes)）を入力します。  
4. **fare_media_id**列に、このチケット商品を保存および使用できるチケットメディアを入力します。  
    * これは、`fare_media.txt`の**fare_media_id**を参照する外部キーです（[チケットメディア](../fare-media)）。  
    * 同じチケット商品に複数のチケットメディアを関連付けることができ、価格が異なる可能性があります。  
    * **fare_media_id**が空の場合、チケットメディアは不明であることを意味します。

チケット商品の詳細については、[ドキュメントを参照してください](../../../reference/#fare_productstxt)。

この例では、`bus_flat_fare`というチケット商品がTranslink Busesの定額運賃を表します。異なる`fare_media_id`値を持つエントリが3つあるため、このチケット商品は現金、非接触型カード、またはCompass Cardで有効化できます。Compass Cardで支払う場合の価格は、他のチケットメディアの選択肢よりも低くなっています。

[**fare_products.txt**](../../../reference/#fare_productstxt)

| fare_product_id | fare_product_name | amount | currency | fare_media_id |
| :---- | :---- | :---- | :---- | :---- |
| bus_flat_fare | Bus Flat Fare | 3.20 | CAD | contactless |
| bus_flat_fare | Bus Flat Fare | 3.20 | CAD | cash |
| bus_flat_fare | Bus Flat Fare | 2.60 | CAD | compass_card |
| … | … | … | … | … |

## ルート・路線系統(route)をグループ化するネットワークを作成する {: #create-networks-that-group-the-routes}


Route-Based Fares では、ルート・路線系統(route)の各グループに異なる運賃が設定されます。これらのグループはネットワークとも呼ばれます。ある事業者のすべてのルート・路線系統(route)が同じ運賃である場合、それらを1つのネットワークにグループ化することができます。

ネットワークは、次のように `networks.txt` で作成します。

1. **network_id** 列に、ネットワークを識別する一意の ID を入力します。  
2. **network_name** 列に、ネットワークの名称を入力します（例: Translink Buses、TTC Subway、STM All Routes）。

[Translink](../intro/#translink-vancouver) の場合、バスは均一運賃であるため、独自のグループに分ける必要があります。これは、運賃が通過するゾーン数に依存する SkyTrain および Seabus とは対照的です（[Zone-Based Fares](../zone-based-fares) セクションを再確認してください）。

この例では、Translink Buses を表すために、translink_bus というネットワークを作成します。

[**networks.txt**](../../../reference/#networkstxt)

| network_id | network_name |
| :---- | :---- |
| translink_bus | Translink Buses |

## ルート・路線系統(route)をネットワークに関連付ける {: #associate-routes-to-networks}


ネットワークを作成した後、それに含まれるルート・路線系統(route)に関連付ける必要があります。ルート・路線系統(route)は、次のように`route_networks.txt`でネットワークに関連付けられます。

1. **route_id**列に、`routes.txt`のルート・路線系統(route)のIDを入力します。  
2. **network_id**列に、`networks.txt`の対応するネットワークのIDを入力します。

ネットワークの詳細については、[ドキュメントを参照してください](../../../reference/#networkstxt)。

この例では、各バスルート・路線系統(route)が`translink_bus`ネットワークに関連付けられています。`route_id`は、`routes.txt`内のバスの`route_id`を参照します。

[**route_networks.txt**](../../../reference/#route_networkstxt)


| route_id | network_id |
| :---- | :---- |
| 10232 | translink_bus |
| 11201 | translink_bus |
| … | … |

## 運賃区間ルールを作成する {: #create-fare-leg-rules}


!!! info "注意"

    **乗車区間(leg)**: 特定のサービスまたはルート・路線系統(route)で利用する旅程(journey)の単一の連続した区間であり、通常は2つの停留所等(stop)間で、乗り換えはありません。

    **乗車区間グループ(Leg Group)**: `fare_leg_rules.txt` ファイルのコンテキストで定義される、特定の共通属性または運賃条件を共有する1つ以上の乗車区間(leg)の集合です。

乗車区間(leg)の運賃は、運賃区間ルールを使用して乗車区間(leg)をチケット商品と照合することで決定されます。ルートベース運賃では、運賃区間ルールは、ルート・路線系統(route)のネットワーク（`networks.txt` で作成）をチケット商品（`fare_products.txt` で作成）に関連付けます。 

ルートベースの運賃区間ルールは、次のように `fare_leg_rules.txt` で作成されます。

1. **leg_group_id** 列に、乗車区間(leg)のグループを識別する一意の ID を入力します。  
2. **network_id** 列に、乗車区間(leg)が対象とするルート・路線系統(route)に関連付けられたネットワークの ID を入力します。  
    * これは、`networks.txt` の **network_id** を参照する外部キーです。  
3. **fare_product_id** 列に、乗車区間(leg)の料金を決定するチケット商品の ID を入力します。  
    * これは、`fare_products.txt` の **fare_product_id** を参照する外部キーです。

運賃区間ルールの詳細については、[ドキュメントを参照してください](../../../reference/#fare_leg_rulestxt)。

[Translink](../intro/#translink-vancouver) では、バスの乗車区間(leg)は、停留所等(stop) A から停留所等(stop) B まで、乗り換えなしで単一の Translink バスを利用することから構成されます。別のバス、交通手段、または事業者に乗り換えると、新しい乗車区間(leg)が開始されます。

この例では、`fare_leg_rules.txt` の運賃区間ルールが `translink_bus` ネットワークを `bus_flat_fare` 商品にリンクし、このネットワーク内のすべての乗車区間(leg)がそれに応じて価格設定されることを保証します。

[**fare_leg_rules.txt**](../../../reference/#fare_leg_rulestxt)

| leg_group_id | network_id | fare_product_id |
| :---- | :---- | :---- |
| flat_fare_leg | translink_bus | bus_flat_fare |
