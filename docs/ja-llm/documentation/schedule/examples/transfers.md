# 乗換 {: #transfers}

## ブロック乗換 {: #block-transfers}


ブロック乗換は、着席したままの乗換とも呼ばれ、便(trip)の集合が以下の条件を満たす場合に利用できます。

1. 便(trip)が連続しています。
2. 同じ車両が両方の便(trip)を運行します。
3. 便(trip)には、交通フィードの [trips.txt](../../reference/#tripstxt) ファイルで同じ `block_id` 値が設定されています。

### ブロック乗り換えを有効にするための `block_id` の使用方法 {: #use-block_id-to-enable-block-transfers}


ブロック乗り換えは、異なるルート・路線系統(route)上の連続する便(trip)間、またはルート・路線系統(route)が環状線である場合は同一のルート・路線系統(route)上で行うことができます。`block_id` フィールドを使用して、どの便(trip)が1つのブロックに含まれるか、および着席したままの乗り換えが利用可能な選択肢となる場所を指定します。

たとえば、次の [trips.txt](../../reference/#tripstxt) および [stop_times.txt](../../reference/#stop_timestxt) の値を考えます。

[**trips.txt**](../../reference/#tripstxt)

| route_id | trip_id     | block_id  |
|----------|-------------|---|
| RouteA   | RouteATrip1 |  Block1 |
| RouteB   | RouteBTrip1 |  Block1 |

[**stop_times.txt**](../../reference/#stop_timestxt)

| trip_id | arrival_time     | departure_time | stop_id | stop_sequence |
|----------|-------------|---|----|-----|
| RouteATrip1  | 12:00:00|  12:01:00 | A | 1 |
| RouteATrip1  | 12:05:00|  12:06:00 | B | 2 | 
| RouteATrip1 | 12:15:00 | | C | 3|
| RouteBTrip1 | | 12:18:00 | C | 1 |
| RouteBTrip1 |12:22:00 | 12:23:00 | D | 2 |
| RouteBTrip1 |12:30:00 |  | E | 3 | 

この例では:

- 停留所等(stop) A から停留所等(stop) E までのルートを検索するユーザーは、Route A の 12:00 に停留所等(stop) A で乗車し、`RouteATrip1` の終了後に車両が停留所等(stop) C に到着した際も車両にとどまるよう案内されます。これは、同じ車両が Route B の `RouteBTrip1` を運行するためです。
- `RouteATrip1` の乗客で `RouteBTrip1` 上の停留所等(stop)まで移動を続けたい場合、この乗り換えでは車両にとどまることができます。
- これらと同じルート・路線系統(route)上を運行する他の車両の他の便(trip)の乗客には、便(trip)ごとに異なる車両を使用するため、この選択肢はありません。

### 環状路線におけるブロック乗換 {: #block-transfer-in-a-loop-line}


環状路線では、便(trip)の最初の停留所等(stop)と最後の停留所等(stop)は同一であり、同じ`stop_id`を持ちます。連続する環状便(trip)が同じ`block_id`を持つ場合、ブロック乗換または着席したままの乗換が有効になり、最初の便(trip)の乗客は、車両が次の周回を継続する際にそのまま乗車し続けることができます。
