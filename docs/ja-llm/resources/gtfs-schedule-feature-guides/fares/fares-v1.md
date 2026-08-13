# レガシー運賃 (v1) {: #legacy-fares-v1}


レガシー運賃 (v1) は、限定的な運賃要素をサポートする、以前の基本的な GTFS 運賃モデルです。出発地から目的地まで、ゾーンベース、およびルートベースの運賃体系と、最小限の乗換規則のみをモデル化できます。後方互換性の目的で GTFS の機能として引き続き存在していますが、包括的な発券および支払い情報を提供し、はるかに幅広い運賃体系と規則をサポートするため、[運賃 (v2)](../../examples/fares-v2/) への移行が推奨されます。

[fare_attributes.txt](../../reference/#fare_attributestxt) および [fare_rules.txt](../../reference/#fare_rulestxt) で構成されるレガシー運賃 (v1) は、歴史的に GTFS における運賃情報の記述の公式な方法でした。しかし、この2つのファイルは、効率的に記述できる要因の範囲が限定されており、実装方法が曖昧です。

## 事業者の運賃ルールを定義する {: #define-an-agencys-fare-rules}


Toronto Transit Commission の地下鉄ネットワークでは、乗客が PRESTO card を使用して支払う場合、1回の乗車に $3.20 CAD がかかります。乗客は、2時間の時間枠内で TTC が運行する他の地下鉄、streetcar、またはバスのルート・路線系統(route)にも乗り換えることができます。

このサービスは、[fare_attributes.txt](../../reference/#fare_attributestxt)、[fare_rules.txt](../../reference/#fare_rulestxt)、および [transfers.txt](../../reference/#transferstxt) のファイルを使用して表現できます。最初のファイルである [fare_attributes.txt](../../reference/#fare_attributestxt) は事業者の運賃を記述します。以下は presto fare の例です。

[**fare_attributes.txt**](../../reference/#fare_attributestxt)

```
fare_id,price,currency_type,payment_method,transfers,transfer_duration
presto_fare,3.2,CAD,1,,7200
```

- 運賃の価格は price および `currency_type` に記載されます
- 乗客は地下鉄に乗車する前に、駅の運賃ゲートで運賃を支払わなければなりません。これは `payment_method=1` で表現されます
- 無制限の乗り換えを表現するため、フィールド transfers は空欄のままにします
- フィールド `transfer_duration` は、2時間の乗り換え時間枠（秒単位）に対応します

2番目のファイルである [fare_rules.txt](../../reference/#fare_rulestxt) は、運賃をルート・路線系統(route)およびそのルート・路線系統(route)上の出発地／目的地に関連付けることで、運賃を旅程(journey)に割り当てます。 

そのため、以下の [routes.txt](../../reference/#routestxt) で2つの地下鉄路線を定義します。

[**routes.txt**](../../reference/#routestxt)

```
agency_id,route_id,route_type
TTC,Line1,1
TTC,Line2,1
```

この例では、Bloor-Yonge Station での乗り換えをモデル化します。そのため、この駅は2つの別個の停留所等(stop)としてモデル化されます。1つ目は Line 1 が運行する Bloor Station、2つ目は Line 2 が運行する Yonge Station です。すべての地下鉄駅を単一の運賃ゾーンにグループ化するため、両方に `zone_id=ttc_subway_stations` を設定します。 

[**stops.txt**](../../reference/#stopstxt)

```
stop_id,stop_name,stop_lat,stop_lon,zone_id
Bloor,Bloor Station,,43.670049,-79.385389,ttc_subway_stations
Yonge,Yonge Station,,43.671049,-79.386789,ttc_subway_stations
```

[fare_rules.txt](../../reference/#fare_rulestxt) では、以下の関係を使用して、PRESTO fare を両方の地下鉄路線および駅に関連付けます。

- `fare_id=presto_fare` の場合、乗客は Line 1（`route_id=line1`）上の任意の2駅間を、`origin_id=ttc_subway_stations` および `destination_id=ttc_subway_stations` で移動できます。

[**fare_rules.txt**](../../reference/#fare_rulestxt) 

```
fare_id,route_id,origin_id,destination_id
presto_fare,line1,ttc_subway_stations,ttc_subway_stations
presto_fare,line2,ttc_subway_stations,ttc_subway_stations
```

3番目のファイルである [transfers.txt](../../reference/#transferstxt) は、異なるルート・路線系統(route)間の乗り換え地点を定義します。Bloor-Yonge station での乗り換えをモデル化するには、2つのエントリが必要です。

[**transfers.txt**](../../reference/#transferstxt) 

```
from_stop_id,to_stop_id,from_route_id,to_route_id,transfer_type
Bloor,Yonge,line1,line2,0
Yonge,Bloor,line2,line1,0
```

- 1つ目は、Bloor Station から Yonge Station への `from_route_id` および `to_route_id` を使用して、Line 1 から Line 2 への乗り換えをモデル化します
- 2つ目は、Yonge Station から Bloor Station への `from_route_id` および `to_route_id` を使用して、Line 2 から Line 1 への乗り換えをモデル化します
- 乗り換えに関する特定の要件または考慮事項がないため、`transfer_type` の値は `0` です

<sup>[例の出典](https://www.ttc.ca/Fares-and-passes)</sup>
