# ルート・路線系統(route)、停留所等(stop)、および便(trip) {: #routes-stops-and-trips}

## ルート・路線系統(route) {: #routes}


ルート・路線系統(route)は、交通ネットワークの地理的な到達範囲を記述するため、固定経路型公共交通の運行の中核となります。GTFS では、ルート・路線系統(route)を定義することが、交通事業者の運行を記述する最初のステップです。 

最初のステップは、以下のファイル [agency.txt](../../reference/#agencytxt) に示すように事業者情報を追加することです。このファイルには、事業者に関する概要情報が含まれます。 

[**agency.txt**](../../reference/#agencytxt)

```
agency_id,agency_name,agency_url,agency_timezone,agency_lang,agency_phone
CT,Calgary Transit,http://www.calgarytransit.com,America/Edmonton,,403-262-1000
```

Calgary Transit は、AB州カルガリーで LRT、BRT、通常のバスサービス、パラトランジット、およびオンデマンド交通を運行しています。この例では、2つのルート・路線系統(route)が定義されており、1つ目はバス、2つ目は LRT です。ファイル [routes.txt](../../reference/#routestxt) を使用して、各ルート・路線系統(route)には一意の ID と、人間が読みやすいように短縮名および正式名が割り当てられます。

[**routes.txt**](../../reference/#routestxt)

```
agency_id,route_id,route_short_name,route_long_name,route_type,route_url,route_color,route_text_color
CT,303-20670,303,MAX Orange Brentwood/Saddletowne,3,www.calgarytransit.com/content/transit/en/home/rider-information/max.html,#ff8000,#ffffff
CT,202-20666,202,Blue Line - Saddletowne/69 Street CTrain,0,www.calgarytransit.com/content/transit/en/home/rider-information/lrt-and-bus-station-maps.html,#ff0000,#ffffff
```

5番目のフィールド（`route_type`）は、ルート・路線系統(route)の種類を区別するために使用されます。

- 1つ目はバスであるため、`route_type=3` です
- 2つ目は LRT であるため、`route_type=0` です
- `route_type` の値の完全な一覧は[こちら](../../reference/#routestxt)で確認できます

残りのフィールドには、ルート・路線系統(route)固有の URL や、地図上でサービスを表すための事業者固有の色など、追加情報が含まれます。

<hr>

## 停留所等(stop) {: #stops}


GTFS では、停留所等(stop)および駅はファイル [stops.txt](../../reference/#stopstxt) を使用して記述されます。以下では、最初のレコードでバス停が定義され、2番目のレコードで LRT 駅が定義されています。 

[**stops.txt**](../../reference/#stopstxt) 

```
stop_id,stop_code,stop_name,stop_lat,stop_lon,location_type
8157,8157,44th Avenue NE (SB),51.091106,-113.958565,0
6810,6810,NB Marlborough CTrain Station,51.058990,-113.981582,1
```

- `stop_id` は一意の識別子です
- `stop_code` および `stop_name` には通常、乗客向けの情報が含まれます
- 正確な位置は座標（`stop_lat` および `stop_lon`）を使用して提供されます
- 6番目のフィールド（`location_type`）は、停留所等(stop)と駅を区別するために使用されます
- 最初のレコードはバス停に対応するため、`location_type=0` です
- 2番目のレコードは駅に対応するため、`location_type=1` です
- `location_type `の値の完全な一覧は[こちら](../../reference/#stopstxt)で確認できます。

<hr>

## 便(trips) {: #trips}


事業者のルート・路線系統(route)を説明した後、各ルート・路線系統(route)で運行される便(trip)を説明できるようになります。 

まず、[calendar.txt](../../reference/#calendartxt)を使用して運行期間を定義する必要があります。

[**calendar.txt**](../../reference/#calendartxt) 

```
service_id,monday,tuesday,wednesday,thursday,friday,saturday,sunday,start_date,end_date
weekend_service,0,0,0,0,0,1,1,20220623,20220903
```

ここでは、土曜日と日曜日にのみ運行するサービスを説明しているため、それらの日のフィールドには1が設定され、残りの日のフィールドには0が設定されています。このサービスは、`start_date`および`end_date`フィールドに示されているとおり、2022年6月23日から2022年9月3日まで運行されます。 

この例では、ファイル[trips.txt](../../reference/#tripstxt)が、上記で説明したMAX Orangeルート・路線系統(route)で運行される3つの週末の便(trip)を説明しています。

[**trips.txt**](../../reference/#tripstxt) 

```
route_id,service_id,trip_id,trip_headsign,direction_id,shape_id
303-20670,weekend_service,60270564,"MAX ORANGE SADDLETOWNE",0,3030026
303-20670,weekend_service,60270565,"MAX ORANGE BRENTWOOD",1,3030027
303-20670,weekend_service,60270566,"MAX ORANGE BRENTWOOD",1,3030027
```

- MAX Orangeに対応する[routes.txt](../../reference/#routestxt)の`route_id`が記載されています
- 週末に対応する[calendar.txt](../../reference/#calendartxt)の`service_id`が記載されています
- 各レコードには、各便(trip)の一意のIDが含まれています。
行先表示(headsign)テキストが提供されており、これは通常、バスの車内および車外の標識に表示されるものです
- フィールド`direction_id`により、同じルート・路線系統(route)で異なる方向に向かう便(trip)を区別できます。たとえば、上り便と下り便、または南行き便と北行き便を区別できます。 
    - この場合、Saddletowne方面の便(trip)には`direction_id=0`が設定され、Brentwood方面の便(trip)には`direction_id=1`が設定されています。direction_idの値自体に固有の意味はなく、ある移動方向と別の移動方向を割り当てるためにのみ使用されます
- 最初のレコードにはSaddletowne方面のMAX Orangeルート・路線系統(route)に対応する[shapes.txt](../../reference/#shapestxt)の`shape_id`が記載され、2番目および3番目のレコードにはBrentwood方面のMAX Orangeルート・路線系統(route)に対応するものが記載されています


`shape_id=3030026`は、Saddletowne方面のMAX Orangeに対応します。以下のファイルには、便(trip)の経路を構成する地点に関する情報と、各地点から便(trip)の開始地点までの距離が含まれています。この情報により、旅程(journey)計画または分析の目的で地図上にルート・路線系統(route)を描画できます。

[**shapes.txt**](../../reference/#shapestxt) 

```
shape_id,shape_pt_lat,shape_pt_lon,shape_pt_sequence,shape_dist_traveled
3030026,51.086506,-114.132259,10001,0.000
3030026,51.086558,-114.132371,10002,0.010
3030026,51.086781,-114.132865,10003,0.052
3030026,51.086938,-114.133179,10004,0.080
3030026,51.086953,-114.133205,10005,0.083
3030026,51.086968,-114.133224,10006,0.085
3030026,51.086992,-114.133249,10007,0.088
3030026,51.087029,-114.133275,10008,0.093
3030026,51.087057,-114.133286,10009,0.096
3030026,51.087278,-114.133356,10010,0.121
3030026,51.087036,-114.132864,10011,0.165
3030026,51.086990,-114.132766,10012,0.173
3030026,51.086937,-114.132663,10013,0.183
```

<hr>

## 運行例外 {: #service-exceptions}


追加の運行日（特別日）や、運休日（祝日に運行しない場合など）といった運行の例外を定義することができます。

例えば、2022年7月17日の日曜日に予定された運行がない場合、その日付は `weekend_service` を2つに分割することで、[calendar.txt](../../reference/#calendartxt) の `weekend_service` から削除できます。

| 運行 | 開始 | 終了 |
| ----- | ----- | ----- |
| `weekend_service1` | `20220623` | `20220716` |
| `weekend_service2` | `20220718` | `20220903` |

ただし、`service_id` が2つに分割され、この分割が [trips.txt](../../reference/#tripstxt) にも波及するため、ファイルが複雑になります。代わりに、以下に示すように [calendar_dates.txt](../../reference/#calendar_datestxt) を使用することで、より簡単に実現できます。

[**calendar_dates.txt**](../../reference/#calendar_datestxt) 

```
service_id,date,exception_type
weekend_service,20220717,2
```

- `service_id` `weekend_service` が記載されています 
- 削除または追加された運行の日付が `date`（2022年7月17日）に記載されています 
- フィールド `exception_type` は2に設定されており、この日は運行が削除されることを意味します 

<sup>[例の出典](https://data.calgary.ca/download/npk7-z3bj/application%2Fzip)</sup>
