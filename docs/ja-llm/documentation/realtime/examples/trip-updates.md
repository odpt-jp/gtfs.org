# 便の更新(trip update) {: #trip-update}


以下の例は、完全データセットの便の更新(trip update)フィードを ASCII で表現したものです。

```python
# ヘッダー情報
header {
  # 仕様のバージョン。現在は "2.0"。有効なバージョンは "2.0"、"1.0" です。
  gtfs_realtime_version: "2.0"
  # データセットが差分か完全かを決定します
  incrementality: FULL_DATASET
  # このデータセットがサーバー上で生成された時点
  timestamp: 1284457468
}

# フィードには複数の entity を含めることができます
entity {
  # entity の一意の識別子
  id: "simple-trip"

  # entity の「型」
  trip_update {
    trip {
      # 影響を受ける GTFS entity (trip) を選択します
      trip_id: "trip-1"
    }
    # ダイヤ情報の更新
    stop_time_update {
      # 影響を受ける停留所等(stop)を選択します
      stop_sequence: 3
      # 車両の到着時刻について
      arrival {
        # 5 秒遅延します
        delay: 5
      }
    }
    # ...この車両の遅延は後続の停留所等(stop)に伝播します。

    # 車両のダイヤに関する次の情報更新
    stop_time_update {
      # stop_sequence により選択されます。これは
      stop_sequence: 8
      # 車両の元の（予定された）到着時刻を
      arrival {
        # 1 秒遅延で更新します。
        delay: 1
      }
    }
    # ...同様に、遅延は後続の停留所等(stop)に伝播します。

    # 車両のダイヤに関する次の情報更新
    stop_time_update {
      # stop_sequence により選択されます。これは車両の到着時刻を更新します
      stop_sequence: 10
      # デフォルトの遅延 0（定時）で更新し、この更新を伝播します
      # 車両の残りの停留所等(stop)に対して。
    }
  }
}

# 別の便(trip)の更新情報を含む 2 番目の entity
entity {
  id: "3"
  trip_update {
    trip {
      # 頻度ベースの便(trip)は、その
      # GTFS 内の trip_id と
      trip_id: "frequency-expanded-trip"
      # start_time によって定義されます
      start_time: "11:15:35"
    }
    stop_time_update {
      stop_sequence: 1
      arrival {
        # 負の遅延は、車両が予定より 2 秒早いことを意味します
        delay: -2
      }
    }
    stop_time_update {
      stop_sequence: 9
    }
  }
}
```
