# 運賃 (v2) {: #fares-v2}


!!! info

    このページの例は古くなっている可能性があります。このページはまもなく非推奨となります。最新の例については、新しい[Fares example section](../intro)を参照してください。

Fares（Fares v2とも呼ばれます）は、[GTFS Schedule feature](../../../../../getting-started/features/overview/)であり、乗客向けの運賃情報を標準化します。これにより、ユーザーは各交通システムおよびその接続の運賃構造と条件に基づいて、チケット発券および料金の選択肢を見つけることができます。

主なFares (v2)の機能には、チケット商品、チケットメディア、ルート・路線系統(route)ベースの運賃、ゾーンベースの運賃、時間ベースの運賃、および乗換ルールが含まれます。

GTFS Fares (v2)は、[GTFS-Fares v2](../../../../../community/extensions/fares-v2)という作業名のもとで開発されている、コミュニティ主導のプロジェクトとして進化を続けています。実験的機能のモデリングに関するガイダンスについては、[完全な提案文書](https://share.mobilitydata.org/gtfs-fares-v2)を参照してください。

## 運賃に関するトレーニングと無料リソース {: #fares-training-and-free-resources}


GTFS Fares (v2) を始めるには、以下の4つのビデオチュートリアルを視聴し、[この文書リソース](https://share.mobilitydata.org/Fares-v2-written-resource-guide-for-videos)に沿って進めることができます。</a>

- [ビデオ 1](https://share.mobilitydata.org/faresv2-intro): GTFS Fares (v2): 概要
- [ビデオ 2](https://share.mobilitydata.org/faresv2-setting-up-google-sheets): GTFS Fares (v2): Google Sheets の設定
- [ビデオ 3](https://share.mobilitydata.org/faresv2-creating-and-maintaining-data): GTFS Fares (v2): データの作成と維持
- [ビデオ 4](https://share.mobilitydata.org/faresv2-exporting-and-publishing): GTFS Fares (v2) のエクスポートと公開

これらのビデオは、交通事業者が Fares (v2) の目的、および Google Sheets を使用して GTFS Fares (v2) データを作成、編集、アップロードする方法を理解できるように作成されています。 

この [Fares (v2) テンプレート](https://share.mobilitydata.org/faresv2-template)は、必要な運賃ファイルをゼロから作成するために使用できます。

## 運賃データモデリングの例 {: #fares-data-modelling-examples}

### 交通運賃を定義する {: #define-a-transit-fare}


Maryland Transit Administrationシステムを利用するための運賃の支払い方法はいくつかあります。<a href="https://www.mta.maryland.gov/regular-fares" target="_blank">通常の全額運賃には4種類の選択肢があります。</a>

- 2.00 USDの片道チケット
- 4.60 USDの1日パス
- 22 USDの週間パス
- 77 USDの月間パス

交通チケットまたは運賃は、GTFSではチケット商品と呼ばれます。これらは[fare_products.txt](../../../reference/#fare_productstxt)ファイルを使用して記述できます。各エントリは特定の運賃に対応します。

[**fare_products.txt**](../../../reference/#fare_productstxt)

| fare_product_id  | fare_product_name  | amount  | currency  |
|------------------------|--------------------|---|---|
| core_local_oneway_fare | One Way Full Fare |  2.00 | USD  |
| core_local_1_day_fare  | 1-Day Pass - Core Service  | 4.60  | USD   |
| core_local_31_day_fare | 31-Day Pass - Core Service  | 77.00  | USD  |
| core_local_7_day_fare  | 7-Day Pass - Core Service |  22.00 | USD  |


<sup>[Maryland Transit AdministrationのローカルバスGTFSフィードをダウンロードする](https://feeds.mta.maryland.gov/gtfs/local-bus)</sup>

<hr>

### 1乗車区間の旅程(journey)に対するルールを作成する {: #create-rules-for-single-leg-journeys}


GTFS では、運賃区間は、乗客が異なる交通手段、ルート・路線系統(route)、ネットワーク、または事業者間で乗り換えることなく行う便(trip)に対応します。Maryland Transit Administration のフィードでは、単一運賃により、乗客は BaltimoreLink バス、Light RailLink、および Metro SubwayLink ルート・路線系統(route)の `core` ネットワーク内にある任意の停留所等(stop)と地下鉄駅の組み合わせの間を移動できます。

乗車区間グループは、ネットワーク内における出発地から目的地までの便(trip)を定義します（エリア ID がグループ化された停留所等(stop)に対応する場合は、出発地の集合から目的地の集合まで）。以下のファイルは、Maryland Transit Administration のコアネットワーク内の任意の場所へ移動するためのルールを記述しています。各ルールは、[公共交通運賃の定義例](#define-a-transit-fare)にある通常運賃チケット商品のいずれか1つに対応します。

[**fare_leg_rules.txt**](../../../reference/#fare_leg_rulestxt)

|  leg_group_id |  network_id | fare_product_id  |
|---|---|---|
| core_local_one_way_trip | core  |  core_local_oneway_fare |
| core_local_one_way_trip | core  |  core_local_1_day_fare |
| core_local_one_way_trip | core  |  core_local_31_day_fare |
| core_local_one_way_trip | core  |  core_local_7_day_fare |

<sup>[Maryland Transit Administration のローカルバス GTFS フィードをダウンロードする](https://feeds.mta.maryland.gov/gtfs/local-bus)</sup>

<hr>

### 乗り換えルールを作成する {: #create-rules-for-transfers}


BaltimoreLink のローカルバス、Metro SubwayLink、または Light RailLink を利用するために片道運賃を購入した乗客には、90 分間の乗り換えが適用されます。これは、90 分の時間枠内でローカルバス、地下鉄、ライトレール間を無制限に乗り換えることができることを意味します。

[**fare_transfer_rules.txt**](../../../reference/#fare_transfer_rulestxt)

| from_leg_group_id       | to_leg_group_id  | duration_limit | duration_limit_type | fare_transfer_type | transfer_count |
|-------------------------|---|----------------|-------------------|---------------------|----------------|
| core_local_one_way_trip | core_local_one_way_trip  | 5400           | 1                 | 0                   | -1             |


上記のファイルは、以下のフィールドを使用して GTFS でこれを表現しています。

- 片道便(`core_local_one_way_trip`)である乗車区間(leg)との間で乗り換えが可能です
- 許可される乗り換え回数に制限がないため、`transfer_count` は `-1` に設定されます
- `duration_limit` は 90 分に相当する `5400` 秒に設定されます
- 乗り換え時間は `core_local_one_way_trip` 運賃乗車区間(fare leg)内の任意のルート・路線系統(route)で乗客が出発した時点から開始し、別の運賃乗車区間(fare leg)で出発した時点で終了するため、`duration_limit_type` は `1` に設定されます。 
- 乗客は最初の運賃のみを支払うため、`fare_transfer_type` は `0` に設定されます。90 分の時間枠内での乗り換えには、乗り換え料金も 2 回目の運賃もありません。したがって、費用は最初の運賃と乗り換え料金の合計としてモデル化できます。
- 乗客は 90 分の `duration_limit` 時間枠内で無制限に乗り換えることができるため、`transfer_count` は `-1` に設定されます。

運賃を定義し、適切な `fare_leg_rule` を作成し、`fare_transfer_rule` を定義すると、旅程検索ツールに $2.00 USD の `core_local_oneway_fare` が表示されます。以下は Transit の例です。

<div class="flex-photos">
    <img src="../../../../../assets/transit-fares-mdot.png" alt="$2 USD の運賃">
</div>

<sup>[Maryland Transit Administration ローカルバス GTFS フィードをダウンロードする](https://feeds.mta.maryland.gov/gtfs/local-bus)</sup>

### 同一運賃ゾーン内のサービス地点を記述する {: #describe-service-locations-in-the-same-fare-zone}


一部の交通事業者は、ゾーンベースの運賃体系を運用しています。運賃ゾーンは、異なる運賃価格に関連付けられた地理的に区分されたエリアです。Bay Area の BART システムでは、運賃は出発地と目的地によって異なり<a href="https://www.bart.gov/sites/default/files/docs/BART%20Clipper%20Fares%20Triangle%20Chart%20July%202022.pdf" target="_blank">（BART の運賃差）</a>、交通機関の乗客は正しい運賃を知る必要があります。運賃エリアは、[stops.txt](../../../reference/#stopstxt) の停留所等(stop)を [areas.txt](../../../reference/#areastxt) に割り当てる [stops_areas.txt](../../../reference/#stop_areastxt) ファイルを使用して記述できます。

まず、[areas.txt](../../../reference/#areastxt) 内のエリアを特定します。エリア名がない場合は、`area_name` を空欄のままにすることができます。以下の表には、`ASHB`、`GLEN`、`OAKL` の3つの `area_id` があります。

[**areas.txt**](../../../reference/#areastxt) 

| area_id | area_name |
|---------|-----------|
| ASHB    |           |
| GLEN    |           | 
| OAKL    |           | 

その後、[stops.txt](../../../reference/#stopstxt) ファイルの `stop_id` を使用して、停留所等(stop)をそれぞれ特定されたエリア（運賃ゾーン）にグループ化します。 

次に、`stop_id` を各 `area_id` にグループ化します。BART の例では、各エリアには 1つの `stop_id` のみが含まれます。たとえば、エリア `ASHB` には停留所等(stop) `ASHB`（Ashby Station）のみが含まれます。ただし、エリアに複数の停留所等(stop)が含まれる場合は、複数の `stop_id` を記載するべきです。

[**stops_areas.txt**](../../../reference/#stop_areastxt)

| area_id | stop_id |
|---------|---------|
| ASHB    | ASHB    |
| GLEN    | GLEN    | 
| OAKL    | OAKL    | 

`fare_leg_rules.txt` では、異なる出発エリアおよび到着エリアに基づいて、異なるチケット商品を特定できます。たとえば、最初のエントリは次を示しています。

* 出発エリアは `ASHB` です
* 到着エリアは `GLEN` です
* 出発／到着エリアのチケット商品は `BA:matrix:ASHB-GLEN` です

[**fare_leg_rules.txt**](../../../reference/#fare_leg_rulestxt)

| leg_group_id | from_area_id|to_area_id|fare_product_id|
|--------------|-----------|------------|---------------|
|   BA    |  ASHB   | GLEN | BA:matrix:ASHB-GLEN |
|     BA         |  ASKB   | OAKL | BA:matrix:ASHB-OAKL |

運賃は `fare_products.txt` で特定されます。 

[**fare_products.txt**](../../../reference/#fare_productstxt)

| fare_product_id     | fare_product_name| amount | currency |
|---------------------|-----------|--------|----------|
| BA:matrix:ASHB-GLEN |  generated  | 4.75   | USD      |
| BA:matrix:ASHB-OAKL  |  generated  | 9.45   | USD       |


<sup><a href="https://511.org/open-data/transit" target="_blank">San Francisco Bay Area Regional feed を参照してください</a></sup>

<hr>

### 受け入れられるチケットメディアを説明する {: #describe-what-fare-media-is-accepted}


San Francisco Muni の乗客は、便(trip)の運賃を支払い、運賃を検証するために、複数の異なる種類のチケットメディアを使用することができます。

- Bay Area の交通カードである <a href="https://www.clippercard.com/ClipperWeb/" target="_blank">Clipper card</a> を使用します
- <a href="https://www.sfmta.com/getting-around/muni/fares/munimobile" target="_blank">Munimobile app</a> を使用します
- 現金で運賃を支払います

これらの検証方法は、GTFS Fares (v2) では `fare_media` と呼ばれ、`fare_media.txt` を使用して記述できます。

以下は、511 SF Bay API でアクセスできる <a href="https://511.org/open-data/transit" target="_blank">San Francisco Bay Area Regional Feed</a> の例の抜粋です。

`Clipper` は、`fare_media_type=2` を持つ物理的な交通カードとして記述されています。`SFMTA Munimobile` は、`fare_media_type=2` を持つモバイルアプリとして記述されています。`Cash` は、チケットを介さずに運転手へ直接渡されるため、チケットメディアを持ちません。その結果、`Cash` は `fare_media_type=0` です。

[**fare_media.txt**](../../../reference/#fare_mediatxt)

| fare_media_id | fare_media_name  | fare_media_type |
|---------------|------------------|-----------------|
| clipper       | Clipper          | 2               |
| munimobile    | SFMTA MuniMobile | 4               |
| cash           | Cash             | 0               |

<sup><a href="https://511.org/open-data/transit" target="_blank">San Francisco Bay Area Regional feed を参照してください</a></sup>

さらに、物理的なチケットをチケットメディアとして記述したいプロデューサーは、`fare_media_type=1` を使用することができます。

<a href="https://www.mbta.com" target="_blank">Massachusetts Bay Transportation Authority (MBTA)</a> では、ユーザーは CharlieTicket と呼ばれる物理的な紙のチケットを使用して、便(trip)およびパスの料金を支払うことができます。これを反映するため、MBTA の feed には、`fare_media_type=1` を持つ `charlieticket` チケットメディアがあります。

[**fare_media.txt**](../../../reference/#fare_mediatxt)

| fare_media_id | fare_media_name  | fare_media_type |
|---------------|------------------|-----------------|
|cash           |Cash              |0                |
|charlieticket  |CharlieTicket     |1                |
|mticket        |m Ticket app      |4                |

<sup><a href="https://www.mbta.com/developers/gtfs" target="_blank">Massachusetts Bay Transportation Authority feed を参照してください</a></sup>

### チケットメディアに基づく運賃差額の定義 {: #define-price-differences-based-on-fare-media}


Muniの運賃価格は、乗客が使用するチケットメディアによって異なります。この例では、現金またはClipper cardを使用した場合に成人ローカル運賃価格がどのように変わるかを扱います。現金で支払う成人ローカル運賃は3 USDであり、Clipper cardで支払う同じ運賃は2.50 USDで、50セント安くなります。

以下の各エントリは、チケットメディアを説明しています。

[**fare_media.txt**](../../../reference/#fare_mediatxt)

| fare_media_id | fare_media_name  | fare_media_type |
|---------------|------------------|-----------------|
| clipper       | Clipper          | 2               |
| cash           | Cash             | 0               |

以下の`fare_products.txt`ファイルの抜粋は、乗客が使用するチケットメディアに応じて、`Muni single local fare`商品のamountがどのように異なるかを示しています。

[**fare_products.txt**](../../../reference/#fare_productstxt)

| fare_product_id | fare_product_name  | amount | currency | fare_media_id |
|---------------|------------------|-------|--- |---------------|
| SF:local:single | Muni single local fare | 3     | USD | cash |
| SF:local:single | Muni single local fare  | 2.5   |USD | clipper |

Apple Mapsでは、乗客は自身の運賃価格がどのように変わるかを確認できます。「Board the Muni J Church train」の案内の下で運賃価格を比較できます。

<div class="flex-photos">
    <img src="../../../../../assets/apple-muni-cash.jpg" alt="3 USDの現金運賃">
    <img src="../../../../../assets/apple-muni-clipper.jpg" alt="2.50 USDのClipper card運賃">
</div>

<sup><a href="https://511.org/open-data/transit" target="_blank">San Francisco Bay Area Regional feedを見る</a></sup>

### 非接触型チケットメディアの選択肢を記述する {: #describe-a-contactless-fare-media-option}


<a href="https://vimeo.com/539436401" target="_blank">Northern Santa Barbara County の Clean Air Express は非接触型決済を受け付けています</a>。クレジットカード、Google Pay、Apple Pay に対応しています。

Clean Air Express のフィードには、cEMV（contactless Europay, Mastercard and Visa）の選択肢であるため、`fare_media_type=3` の `tap_to_ride` チケットメディアがあります。

| fare_media_id | fare_media_name | fare_media_type |
|---------------|-----------------|-----------------|
| tap_to_ride   | Tap to Ride   | 3  |

以下に示す片道乗車チケット商品には、`cash` と `tap-to-ride` の両方のチケットメディアの選択肢があります。片道乗車を `tap-to-ride` チケットメディアで支払う場合、1 USD ドル安くなります。

[**fare_products.txt**](../../../reference/#fare_productstxt)

| fare_product_id | fare_product_name  | fare_media_id | amount | currency |
|---------------|------------------|---------------|--------|----------|
| single-ride | Single Ride | tap_to_ride       | 6      | USD      |
| single-ride | Single Ride |       | 7      | USD      |

<sup><a href="https://gtfs.calitp.org/production/CleanAirExpressFaresv2.zip" target="_blank">Clean Air Express のフィードをダウンロードする</a></sup>

### 便(trip)の時刻および曜日に基づく運賃差の定義 {: #define-price-differences-based-on-time-and-day-of-trip}


一部の交通事業者は、時刻および/または曜日に基づいて運賃を変動させています。これは、運賃が、ピーク時間帯、オフピーク時間帯、週末など、便(trip)が利用される時間帯に関連付けられることを意味します。 

Washington DC の Metrorail の運賃は、便(trip)の曜日および時刻を含む複数の要因に基づいて変動します。GTFS における時刻変動運賃は `timeframes.txt` を使用して定義できます。このファイルでは、特定の時間帯を指定でき、その後 `fare_leg_rules.txt` で関連付けることで、便(trip)が利用される時刻に対応する適用可能なチケット商品を割り当てることができます。以下は、2023年春時点の WMATA の運賃に基づく架空の例です。 

まず、運行日(service day)を `calendar.txt` を使用して定義します。

[**calendar.txt**](../../../reference/#calendartxt)

| service_id       | monday | tuesday | wednesday | thursday | friday | saturday | sunday | start_date | end_date |
|------------------|--------|---------|-----------|----------|--------|----------|--------|------------|----------|
| weekday_service  | 1      | 1       | 1         | 1        | 1      | 0        | 0      | 20220708   | 20221231 |
| saturday_service | 0      | 0       | 0         | 0        | 0      | 1        | 0      | 20220708   | 20221231 |
| sunday_service   | 0      | 0       | 0         | 0        | 0      | 0        | 1      | 20220708   | 20221231 |


その後、必要な時間枠を `timeframes.txt` で定義します。ID、`calendar.service_id` への参照による適用日、および該当する場合は各時間帯の開始時刻と終了時刻を指定します。

[**timeframes.txt**](../../../reference/#timeframestxt)

| timeframe_group_id | start_time | end_time | service_id       |
|--------------------|------------|----------|------------------|
| weekday_peak       | 5:00:00    | 9:30:00  | weekday_service  |
| weekday_offpeak    | 9:30:00    | 15:00:00 | weekday_service  |
| weekday_peak       | 15:00:00   | 19:00:00 | weekday_service  |
| weekday_offpeak    | 19:00:00   | 21:30:00 | weekday_service  |
| weekday_late_night | 21:30:00   | 24:00:00 | weekday_service  |
| weekday_late_night | 00:00:00   | 5:00:00  | weekday_service  |
| weekend            |            |          | saturday_service |
| weekend            |            |          | sunday_service   |

次に、対応する時刻固有の運賃を `fare_products.txt` に作成します（例: ピーク運賃）。

[**fare_products.txt**](../../../reference/#fare_productstxt)

| fare_product_id | fare_product_name                             | amount | currency |
|-----------------|-----------------------------------------------|--------|----------|
| peak_fare       | ピーク運賃                                    | 5      | USD      |
| regular_fare    | オフピーク運賃                                | 3      | USD      |
| weekend_fare    | 週末の Metrorail 片道運賃                     | 2      | USD      |
| late_night_fare | 深夜一律運賃（月曜～金曜の午後9時30分以降）   | 2      | USD      |

最後に、`fare_leg_rules.txt` において、`from_timeframe_group_id` および `to_timeframe_group_id` フィールドを使用して、時間枠をチケット商品に関連付けます。これらのフィールドは、運賃が乗車区間(leg)の開始時刻のみに適用されるか、乗車区間(leg)の開始時刻と終了時刻の両方に適用されるかを決定します。
この例では、WMATA の運賃に基づき、運賃は乗車区間(leg)の出発時間枠のみに依存するため、`to_timeframe_group_id` は空欄のままにします。 

[**fare_leg_rules.txt**](../../../reference/#fare_leg_rulestxt)

| network_id | fare_product_id | from_timeframe_group_id | to_timeframe_group_id |
|------------|-----------------|-------------------------|-----------------------|
| 1          | weekend_fare    | weekend                 |                       |
| 1          | late_night_fare | weekday_late_night      |                       |
| 1          | peak_fare       | weekday_peak            |                       |
| 1          | regular_fare    | weekday_offpeak         |                       |

`network_id` は外部 ID である `networks.network_id` または `routes.network_id` を参照すること、および各便(trip)に対する正しいチケット商品の選択は、`stop_times.txt` の到着時刻および出発時刻と `timeframes.txt` で定義された時刻の組み合わせによって行われることに注意してください。 

この場合、午前7時30分に出発する便(trip)の料金を支払う利用者は 5.00 USD（ピーク運賃）を支払わなければなりませんが、午前11時30分に出発する別の利用者は 3.00 USD の運賃（オフピーク運賃）のみを支払えばよいです。

### ゾーンベース運賃と併せて時間変動運賃を定義する {: #define-time-variable-fares-along-with-zone-based-fares}


ニューヨークのMTA Metro-North鉄道ネットワークでは、運賃は便(trip)の時間帯と、便の出発地および到着地のエリアの両方に基づいて変動します。以下の例は、Grand Central StationからCold Spring（ニューヨーク州、米国）への便に適用される運賃ルールを示しています。

この例は、6つの異なるエリアに分布する10の停留所等(stop)を利用する便を特徴とする、<a href="https://www.itoworld.com/" target="_blank">ITO World</a>が作成した<a href="https://docs.google.com/spreadsheets/d/1-cD-R2OH5xAQAbNWNlrXD7WOw594lVdW-bomuLo6bI8/edit?usp=sharing" target="_blank">データセット</a>に基づいています。

[**stops.txt**](../../../reference/#stopstxt)

| stop_id | stop_name           | stop_lat  | stop_lon   |
|---------|---------------------|-----------|------------|
| ITO1669 | Peekskill           | 41.285103 | -73.930916 |
| ITO1777 | Beacon              | 41.505814 | -73.984474 |
| ITO1789 | New Hamburg         |  41.58691 | -73.947624 |
| ITO1804 | Croton-Harmon       | 41.190002 | -73.882393 |
| ITO1824 | Cortlandt           | 41.246258 | -73.921783 |
| ITO1856 | Garrison            | 41.381126 | -73.947334 |
| ITO1887 | Harlem-125th Street | 40.805256 | -73.939148 |
| ITO1897 | Cold Spring         | 41.415382 | -73.958092 |
| ITO2096 | Poughkeepsie        | 41.707058 |  -73.93792 |
| ITO2383 | Grand Central       | 40.752823 | -73.977196 |


[**stop_areas.txt**](../../../reference/#stop_areastxt)

| area_id   | stop_id |
|-----------|---------|
| mnr_1     | ITO1887 |
| mnr_1     | ITO2383 |
| mnr_HUD-5 | ITO1804 |
| mnr_HUD-6 | ITO1669 |
| mnr_HUD-6 | ITO1824 |
| mnr_HUD-7 | ITO1856 |
| mnr_HUD-7 | ITO1897 |
| mnr_HUD-8 | ITO1777 |
| mnr_HUD-8 | ITO1789 |
| mnr_HUD-9 | ITO2096 |


[**route_networks.txt**](../../../reference/#route_networkstxt)

| network_id | route_id |
|------------|----------|
| mnr_hudson | 669      |


[**networks.txt**](../../../reference/#networkstxt)

| network_id | network_name    |
|------------|-----------------|
| mnr_hudson | MNR Hudson Line |

列車サービス3および13の運行日は、`calendar.txt`を使用して定義されています。特に、どの便にも関連付けられていない汎用的な日（すなわち、平日、週末、および毎日）のその他のレコードが定義されており、これらは`time-variable fares`をモデル化するためにtimeframesに関連付けられます。

[**calendar.txt**](../../../reference/#calendartxt)

| service_id | monday | tuesday | wednesday | thursday | friday | saturday | sunday | start_date | end_date |
|------------|--------|---------|-----------|----------|--------|----------|--------|------------|----------|
| 13         | 1      | 1       | 1         | 1        | 1      | 0        | 0      | 20230612   | 20231006 |
| 3          | 1      | 1       | 1         | 1        | 1      | 0        | 0      | 20230609   | 20231006 |
| weekdays   | 1      | 1       | 1         | 1        | 1      | 0        | 0      | 20220101   | 20240101 |
| weekends   | 0      | 0       | 0         | 0        | 0      | 1        | 1      | 20220101   | 20240101 |
| anyday     | 1      | 1       | 1         | 1        | 1      | 1        | 1      | 20220101   | 20240101 |


`timeframes.txt`には、時刻が24時間の範囲をカバーするケース（`anytime`、`weekdays`、および`weekends`）と、ピーク時間帯およびオフピーク時間帯を含むレコードが作成されます。

* AM Peak: 平日の午前6時から午前10時まで
* AM2PM Peak: 平日の午前6時から午前9時まで、および午後4時から午後8時まで
* Not AM Peak: AM Peakに含まれない平日の時間
* Not AM2PM Peak: AM2PM Peakに含まれない平日の時間

[**timeframes.txt**](../../../reference/#timeframestxt)

| timeframe_group_id | start_time | end_time | service_id |
|:------------------:|:----------:|:--------:|:----------:|
|       anytime      |  00:00:00  | 24:00:00 |   anyday   |
|      weekdays      |  00:00:00  | 24:00:00 |  weekdays  |
|      weekends      |  00:00:00  | 24:00:00 |  weekends  |
|     mnr_ampeak     |  06:00:00  | 10:00:00 |  weekdays  |
|    mnr_notampeak   |  00:00:00  | 06:00:00 |  weekdays  |
|    mnr_notampeak   |  10:00:00  | 24:00:00 |  weekdays  |
|    mnr_am2pmpeak   |  06:00:00  | 09:00:00 |  weekdays  |
|    mnr_am2pmpeak   |  16:00:00  | 20:00:00 |  weekdays  |
|  mnr_notam2pmpeak  |  00:00:00  | 06:00:00 |  weekdays  |
| mnr_notam2pmpeak   | 09:00:00   | 16:00:00 | weekdays   |
| mnr_notam2pmpeak   | 20:00:00   | 24:00:00 | weekdays   |


個々の各チケット商品は`fare_products.txt`で定義されます。Cold Springはゾーン7に位置するため、この例ではゾーン1と7の間の便のみを一覧にしています。完全なデータセットには、時間とゾーンの組み合わせによって定義される各価格のレコードが含まれます。さらに、この例では1つのチケットメディア（`paper`）のみを表示していますが、価格がチケットメディアによっても変動する場合は、追加の組み合わせを作成できます。

[**fare_products.txt**](../../../reference/#fare_productstxt)

| fare_product_id        | fare_product_name                  | fare_media_id | amount | currency |
|------------------------|------------------------------------|---------------|--------|----------|
| mnr_1:HUD-7_adult_peak | Outbound Adult Peak Zonal Fare     | paper         | 20.00  | USD      |
| mnr_1:HUD-7_adult      | Outbound Adult Off Peak Zonal Fare | paper         | 15.00  | USD      |
| mnr_HUD-7:1_adult_peak | Inbound Adult Peak Zonal Fare      | paper         | 20.00  | USD      |
| mnr_HUD-7:1_adult      | Inbound Adult Off Peak Zonal Fare  | paper         | 15.00  | USD      |


最後に、出発地および到着地のエリアの組み合わせと、それぞれのtimeframesが、`fare_leg_rules.txt`内の対応するチケット商品に関連付けられます。ここでは、ピーク時間帯にゾーン1（すなわち`area_id=mnr_1`）で出発または到着する便には、便の到着および出発ゾーンに対応する特定のピーク運賃（すなわち`fare_product_id=mnr_1:HUD-7_adult_peak`）が適用されます。

[**fare_leg_rules.txt**](../../../reference/#fare_leg_rulestxt)

| network_id | from_area_id | to_area_id | fare_product_id        | from_timeframe_group_id | to_timeframe_group_id |
|------------|--------------|------------|------------------------|-------------------------|-----------------------|
| mnr_hudson | mnr_1        | mnr_HUD-7  | mnr_1:HUD-7_adult      | mnr_notam2pmpeak        | anytime               |
| mnr_hudson | mnr_1        | mnr_HUD-7  | mnr_1:HUD-7_adult      | weekends                | anytime               |
| mnr_hudson | mnr_1        | mnr_HUD-7  | mnr_1:HUD-7_adult_peak | mnr_am2pmpeak           | anytime               |
| mnr_hudson | mnr_HUD-7    | mnr_1      | mnr_HUD-7:1_adult      | weekdays                | mnr_notampeak         |
| mnr_hudson | mnr_HUD-7    | mnr_1      | mnr_HUD-7:1_adult      | weekends                | anytime               |
| mnr_hudson | mnr_HUD-7    | mnr_1      | mnr_HUD-7:1_adult_peak | weekdays                | mnr_ampeak            |


このデータセットを使用すると、Grand Central（ゾーン`mnr_1`）を午後6時45分に出発予定の列車#869（`service_id=3`）に乗車する利用者は、便が`mnr_am2pmpeak`の時間帯に`zone mnr_1`から出発するため、20.00 USDのOutbound Adult Peak Zonal Fareを支払う必要があります。

一方、列車#883（`service_id=13`）を利用する利用者は、この列車がGrand Central（ゾーン`mnr_1`）を午後9時4分に出発予定であるため、15.00 USDのOutbound Adult Off Peak Zonal Fareのみを支払います。

<a href="https://apple.com/maps" target="_blank">Apple Maps</a>では、乗客は運賃価格がどのように変動するかを確認し、列車の出発予定時刻の横で運賃価格を比較できます。

<div class="flex-photos">
    <img src="../../../../../assets/TimeVariableFares-Peak.png" alt="20.00 USDのOutbound Adult Peak Zonal Fare">
    <img src="../../../../../assets/TimeVariableFares-OffPeak.png" alt="15.00 USDのOutbound Adult Off Peak Zonal Fare">
</div>
