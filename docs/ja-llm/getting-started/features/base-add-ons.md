# :material-plus-box-multiple-outline: 基本追加機能 {: #material-plus-box-multiple-outline-base-add-ons}

これらの機能は、基本で説明されている機能を拡張するものであり、乗客により良い体験を提供するためにGTFSデータセットの包括性を高める、または事業者、データベンダー、データ再利用者間の協力を促進します。これらの拡張には、基本で説明されているファイル内への新しいフィールドの導入、または新しいファイルの作成が含まれることがあります。

## フィード情報 {: #feed-information}


フィード情報は、有効期間（開始日および終了日）、公開組織、GTFSデータセットおよびデータ公開慣行に関する問い合わせ先情報など、フィードに関する重要な情報を伝達します。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[feed_info.txt](../../../documentation/schedule/reference/#feed_infotxt)|`feed_publisher_name`, `feed_publisher_url`, `feed_lang`, `default_lang`, `feed_start_date`, `feed_end_date`, `feed_version`, `feed_contact_email`, `feed_contact_url` |


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#feed_infotxt"><b>feed_info.txt</b></a> <br>
        </p>

        | feed_publisher_name	      | feed_publisher_url   | feed_lang | default_lang | feed_start_date | feed_end_date | feed_version | feed_contact_email | feed_contact_url             |
        |--------------------------|----------------------|-----------|--------------|-----------------|---------------|--------------|--------------------|------------------------------|
        | Greater Region Transport | https://www.gra1.org | en        | en           |        20240101 |      20241231 |          3.1 | support@gra1.org   | https://www.gra1.org/support |

## ルート形状(shape) {: #shapes}

ルート形状(shape)は定義して便(trip)に関連付けることができ、これにより経路検索アプリケーションは便(trip)を地図上に表示し、乗客に交通機関の車両で移動する必要がある距離を知らせることができます。`shape_dist_traveled`フィールドは、乗客に地図を表示する際にルート形状(shape)のどの部分を描画するかをプログラムで決定するために使用されます。
ルート形状(shape)を定義する際には、その詳細度（例: 道路の正確な曲線に従うこと）と、必要な情報のみを効率的に伝えることの間でバランスを取る必要があります。

|含まれるファイル                             |含まれるフィールド            |
|----------------------------------|-------------------|
|[shapes.txt](../../../documentation/schedule/reference/#shapestxt)                        |`shape_id`, `shape_pt_lat`, `shape_pt_lon`, `shape_pt_sequence`, `shape_dist_traveled`           |
|[trips.txt](../../../documentation/schedule/reference/#tripstxt)                         |`shape_id`           |
|[stop_times.txt](../../../documentation/schedule/reference/#stop_timestxt)                    |`shape_dist_traveled`|


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、TriMet GTFSフィードのルート形状(shape)の一部を示しています（<a     href="https://developer.trimet.org/GTFS.shtml">こちら</a>からダウンロードしてください）。 <br><br>
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#shapestxt">shapes.txt</a> <br>
        </p>
    
        | shape_id | shape_pt_lat | shape_pt_lon | shape_pt_sequence | shape_dist_traveled |
        | --------- | ------------- | ------------- | ------------------ | ------------------- |
        | 558674     | 45.47623       | -122.721885    | 1                   | 0.0                  |
        | 558674     | 45.476235      | -122.72236     | 2                   | 121.9                |
        | 558674     | 45.476237      | -122.722523    | 3                   | 163.7                |
        | 558674     | 45.476242      | -122.723024    | 4                   | 292.2                |
        | 558674     | 45.476244      | -122.72316     | 5                    | 327.1               |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#tripstxt">trips.txt</a> <br>
        </p>
        
        |trip_id |shape_id|
        |--------|--------|
        |13302375|558674  |

    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stop_timestxt">stop_times.txt</a> <br>
        </p>
        
        |trip_id |stop_sequence|shape_dist_traveled|
        |--------|-------------|-------------------|
        |13302375|1            |0                  |
        |13302375|2            |461.7              |
        |13302375|3            |1245               |

## ルート・路線系統(route)の色 {: #route-colors}


ルート・路線系統(route)の色を使用すると、事業者のデザインガイドラインによって特定のルート・路線系統(route)に割り当てられた配色を正確に表現し、伝達できます。これにより、ユーザーは公式の色によって交通サービスを容易に識別できます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[routes.txt](../../../documentation/schedule/reference/#routestxt)|`route_color`, `route_text_color` |


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、ルート・路線系統(route) `RA` がHEXカラーコード `D95700` を使用したオレンジ色であり、テキストがHEXカラーコード `0` を使用した黒色で描画されるべきであることを示しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#routestxt"><b>routes.txt</b></a> <br>
        </p>

        | route_id | agency_id | route_short_name | route_long_name    | route_type | route_color | route_text_color |
        |----------|-----------|------------------|--------------------|------------|-------------|------------------|
        | RA       | agency001 |               17 | Mission - Downtown |          3 | D95700      |                0 |

## 自転車の持ち込み可否 {: #bike-allowed}


自転車の持ち込み可否は、特定の便(trip)を運行する車両が自転車を収容できるかどうかを示し、ユーザーが複数の交通手段を利用する旅程を実現できるサービスを計画・利用するのに役立ちます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[trips.txt](../../../documentation/schedule/reference/#tripstxt)|`bikes_allowed` |


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、便 `AWE1` で使用される車両が車内に少なくとも1台の自転車を収容できること（`bikes_allowed=1`）、および便 `AWE2` で使用される車両は収容できないこと（`bikes_allowed=2`）を示しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#tripstxt"><b>trips.txt</b></a> <br>
        </p>

        | route_id | service_id | trip_id | bikes_allowed |
        |----------|------------|---------|---------------|
        | RA       | WE         | AWE1    |             1 |
        | RA       | WE         | AWE2    |             2 |

## 行先表示(headsign) {: #headsigns}


行先表示(headsign)により、車両が便(trip)の目的地を示すために使用する表示を伝えることができ、利用者が正しい公共交通サービスを識別しやすくなります。この機能は、特定のルート・路線系統(route)に沿った行先表示(headsign)の変更をサポートします。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[trips.txt](../../../documentation/schedule/reference/#tripstxt)|`trip_headsign` |
|[stop_times.txt](../../../documentation/schedule/reference/#stop_timestxt)|`stop_headsign`|

**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルでは、最初の表は便(trip) `AWE1` および `AWE2` で使用する行先表示(headsign)を指定し、2番目の表は `AWE1` の行先表示(headsign)が停留所等(stop) `TAS004` の後に変更され、`trips.txt` で指定されたものを上書きすることを示しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#tripstxt"><b>trips.txt</b></a> <br>
        </p>

        | route_id | service_id | trip_id | trip_headsign |
        |----------|------------|---------|---------------|
        | RA       | WE         | AWE1    | Downtown      |
        | RA       | WE         | AWE2    | Mission       |

    
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stop_timestxt"><b>stop_times.txt</b></a> <br>
        </p>

        | trip_id | arrival_time | departure_time | stop_id | stop_sequence | stop_headsign          |
        |---------|--------------|----------------|---------|---------------|------------------------|
        | AWE1    |      6:10:00 |        6:10:00 | TAS001  |             1 |                        |
        | AWE1    |      6:14:00 |        6:14:00 | TAS002  |             2 |                        |
        | AWE1    |      6:20:00 |        6:20:00 | TAS003  |             3 |                        |
        | AWE1    |      6:23:00 |        6:23:00 | TAS004  |             4 | Downtown - Main Square |
        | AWE1    |      6:25:00 |        6:25:00 | TAS005  |             5 | Downtown - Main Square |

## 位置タイプ {: #location-types}


位置タイプは、出入口、ノード、乗車エリアなどの交通駅構内の主要なエリア、およびそれらの関係を分類するために使用されます。位置タイプは、構内通路(pathway)を使用して交通駅をモデル化するための基盤となります。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stops.txt](../../../documentation/schedule/reference/#stopstxt)|`location_type`, `parent_station` |


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、`stops.txt` 内の交通駅構内の複数の位置を示しています。主要な位置を表す親駅と、プラットフォーム、出入口、汎用ノードなどの子位置です。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stopstxt"><b>stops.txt</b></a> <br>
        </p>

        | stop_id      | stop_name                                             | location_type | parent_station |
        |--------------|-------------------------------------------------------|---------------|----------------|
        | Station_A102 | Main Street 駅                                   |             1 |                |
        | A102_B01     | Main Street 駅 - 北側プラットフォーム                  |             0 | Station_A102   |
        | A102_B02     | Main Street 駅 - 南側プラットフォーム                  |             0 | Station_A102   |
        | A102_E01     | Main Street 駅 - 出入口                   |             2 | Station_A102   |
        | A102_S01     | Main Street 駅 - 入口階段上部          |             3 | Station_A102   |
        | A102_S02     | Main Street 駅 - 入口階段下部       |             3 | Station_A102   |
        | A102_S03     | Main Street 駅 - 北側プラットフォーム階段上部    |             3 | Station_A102   |
        | A102_S04     | Main Street 駅 - 北側プラットフォーム階段下部 |             3 | Station_A102   |
        | A102_S05     | Main Street 駅 - 南側プラットフォーム階段上部    |             3 | Station_A102   |
        | A102_S06     | Main Street 駅 - 南側プラットフォーム階段下部 |             3 | Station_A102   |
        | A102_F01     | Main Street 駅 - 改札内側          |             3 | Station_A102   |
        | A102_F02     | Main Street 駅 - 改札外側        |             3 | Station_A102   |

## 運行間隔 {: #frequencies}


運行間隔は、10分ごとに運行するバスや、指定された時間帯に2分間隔で運行する地下鉄サービスなど、一定の運行間隔に基づいて運行するサービスをモデル化するために使用できます。
一定の運行間隔で運行するサービスをモデル化する場合、`stop_times.txt` には、乗客に表示する時刻を決定するために、停留所等(stop)間の相対時刻が含まれます。 

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[frequencies.txt](../../../documentation/schedule/reference/#frequenciestxt)|`trip_id`, `start_time`, `end_time`, `headway_secs`, `exact_times` |


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、2つの異なる便(trip)を示しています。便(trip) `AWE1` は30分ごとに運行し（`headway_secs=1800`）、便(trip) `AWE2` は15分ごとに運行します（`headway_secs=900`）。  
    <p style="font-size:16px">
    `exact_times` フィールドは、スケジュールが `start_time` フィールドに入力された正確な開始時刻に従うかどうかを示します。 
    - 便(trip) `AWE1` は、午前6時10分から午後12時00分まで30分ごとに出発します。
    - 便(trip) `AW2` は、午前6時00分、午前6時15分、午前6時30分などに出発します。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#frequenciestxt"><b>frequencies.txt</b></a> <br>
        </p>

        | trip_id | start_time | end_time | headway_secs | exact_times |
        ---------|------------|----------|--------------|-------------|
         AWE1    |    6:10:00 | 12:00:00 |         1800 |           0 |
         AWE2    |    6:00:00 | 19:50:00 |          900 |           1 |

## 乗換 {: #transfers}


乗換は、異なる移動区間（または乗車区間(leg)）間の移行に関する詳細を提供し、乗換を含む旅程(journey)の実現可能性を経路検索システムが判断できるようにします。乗換を指定しても、乗客が他の場所で乗換できないことを意味するわけではなく、特定の乗換が不可能であるか、または乗換に最低限必要な時間を示すだけです。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[transfers.txt](../../../documentation/schedule/reference/#transferstxt)|`from_stop_id`, `to_stop_id`, `from_route_id`, `to_route_id`, `from_trip_id`, `to_trip_id`, `transfer_type`, `min_transfer_time` |


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、3種類の異なる乗換を示しています。5分の最低乗換時間を必要とする停留所等(stop)間の乗換、2つのルート・路線系統(route)間の時刻指定乗換地点、および同じ車両によって運行される2つの便(trip)間の座席に座ったままの乗換です。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#transferstxt"><b>transfers.txt</b></a> <br>
        </p>

        | from_stop_id | to_stop_id | from_route_id | to_route_id | from_trip_id | to_trip_id | transfer_type | min_transfer_time |
        |--------------|------------|---------------|-------------|--------------|------------|---------------|-------------------|
        | s6           | s7         |               |             |              |            |             2 |               300 |
        |              |            |               |             | PL04-003     | DL57-008   |             4 |                   |
        |              |            | BR09          | CR01        | BR09-012     | CR01-005   |             1 |                   |

## 翻訳 {: #translations}


翻訳により、駅名などのサービス情報を複数の言語で提供でき、旅行計画ツールはユーザーの言語および位置情報の設定に応じて、特定の言語で情報を表示できます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[translations.txt](../../../documentation/schedule/reference/#translationstxt)|`table_name`,`field_name`,`language`,`translation`,`record_id`,`record_sub_id`,`field_value` |


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、`routes.txt` で使用される2つのフィールド、`route_long_name` および `route_desc` に対して、フランス語とスペイン語の翻訳が提供されていることを示しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#translationstxt"><b>translations.txt</b></a> <br>
        </p>

        | table_name | field_name      | language | translation                                           | record_id | record_sub_id | field_value |
        |------------|-----------------|----------|-------------------------------------------------------|-----------|---------------|-------------|
        | routes     | route_long_name | ES       | Mission - Centro                                      | RA        |               |             |
        | routes     | route_long_name | FR       | Mission - Centre ville                                | RA        |               |             |
        | routes     | route_desc      | ES       | La ruta "A" viaja desde Lower Mission hasta el centro | RA        |               |             |
        | routes     | route_desc      | FR       | La route « A » relie Lower Mission au centre-ville.   | RA        |               |             |

## 帰属情報 {: #attributions}


帰属情報により、データセットの作成に関与する組織（作成者、運行事業者、当局など）に関する追加の詳細を共有することができます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[attributions.txt](../../../documentation/schedule/reference/#attributionstxt) |`attribution_id`, `agency_id`, `route_id`, `trip_id`, `organization_name`, `is_producer`, `is_operator`, `is_authority`, `attribution_url`, `attribution_email`, `attribution_phone` |


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px"> 
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#attributionstxt"><b>attributions.txt</b></a> <br>
        </p>

        | attribution_id | agency_id | route_id | trip_id | organization_name        | is_producer | is_operator | is_authority | attribution_url                  | attribution_email       | attribution_phone |
        |----------------|-----------|----------|---------|--------------------------|-------------|-------------|--------------|----------------------------------|-------------------------|      -------------------|
        | op01           | tb        |          |         | Transit Bus              |             |           1 |              | https://www.transitbus.org/fares | contact@transitbus.org  | (777)        555-7777    |
        | au01           | gra       |          |         | Greater Region Transport |           1 |             |            1 | https://www.gra1.org             | contact@gra1.org        | (555)        555-5555    |
        | op02           |           | rtd023   |         | Bus company A            |             |           1 |              | https://www.buscompanya.com      | contact@buscompanya.com | (333)        333-3333    |
        | op03           |           | rtd025   |         | Bus company B            |             |           1 |              | https://www.buscompanyb.com      | contact@buscompanyb.com | (888)        888-8888    |

## 車両の乗用車搭載可否 {: #cars-allowed}


車両の乗用車搭載可否は、特定の便(trip)を運行する車両（カーフェリーや乗用車を輸送可能な列車など）が、車両内に乗用車を収容できるかどうかを示します。この機能は、複数の交通手段を利用する旅程(journey)を計画し、利用できるサービスに乗客がアクセスするのに役立ちます。


| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[trips.txt](../../../documentation/schedule/reference/#tripstxt)|`cars_allowed` |


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、便(trip) `AWE1` で使用される車両が車内に少なくとも1台の乗用車を収容できること（`cars_allowed=1`）、および便(trip) `AWE2` で使用される車両は収容できないこと（`cars_allowed=2`）を指定しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#tripstxt"><b>trips.txt</b></a> <br>
        </p>

        | route_id | service_id | trip_id | cars_allowed |
        |----------|------------|---------|---------------|
        | RA       | WE         | AWE1    |             1 |
        | RA       | WE         | AWE2    |             2 |

## 停留所等(stop)へのアクセス {: #stop-access}


停留所等(stop)へのアクセスは、停留所等(stop)またはプラットフォームに道路ネットワークから直接アクセスできるかどうかを示します。この機能により、経路検索ツールは停留所等(stop)またはプラットフォームに到達するための、より正確な案内を生成できます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stops.txt](../../../documentation/schedule/reference/#tripstxt)|`stop_access` |


**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルでは、`stop_id` = `STOP1` の停留所等(stop)には駅入口から、または構内通路(pathway)を使用してアクセスしなければならないこと（`stop_access=0`）、および `stop_id` = `STOP2` の停留所等(stop)には親駅 `STATION0` の入口または構内通路(pathway)を考慮せずに直接アクセスできること（`stop_access=1`）を指定しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stopstxt"><b>stops.txt</b></a> <br>
        </p>

        | stop_id | stop_name | location_type | parent_station | stop_access |
        |----------|--|------------|---------|---------------|
        | STATION0   | Main Street Bus Station                | 1            |       |      |
        | STOP1      |  Main Street Bus Station - Platform 1  | 0            | STATION0   | 0 |
        | STOP2   |  |  Main Street Station - Street Bus Stop  | STATION0    |             1 |
