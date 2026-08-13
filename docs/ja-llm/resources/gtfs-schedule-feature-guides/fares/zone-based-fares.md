# ゾーンベース運賃 {: #zone-based-fares}


*主なファイル: fare_leg_rules.txt、areas.txt、stop_areas.txt*  
*例: [Translink（Vancouver）](../intro/#translink-vancouver)*

!!! info "注意"

    ゾーンベース運賃は、ある特定のゾーンから別のゾーンへ移動する際に特定の運賃が適用される、ゾーンベースの運賃システムを表現するために使用されます。ゾーンは、エリアまたは停留所等(stop)のグループによって定義されます。詳細については、Introduction ページの[機能のセクション](../intro/#fares-features-and-their-files)を再確認してください。

!!! Note

    このセクションには、Contactless 運賃のみの例が含まれています。他のチケットメディアの種類をサポートするには、該当する `fare_products.txt` の行を複製し、それに応じて amount および `fare_media_id` フィールドを更新してください。

## ゾーンを定義する {: #define-zones}


ゾーンベースの運賃で運行するルート・路線系統(route)では、そのルート・路線系統(route)が運行する各停留所等(stop)はゾーン内に位置します。ゾーンは、次のように `areas.txt` で作成します。

1. **area_id** にゾーンの一意の識別子を入力します。  
2. **area_name** にゾーンの名称を入力します。

ゾーンの詳細については、[ドキュメントを参照してください](../../../reference/#areastxt)。

!!! info "注意"

    [Translink](../intro/#translink-vancouver)では、バスは均一運賃を使用します。ただし、SkyTrain と SeaBus はゾーン運賃を使用します。各停留所等(stop)は、ZN1、ZN2、ZN3 の3つのゾーンのいずれかに割り当てられます。

    - **1ゾーン**運賃は、1つのゾーン内のみの乗車区間(leg)に適用されます（ZN1 から ZN1、ZN2 から ZN2、ZN3 から ZN3）  
    - **2ゾーン**運賃は、2つのゾーンのみをまたぐ乗車区間(leg)に適用されます（ZN1 から ZN2、ZN2 から ZN3）  
    - **3ゾーン**運賃は、3つすべてのゾーンをまたぐ乗車区間(leg)に適用されます（ZN1 から ZN3、ZN2 を経由）

    さらに、ZN2 内には Sea Island と呼ばれる追加のゾーンがあり、**Vancouver Airport (YVR)**、**Sea Island Centre**、および **Templeton** 駅が含まれます。 

    * Sea Island から出発する旅程(journey)には、ZN2 から出発する旅程(journey)に対して追加で CAD 5.00 が課金されます。   
    * Sea Island で終了する旅程(journey)には、ZN2 で終了する旅程(journey)と同額が課金されます。Sea Island 内で完結する旅程(journey)は無料です。

この例では、各ゾーンに1つずつ、さらに Sea Island 用に1つ、合計4つのエリアを作成します。各ゾーンには、一意の識別子（`ZN1`、`ZN2`、`ZN3`、`sea_island`）と、それぞれの名称を `area_name` に割り当てます。 

[**areas.txt**](../../../reference/#areastxt)

| area_id | area_name |
| :---- | :---- |
| ZN1 | ゾーン1 - Vancouver |
| ZN2 | ゾーン2 - Burnaby、Richmond、North Vancouver |
| ZN3 | ゾーン3 - Surrey、Coquitlam |
| sea_island | Sea Island（Vancouver Airport YVR Airport、Sea Island Centre、Templeton） |

## 停留所等(stop)をゾーンに割り当てる {: #assign-stops-to-zones}


`stops.txt` の各停留所等(stop)は、それを含むゾーンに割り当てる必要があります。停留所等(stop)は、以下のように `stop_areas.txt` のゾーンに関連付けられます。

1. **stop_id** に `stops.txt` の停留所等(stop)の id を入力します。  
2. **area_id** に `areas.txt` のゾーンの id を入力します。

エリアに関する詳細は、[ドキュメントを参照してください](../../../reference/#areastxt)。

この例では、Translink のサービスエリア内の各停留所等(stop)は、`ZN1`、`ZN2`、または `ZN3` に割り当てられています。Sea Island の停留所等(stop)（`99901`、`99902`、`99903`）は、両方のゾーン内に存在するため、`ZN2` と `sea_island` の両方に関連付けられています。このセクションの後半では、`rule_priority` が `sea_island` の乗車区間(leg)と `ZN2` の乗車区間(leg)を区別するのに役立ちます。

[**stop_areas.txt**](../../../reference/#stop_areastxt)

| stop_id | area_id |
| :---- | :---- |
| 8039 | ZN1 |
| 8066 | ZN2 |
| … | … |
| 99901 | ZN2 |
| 99902 | ZN2 |
| 99903 | ZN2 |
| 99901 | sea_island |
| 99902 | sea_island |
| 99903 | sea_island |

以下は `stops.txt` の概要であり、`stop_areas.txt` に含まれる一部の停留所等(stop)の stop_id を示しています。

[**stops.txt**](../../../reference/#stopstxt)

| stop_id | stop_name |
| :---- | :---- |
| 8039 | Waterfront Station @ Platform 2 |
| 8066 | Edmonds Station @ Platform 1 |
| 99901 | YVR-Airport Station @ Canada Line |
| … | … |

## チケット商品を作成する {: #create-fare-products}


[Route-Based Fares](../route-based-fares)と同様に、Zone-Based Fares のチケット商品は、以下のように `fare_products.txt` で作成します。

1. **fare_product_id** 列に、チケット商品を識別する一意の ID を入力します。  
2. **fare_product_name** 列に、乗客向けのチケット商品の名称（例: 1-Zone Fare、2-Zone Fare、1-Zone Fare Monthly）を入力します。  
3. **amount** 列および **currency** 列に、運賃の料金とその通貨（[currency codes](https://en.wikipedia.org/wiki/ISO_4217#Active_codes)）を入力します。  
4. **fare_media_id** 列に、このチケット商品を保存および使用できるチケットメディアを入力します。 
    * これは、`fare_media.txt` の **fare_media_id** を参照する外部キーです（[Fare Media](../../../reference/#faremediatxt)）。  
    * 同じチケット商品に複数のチケットメディアを関連付けることができ、価格が異なる可能性があります。  
    * **fare_media_id** が空の場合、チケットメディアは不明であることを意味します。

チケット商品の詳細については、[ドキュメント](../../../reference/#fare_productstxt)を参照してください。

この例では、`fare_products.txt` において、ゾーンベース運賃ごとにチケット商品を作成します。

* 1ゾーン運賃は CAD 3.20: 旅程は1つのゾーン内でのみ完結します。  
* 2ゾーン運賃は CAD 4.65: 旅程はあるゾーンから別のゾーンへ移動します。  
* 3ゾーン運賃は CAD 6.35: 旅程は Zone 1 から Zone 2（または Sea Island）を経由して Zone 3 へ移動します。  
* 1ゾーン Sea Island 運賃は CAD 8.20（CAD 5.00 + CAD 3.20）: Sea Island から Zone 2 への移動です。  
* 2ゾーン Sea Island 運賃は CAD 9.65（CAD 5.00 + CAD 4.65）: Sea Island から Zone 1 または Zone 3 への移動です。  
* Sea Island 内で開始および終了する旅程は CAD 0 です。


[**fare_products.txt**](../../../reference/#fare_productstxt)

| fare_product_id | fare_product_name | amount | currency | fare_media_id |
| :---- | :---- | :---- | :---- | :---- |
| bus_flat_fare | Bus Flat Fare | 3.20 | CAD | contactless |
| 1_zone_fare | 1-Zone Fare | 3.20 | CAD | contactless |
| 2_zone_fare | 2-Zone Fare | 4.65 | CAD | contactless |
| 3_zone_fare | 3-Zone Fare | 6.35 | CAD | contactless |
| sea_island_1_zone_fare | Sea Island travel + 1-zone Fare | 8.20 | CAD | contactless |
| sea_island_2_zone_fare | Sea Island travel + 2-zone fare | 9.65 | CAD | contactless |
| sea_island_sea_island_fare | Free fare inside Sea Island | 0 | CAD | contactless |

## ルート・路線系統(route)をグループ化するネットワークを作成する {: #create-networks-that-group-the-routes}


ゾーンベース運賃では、関連するルート・路線系統(route)は同じ運賃体系を持つため、ネットワークの下でグループ化する必要があります。

ネットワークは、次のように `networks.txt` で作成します。

1. **network_id** 列に、ネットワークを識別する一意の ID を入力します。  
2. **network_name** 列に、ネットワークの名前（例: Translink Buses、TTC Subway、STM All Routes）を入力します。

ネットワークの詳細については、[ドキュメントを参照してください](../../../reference/#networkstxt)。

[Translink](../intro/#translink-vancouver) の場合、バスは均一運賃体系を持つため、以前は独自のネットワークに分けられていました（[ルートベース運賃](../route-based-fares)のセクションを参照してください）。同様に、SkyTrain と Seabus は、運賃が通過するゾーン数に依存するため、1つのネットワークの下にグループ化されます。`skytrain_seabus` という `network_id` が作成されます。

[**networks.txt**](../../../reference/#networkstxt)

| network_id | network_name |
| :---- | :---- |
| skytrain_seabus | SkyTrain and SeaBus |

## ルート・路線系統(route)をネットワークに関連付ける {: #associate-routes-to-networks}


ネットワークを作成した後、それに含まれるルート・路線系統(route)を関連付ける必要があります。ルート・路線系統(route)は、次のように`route_networks.txt`でネットワークに関連付けられます。

1. **route_id**列に、`routes.txt`のルート・路線系統(route)のIDを入力します。
2. **network_id**列に、`networks.txt`の対応するネットワークのIDを入力します。

ルート・路線系統(route)ネットワークの詳細については、[ドキュメントを参照してください](../../../reference/#route_networkstxt)。

この例では、SkyTrainのルート・路線系統(route)（Canada Line、Millennium Line、Expo Line）およびSeaBusの`route_ids`が、`route_networks.txt`内の`network_id` `skytrain_seabus`に関連付けられています。以下のスナップショットでは、*13686*はCanada Lineの`route_id`であり、`30052`はMillennium Lineのroute_idです。

[**route_networks.txt**](../../../reference/#route_networkstxt)

| route_id | network_id |
| :---- | :---- |
| 13686 | skytrain_seabus |
| 30052 | skytrain_seabus |
| … | … |

## 運賃区間ルールを作成する {: #create-fare-leg-rules}


!!! info "リマインダー"

    **乗車区間(leg)**: 特定のサービスまたはルート・路線系統(route)で利用する旅程(journey)の単一の連続した区間であり、通常は2つの停留所等(stop)間で、乗換を伴いません。

    **乗車区間グループ**: `fare_leg_rules.txt` ファイルのコンテキストで定義される、特定の共通属性または運賃条件を共有する1つ以上の乗車区間(leg)の集合です。

乗車区間(leg)の運賃は、運賃区間ルールを使用して乗車区間(leg)をチケット商品と照合することで決定されます。ゾーンベース運賃では、運賃区間ルールは、ゾーン（`areas.txt` で定義）間を運行するルート・路線系統(route)のネットワーク（`networks.txt` で作成）を、チケット商品（`fare_products.txt` で作成）に関連付けます。

ゾーンベースの運賃区間ルールは、次のように `fare_leg_rules.txt` で作成されます。

1. **leg_group_id** 列に、乗車区間(leg)のグループを識別する一意の ID を入力します。  
2. **network_id** 列に、乗車区間(leg)が対象とするルート・路線系統(route)に関連付けられたネットワークの ID を入力します。  
    * これは `networks.txt` の **network_id** を参照する外部キーです。  
3. **from_area_id** に、乗車区間(leg)が出発するゾーンの ID を入力します。  
4. **to_area_id** に、乗車区間(leg)が到着するゾーンの ID を入力します。  
5. **fare_product_id** 列に、乗車区間(leg)の料金を決定するチケット商品の ID を入力します。  
    * これは `fare_products.txt` の **fare_product_id** を参照する外部キーです。

運賃区間ルールの詳細については、[ドキュメントを参照してください](../../../reference/#fare_leg_rulestxt)。

この例では、可能な各ゾーンの組み合わせに対して複数の乗車区間グループが追加されています。たとえば、`from_area_id=ZN1` および `to_area_id=ZN1` であるため、`ZN1_ZN1` はゾーン1内にとどまる乗車区間(leg)です。`ZN1_ZN1` は `fare_product_id=1_zone_fare` に関連付けられています。 

以下の例では、`ZN1_ZN2` が2回記載されていることに注意してください。最初に (`from_area_id=ZN1`, `to_area_id=ZN2`) と関連付けられ、次に2行目で (`from_area_id=ZN2`, `to_area_id=ZN1`) と関連付けられています。これは、`ZN1_ZN2` が `ZN1` と `ZN2` 間の両方向の移動に対する運賃ルールに一致する乗車区間グループを表すことを意味します。

!!! Note

    以下の例には Sea Island の乗車区間ルールは含まれていません。これらは次の手順で扱います。

[**fare_leg_rules.txt**](../../../reference/#fare_leg_rulestxt)

| leg_group_id | network_id | fare_product_id | from_area_id | to_area_id |
| :---- | :---- | :---- | :---- | :---- |
| … | … | … |  |  |
| ZN1_ZN1 | skytrain_seabus | 1_zone_fare | ZN1 | ZN1 |
| ZN2_ZN2 | skytrain_seabus | 1_zone_fare | ZN2 | ZN2 |
| ZN3_ZN3 | skytrain_seabus | 1_zone_fare | ZN3 | ZN3 |
| ZN1_ZN2 | skytrain_seabus | 2_zone_fare | ZN1 | ZN2 |
| ZN1_ZN2 | skytrain_seabus | 2_zone_fare | ZN2 | ZN1 |
| ZN2_ZN3 | skytrain_seabus | 2_zone_fare | ZN2 | ZN3 |
| ZN2_ZN3 | skytrain_seabus | 2_zone_fare | ZN3 | ZN2 |
| ZN1_ZN3 | skytrain_seabus | 3_zone_fare | ZN1 | ZN3 |
| ZN1_ZN3 | skytrain_seabus | 3_zone_fare | ZN3 | ZN1 |

## 重複するゾーンに優先順位を付ける {: #prioritize-overlapping-zones}


場合によっては、複数のゾーンが同じ停留所等(stop)を共有することがあります。これにより、乗車区間(leg)に運賃区間ルールを適用する際に、どのゾーンを使用するべきかについて曖昧さが生じることがあります。これを解決するために、`fare_leg_rules.txt` では `rule_priority` フィールドを使用します。これは、一致するルールが適用される順序を決定します。同じ条件に一致するルールの集合では、`rule_priority` の値が高いルールが、値が低いまたは空のルールよりも優先されます。

これは `fare_leg_rules.txt` で次のように行います。

1. 前の手順の指示に従って、考えられるすべての運賃区間ルールを作成します。  
2. **rule_priority** に、乗車区間(leg)の優先順位を入力します。**rule_priority** の値が高いほど、運賃区間ルールの優先順位が高くなります。

運賃区間ルールの詳細については、[ドキュメントを参照してください](../../../reference/#fare_transfer_rulestxt)。

この例では、Sea Island は Zone 2 の内部に存在するため、乗車区間(leg)が Sea Island から始まり Zone 2 で終わる場合、それは「Sea Island から Zone 2」への乗車区間(leg)、「Zone 2 から Zone 2」への乗車区間(leg)、または「Sea Island から Sea Island」への乗車区間(leg)のどれと見なされるのでしょうか。実際には、この乗車区間(leg)は3つすべての可能性に一致し、曖昧さが生じます。

まず、Sea Island から始まる乗車区間(leg)を追加します。

* 乗車区間(leg) `sea_island_ZN1` および `sea_island_ZN3` は、いずれも CAD 5.00 に2ゾーン運賃を加えた料金です。  
* 乗車区間(leg) `sea_island_ZN2` は、CAD 5.00 に1ゾーン運賃を加えた料金です。  
* 乗車区間(leg) `sea_island_sea_island` は無料です。

次に、`rule_priority` に適切な値を入力します。

* `sea_island_sea_island` は最も高い優先順位（`rule_priority=2`）を持ちます。これにより、乗車区間(leg)の出発停留所等(stop)と到着停留所等(stop)が `sea_island`（Zone 2 内）にある場合、優先される乗車区間(leg)は `sea_island_sea_island` になります。 
* Sea Island から始まり、他の場所（Zone 1、Zone 3、Sea Island 外の Zone 2）で終わる乗車区間(leg)は、`rule_priority=1` を持ちます。  
* 残りの乗車区間(leg)は最も低い優先順位を持ちます。`rule_priority=0`（または空）です。

[**fare_leg_rules.txt**](../../../reference/#fare_leg_rulestxt)

| leg_group_id | network_id | fare_product_id | from_area_id | to_area_id | rule_priority |
| :---- | :---- | :---- | :---- | :---- | :---- |
| … | … | … |  |  |  |
| sea_island_ZN1 | skytrain_seabus | sea_island_2_zone_fare | sea_island | ZN1 | 1 |
| sea_island_ZN2 | skytrain_seabus | sea_island_1_zone_fare | sea_island | ZN2 | 1 |
| sea_island_ZN3 | skytrain_seabus | sea_island_2_zone_fare | sea_island | ZN3 | 1 |
| sea_island_sea_island | skytrain_seabus | sea_island_sea_island_fare | sea_island | sea_island | 2 |
| ZN1_ZN1 | skytrain_seabus | 1_zone_fare | ZN1 | ZN1 |  |
| ZN2_ZN2 | skytrain_seabus | 1_zone_fare | ZN2 | ZN2 |  |
| ZN3_ZN3 | skytrain_seabus | 1_zone_fare | ZN3 | ZN3 |  |
| ZN1_ZN2 | skytrain_seabus | 2_zone_fare | ZN1 | ZN2 |  |
| ZN1_ZN2 | skytrain_seabus | 2_zone_fare | ZN2 | ZN1 |  |
| ZN2_ZN3 | skytrain_seabus | 2_zone_fare | ZN2 | ZN3 |  |
| ZN2_ZN3 | skytrain_seabus | 2_zone_fare | ZN3 | ZN2 |  |
| ZN1_ZN3 | skytrain_seabus | 3_zone_fare | ZN1 | ZN3 |  |
| ZN1_ZN3 | skytrain_seabus | 3_zone_fare | ZN3 | ZN1 |  |
