# サービス運行情報 {: #service-alert}


以下の例は、Alert フィードの ASCII 表現です。

```python
# ヘッダー情報
header {
  # 仕様のバージョン。現在は "2.0"。有効なバージョンは "2.0"、"1.0" です。
  gtfs_realtime_version: "2.0"

  # データセットが差分か完全版かを決定します
  incrementality: FULL_DATASET

  # このデータセットがサーバー上で生成された時刻
  # 運行情報(alert)フィードの順序を決定するために使用します
  timestamp: 1284457468
}
# フィードには複数の entity を含めることができます
entity {
  # entity の一意の識別子
  id: "0"

  # entity の「タイプ」
  alert {
    # 運行情報(alert)が有効な期間を複数定義できます
    active_period {
      # POSIX epoch 形式の開始時刻
      start: 1284457468
      # POSIX epoch 形式の終了時刻
      end: 1284468072
    }
    # 影響を受ける GTFS entity を選択します
    informed_entity {
      # 有効なパラメータ: 
      # agency_id、route_id、route_type、stop_id、trip（TripDescriptor を参照）
      route_id: "219"
    }
    # 1つの運行情報(alert) entity に複数のセレクター（informed_entity）を含めることができます
    informed_entity {
      stop_id: "16230"
    }
    # 1つの informed_entity に複数のフィールドを含めることができます
    informed_entity {
      stop_id: "16299"
      route_id: "100"
      # この例は、停留所等(stop) 16299 におけるルート・路線系統(route) 100 を意味します。
      # これは、ルート・路線系統(route) 100 上の他の停留所等(stop)、および停留所等(stop) 16299 における他のルート・路線系統(route)には適用されません。
    }

    # 運行情報(alert)の原因 - 有効な値については gtfs-realtime.proto を参照してください
    cause: CONSTRUCTION
    # 運行情報(alert)の影響 - 有効な値については gtfs-realtime.proto を参照してください
    effect: DETOUR

    # 指定された url は追加情報を提供します
    url {
      # 複数の言語／翻訳をサポートします
      translation {
        # Google 外部でホストされるページ（提供者／事業者など）
        text: "http://www.sometransitagency/alerts"
        language: "en"
      }
    }

    # 運行情報(alert)のヘッダーが強調表示されます
    header_text {
      # 複数の言語／翻訳をサポートします
      translation {
        text: "Elm street の停留所等(stop)は閉鎖されています。Oak street に仮設停留所等(stop)があります"
        language: "en"
      }
    }

    # 運行情報(alert)の説明。ヘッダーテキストに対する追加情報です
    description_text {
      # 複数の言語／翻訳をサポートします
      translation {
        text: "Elm street での工事により、停留所等(stop)は閉鎖されています。仮設停留所等(stop)は、Oak street の北300メートルにあります"
        language: "en"
      }
    }
  }
}
```
