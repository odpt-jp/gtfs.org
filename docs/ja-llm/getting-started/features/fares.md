# :material-cash: 運賃 {: #material-cash-fares}

GTFS は、世界中のさまざまな交通事業者が採用している多様な運賃体系を正確にモデル化することができます。たとえば、ゾーン制運賃、移動距離に基づく運賃、時間帯別運賃などです。GTFS の運賃情報は、乗客に対してその便(trip)に適用される料金および支払いに使用できるチケットメディアを知らせます。

## チケット商品(Fare Products) {: #fare-products}


チケット商品(Fare Products)は、交通事業者がサービスを利用するために提供するチケットや運賃の種類（例：片道運賃、月間パス、乗り継ぎ料金など）を一覧化したものです。チケット商品は、事業者の運賃体系をモデル化するための基盤となり、`fare_leg_rules.txt` に記載された仕組みを通じて交通サービスと関連付けられます。チケット商品がルート、エリア、時間などのさまざまな旅行条件に関連付けられることで、個々の乗車区間や乗り継ぎに対する運賃が決定されます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[fare_products.txt](../../../documentation/schedule/reference/#fare_productstxt)|`fare_product_id`, `fare_product_name`, `amount`, `currency`, `fare_media_id` |
|[fare_leg_rules.txt](../../../documentation/schedule/reference/#fare_leg_rulestxt)|`fare_product_id`|


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、単純なチケット商品（片道運賃 $2.75 USD）を示しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_productstxt"><b>fare_products.txt</b></a> <br>
        </p>

        | fare_product_id  | fare_product_name      | amount  | currency  |
        |------------------|--------------------    |---      |---        |
        | single_ride      | Single Ride Fare       |  2.75   | USD       |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#fare_leg_rulestxt"><b>fare_leg_rules.txt</b></a> <br>
        </p>

        | fare_product_id  |
        |------------------|
        | single_ride |

## チケットメディア(Fare Media) {: #fare-media}


チケットメディア(Fare Media)は、チケット商品(Fare product)を保持および／または認証するために使用できるサポート対象のメディアを定義します。これは、紙のチケット、再利用可能な交通系ICカード、さらにはクレジットカードやスマートフォンによる非接触決済など、物理的または仮想的なコンテナを指します。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[fare_media.txt](../../../documentation/schedule/reference/#fare_mediatxt)|`fare_media_id`, `fare_media_name`, `fare_media_type`|
|[fare_products.txt](../../../documentation/schedule/reference/#fare_productstxt)|`fare_media_id`|


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、サンフランシスコ湾岸地域におけるさまざまなチケットメディア(Fare Media)の一部を示しています。`Clipper` は `fare_media_type=2` の物理的な交通系ICカードとして記述されています。`SFMTA Munimobile` は `fare_media_type=4` のモバイルアプリとして記述されています。運転手に直接渡され、チケットを伴わない `Cash` は `fare_media_type=0` です。
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

## 乗客カテゴリ {: #rider-categories}

乗客カテゴリは、高齢者、学生、一般成人など、特定の運賃料金に該当するさまざまな公共交通の乗客タイプを表すために使用されます。経路検索アプリケーションは、この情報を使用して利用可能なカテゴリを表示し、フィードを提供する事業者によって設定されたデフォルト運賃を示すことができます。

| 含まれるファイル | 含まれるフィールド |
|------------------|-------------------|
|[rider_categories.txt](../../../documentation/schedule/reference/#rider_categoriestxt)|`rider_category_id`, `rider_category_name`, `is_default_fare_category`, `eligibility_url`|
|[fare_products.txt](../../../documentation/schedule/reference/#fare_productstxt)|`rider_category_id`|

**前提条件**: 

- [基本機能](../base)
- [チケット商品機能](#fare-products)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、3つの異なる乗客カテゴリを示しており、「Adult」カテゴリがデフォルトとして設定されています。
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

## ルートベース運賃 {: #route-based-fares}

ルートベース運賃は、特定のルート群に対して異なる運賃を設定するために使用されます。たとえば、急行サービスに対する特別運賃や、バス高速輸送サービス（Bus Rapid Transit）と従来のバスサービスとの運賃を区別する場合などです。

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
    次のサンプルは、ルートを急行とローカルのカテゴリに分類し、それぞれに異なるチケット商品を関連付けるシステムを示しています。</p>

    <p style="font-size:16px"> **`networks.txt` + `route_networks.txt` を使用する場合** </p>

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#networkstxt"><b>networks.txt</b></a> <br>
        </p>

        | network_id | network_name    |
        |------------|-----------------|
        | express    | Express         |
        | local      | Local           |

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


時間帯別運賃(Time-Based Fares)は、特定の時間帯や曜日に応じて運賃を設定するために使用されます。たとえば、ピーク時運賃、オフピーク時運賃、週末運賃などです。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[fare_leg_rules.txt](../../../documentation/schedule/reference/#fare_leg_rulestxt)|`fare_product_id`, `from_timeframe_group_id`, `to_timeframe_group_id`|
|[timeframes.txt](../../../documentation/schedule/reference/#timeframestxt)|`timeframe_group_id`, `start_time`, `end_time`, `service_id`|

**前提条件**:

- [基本機能](../base)
- [チケット商品機能](../fares/#fare-products)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、ピーク時間が8:00から10:00までで、それ以外の時間がオフピークであるシステムを示しています。</p>

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


ゾーンベース運賃は、特定のゾーンから別のゾーンへ移動する際に特定の運賃が適用される、ゾーンベースの運賃体系を表現するために使用されます。ゾーンは、停留所等(stop)のグループによって定義されます。

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
    以下のサンプルは、ゾーンAからゾーンBへの運賃を示しています。</p>

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

## 運賃乗り継ぎ (Fare Transfers) {: #fare-transfers}


運賃乗り継ぎ(Fare Transfers)は、乗車区間(leg)間（または個々の移動区間）を乗り継ぐ際に適用されるルールを定義するために使用されます。これにより、特定の時間内の無料乗り継ぎや、すでに利用した乗車区間に基づく割引運賃の適用など、特別な乗り継ぎポリシーを考慮した複数区間の旅程(journey)全体の総運賃をモデル化することができます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[fare_leg_rules.txt](../../../documentation/schedule/reference/#fare_leg_rulestxt)|`leg_group_id`|
|[fare_transfer_rules.txt](../../../documentation/schedule/reference/#fare_transfer_rulestxt)|`from_leg_group_id`, `to_leg_group_id`, `transfer_count`, `duration_limit`, `duration_limit_type`, `fare_transfer_type`, `fare_product_id`|

**前提条件**:

- [基本機能](../base)
- [チケット商品(Fare Products)機能](../fares/#fare-products)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、システム内で2時間の時間枠内において、乗車区間A間で無制限の無料乗り継ぎが許可されていることを示しています。</p>

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

## 非接触型EMV対応 {: #contactless-emv-support}

非接触型EMV対応機能は、乗客が非接触型カードやデバイス（例：タップ決済システム）を使用して交通サービスを利用できるかどうかを、データ提供者が示すことを可能にします。  
この機能は、事業者またはルート・路線系統レベルで非接触型決済の利用可否を簡易的に伝えるための代替手段を提供しますが、[Fare Media](../fares/#fare-media)で提供される詳細な運賃情報の代替ではありません。

| 含まれるファイル | 含まれるフィールド |
|------------------|-------------------|
|[agency.txt](../../../documentation/schedule/reference/#agencytxt)|`cemv_support`|
|[routes.txt](../../../documentation/schedule/reference/#routestxt)|`cemv_support`|

**前提条件**:

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次の例では、最初の表において、事業者 `AA` が運行するすべてのサービスが、非接触型カードまたはデバイス（cEMV）による支払いで乗客が利用できることを示しています。</p>

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
    2つ目の表では、特定のルート・路線系統（`BB001`、`BB003`、および `CC001`）のみが、非接触型カードまたはデバイス（cEMV）による支払いで乗客が利用できることを示しています。事業者 `BB` および `CC` のその他のルート・路線系統は、非接触型決済に対応していません。</p>

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

## Fares v1 {: #fares-v1}


Fares v1 は、上記で説明されている他の運賃(Fares)機能に対するレガシーな代替手段です。これは、`fare_rules.txt` および `fare_attributes.txt` ファイルを使用して、運賃価格、支払い方法、乗り継ぎ、ゾーンベースの運賃などの基本的な運賃情報をモデル化することを可能にします。作成はより簡単ですが、より複雑な運賃体系をモデル化する能力は低く、他の運賃機能（いわゆる Fares v2）の十分な支持を得た場合には廃止される可能性があります。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stops.txt](../../../documentation/schedule/reference/#stopstxt)|`zone_id`|
|[fare_attributes.txt](../../../documentation/schedule/reference/#fare_attributestxt)|`fare_id` `price` `currency_type` `payment_method` `transfers` `agency_id` `transfer_duration`|
|[fare_rules.txt](../../../documentation/schedule/reference/#fare_rulestxt)|`fare_id` `route_id` `origin_id` `destination_id` `contains_id`|


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、プリペイドカードを使用した場合に、ネットワーク上の1回の便(trip)が 3.20 カナダドルであり、2時間以内の無料乗り継ぎが可能であることを示しています。 </p>

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
