# :material-wheelchair: アクセシビリティ {: #material-wheelchair-accessibility}

アクセシビリティ機能は、障害のある人々がサービスを利用するために必要な情報を提供することを目的としています。

## 停留所等(stop)の車椅子対応 {: #stops-wheelchair-accessibility}


停留所等(stop)の車椅子対応では、指定された場所から車椅子で乗車できるかどうかを示すことができます。車椅子を利用する乗客にサービスを提供するためには、車椅子で乗車できないことを指定するのと同様に、車椅子で乗車できることを指定することも重要です。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stops.txt](../../../documentation/schedule/reference/#stopstxt)|`wheelchair_boarding` |

**前提条件**:

- [基本機能](../base)
- 出入口や乗車エリアなどの駅の場所に関するアクセシビリティ情報を定義する場合は、[場所の種類](../base_add-ons/#location-types)。

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、`wheelchair_boarding=1` を使用して、停留所等(stop) `TAS001` で車椅子による乗車が可能であることを示しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stopstxt"><b>stops.txt</b></a> <br>
        </p>

        | stop_id | stop_name  | stop_lat  | stop_lon   | location_type | wheelchair_boarding |
        |---------|------------|-----------|------------|---------------|---------------------|
        | TAS001  | 5 Av/53 St | 40.760167 | -73.975224 |               |                   1 |

## 便(trip)の車椅子対応 {: #trips-wheelchair-accessibility}


便(trip)の車椅子対応により、車両が車椅子を使用する乗客を収容できるかどうかを示すことができます。車椅子を使用する乗客にサービスを提供するためには、車両が車椅子を使用する乗客を収容できることを指定することは、車両が収容できないことを指定するのと同様に重要です。乗客が指定された停留所等(stop)で便(trip)を利用できるためには、停留所等(stop)と便(trip)の両方が車椅子対応でなければなりません。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[trips.txt](../../../documentation/schedule/reference/#tripstxt)|`wheelchair_accessible`|

**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、便(trip) `AWE1` で使用される車両が少なくとも1台の車椅子を収容できる設備を備えており、便(trip) `AWE2` で使用される車両は備えていないことを示しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#tripstxt"><b>trips.txt</b></a> <br>
        </p>

        | route_id | service_id | trip_id | wheelchair_accessible |
        |----------|------------|---------|-----------------------|
        | RA       | WE         | AWE1    |                     1 |
        | RA       | WE         | AWE2    |                     2 |

## 読み上げ用フィールド(text-to-speech) {: #text-to-speech}


読み上げ用フィールド(text-to-speech)では、テキストを音声に変換するために必要な入力を提供できます。これにより、テキストを読み上げる支援技術を使用する乗客が、交通サービスを利用する際に正しい停留所等(stop)名を取得できるようになります。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[stops.txt](../../../documentation/schedule/reference/#stopstxt)|`tts_stop_name` |

**前提条件**: 

- [基本機能](../base)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、停留所等(stop)名の読みやすいバージョンを提供し、読み上げ用フィールド(text-to-speech)ツールがその名前を読み上げられるようにします。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stopstxt"><b>stops.txt</b></a> <br>
        </p>

        | stop_id | stop_name  | stop_lat    | stop_lon   | tts_stop_name            |
        |---------|------------|-------------|------------|--------------------------|
        | TAS001  | 5 Av/53 St | 45.5035680  | -73.587079 | 5th avenue and 53 street |
