# 旧運賃モデル (v1) {: #legacy-fares-v1}

旧運賃モデル (v1) は、初期の基本的な GTFS 運賃モデルであり、限られた運賃要素のみをサポートしています。このモデルでは、出発地から目的地までの運賃、ゾーンベースの運賃、ルートベースの運賃構造、そして最小限の乗り換えルールのみを表現することができます。後方互換性のために GTFS において引き続き利用可能ではありますが、より包括的なチケット発行、支払い情報、そしてより広範な運賃構造およびルールをサポートする [運賃モデル (v2)](../../examples/fares-v2/) への移行が推奨されています。

[fare_attributes.txt](../../reference/#fare_attributestxt) と [fare_rules.txt](../../reference/#fare_rulestxt) で構成される旧運賃モデル (v1) は、歴史的に GTFS における運賃情報を記述する公式な方法でした。しかし、これら2つのファイルは効率的に記述できる要素の範囲が限られており、実装において曖昧さが残るという制約があります。

## 事業者の運賃ルールを定義する {: #define-an-agencys-fare-rules}

トロント交通委員会（Toronto Transit Commission、TTC）の地下鉄ネットワークでは、乗客が PRESTO カードを使用して支払う場合、1回の乗車料金は 3.20 カナダドルです。乗客は、2時間以内であれば、TTC が運行する他の地下鉄、ストリートカー、またはバス路線に乗り換えることもできます。

このサービスは、[fare_attributes.txt](../../reference/#fare_attributestxt)、[fare_rules.txt](../../reference/#fare_rulestxt)、および [transfers.txt](../../reference/#transferstxt) のファイルを使用して表現することができます。最初のファイル [fare_attributes.txt](../../reference/#fare_attributestxt) は事業者の運賃を記述しており、以下は PRESTO 運賃の例です。

[**fare_attributes.txt**](../../reference/#fare_attributestxt)

```
fare_id,price,currency_type,payment_method,transfers,transfer_duration
presto_fare,3.2,CAD,1,,7200
```

- 運賃の金額は `price` および `currency_type` に記載されています。
- 乗客は地下鉄に乗車する前に、駅の改札口で運賃を支払う必要があります。これは `payment_method=1` で表されます。
- `transfers` フィールドは空欄のままにしておき、無制限の乗り換えを表します。
- `transfer_duration` フィールドは、2時間の乗り換え可能時間（秒単位）に対応します。

2つ目のファイル [fare_rules.txt](../../reference/#fare_rulestxt) は、運賃をルートおよびそのルート上の出発地／目的地に関連付けることで、旅程に運賃を割り当てます。

そのために、以下のように [routes.txt](../../reference/#routestxt) で2つの地下鉄路線を定義します。

[**routes.txt**](../../reference/#routestxt)

```
agency_id,route_id,route_type
TTC,Line1,1
TTC,Line2,1
```

この例では、ブロア・ヤング駅（Bloor-Yonge Station）での乗り換えをモデル化しています。この駅は2つの別々の停留所(stop)としてモデル化されます。1つ目は Line 1 によって運行される Bloor Station、2つ目は Line 2 によって運行される Yonge Station です。両方の停留所には、すべての地下鉄駅を1つの運賃ゾーンにまとめるために `zone_id=ttc_subway_stations` が設定されています。

[**stops.txt**](../../reference/#stopstxt)

```
stop_id,stop_name,stop_lat,stop_lon,zone_id
Bloor,Bloor Station,,43.670049,-79.385389,ttc_subway_stations
Yonge,Yonge Station,,43.671049,-79.386789,ttc_subway_stations
```

[fare_rules.txt](../../reference/#fare_rulestxt) では、PRESTO 運賃が次の関係を使用して両方の地下鉄路線および駅に関連付けられています。

- `fare_id=presto_fare` の場合、乗客は Line 1（`route_id=line1`）上の任意の2駅間を、`origin_id=ttc_subway_stations` および `destination_id=ttc_subway_stations` として移動することができます。

[**fare_rules.txt**](../../reference/#fare_rulestxt)

```
fare_id,route_id,origin_id,destination_id
presto_fare,line1,ttc_subway_stations,ttc_subway_stations
presto_fare,line2,ttc_subway_stations,ttc_subway_stations
```

3つ目のファイル [transfers.txt](../../reference/#transferstxt) は、異なる路線間の乗り換えポイントを定義します。ブロア・ヤング駅での乗り換えをモデル化するためには、2つのエントリが必要です。

[**transfers.txt**](../../reference/#transferstxt)

```
from_stop_id,to_stop_id,from_route_id,to_route_id,transfer_type
Bloor,Yonge,line1,line2,0
Yonge,Bloor,line2,line1,0
```

- 1つ目のエントリは、Bloor Station から Yonge Station への Line 1 から Line 2 への乗り換えを、`from_route_id` および `to_route_id` を使用してモデル化しています。
- 2つ目のエントリは、Yonge Station から Bloor Station への Line 2 から Line 1 への乗り換えを、`from_route_id` および `to_route_id` を使用してモデル化しています。
- `transfer_type` の値は `0` であり、乗り換えに特別な要件や考慮事項がないことを示しています。

<sup>[例の出典](https://www.ttc.ca/Fares-and-passes)</sup>
