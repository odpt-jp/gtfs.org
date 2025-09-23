# 便の更新(trip update) {: #trip-update}


以下の例は、完全データセットの便の更新(trip update)フィードを ASCII で表現したものです。

```python
# ヘッダー情報
header {
  # 速度仕様のバージョン。現在は "2.0"。有効なバージョンは "2.0", "1.0"。
  gtfs_realtime_version: "2.0"
  # データセットが増分か完全かを決定
  incrementality: FULL_DATASET
  # このデータセットがサーバーで生成された時刻
  timestamp: 1284457468
}

# フィードには複数のエンティティを含めることができます
entity {
  # エンティティの一意の識別子
  id: "simple-trip"

  # エンティティの "type"
  trip_update {
    trip {
      # どの GTFS エンティティ (trip) が影響を受けるかを選択
      trip_id: "trip-1"
    }
    # 運行スケジュール情報の更新
    stop_time_update {
      # 影響を受ける停留所等(stop)を選択
      stop_sequence: 3
      # 車両の到着時刻に対して
      arrival {
        # 5秒の遅延
        delay: 5
      }
    }
    # ...この車両の遅延は後続の停留所等(stop)に伝播します。

    # 車両のスケジュールに関する次の更新情報
    stop_time_update {
      # stop_sequence で選択。更新対象
      stop_sequence: 8
      # 車両の元の（予定された）到着時刻を
      arrival {
        # 1秒の遅延で更新
        delay: 1
      }
    }
    # ...同様に遅延は後続の停留所等(stop)に伝播します。

    # 車両のスケジュールに関する次の更新情報
    stop_time_update {
      # stop_sequence で選択。車両の到着時刻を更新
      stop_sequence: 10
      # デフォルトの遅延 0（定刻通り）で更新し、
      # 残りの停留所等(stop)にこの更新を伝播
    }
  }
}

# 別の便に関する更新情報を含む2つ目のエンティティ
entity {
  id: "3"
  trip_update {
    trip {
      # 頻度ベースの便は GTFS の trip_id によって定義され
      trip_id: "frequency-expanded-trip"
      # start_time
      start_time: "11:15:35"
    }
    stop_time_update {
      stop_sequence: 1
      arrival {
        # 負の遅延は車両が予定より2秒早いことを意味
        delay: -2
      }
    }
    stop_time_update {
      stop_sequence: 9
    }
  }
}
```
