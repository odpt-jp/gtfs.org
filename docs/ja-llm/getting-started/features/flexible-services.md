# :material-transit-detour: フレキシブルサービス {: #material-transit-detour-flexible-services}

フレキシブルサービスは、デマンド型サービスとも呼ばれ、時刻表に基づくサービスおよび／または固定サービスの一般的な挙動に従わないサービスです。

## 連続停車 {: #continuous-stops}


連続停車は、乗客が予定された停留所等(stop)間で乗車および／または降車できる場合に使用されます。
これは、`routes.txt` で指定できます。この場合、ルート・路線系統(route)のすべての便(trip)について、車両の走行経路上の任意の地点で乗客が乗車または降車できることを示します。または、ルート・路線系統(route)の特定区間について `stop_times.txt` で指定することもできます。  

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stop_times.txt](../../../documentation/schedule/reference/#stop_timestxt)|`continuous_pickup`, `continuous_drop_off` |
|[routes.txt](../../../documentation/schedule/reference/#routestxt)|`continuous_pickup`, `continuous_drop_off` |

**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、連続停車を表現する2つの方法を示しています。
    </p>
    
    <p style="font-size:16px">
    最初のサンプルは、ルート・路線系統(route) `RA` の経路上の任意の地点で乗車および降車が許可されていることを示しています。
     </p>

    <p style="font-size:16px">
    2つ目のサンプルは、`stop_sequence=3` および `stop_sequence=4` に `continuous_pickup` と `continuous_drop_off` の値を割り当てることで、便(trip) `AWE1` の3番目から5番目の停留所等(stop)の間で乗車および降車が許可されていることを示しています。
    </p>

    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#routestxt"><b>routes.txt</b></a> <br>
        </p>

        | route_id | route_short_name | route_type | continuous_pickup | continuous_drop_off |
        |----------|------------------|------------|-------------------|---------------------|
        | RA       |               17 |          3 |                 0 |                   0 |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stop_timestxt"><b>stop_times.txt</b></a> <br>
        </p>

        | trip_id | arrival_time | departure_time | stop_id | stop_sequence | continuous_pickup | continuous_drop_off |
        |---------|--------------|----------------|---------|---------------|-------------------|---------------------|
        | AWE1    |      6:10:00 |        6:10:00 | TAS001  |             1 |                   |                     |
        | AWE1    |      6:14:00 |        6:14:00 | TAS002  |             2 |                   |                     |
        | AWE1    |      6:20:00 |        6:20:00 | TAS003  |             3 |                 0 |                   0 |
        | AWE1    |      6:23:00 |        6:23:00 | TAS004  |             4 |                 0 |                   0 |
        | AWE1    |      6:25:00 |        6:25:00 | TAS005  |             5 |                   |                     |

## 予約ルール {: #booking-rules}


予約ルールは、ユーザーがデマンド型サービスの便(trip)を予約できるようにするために使用できます。これらのルールは、予約を成功させるために必要な前提条件を示し、ユーザーが便(trip)の予約を行える連絡先情報を提供します。この機能は、そのようなサービスで予約が必要な場合、[逸脱を伴う事前定義ルート](#predefined-routes-with-deviation)、[ゾーンベースのデマンド型サービス](#zone-based-demand-responsive-services)、および[固定停留所のデマンド型サービス](#fixed-stops-demand-responsive-services)機能と併用するべきです。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[booking_rules.txt](../../../documentation/schedule/reference/#booking_rulestxt)|`booking_rule_id`, `booking_type`, `prior_notice_duration_min`, `prior_notice_duration_max`, `prior_notice_last_day`, `prior_notice_last_time`, `prior_notice_start_day`, `prior_notice_start_time`, `prior_notice_service_id`, `message`, `pickup_message`, `drop_off_message`, `phone_number`, `info_url`, `booking_url` |


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、2つの異なる予約ルールのセットを示しています。1つ目は、少なくとも1日前（午後1時前）までに予約しなければならず、14日前を超えて予約できない便(trip)用です。2つ目は、便(trip)の少なくとも45分前から、5時間前までに予約できる便(trip)用です。

    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#booking_rulestxt"><b>booking_rules.txt</b></a> <br>
        </p>

        | booking_rule_id | booking_type | prior_notice_duration_min | prior_notice_duration_max | prior_notice_last_day | prior_notice_last_time | prior_notice_start_day | prior_notice_start_time | prior_notice_service_id | message                                                                                                                                            | pickup_message | drop_off_message | phone_number   | info_url             | booking_url             |
        |-----------------|--------------|---------------------------|---------------------------|-----------------------|------------------------|------------------------|-------------------------|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------------|------------------|----------------|----------------------|-------------------------|
        | route_br_1818   |            2 |                           |                           |                     1 |                  13:00 |                     14 |                    9:00 |                         | 乗車を依頼するには、便(trip)の少なくとも1営業日前の午後1時までに123-111-2233へお電話ください。便(trip)は14営業日前まで予約できます。 |                |                  | (123)-111-2233 | flexservice.org/info | flexservice.org/booking |
        | route_br_4545   |            1 |                        45 |                       300 |                       |                        |                        |                         |                         | 乗車を依頼するには、当社ウェブサイトの公式予約システムを使用してください。便(trip)は少なくとも45分前までに予約しなければなりません。                                 |                |                  | (123)-111-2233 | flexservice.org/info | flexservice.org/booking |

## 逸脱を伴う事前定義ルート {: #predefined-routes-with-deviation}


逸脱を伴う事前定義ルートは、車両がルート沿いの特定エリア内で便(trip)を予約した利用者を乗車させるために、特定のルートから短時間逸脱できる柔軟なサービスをモデル化するために使用できます。これは、従来の停留所等(stop)（通常の定時運行サービスのようなもの）と、`locations.geojson` を使用したゾーンを組み合わせて使用します。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stop_times.txt](../../../documentation/schedule/reference/#stop_timestxt)|`location_id`, `start_pickup_drop_off_window`, `end_pickup_drop_off_window`, `pickup_booking_rule_id`, `drop_off_booking_rule_id`|
|[locations.geojson](../../../documentation/schedule/reference/#locationsgeojson)|`Type`, `Features`, `Features:Type`, `Features:Id`, `Features:Properties`, `Features:Properties:Stop_name`, `Features:Properties:Stop_description`, `Features:Geometry`, `Features:Geometry:Type`, `Features:Geometry:Coordinates` |

**前提条件**:

- [基本機能](../base)
- サービスで予約が必要な場合は[予約ルール](#booking-rules)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、3つの固定停留所等(stop)を持ち、固定停留所等(stop)間に定義された特定エリア内の任意の場所で乗客を降車させることもできる便(trip)を示しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stop_timestxt"><b>stop_times.txt</b></a> <br>
        </p>

        | trip_id  | arrival_time | departure_time | stop_id | location_id           | stop_sequence | start_pickup_drop_off_window | end_pickup_drop_off_window | pickup_type | drop_off_type | shape_dist_traveled | pickup_booking_rule_id | drop_off_booking_rule_id |
        |----------|--------------|----------------|---------|-----------------------|---------------|------------------------------|----------------------------|-------------|---------------|---------------------|------------------------|--------------------------|
        | 4545_001 |     10:00:00 |       10:00:00 | S50122  |                       |             1 |                              |                            |             |               |                   0 |                        |                          |
        | 4545_001 |              |                |         | zone_S50122_to_S50123 |             2 |                     10:00:00 |                   10:06:00 |           1 |             3 |                     | br_1234                | br_1234                  |
        | 4545_001 |     10:06:00 |       10:06:00 | S50123  |                       |             3 |                              |                            |             |               |                 983 |                        |                          |
        | 4545_001 |              |                |         | zone_S50123_to_S50124 |             4 |                     10:06:00 |                   10:15:00 |           1 |             3 |                     | br_1234                | br_1234                  |
        | 4545_001 |     10:15:00 |       10:15:00 | S50124  |                       |             5 |                              |                            |             |               |                2109 |                        |                          |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#locationsgeojson"><b>locations.geojson</b></a> <br>
        </p>

        ~~~
        {
          "type": "FeatureCollection",
          "features": [
            {
              "id": "zone_S50122_to_S50123",
              "type": "Feature",
              "geometry": {
                "type": "Polygon",
                # 簡略化のため、ここでは3つの座標のみを示しています。
                "coordinates": [
                  [
                    [
                      -73.575952,
                      45.514974
                    ],
                    [
                      -73.577314,
                      45.513433
                    ],
                    [
                      -73.569794,
                      45.5098370
                    ]
                  ]
                ]
              },
              "properties": {}
            },
        {
              "id": "zone_S50123_to_S50124",
              "type": "Feature",
              "geometry": {
                "type": "Polygon",
                # 簡略化のため、ここでは3つの座標のみを示しています。
                "coordinates": [
                  [
                    [
                      -73.561332,
                      45.5085599
                    ],
                    [
                     -73.5701298,
                      45.5124057
                    ],
                    [
                      -73.571302,
                      45.5105563
                    ]
                  ]
                ]
              },
              "properties": {}
            }
           ]
        } 
        ~~~

## ゾーンベースのデマンド型サービス {: #zone-based-demand-responsive-services}


ゾーンベースのデマンド型サービスは、便を予約する利用者が特定のエリア内の任意の場所で乗車および／または降車できるサービスをモデル化するために使用されます。これらのエリアは `locations.geojson` を使用して定義されるため、`stops.txt`、`stop_times.arrival_time`、および `stop_times.departure_time` を使用する必要はありません。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stop_times.txt](../../../documentation/schedule/reference/#stop_timestxt)|`location_id`、`start_pickup_drop_off_window`、`end_pickup_drop_off_window`、`pickup_booking_rule_id`、`drop_off_booking_rule_id`|
|[locations.geojson](../../../documentation/schedule/reference/#locationsgeojson)|`Type`、`Features`、`Features:Type`、`Features:Id`、`Features:Properties`、`Features:Properties:Stop_name`、`Features:Properties:Stop_description`、`Features:Geometry`、`Features:Geometry:Type`、`Features:Geometry:Coordinates` |

**前提条件**:

- [基本機能](../base)
- サービスで予約が必要な場合は[予約ルール](#booking-rules)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、午前9時から午後6時までの間、特定のエリア内の任意の場所で事前予約した乗客を乗車・降車させることができるサービスを示しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stop_timestxt"><b>stop_times.txt</b></a> <br>
        </p>

        | trip_id  | location_id | stop_sequence | start_pickup_drop_off_window | end_pickup_drop_off_window | pickup_type | drop_off_type | pickup_booking_rule_id | drop_off_booking_rule_id |
        |----------|-------------|---------------|------------------------------|----------------------------|-------------|---------------|------------------------|--------------------------|
        | 2708_001 | area_001    |             1 |                      9:00:00 |                   18:00:00 |           2 |             1 | br_3289                | br_3289                  |
        | 2708_001 | area_001    |             2 |                      9:00:00 |                   18:00:00 |           1 |             2 | br_3289                | br_3289                  |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#locationsgeojson"><b>locations.geojson</b></a> <br>
        </p>

        ~~~
        {
          "type": "FeatureCollection",
          "features": [
            {
              "id": "area_001",
              "type": "Feature",
              "geometry": {
                "type": "Polygon",
                # 簡略化のため、ここでは3つの座標のみを示しています。
                "coordinates": [
                  [
                    [
                      -73.644437,
                      45.5023960
                    ],
                    [
                      -73.641593,
                      45.5054392
                    ],
                    [
                      -73.636580,
                      45.5081683
                    ]
                  ]
                ]
              },
              "properties": {}
            }
          ]
        }
        ~~~

## 固定停留所型デマンド型サービス {: #fixed-stops-demand-responsive-services}

固定停留所型デマンド型サービスは、便を予約する利用者が、事前定義された停留所等(stop)のグループ内にある任意の場所で乗車および／または降車できるサービスをモデル化するために使用されます。これらの停留所等(stop)のグループは、`location_groups.txt` および `location_group_stops.txt` を使用して定義されます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stop_times.txt](../../../documentation/schedule/reference/#stop_timestxt)|`location_group_id`, `start_pickup_drop_off_window`, `end_pickup_drop_off_window`, `pickup_booking_rule_id`, `drop_off_booking_rule_id`|
|[location_groups.txt](../../../documentation/schedule/reference/#location_groupstxt)|`location_group_id`, `location_group_name`|
|[location_group_stops.txt](../../../documentation/schedule/reference/#location_group_stopstxt)|`location_group_id`, `stop_id`|

**前提条件**:

- [基本機能](../base)
- サービスで予約が必要な場合は、[予約ルール](#booking-rules)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、午前7時から午前10時までの間に、事前予約した乗客を4つの異なる停留所等(stop)で乗車および降車させることができるサービスを示しています。

    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#location_groupstxt"><b>location_groups.txt</b></a> <br>
        </p>

        | location_group_id | location_group_name           |
        |-------------------|-------------------------------|
        | r27_stops         | Yellow Borough Route 27 の停留所等 |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#location_group_stopstxt"><b>location_group_stops.txt</b></a> <br>
        </p>

        | location_group_id | stop_id |
        |-------------------|---------|
        | r27_stops         | syb029  |
        | r27_stops         | syb030  |
        | r27_stops         | syb031  |
        | r27_stops         | syb032  |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stop_timestxt"><b>stop_times.txt</b></a> <br>
        </p>

        | trip_id  | location_group_id | stop_sequence | start_pickup_drop_off_window | end_pickup_drop_off_window | pickup_type | drop_off_type | pickup_booking_rule_id | drop_off_booking_rule_id |
        |----------|-------------------|---------------|------------------------------|----------------------------|-------------|---------------|------------------------|--------------------------|
        | 2714_002 | r27_stops         |             1 |                      7:00:00 |                   10:00:00 |           2 |             1 | br_5478                | br_5478                  |
        | 2714_002 | r27_stops         |             2 |                      7:00:00 |                   10:00:00 |           1 |             2 | br_5478                | br_5478
