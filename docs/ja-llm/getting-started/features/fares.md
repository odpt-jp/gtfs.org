# :material-cash: 運賃 {: #material-cash-fares}


GTFS では、ゾーン、移動距離、時間帯に基づく運賃など、世界中のさまざまな交通事業者が使用する多種多様な運賃体系を正確にモデル化できます。GTFS Fares は、乗客に対して、その便に適用される料金と支払いに使用できるチケットメディアを通知します。

## チケット商品 {: #fare-products}


チケット商品は、サービスを利用するために交通事業者が提供するチケットまたは運賃の種類（例：片道運賃、定期券、乗換料金など）を一覧化したものです。チケット商品は、事業者の運賃体系をモデル化するための基盤となり、`fare_leg_rules.txt` で説明されている仕組みを通じて交通サービスに関連付けられます。チケット商品をルート・路線系統(route)、エリア、時間などのさまざまな移動条件に関連付けることで、個々の移動区間および乗換に対する運賃が決定されます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[fare_products.txt](../../../documentation/schedule/reference/#fare_productstxt)|`fare_product_id`, `fare_product_name`, `amount`, `currency`, `fare_media_id` |
|[fare_leg_rules.txt](../../../documentation/schedule/reference/#fare_leg_rulestxt)|`fare_product_id`|


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、単純なチケット商品（1回乗車 2.75 USD）を示しています。 
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_productstxt"><b>fare_products.txt</b></a> <br>
        </p>

        | fare_product_id  | fare_product_name      | amount  | currency  |
        |------------------|--------------------    |---      |---        |
        | single_ride      | 1回乗車運賃       |  2.75   | USD       |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_leg_rulestxt"><b>fare_leg_rules.txt</b></a> <br>
        </p>

        | fare_product_id  |
        |------------------|
        | single_ride |

## チケットメディア {: #fare-media}


チケットメディアは、チケット商品を保持および／または検証するために使用できる、対応するメディアを定義します。これは、紙の乗車券、チャージ可能な交通カード、さらにはクレジットカードやスマートフォンによる非接触決済などの、物理的または仮想的な媒体を指します。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[fare_media.txt](../../../documentation/schedule/reference/#fare_mediatxt)|`fare_media_id`, `fare_media_name`, `fare_media_type`|
|[fare_products.txt](../../../documentation/schedule/reference/#fare_productstxt)|`fare_media_id`|


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、San Francisco Bay Areaにおける異なるチケットメディアの抜粋を示しています。`Clipper` は、`fare_media_type=2` の物理的な交通カードとして記述されています。`SFMTA Munimobile` は、`fare_media_type=2` のモバイルアプリとして記述されています。乗車券なしで運転手に直接渡される `Cash` は、`fare_media_type=0` です。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_mediatxt"><b>fare_media.txt</b></a> <br>
        </p>

        | fare_media_id | fare_media_name  | fare_media_type |
        |---------------|------------------|-----------------|
        | clipper       | Clipper          | 2               |
        | munimobile    | SFMTA MuniMobile | 4               |
        | cash          | Cash             | 0               |  

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_productstxt"><b>fare_products.txt</b></a> <br>
        </p>

        | fare_product_id  | fare_product_name      | amount  | currency  | fare_media_id |
        |------------------|--------------------    |---      |---        | ---           |
        | single_ride      | Single Ride Fare       |  2.75   | USD       | munimobile          |

## 乗客カテゴリー


乗客カテゴリーは、高齢者、学生、成人など、特定の運賃料金の対象となるさまざまな公共交通機関の乗客の種類を表すために使用されます。経路検索アプリケーションは、この情報を使用して利用可能なカテゴリーを表示し、フィードを提供する事業者が設定したデフォルト運賃を表示することができます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[rider_categories.txt](../../../documentation/schedule/reference/#rider_categoriestxt)|`rider_category_id`, `rider_category_name`, `is_default_fare_category`, `eligibility_url`|
|[fare_products.txt](../../../documentation/schedule/reference/#fare_productstxt)|`rider_category_id`|


**前提条件**: 

- [基本機能](../base)
- [チケット商品機能](#fare-products)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、成人カテゴリーをデフォルトとして設定した、3つの異なる乗客カテゴリーを示しています。 
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#rider_categoriestxt"><b>rider_categories.txt</b></a> <br>
        </p>

        | rider_category_id | rider_category_name | is_default_fare_category | eligibility_url |
        |---|---|---|---|
        | rc01-adult | Adult | 1 |  |
        | rc02-senior | Senior (65+) | 0 | https://www.agency-abcd.org/info/reduced-fare-65 |
        | rc03-student | Student | 0 | https://www.agency-abcd.org/info/reduced-fare-students |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_productstxt"><b>fare_products.txt</b></a> <br>
        </p>

        | fare_product_id | fare_product_name          | rider_category_id | amount | currency |
        |-----------------|----------------------------|-------------------|--------|----------|
        | single_ride     | Single Ride Fare           | rc01-adult        |   2.75 | USD      |
        | single_ride     | Single Ride Fare - Student | rc03-student      |   1.50 | USD      |

## ルート・路線系統(route)ベースの運賃 {: #route-based-fares}


ルート・路線系統(route)ベースの運賃は、急行サービス向けの特別運賃や、Bus Rapid Transit サービスと従来のバスサービス間での運賃の差別化など、特定のルート・路線系統(route)グループに異なる運賃を割り当てるために使用されます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[routes.txt](../../../documentation/schedule/reference/#routestxt)|`network_id`|
|[fare_leg_rules.txt](../../../documentation/schedule/reference/#fare_leg_rulestxt)|`fare_product_id`, `network_id`|
|[networks.txt](../../../documentation/schedule/reference/#networkstxt)|`network_id`, `network_name`|
|[route_networks.txt](../../../documentation/schedule/reference/#route_networkstxt)|`network_id`, `route_id`|

**前提条件**:

- [基本機能](../base)
- [チケット商品機能](#fare-products)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、ルート・路線系統(route)を急行と各駅停車のカテゴリに分類し、それぞれに異なるチケット商品を関連付けるシステムを示しています。 </p>

    <p style="font-size:16px"> **`networks.txt` + `route_networks.txt` を使用する場合** </p>

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#networkstxt"><b>networks.txt</b></a> <br>
        </p>

        | network_id | network_name    |
        |------------|-----------------|
        | express    | 急行         |
        | local      | 各駅停車           |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#route_networkstxt"><b>route_networks.txt</b></a> <br>
        </p>

        | network_id | route_id |
        |------------|-----------|
        | express    | express_a |
        | express    | express_b |
        | local      | local_1   |
        | local      | local_2   |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_leg_rulestxt"><b>fare_leg_rules.txt</b></a> <br>
        </p>

        | network_id | fare_product_id |
        |------------|-----------------|
        | express    | express_single_ride |
        | local      | local_single_ride   |

    <p style="font-size:16px"> **または `routes.networks_id` を使用する場合** </p>

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#routestxt"><b>routes.txt</b></a> <br>
        </p>

        | route_id   | network_id |
        |------------|------------|
        | express_a  | express    |
        | express_b  | express    |
        | local_1    | local      |
        | local_2    | local      |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_leg_rulestxt"><b>fare_leg_rules.txt</b></a> <br>
        </p>

        | network_id | fare_product_id |
        |------------|-----------------|
        | express    | express_single_ride |
        | local      | local_single_ride   |

## 時間帯別運賃 {: #time-based-fares}


時間帯別運賃は、ピーク時・オフピーク時の運賃や週末運賃など、特定の時間帯または曜日に対して運賃を割り当てるために使用されます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[fare_leg_rules.txt](../../../documentation/schedule/reference/#fare_leg_rulestxt)|`fare_product_id`, `from_timeframe_group_id`, `to_timeframe_group_id`|
|[timeframes.txt](../../../documentation/schedule/reference/#timeframestxt)|`timeframe_group_id`, `start_time`, `end_time`, `service_id`|

**前提条件**:

- [基本機能](../base)
- [チケット商品機能](../fares/#fare-products)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、ピーク時間帯が8:00から10:00までであり、残りの時間帯がオフピークであるシステムを示しています。 </p>

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#timeframestxt"><b>timeframes.txt</b></a> <br>
        </p>

        | timeframe_group_id | start_time | end_time | service_id |
        |--------------------|------------|----------|------------|
        | peak               | 8:00:00    | 10:00:00 | all_day    |
        | regular            | 0:00:00    | 08:00:00 | all_day    |
        | regular            | 10:00:00   | 24:00:00 | all_day    |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_leg_rulestxt"><b>fare_leg_rules.txt</b></a> <br>
        </p>

        | from_timeframe_group_id | fare_product_id     |
        |-------------------------|---------------------|
        | peak                    | peak_single_ride    |
        | regular                 | regular_single_ride |

## ゾーンベース運賃 {: #zone-based-fares}


ゾーンベース運賃は、ある特定のゾーンから別の特定のゾーンへ移動する際に特定の運賃が適用される、ゾーンベースのシステムを表現するために使用されます。ゾーンは、停留所等(stop)のグループによって定義されます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[fare_leg_rules.txt](../../../documentation/schedule/reference/#fare_leg_rulestxt)|`fare_product_id`, `from_area_id`, `to_area_id`|
|[areas.txt](../../../documentation/schedule/reference/#areastxt)|`area_id`, `area_name`|
|[stop_areas.txt](../../../documentation/schedule/reference/#stop_areastxt)|`area_id`, `stop_id`|

**前提条件**:

- [基本機能](../base)
- [チケット商品機能](../fares/#fare-products)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、ゾーンAからゾーンBまでの運賃を示しています。 </p>

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#areastxt"><b>areas.txt</b></a> <br>
        </p>

        | area_id | area_name |
        |---------|-----------|
        | zone_a  | Zone A    |
        | zone_b  | Zone B    |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stop_areastxt"><b>stop_areas.txt</b></a> <br>
        </p>

        | area_id | stop_id |
        |---------|---------|
        | zone_a  | stop_a  |
        | zone_a  | stop_b  |
        | zone_b  | stop_c  |
        | zone_b  | stop_d  |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_leg_rulestxt"><b>fare_leg_rules.txt</b></a> <br>
        </p>

        | from_area_id | to_area_id | fare_product_id |
        |--------------|------------|-----------------|
        | zone_a       | zone_b     | zone_a_b_single |

## 運賃乗換 {: #fare-transfers}


運賃乗換は、乗車区間(leg)（または個々の移動区間）間で乗り換える際に適用されるルールを定義するために使用されます。これにより、特定の時間制限内の無料乗換や、すでに移動した乗車区間(leg)に基づく運賃割引の適用など、特別な乗換ポリシーを考慮して、複数の乗車区間(leg)からなる旅程(journey)の総費用をモデル化できます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[fare_leg_rules.txt](../../../documentation/schedule/reference/#fare_leg_rulestxt)|`leg_group_id`|
|[fare_transfer_rules.txt](../../../documentation/schedule/reference/#fare_transfer_rulestxt)|`from_leg_group_id`, `to_leg_group_id`, `transfer_count`, `duration_limit`, `duration_limit_type`, `fare_transfer_type`, `fare_product_id`|

**前提条件**:

- [基本機能](../base)
- [チケット商品機能](../fares/#fare-products)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、2時間の時間枠内において、システム内の乗車区間(leg) A 間で無制限の無料乗換が許可されることを示しています。 </p>

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_leg_rulestxt"><b>fare_leg_rules.txt</b></a> <br>
        </p>

        | leg_group_id  |
        |---------------|
        | a             |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_transfer_rulestxt"><b>fare_transfer_rules.txt</b></a> <br>
        </p>

        | from_leg_group_id | to_leg_group_id | transfer_count | duration_limit | duration_limit_type | fare_transfer_type | fare_product_id |
        |-------------------|-----------------|----------------|----------------|---------------------|--------------------|-----------------|
        | a                 | a               | -1             | 7200           | 1                   | 0                  | free_transfer   |

## 非接触 EMV サポート {: #contactless-emv-support}


非接触 EMV サポート機能により、データ提供者は、乗客が非接触型カードまたはデバイス（例: タップ決済システム）を使用して交通サービスを利用できるかどうかを示すことができます。
この機能は、事業者またはルート・路線系統(route)レベルで非接触決済の利用可否を伝えるための簡易的な代替手段を提供しますが、[Fare Media](../fares/#fare-media) を通じて提供される詳細な運賃情報の代替ではありません。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[agency.txt](../../../documentation/schedule/reference/#agencytxt)|`cemv_support`|
|[routes.txt](../../../documentation/schedule/reference/#routestxt)|`cemv_support`|

**前提条件**:

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次の例では、最初の表は、事業者 `AA` が運行するすべてのサービスを、非接触型カードまたはデバイス（cEMV）で支払う乗客が利用できることを示しています。 </p>

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#agencytxt"><b>agency.txt</b></a> <br>
        </p>

        | agency_id | agency_name | agency_url | agency_timezone | cemv_support |
        | :---- | :---- | :---- | :---- | :---- |
        | AA | Agency A | [www.gtfsagencya.org](http://www.gtfsagencya.org) | America/Denver | 1 |
        | BB | Agency B | [www.gtfsagencyb.org](http://www.gtfsagencyb.org) | America/Denver |  |
        | CC | Agency C | [www.gtfsagencyc.org](http://www.gtfsagencyc.org) | America/Denver |  |

    <p style="font-size:16px">
    2番目の表では、特定のルート・路線系統(route)（`BB001`、`BB003`、および `CC001`）のみを、非接触型カードまたはデバイス（cEMV）で支払う乗客が利用できます。事業者 `BB` および `CC` のその他すべてのルート・路線系統(route)は、非接触決済をサポートしていません。 </p>

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#routestxt"><b>routes.txt</b></a> <br>
        </p>

        | route_id | agency_id | route_short\_name | route_type | cemv_support |
        | :---- | :---- | :---- | :---- | :---- |
        | AA001 | AA | A1 | 3 |  |
        | AA002 | AA | A2 | 3 |  |
        | AA003 | AA | A3 | 3 |  |
        | BB001 | BB | B1 | 3 | 1 |
        | BB002 | BB | B2 | 3 | 2 |
        | BB003 | BB | B3 | 3 | 1 |
        | CC001 | CC | C1 | 3 | 1 |
        | CC002 | CC | C2 | 3 | 2 |
        | CC003 | CC | C3 | 3 | 2 |

## 運賃 v1 {: #fares-v1}


運賃 v1 は、上記で説明した他の運賃機能に代わるレガシーな選択肢です。`fare_rules.txt` および `fare_attributes.txt` ファイルを使用して、運賃価格、支払い方法、乗換、およびゾーンベースの運賃などの基本的な運賃情報をモデル化できます。作成はより簡単ですが、より複雑な運賃体系をモデル化する能力は低く、他の運賃機能（Fares v2 と呼ばれるものの一部）が十分に支持された場合には非推奨となる可能性があります。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stops.txt](../../../documentation/schedule/reference/#stopstxt)|`zone_id`|
|[fare_attributes.txt](../../../documentation/schedule/reference/#fare_attributestxt)|`fare_id` `price` `currency_type` `payment_method` `transfers` `agency_id` `transfer_duration`|
|[fare_rules.txt](../../../documentation/schedule/reference/#fare_rulestxt)|`fare_id` `route_id` `origin_id` `destination_id` `contains_id`|


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、ネットワーク上の便がプリペイドカードを使用して 3.20 CAD であり、2 時間の時間枠内で無料乗換が可能であることを示しています。 </p>

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_attributestxt"><b>fare_attributes.txt</b></a> <br>
        </p>

        | fare_id           | price | currency_type | payment_method | transfers | transfer_duration |
        |-------------------|-------|---------------|----------------|-----------|-------------------|
        | prepaid-card_fare | 3.2   | CAD           | 1              |           | 7200              |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_rulestxt"><b>fare_rules.txt</b></a> <br>
        </p>

        | fare_id           | route_id | origin_id       | destination_id  |
        |-------------------|----------|-----------------|-----------------|
        | prepaid-card_fare | line1    | subway_stations | subway_stations |
        | prepaid-card_fare | line2    | subway_stations | subway_stations |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stopstxt"><b>stops.txt</b></a> <br>
        </p>

        | stop_id | stop_name | stop_lat  | stop_lon   | zone_id         |
        |---------|-----------|-----------|------------|-----------------|
        | A       | stopA     | 43.670049 | -79.385389 | subway_stations |
        | B       | stopB     | 43.671049 | -79.386789 | subway_stations |


        | stop_id | stop_name | stop_lat  | stop_lon   | zone_id         |
        |---------|-----------|-----------|------------|-----------------|
        | A       | stopA     | 43.670049 | -79.385389 | subway_stations |
        | B       | stopB     | 43.671049 | -79.386789 | subway_stations |
