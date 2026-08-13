# 運行頻度 {: #frequencies}

## 頻度ベースのサービスを記述する {: #describe-a-frequency-based-service}


Société de Transport de Montréal はモントリオールで公共交通サービスを運営しており、地下鉄路線に対して頻度ベースのサービスを運行しています。したがって、GTFS データセットで到着時刻および出発時刻を含む時刻表を提供する代わりに、サービスの運行時間帯全体にわたる運行頻度を記述するために、ファイル [frequencies.txt](../../reference/#frequenciestxt) が使用されます。便(trip)を繰り返す方法は、すべての停留所等(stop)において停留所等(stop)間の時刻が一貫している場合にのみ機能します。頻度ベースのサービスをモデル化する場合、stop_times.txt (@TODO link) には、乗客に表示する時刻を決定するために、停留所等(stop)間の相対時刻が含まれます。 

[**frequencies.txt**](../../reference/#frequenciestxt)

```
trip_id,start_time,end_time,headway_secs
22M-GLOBAUX-00-S_1_2,16:01:25,16:19:25,180
22M-GLOBAUX-00-S_1_2,16:19:25,17:03:25,165
```

上記では、`trip_id=22M-GLOBAUX-00-S_1_2` を持つグリーンラインの便(trip)を例として使用しています。この便(trip)について、最初のレコードは、午後4時01分25秒から午後4時19分25秒まで、列車が180秒ごと（または3分ごと）に運行することを示しています。同様に、2番目のレコードは、午後4時19分25秒から午後5時03分25秒まで、列車が165秒ごとに運行することを示しています。



<sup>[例の出典](https://www.stm.info/en/about/developers)</sup>
