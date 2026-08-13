# デマンド型サービス {: #demand-responsive-services}


GTFS Flex は GTFS 拡張プロジェクトであり、2024年3月に GTFS 仕様へ正式に採用されました。その目的は、デマンド型交通（DRT）サービスの発見可能性を促進することです。
デマンド型サービスには、世界の地域によって異なる用語が存在することに注意してください。詳細は[用語集](#glossary)を参照してください。

以下の例では、Flex を使用してさまざまなデマンド型サービスのユースケースをモデル化する方法を示します。**以下の例は、必ずしも事業者のサービスを正確または完全に表現したものではないことに注意してください。**

## 単一ゾーン内のオンデマンドサービス {: #on-demand-services-within-a-single-zone}


デマンド型サービスは特定のゾーン内で運行でき、乗客はゾーン内の任意の地点Aでの乗車と、同じゾーン内の任意の地点Bでの降車を予約できます。この例として、米国ミネソタ州の[Heartland Express Transit](https://www.co.brown.mn.us/heartland-express-transit?view=category&id=56)サービスがあります。

<sup>[Heartland Express のデータセット例をダウンロード](../../../assets/on-demand-services-within-a-single-zone.zip)</sup>

### 便(trips)の定義 {: #define-trips}


Heartland Express の運行時間は以下の通りです。

- 平日:
  - 午前8:00 - 午後5:00
  - 午前6:15 – 午後5:45（New Ulm zone のみ）
- 日曜日: 午前8:00 - 正午（New Ulm zone のみ）

New Ulm city zone は Brown County zone 内に含まれています。["zone overlap constraint"](#zone-overlap-constraint) の問題を回避するため、Heartland Express は4つの便(trip)で定義することができます。

- 平日の午前6:15から午前8:00までの New Ulm zone 内のサービス。
- 平日の午前8:00から午後5:00までの郡全域のサービス。
- 平日の午後5:00から午後5:45までの New Ulm zone 内のサービス。
- 日曜日の午前8:00から午後12:00までの New Ulm zone 内のサービス。
  
[**trips.txt**](../../reference/#tripstxt)

route_id | service_id | trip_id
-- | -- | -- 
74362 | c_67295_b_77497_d_31 | t_5374945_b_77497_tn_0
74362 | c_67295_b_77497_d_31 | t_5374946_b_77497_tn_0
74362 | c_67295_b_77497_d_31 | t_5374944_b_77497_tn_0
74362 | c_67295_b_77497_d_64 | t_5374947_b_77497_tn_0

`service_id = c_67295_b_77497_d_31` は平日を指し、`service_id = c_67295_b_77497_d_64` は日曜日を指します。

### ゾーンの定義（GeoJSON locations） {: #define-zone-geojson-locations}


Heartland Express サービスの運行ゾーンを定義するために [locations.geojson](../../reference/#locationsgeojson) を使用する場合、Brown County と New Ulm City にはそれぞれ別個のゾーンを定義しなければなりません。以下は、Brown County のゾーンを定義する簡略化された GeoJSON です。
```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "id": "area_708",
      "type": "Feature",
      "geometry": {
        "type": "Polygon",
        # 簡略化のため、ここでは 3 つの座標のみを示しています。
        "coordinates": [
          [
            [
              -94.7805702,
              44.4560958
            ],
            [
              -94.7805608,
              44.4559928
            ],
            [
              -94.7805218,
              44.4559649
            ]
          ]
        ]
      },
      "properties": {}
    }
  ]
```

### 予約ルールを定義する {: #define-booking-rules}


以下は、すべてのHeartland Expressサービスに適用される予約ルールです。

- 乗車リクエストは、平日の午前8時から午後3時までに行うべきです。 
- 乗車は、乗車日の1営業日前までにリクエストしなければなりません。 
- 乗車リクエストは、最大14日前から行うことができます。

`booking_type = 2` を使用すると、サービスに前日までの予約が必要であることを示します。`prior_notice_last_day = 1` および `prior_notice_start_day = 14` は、サービスを14日前から前日まで予約できることを示します。

[**booking_rules.txt**](../../reference/#booking_rulestxt)

booking_rule_id | booking_type | prior_notice_start_day | prior_notice_start_time | prior_notice_last_day | prior_notice_last_time | message | phone_number | info_url
-- | -- | -- | -- | -- | -- | -- | -- | --
booking_route_74362 | 2 | 14 | 8:00:00 | 1 | 15:00:00 | Brown County Heartland Expressは、ドアツードアのオンデマンド交通を提供しています。乗車をリクエストするには、旅行の少なくとも1営業日前の午後3時までに、1-507-359-2717または1-800-707-2717へお電話ください。 | (507) 359-2717 | https://www.co.brown.mn.us/heartland-express-transit

### 停車時刻(stop_times)を定義する {: #define-stop-times}


運行時間は、`start_pickup_drop_off_window` および `end_pickup_drop_off_window` フィールドを使用して定義されます。同一ゾーン内の移動には、同じ `location_id` を持つ stop_times.txt 内の2つのレコードが必要です。

- `pickup_type = 2` および `drop_off_type = 1` を持つ最初のレコードは、そのゾーンで予約乗車が許可されていることを示します。
- `pickup_type = 1` および `drop_off_type = 2` を持つ2番目のレコードは、そのゾーンで予約降車が許可されていることを示します。
 
[**stop_times.txt**](../../reference/#stop_timestxt)

trip_id | location_id | stop_sequence | start_pickup_drop_off_window | end_pickup_drop_off_window | pickup_type | drop_off_type | pickup_booking_rule_id | drop_off_booking_rule_id
-- | -- | -- | -- | -- | -- | -- | -- | --
t_5374944_b_77497_tn_0 | area_715 | 1 | 06:15:00 | 08:00:00 | 2 | 1 | booking_route_74362 | booking_route_74362
t_5374944_b_77497_tn_0 | area_715 | 2 | 06:15:00 | 08:00:00 | 1 | 2 | booking_route_74362 | booking_route_74362
t_5374945_b_77497_tn_0 | area_708 | 1 | 08:00:00 | 17:00:00 | 2 | 1 | booking_route_74362 | booking_route_74362
t_5374945_b_77497_tn_0 | area_708 | 2 | 08:00:00 | 17:00:00 | 1 | 2 | booking_route_74362 | booking_route_74362
t_5374946_b_77497_tn_0 | area_715 | 1 | 17:00:00 | 17:45:00 | 2 | 1 | booking_route_74362 | booking_route_74362
t_5374946_b_77497_tn_0 | area_715 | 2 | 17:00:00 | 17:45:00 | 1 | 2 | booking_route_74362 | booking_route_74362
t_5374947_b_77497_tn_0 | area_715 | 1 | 08:00:00 | 12:00:00 | 2 | 1 | booking_route_74362 | booking_route_74362
t_5374947_b_77497_tn_0 | area_715 | 2 | 08:00:00 | 12:45:00 | 1 | 2 | booking_route_74362 | booking_route_74362

`area_715` は New Ulm City ゾーンを、`area_708` は Brown County ゾーンを指します。

## 複数ゾーンにまたがるオンデマンドサービス {: #on-demand-services-across-multiple-zones}


一部のデマンド型サービスは、複数の異なるゾーンにまたがって運行されており、乗客はあるエリア内の任意の場所Aでの乗車と、別のエリア内の任意の場所での降車を予約できます。例えば、[Minnesota River Valley Transit](https://www.saintpetermn.gov/330/Dial-a-Ride) は、Saint Peter市とKasota市の間でオンデマンドサービスを提供しています。

<sup>[River Valley Transitのデータセット例をダウンロード](../../../assets/on-demand-services-between-multiple-zones-river-valley.zip)</sup>

### 便(trips)を定義する {: #define-trips}


前の例と同様に、運行時間は曜日によって異なるため、平日と土曜日について便(trips)を別々に定義する必要があります。 

[**trips.txt**](../../reference/#tripstxt)

route_id | service_id | trip_id 
-- | -- | -- 
74375 | weekdays | t_5298036_b_77503_tn_0 
74375 | saturdays | t_5298041_b_77503_tn_0 

（前の例と同じ方法で、[booking_rules.txt](../../reference/#booking_rulestxt) および [locations.geojson](../../reference/#locationsgeojson) を使用して予約ルールとゾーンを定義します）

### 停車時刻(stop_time)を定義する {: #define-stop-times}


以下のデータは、乗車は一方のゾーンでのみ許可され、降車は別のゾーンでのみ許可されることを示しています。同じゾーンでの乗車および降車は許可されません。

[**stop_times.txt**](../../reference/#stop_timestxt)

trip_id | location_id | stop_sequence | start_pickup_drop_off_window | end_pickup_drop_off_window | pickup_type | drop_off_type | pickup_booking_rule_id | drop_off_booking_rule_id
-- | -- | -- | -- | -- | -- | -- | -- | --
t_5298036_b_77503_tn_0 | area_713 | 1 | 06:30:00 | 20:00:00 | 2 | 1 | booking_route_74375 | booking_route_74375
t_5298036_b_77503_tn_0 | area_714 | 2 | 06:30:00 | 20:00:00 | 1 | 2 | booking_route_74375 | booking_route_74375
t_5298041_b_77503_tn_0 | area_713 | 1 | 09:00:00 | 19:00:00 | 2 | 1 | booking_route_74375 | booking_route_74375
t_5298041_b_77503_tn_0 | area_714 | 2 | 09:00:00 | 19:00:00 | 1 | 2 | booking_route_74375 | booking_route_74375

## 乗客が特定の場所で乗車・降車しなければならないデマンド型サービス {: #on-demand-services-where-riders-must-be-picked-up-and-dropped-off-at-specific-locations}


一部のデマンド型サービスでは、乗客はゾーン内の任意の場所での乗車および降車を指定できません。代わりに、乗客は特定の指定された停留所等(stop)（集合地点／仮想停留所）で乗車および降車するようにのみ予約できます。この例として、ドイツのAngermündeおよびGartzにおける[RufBus service](https://uvg-online.com/rufbus-angermuende/)があります。

### 便(trip)の定義 {: #define-trips}


ルート・路線系統(route) 476 は、Angermünde 地域の各停留所等(stop)間でオンデマンドサービスを提供しています。平日用と週末用の2つのサービスを運行しており、それぞれに1つの trip_id が関連付けられています。 

[**trips.txt**](../../reference/#tripstxt)

route_id | service_id | trip_id 
-- | -- | -- 
476 | on_demand_weekdays | 476_weekdays 
476 | on_demand_weekends | 476_weekends

### 位置グループを定義する {: #define-location-groups}


乗客は各停留所等(stop)間のサービスを予約できるため、stop_times.txt ですべての停留所等(stop)間の組み合わせを定義することを避けるには、location_groups.txt および location_group_stops.txt を使用して、これらの停留所等(stop)を位置グループとして定義することが適切です。

[**location_groups.txt**](../../reference/#location_groupstxt)

location_group_id | location_group_name 
-- | -- 
476_stops | durch den RufBus 476 bedientes Gebiet im Raum Angermünde

[**location_group_stops.txt**](../../reference/#location_group_stopstxt)

location_group_id | stop_id 
-- | -- 
476_stops | de:12073:900340004::1
476_stops | de:12073:900340004::2
476_stops | de:12073:900340004::3
476_stops | de:12073:900340004::4
476_stops | de:12073:900340100::1
476_stops | de:12073:900340100::2
476_stops | ...

### 予約ルールの定義 {: #define-booking-rules}


476 route serviceでは、少なくとも1時間前までの予約が必要です。`booking_type = 1` を使用すると、このサービスでは事前通知を伴う当日予約までが必要であることを示します。`prior_notice_duration_min = 60` は、少なくとも60分前までの予約が必要であることを示します。 

平日と週末の予約にはわずかな違いがあるため、平日サービスと休日サービスに対して別々の予約ルールを定義できます。詳細は `message` フィールドで提供できます。情報ページおよび予約ページへのリンクは、`info_url` フィールドおよび `booking_url` フィールドで提供できます。

[**booking_rules.txt**](../../reference/#booking_rulestxt)

booking_rule_id | booking_type | prior_notice_duration_min | message | phone_number | info_url | booking_url
-- | -- | -- | -- | -- | -- | --
flächenrufbus_angermünde_weekdays | 1 | 60 | Anmeldung mind. 60min vorher erforderlich, per Anruf zwischen 08:00 und 24:00 möglich, oder online rund um die Uhr | +49 3332 442 755 | https://uvg-online.com/rufbus-angermuende/ | https://uvg.tdimo.net/bapp/#/astBuchungenView
flächenrufbus_angermünde_weekends | 1 | 60 | 1€ Komfortzuschlag pro Person; Anmeldung mind. 60min vorher erforderlich, per Anruf zwischen 08:00 und 24:00 möglich, oder online rund um die Uhr | +49 3332 442 755 | https://uvg-online.com/rufbus-angermuende/ | https://uvg.tdimo.net/bapp/#/astBuchungenView

### 停車時刻(stop_time)を定義する {: #define-stop-times}


476 routeは、平日は午後5時30分から午後10時まで、週末は午前8時から午後10時まで運行します。運行時間は、`start_pickup_drop_off_window`および`end_pickup_drop_off_window`フィールドを使用して定義されます。同じlocation group内の移動には、同じ`location_group_id`を持つstop_times.txt内の2つのレコードが必要です。

  - `pickup_type = 2`および`drop_off_type = 1`を持つ最初のレコードは、そのlocation groupで予約乗車が許可されていることを示します。
  - `pickup_type = 1`および`drop_off_type = 2`を持つ2番目のレコードは、そのlocation groupで予約降車が許可されていることを示します。

[**stop_times.txt**](../../reference/#stop_timestxt)

trip_id | location_group_id | stop_sequence | start_pickup_drop_off_window | end_pickup_drop_off_window | pickup_type | drop_off_type | pickup_booking_rule_id | drop_off_booking_rule_id
-- | -- | -- | -- | -- | -- | -- | -- | --
476_weekdays | 476_stops | 1 | 17:30:00 | 22:00:00 | 2 | 1 | flächenrufbus_angermünde_weekdays | flächenrufbus_angermünde_weekdays
476_weekdays | 476_stops | 2 | 17:30:00 | 22:00:00 | 1 | 2 | flächenrufbus_angermünde_weekdays | flächenrufbus_angermünde_weekdays
476_weekends | 476_stops | 1 | 08:00:00 | 22:00:00 | 2 | 1 | flächenrufbus-angermünde_weekdays | flächenrufbus_angermünde_weekends
476_weekends | 476_stops | 2 | 08:00:00 | 22:00:00 | 1 | 2 | flächenrufbus-angermünde_weekdays | flächenrufbus-angermünde_weekends

## 迂回ルート {: #deviated-route}


「Route deviation」とは、車両が定められた停留所等(stop)の順序を持つ固定ルートに従って運行しつつ、停留所等(stop)間で乗客を乗車または降車させるためにこのルートから迂回できる柔軟性を持つサービスを指します。通常、サービスの定時性を維持するために迂回は制限されており、迂回しての乗車および降車には事前予約が必要です。 

この例では、New Ulm Cityの[Hermann Express](https://www.newulmmn.gov/553/Hermann-Express-City-Bus-Service)サービスは、利用者が固定停留所等(stop)でのみ乗車し、これらの停留所等(stop)間にある特定の迂回エリア内の任意の地点で降車できるようにしています。

**以下の例は簡略化されています。詳細については、[Hermann Express example dataset](../../../assets/deviated-drop-off-route.zip)をダウンロードしてください。**

### 便(trips)の定義 {: #define-trips}


この種のサービスも一連の固定された停留所等(stop)と固定された時刻表を伴うため、便(trips)の定義は通常の固定ルートのバスサービスと類似しています。関連するすべての運行期間にわたり、各ルート・路線系統(route)が運行する便(trips)を定義する必要があります。

[**trips.txt**](../../reference/#tripstxt)

route_id | service_id | trip_id | share_id
-- | -- | -- | -- 
74513 | c_67295_b_77497_d_31 | t_5374704_b_77497_tn_0 | p_1426044
74513 | c_67295_b_77497_d_31 | t_5374699_b_77497_tn_0 | p_1426044
74513 | c_67295_b_77497_d_31 | t_5374698_b_77497_tn_0 | p_1426044
74513 | c_67295_b_77497_d_31 | t_5374697_b_77497_tn_0 | p_1426044
... | ... | ... | ...

### ゾーンを定義する（GeoJSON location） {: #define-zones-geojson-location}


迂回ルートのゾーンを定義するために、[locations.geojson](../../reference/#locationsgeojson) を使用します。通常、サービスを定刻どおりに運行するため、迂回は制限されます。したがって、車両の走行に伴い、各固定停留所間の迂回エリアはそれに応じて変化する場合があります。ルート迂回のエリアは、以下の画像のようになります。

<div class="flex-photos">
    <img src="../../../../assets/deviated-route-zones.png" alt="迂回ルートのゾーン">
</div>

### 停車時刻(stop_time)を定義する {: #define-stop-times}


固定停留所については、通常のバス路線と同様の方法で、`arrival_time`、`departure_time`、`stop_id` などのフィールドを定義します。固定停留所間では、迂回が許可されるゾーンを定義します。`pickup_type = 1` および `drop_off_type = 3` は、迂回による乗車が許可されないこと（乗車を固定停留所のみに制限すること）、および乗客が迂回ゾーンで降車するために運転手と調整しなければならないことを示します。

[**stop_times.txt**](../../reference/#stop_timestxt)

trip_id | arrival_time | departure_time | stop_id | location_id | stop_sequence | start_pickup_drop_off_window | end_pickup_drop_off_window | pickup_type | drop_off_type | shape_dist_traveled | pickup_booking_rule_id | drop_off_booking_rule_id
-- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | --
t_5374696_b_77497_tn_0 | 08:00:00 | 08:00:00 | 4149546 | | 1 | | | | | 0 | | 
t_5374696_b_77497_tn_0 | | | | radius_300_s_4149546_s_4149547 | 2 | 08:00:00 | 8:02:22 | 1 | 3 | | booking_route_74513 | booking_route_74513
t_5374696_b_77497_tn_0 | 08:02:22 | 08:02:22 | 4149547 | | 3 | | | | | 1221.627114 | | 
t_5374696_b_77497_tn_0 | | | | radius_300_s_4149546_s_4149548 | 4 | 08:02:22 | 8:03:00 | 1 | 3 | | booking_route_74513 | booking_route_74513
t_5374696_b_77497_tn_0 | 08:03:22 | 08:03:22 | 4149548 | | 5 | | | | | 1548.216356 | | 
t_5374696_b_77497_tn_0 | | | | radius_300_s_4149546_s_4149549 | 6 | 08:03:22 | 8:05:00 | 1 | 3 | | booking_route_74513 | booking_route_74513
... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ...
t_5374696_b_77497_tn_0 | 08:50:00 | 08:50:00 | 4210601 | | 35 | | | | | 23429.19558 | | 
t_5374696_b_77497_tn_0 | 08:56:00 | 08:56:00 | 4149564 | | 36 | | | | | 25320.8471 | |

## 経路探索の動作 {: #routing-behavior}

### pickup/drop-off Window を持つ中間 stop_times レコードの無視 {: #ignoring-intermediate-stop-times-records-with-pickupdrop-off-windows}


出発地と目的地の間の経路案内または所要時間を提供する場合、データ利用者は、`start_pickup_drop_off_window` および `end_pickup_drop_off_window` が定義されている中間の stop_times.txt レコードを無視するべきです。例:

trip_id | location_id | stop_sequence | pickup_type | drop_off_type | start_pickup_drop_off_window | end_pickup_drop_off_window
-- | -- | -- | -- | -- | -- | --
tripA | Zone1 | 1 | 2 | 1 | 08:00:00 | 18:00:00
tripA | Zone2 | 2 | 1 | 2 | 08:00:00 | 14:00:00
tripA | Zone3 | 3 | 1 | 2 | 10:00:00 | 18:00:00

データ利用者は、Zone1 から Zone3 への便(trip)の経路案内または所要時間を提供する際に、Zone2 を考慮するべきではありません。

### ゾーン重複制約 {: #zone-overlap-constraint}


同じ `trip_id` を持つ2つ以上の stop_times.txt レコード間で、locations.geojson の `id` ジオメトリ、`start/end_pickup_drop_off_window` 時刻、および `pickup_type` または `drop_off_type` が同時に重複することは禁止されています。

例:
（`northportland` は `portland` 内のゾーンを指します）

**禁止**

[**stop_times.txt**](../../reference/#stop_timestxt)

trip_id | location_id | stop_sequence | pickup_type | drop_off_type | start_pickup_drop_off_window | end_pickup_drop_off_window
-- | -- | -- | -- | -- | -- | --
tripA | portland | 1 | 2 | 1 | 08:00:00 | 12:00:00
tripA | northportland | 2 | 2 | 1 | 10:00:00 | 14:00:00
tripA | vancouver | 3 | 1 | 2 | 10:00:00 | 14:00:00

**許可**

[**stop_times.txt**](../../reference/#stop_timestxt)

trip_id | location_id | stop_sequence | pickup_type | drop_off_type | start_pickup_drop_off_window | end_pickup_drop_off_window
-- | -- | -- | -- | -- | -- | --
tripA | portland | 1 | 2 | 1 | 08:00:00 | 12:00:00
tripA | northportland | 2 | 2 | 1 | 12:00:00 | 14:00:00
tripA | vancouver | 3 | 1 | 2 | 10:00:00 | 14:00:00

または

[**stop_times.txt**](../../reference/#stop_timestxt)

trip_id | location_id | stop_sequence | pickup_type | drop_off_type | start_pickup_drop_off_window | end_pickup_drop_off_window
-- | -- | -- | -- | -- | -- | --
tripA | portland | 1 | 2 | 1 | 08:00:00 | 12:00:00
tripA | northportland | 2 | 1 | 2 | 10:00:00 | 14:00:00
tripA | vancouver | 3 | 1 | 2 | 10:00:00 | 14:00:00

または

[**stop_times.txt**](../../reference/#stop_timestxt)

trip_id | location_id | stop_sequence | pickup_type | drop_off_type | start_pickup_drop_off_window | end_pickup_drop_off_window
-- | -- | -- | -- | -- | -- | --
tripA | portland | 1 | 2 | 1 | 08:00:00 | 12:00:00
tripA | gresham | 2 | 2 | 1 | 10:00:00 | 14:00:00
tripA | vancouver | 3 | 1 | 2 | 10:00:00 | 14:00:00

## 用語集 {: #glossary}


📲 Dial-a-ride は、ヨーロッパ全域で使用されている複数の用語のバリエーションです。 

🇨🇭 スイスでは、Rufbus / On-call bus という用語に該当します。[PostAuto による PubliCar system](https://www.postauto.ch/en/timetable-and-network/publicar)も利用可能です。この提案のもとでは、PubliCar App およびサービスは、ユーザーが優先する旅程計画アプリで検索可能になります。 

🇦🇹 オーストリアでは、dial-a-ride は Rufbus とも呼ばれ、Bedarfsverkehr (Demand Responsive Transport) および Mikro-ÖV (Microtransit) というより大きな枠組みに含まれます。
    
- [bedarfsverkehr.at](https://www.bedarfsverkehr.at/)
- [Wiener Linien](https://www.wienerlinien.at/documents/843721/4770179/Anleitung_Rufbus_359531.pdf/df430b95-9dd4-0d13-ffdf-bdace15932a8?t=1614165175643)
- Rufbus（英語: dial-a-bus、以前は Anruf-Sammel-Taxi または ASTAX call-collect-taxi）
- 現在の GTFS 実装は[年間を通じた運行情報(alert)](https://www.google.com/maps/dir/S%C3%BC%C3%9Fenbrunner+Pl.,+1220+Wien,+Austria/2201+Gerasdorf,+Austria/@48.2867283,16.4429959,13z/am=t/data=!4m15!4m14!1m5!1m1!1s0x476d0393b15bc6d9:0x517f69839511fb31!2m2!1d16.4958186!2d48.2772635!1m5!1m1!1s0x476d0488292e6f61:0xeee80d3d2bb6b1f5!2m2!1d16.4690073!2d48.2962096!3e3!5i1?entry=ttu )として提供されています

🇩🇰 デンマークでは、NT / midttrafik / sydtrafik / FYNBUS / movia (https://flextur.dk/) と呼ばれることがあります。
    
- flextur（英語: flex trip）
- 以前は flextrafik（英語: flex transit）

🇫🇷 ⚠️ フランスでは、パラトランジットサービスに対して TDA (Transport à la Demande) および PMR (Personnes à Mobilité Réduite) という用語が使用されています。

- [Reseau Mistral](https://www.reseaumistral.com/services/service-appel-bus) 
- Appel bus（英語: call bus）

🇩🇪 ドイツでは、On-Demand-Angebot、Flexible Fahrt、および AST と呼ばれています。
   
- [BVG](https://www.bvg.de/de/verbindungen/bvg-muva/flexible-fahrt)
- ブランド: Muva
- On-Demand-Angebot（英語: on-demand-service）
- Flexible Fahrt（英語: flexible trip）
- その他の地域
- Anruf-sammel-taxi または AST（英語: call-collect-taxi）

🇬🇧 イギリスには、以下のサービスがあります。

- [go2 Sevenoaks](https://www.go-coach.co.uk/go2/ )
- On-demand service

用語は国境を越えて異なりますが、一般に dial-a-ride とは、乗客が事業者に何らかの形で連絡する必要がある、あらゆるデマンド型サービスであると考えることができます。 
<hr>
