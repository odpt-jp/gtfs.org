# :material-subway-variant: 基本 {: #material-subway-variant-base}

以下の機能は、GTFS が公共交通サービスを表現するために必要となる、最も基本的かつ不可欠な要素を提供します。GTFS はルート・路線系統(route)で構成され、それぞれに関連する便(trip)があります。これらの便(trip)は、特定の時刻に1つ以上の停留所等(stop)を訪れます。便(trip)には時刻情報のみが含まれ、運行する日はカレンダーによって決定されます。
動作する GTFS feed を実現するために、これらすべての機能を一緒に実装しなければなりません。

## 事業者 {: #agency}


事業者には、名称、ウェブサイトURL、サービスが運行される言語およびタイムゾーンなど、交通サービスを担う事業者に関する基本情報が含まれます。これにより、特定のサービスを対応する事業者と照合できます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[agency.txt](../../../documentation/schedule/reference/#agencytxt)|`agency_id`, `agency_name`, `agency_url`, `agency_timezone`, `agency_lang`, `agency_phone`, `agency_fare_url`, `agency_email` |

**前提条件**: 

- その他すべての基本機能

??? note "サンプルデータ"

    <p style="font-size:16px">
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#agencytxt"><b>agency.txt</b></a> <br>
        </p>

        | agency_id | agency_name | agency_url                 | agency_timezone     | agency_lang | agency_phone   | agency_fare_url                  | agency_email           |
        |-----------|-------------|----------------------------|---------------------|-------------|----------------|----------------------------------|------------------------|
        | tb        | Transit Bus | https://www.transitbus.org | America/Los_Angeles | EN          | (777) 555-7777 | https://www.transitbus.org/fares | contact@transitbus.org |

## 停留所等(stop) {: #stops}


停留所等(stop)は、公共交通サービスが乗客を乗車・降車させる場所を識別するために使用される基本要素を表します。これは地下鉄駅またはバス停である場合があります。各停留所等(stop)には、他の属性の中でも、地図上でその位置を特定するための地理座標、および事業者の乗客向け資料と一致する名称があります。停留所等(stop)は、停車時刻(stop_time)を使用して便(trip)に関連付けられます。 
GTFSでは、[構内通路(pathway)](/getting-started/features/pathways)を使用して、鉄道駅やバス車庫などの大規模な駅構内を記述することもできます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stops.txt](../../../documentation/schedule/reference/#stopstxt)|`stop_id`, `stop_code`, `stop_name`, `stop_desc`, `stop_lat`, `stop_lon`, `stop_url`, `stop_timezone`, `platform_code` |

**前提条件**: 

- その他すべての基本機能

??? note "サンプルデータ"

    <p style="font-size:16px">
    
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stopstxt"><b>stops.txt</b></a> <br>
        </p>

        | stop_id | stop_code | stop_desc                                  | stop_name  | stop_lat  | stop_lon   | stop_url                                | stop_timezone | platform_code |
        |---------|-----------|--------------------------------------------|------------|-----------|------------|-----------------------------------------|---------------|---------------|
        | TAS001  | TAS001    | Southwest corner of 5 Avenue and 53 Street | 5 Av/53 St | 45.503568 | -73.587079 | https://www.transitbus.org/stops/TAS001 |               |               |

## ルート・路線系統(route) {: #routes}


ルート・路線系統(route)とは、同一のブランドのもとにある便(trip)のグループであり、乗客に対して単一のサービスとして表示されるものです。各ルート・路線系統(route)には、他の属性の中でも、事業者の乗客向け資料と一致する名称、および表現されるサービスの種類（バス、地下鉄またはメトロ、フェリーなど）があります。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[routes.txt](../../../documentation/schedule/reference/#routestxt)|`route_id`, `agency_id`, `route_desc`, `route_type`, `route_url`, `route_sort_order`, `route_short_name`, `route_long_name`|

**前提条件**: 

- その他すべての Base 機能

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、バスルート・路線系統(route)（`route_type=3`）を定義しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#routestxt"><b>routes.txt</b></a> <br>
        </p>

        | route_id | agency_id | route_short_name | route_long_name    | route_desc                                            | route_type | route_url                            | route_sort_order |
        |----------|-----------|------------------|--------------------|-------------------------------------------------------|------------|--------------------------------------|------------------|
        | RA       | tb        |               17 | Mission - Downtown | 「A」ルート・路線系統(route)は、Mission 下部から Downtown まで運行します。 |          3 | https://www.transitbus.org/routes/ra |               12 |

## 運行日 {: #service-dates}


運行日は、サービスが運行される日付の範囲を示すとともに、特定の日付における休日やその他の特別サービスなど、サービスの例外を作成します。
これは、`calendars.txt` で開始日と終了日を定義し、運行する各曜日のマーカーを設定することで機能します。この期間中に1日単位のスケジュール変更が発生する場合は、`calendar_dates.txt` ファイルを使用して、これらの日ごとのスケジュールを上書きできます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[calendar.txt](../../../documentation/schedule/reference/#calendartxt)|`service_id`, `monday`, `tuesday`, `wednesday`, `thursday`, `friday`, `saturday`, `sunday`, `start_date`, `end_date`|
|[calendar_dates.txt](../../../documentation/schedule/reference/#calendar_datestxt)|`service_id`, `date`, `exception_type`|

**前提条件**: 

- その他すべての基本機能

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルでは、2024年7月について、2つのサービス（平日および週末）を定義しています。7月4日の特別な休日サービスは、週末サービスとして運行します。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#calendartxt"><b>calendar.txt</b></a> <br>
        </p>

        | service_id | monday | tuesday | wednesday | thursday | friday | saturday | sunday | start_date | end_date |
        |------------|--------|---------|-----------|----------|--------|----------|--------|------------|----------|
        | WE         |      0 |       0 |         0 |        0 |      0 |        1 |      1 |   20240701 | 20240731 |
        | WD         |      1 |       1 |         1 |        1 |      1 |        0 |      0 |   20240701 | 20240731 |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#calendar_datestxt"><b>calendar_dates.txt</b></a> <br>
        </p>

        | service_id | date     | exception_type |
        |------------|----------|----------------|
        | WD         | 20240704 |              2 |
        | WE         | 20240704 |              1 |

## 便(trips) {: #trips}


便(trips)は、ルート・路線系統(route)と運行日(service day)を組み合わせ、乗客が利用できる旅程(journey)を作成します。便(trips)は、停車時刻(stop_time)を使用して停留所等(stop)に関連付けられます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[trips.txt](../../../documentation/schedule/reference/#tripstxt)|`route_id`, `service_id`, `trip_id`, `trip_short_name`, `direction_id`, `block_id`|

**前提条件**: 

- その他すべての基本機能

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、RA route において両方向に運行する2つの便を定義しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#tripstxt"><b>trips.txt</b></a> <br>
        </p>

        | route_id | service_id | trip_id | trip_short_name | direction_id | block_id |
        |----------|------------|---------|-----------------|--------------|----------|
        | RA       | WE         | AWE1    |            3885 |            0 |        1 |
        | RA       | WE         | AWE2    |            3887 |            1 |        2 |

## 停車時刻(stop_times) {: #stop-times}


停車時刻(stop_times)は、各便(trip)における個々の停留所等(stop)への到着時刻および出発時刻を表すために使用され、乗客がバス、鉄道、またはフェリーが特定の場所に正確に何時に到着・出発するかを把握できるようにします。`stop_times.txt`ファイルは、通常、GTFSフィード内で最大のファイルです。 
一部のサービスは、特定の到着時刻および出発時刻を持つのではなく、一定の頻度（例：5分ごとに運行する地下鉄路線）で運行されます。これは[Frequency-based services](../base_add-ons/#frequency-based-service)を使用してモデル化でき、`stop_times.txt`と組み合わせてモデル化することもできます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stop_times.txt](../../../documentation/schedule/reference/#stop_timestxt)|`trip_id`, `arrival_time`, `departure_time`, `stop_id`, `stop_sequence`, `pickup_type`, `drop_off_type`, `timepoint` |

**前提条件**: 

- その他すべてのBase機能

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、5つの停留所等(stop)における便(trip)のスケジュールを定義しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stop_timestxt"><b>stop_times.txt</b></a> <br>
        </p>

        | trip_id | arrival_time | departure_time | stop_id | stop_sequence | pickup_type | drop_off_type | timepoint |
        |---------|--------------|----------------|---------|---------------|-------------|---------------|-----------|
        | AWE1    |      6:10:00 |        6:10:00 | TAS001  |             1 |           0 |             0 |         1 |
        | AWE1    |      6:14:00 |        6:14:00 | TAS002  |             2 |           0 |             0 |         1 |
        | AWE1    |      6:20:00 |        6:20:00 | TAS003  |             3 |           0 |             0 |         1 |
        | AWE1    |      6:23:00 |        6:23:00 | TAS004  |             4 |           0 |             0 |         1 |
        | AWE1    |      6:25:00 |        6:25:00 | TAS005  |             5 |           0 |             0 |         1 |
