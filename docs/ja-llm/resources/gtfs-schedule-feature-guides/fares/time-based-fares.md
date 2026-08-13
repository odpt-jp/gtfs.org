# 時間帯別運賃 {: #time-based-fares}


*主なファイル: fare_leg_rules.txt、timeframes.txt*  
*例: [Translink（バンクーバー）](../intro/#translink-vancouver)*

!!! info "注意"

    時間帯別運賃では、ピーク時・オフピーク時の運賃や週末運賃など、特定の時刻帯または曜日に対して運賃を設定します。詳細については、Introduction ページの[機能のセクション](../intro/#fares-features-and-their-files)を再確認してください。

## チケット商品と運賃区間ルールを作成する {: #create-fare-products-and-fare-leg-rules}


チケット商品と運賃区間ルールを作成します。運賃体系に基づいて、関連するチケット商品と運賃区間ルールを追加します。

* ルート・路線系統(route)ベースの運賃については、[Route-Based Fares](../route-based-fares)セクションを参照してください。  
* ゾーンベースの運賃については、[Zone-Based Fares](../zone-based-fares)セクションを参照してください。

この例では、Translink のルート・路線系統(route)ベースおよびゾーンベースの運賃の両方が、Route-Based Fares および Zone-Based Fares セクションで定義されています。

## 時間枠を作成する {: #create-timeframes}


時間枠は、異なる運賃の期間を囲む時間帯です。例: ピーク運賃のための平日朝のラッシュアワーを囲む時間枠、割引運賃のための週末の夜間を囲む時間枠などです。

時間枠には、運賃が有効となる曜日と時刻の両方が含まれます。これらは次のように `timeframes.txt` で作成されます。

1. **timeframe_group_id** に、時間枠グループの ID を入力します。  
2. **start_time** に、特別運賃の時間枠の開始時刻を入力します。  
3. **end_time** に、特別運賃の時間枠の終了時刻を入力します。  
4. **service_id** に、`calendar.txt` または `calendar_dates.txt` の **service_id** を参照する ID を入力します。これにより、時間ベースの運賃を運行日(service day)または運行日の範囲に対応付けることができます。

`timeframes.txt` およびその設定方法の詳細については、[ドキュメントを参照してください](../../../reference/#timeframestxt)。

!!! 注記  

	時間枠は end_time の1秒前に終了します。例: `end_time=11:00:00` の場合、時間枠は 10:59:59 に終了します。  
	時間枠が深夜をまたぐ場合、同じ `timeframe_group_id` を持つ2つの時間枠行に、深夜で分割するべきです。24:00:00 を超える値は禁止されています。

!!! info "リマインダー"

    Translink は、夜間（午後6時30分から午前3時まで）および週末に割引運賃を提供しています。すべての SkyTrain および Seabus の運賃は1ゾーン運賃（CAD 3.20）になります。夜間および週末に Sea Island から移動する場合は、CAD 8.20（CAD 5.00 の追加料金 + CAD 3.20 の1ゾーン運賃）がかかります。

これは、`timeframes.txt` ファイルを使用してモデル化できます。より安価な時間ベースの運賃（チケット商品および有効運賃区間(effective fare leg)ルール）は、前のセクション（[ルート・路線系統(route)ベースの運賃](../route-based-fares)セクション、[ゾーンベースの運賃](../zone-based-fares)セクション）で作成されたすべてのチケット商品に追加されます。

この例では、3つの時間枠グループが作成されます。 

まず、午前3時から午後6時30分までの `weekday_daytime` 時間枠を定義します。

次に、`weekday_evening` 時間枠は深夜をまたぐため、午後6時30分から深夜まで、および深夜から午前3時までの2つの部分に分割されます。両方の部分は平日の運行日に関連付けられます。

最後に、週末の終日を対象とする `weekend` 時間枠を作成します。この時間枠では、`start_time` と `end_time` を空欄のままにします。これは、`weekend_service` の全期間に適用されることを意味します。

[**timeframes.txt**](../../../reference/#timeframestxt)

| timeframe_group_id | start_time | end_time | service_id |
| :---- | :---- | :---- | :---- |
| weekday_daytime | 03:00:00 | 18:30:00 | weekday_service |
| weekday_evening | 18:30:00 | 24:00:00 | weekday_service |
| weekday_evening | 00:00:00 | 03:00:00 | weekday_service |
| weekend |  |  | weekend_service |

以下は、`timeframes.txt` に出現する service_id を示す `calendar.txt` の概要です。

[**calendar.txt**](../../../reference/#calendartxt)

| service_id | monday | tuesday  | wednesday | thursday | friday | saturday | sunday | start_date | end_date |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| weekday_service | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 20250101 | 20251231 |
| weekend_service | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 20250101 | 20251231 |

## 運賃区間ルールを時間枠に関連付ける {: #associate-fare-leg-rules-to-timeframes}


運賃区間ルールは、運賃が適用される時間帯に乗車区間(leg)の照合が制限されるよう、異なる時間枠に関連付けられます。これは `fare_leg_rules.txt` で次のように行います。

1. 運賃が適用される時間枠（または時間枠グループ）の ID を、運賃区間ルールの **from_timeframe_group_id** および **to_timeframe_group_id** に入力します。  
    * これは `timeframes.txt` の **timeframe_group_id** を参照する外部キーです。  
2. 同様の照合ルール（**network_id**、**from_area_id**、**to_area_id**）を持つものの、時間枠および料金が異なる乗車区間(leg)を表すため、同じ **leg_group_id** を異なる **from_timeframe_group_id**、**to_timeframe_group_id**、および **fare_product_id** で複製します。

!!! 注記

    Translink では、乗車区間(leg)の終了時刻が運賃に影響するかどうかに関する情報がないため、影響しないものと仮定します。これは、乗車区間(leg)が `weekday_daytime` の時間枠中に開始する場合、異なる時間枠中に終了したとしても、その時間枠の一部として扱われることを意味します。

この例では、`flat_fare_leg` は `weekday_evening` の時間枠用と `weekend` の時間枠用に、それぞれ1回ずつ、計2回繰り返されています。これにより、夜間および週末における SkyTrain と Seabus への1ゾーン／均一料金運賃の関連付けが可能になります。

さらに、`flat_fare_sea_island_leg` は、`daytime_evening` および `weekend` 中に `sea_island` から任意のゾーンへ出発する Sea Island の乗車区間(leg)を、`sea_island_1_zone_fare` に関連付けるために作成されました。

Sea Island の乗車区間(leg)では `rule_priority=1` とすることで、追加の CAD 5.00 運賃を適用する際の優先順位が維持されます。平日昼間以外のすべての運賃は1ゾーン運賃であるため、追加の CAD 5.00 は1ゾーン運賃に適用され、Sea Island を出発する便(trip)の新しい運賃は、CAD 5.00 + CAD 3.20 = CAD 8.20 の金額を持つ `sea_island_1_zone_fare` となります。

!!! 注記

    `rule_priority` 列が存在する場合、`from_area_id`（それぞれ `to_area_id`）を空欄にすると、出発ゾーン `from_area_id`（それぞれ到着ゾーン `to_area_id`）は乗車区間(leg)の照合に影響しないことを意味します。同様に、`from_timeframe_group_id` と `to_timeframe_group_id` のいずれかを空欄にすると、そのフィールドは照合プロセスに無関係となります。

**[fare_leg_rules.txt](../../../reference/#fare_leg_rulestxt)（完全なファイル）**	

| leg_group_id | network_id | fare_product_id | from_area_id | to_area_id | from_timeframe_group_id | to_timeframe_group_id | rule_priority |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| flat_fare_leg | translink_bus | bus_flat_fare |  |  |  |  |  |
| ZN1_ZN1 | skytrain_seabus | 1_zone_fare | ZN1 | ZN1 | weekday_daytime |  |  |
| ZN2_ZN2 | skytrain_seabus | 1_zone_fare | ZN2 | ZN2 | weekday_daytime |  |  |
| ZN3_ZN3 | skytrain_seabus | 1_zone_fare | ZN3 | ZN3 | weekday_daytime |  |  |
| ZN1_ZN2 | skytrain_seabus | 2_zone_fare | ZN1 | ZN2 | weekday_daytime |  |  |
| ZN2_ZN3 | skytrain_seabus | 2_zone_fare | ZN2 | ZN3 | weekday_daytime |  |  |
| ZN1_ZN2 | skytrain_seabus | 2_zone_fare | ZN2 | ZN1 | weekday_daytime |  |  |
| ZN2_ZN3 | skytrain_seabus | 2_zone_fare | ZN3 | ZN2 | weekday_daytime |  |  |
| ZN1_ZN3 | skytrain_seabus | 3_zone_fare | ZN1 | ZN3 | weekday_daytime |  |  |
| ZN1_ZN3 | skytrain_seabus | 3_zone_fare | ZN3 | ZN1 | weekday_daytime |  |  |
| sea_island_ZN1 | skytrain_seabus | sea_island_2_zone_fare | sea_island | ZN1 | weekday_daytime |  | 1 |
| sea_island_ZN2 | skytrain_seabus | sea_island_1_zone_fare | sea_island | ZN2 | weekday_daytime |  | 1 |
| sea_island_ZN3 | skytrain_seabus | sea_island_2_zone_fare | sea_island | ZN3 | weekday_daytime |  | 1 |
| sea_island_sea_island | skytrain_seabus | sea_island_sea_island_fare | sea_island | sea_island |  |  | 2 |
| flat_fare_leg | skytrain_seabus | 1_zone_fare |  |  | weekday_evening |  |  |
| flat_fare_leg | skytrain_seabus | 1_zone_fare |  |  | weekend |  |  |
| flat_fare_sea_island_leg | skytrain_seabus | sea_island_1_zone_fare | sea_island |  | weekday_evening |  | 1 |
| flat_fare_sea_island_leg | skytrain_seabus | sea_island_1_zone_fare | sea_island |  | weekend |  | 1 |

## rule_priority を使用した簡素化 {: #simplify-using-rule_priority}


!!! info "注意"

    `rule_priority` フィールドは、一致するルールが適用される順序を決定します。同じ条件に一致するルールのセットでは、`rule_priority` 値が高いルールが、低い値または空の値を持つルールより優先されます。

平日夜間および週末の運賃は均一運賃または1ゾーン運賃と同じであるため、**rule_priority** を使用してさらに簡素化できます。これは `fare_leg_rules.txt` で次のように行います。

1. 夜間／週末の乗車区間(leg)により高い **rule_priority** を割り当てます。  
2. 平日日中の乗車区間(leg)から時間枠の関連付けを削除します。

この例では、平日夜間および週末の乗車区間(leg)により高い `rule_priority` を設定し、平日日中の乗車区間(leg)の `rule_priority` フィールドを空のままにします（これは 0 に設定することと同じです）。これにより、平日夜間および週末の乗車区間(leg)が有効である場合（それらの時間枠の間）、平日日中の乗車区間(leg)より優先されます。これにより、正しい運賃が計算されます。

* 平日夜間および週末の乗車区間(leg) `flat_fare_leg` には `rule_priority=1` が割り当てられ、他のすべての均一運賃の乗車区間(leg)またはゾーンベースの乗車区間(leg)より優先されます。そのため、旅程(journey)が時間枠 `weekday_evening` または `weekend` 内で行われる場合、優先されるため、均一運賃の乗車区間(leg)が他のすべての乗車区間(leg)（Sea Island の乗車区間(leg)を除く）より優先して選択されます。  
* 夜間および週末の乗車区間(leg) `flat_fare_leg_sea_island` には `rule_priority=2` が割り当てられ、これらの時間枠の間、Sea Island を出発する他の乗車区間(leg)（前の[ゾーンベース運賃](../zone-based-fares)セクションで `rule_priority` に 1 が割り当てられたもの）より優先されます。  
* 乗車区間(leg) `sea_island_sea_island_leg` には `rule_priority=3` が割り当てられ、常に `from_area_it=sea_island` および `to_area_it=sea_island` に一致する他のすべての乗車区間(leg)より優先されます。これにより、時間枠にかかわらず、常に Sea Island 内の無料運賃が保証されます。

**[fare_leg_rules.txt](../../../reference/#fare_leg_rulestxt)（完全なファイル）**	

| leg_group_id | network_id | fare_product_id | from_area_id | to_area_id | from_timeframe_group_id | to_timeframe_group_id | rule_priority |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| flat_fare_leg | translink_bus | bus_flat_fare |  |  |  |  |  |
| ZN1_ZN1 | skytrain_seabus | 1_zone_fare | ZN1 | ZN1 |  |  |  |
| ZN2_ZN2 | skytrain_seabus | 1_zone_fare | ZN2 | ZN2 |  |  |  |
| ZN3_ZN3 | skytrain_seabus | 1_zone_fare | ZN3 | ZN3 |  |  |  |
| ZN1_ZN2 | skytrain_seabus | 2_zone_fare | ZN1 | ZN2 |  |  |  |
| ZN2_ZN3 | skytrain_seabus | 2_zone_fare | ZN2 | ZN3 |  |  |  |
| ZN1_ZN2 | skytrain_seabus | 2_zone_fare | ZN2 | ZN1 |  |  |  |
| ZN2_ZN3 | skytrain_seabus | 2_zone_fare | ZN3 | ZN2 |  |  |  |
| ZN1_ZN3 | skytrain_seabus | 3_zone_fare | ZN1 | ZN3 |  |  |  |
| ZN1_ZN3 | skytrain_seabus | 3_zone_fare | ZN3 | ZN1 |  |  |  |
| sea_island_ZN1 | skytrain_seabus | sea_island_2_zone_fare | sea_island | ZN1 |  |  | 1 |
| sea_island_ZN2 | skytrain_seabus | sea_island_1_zone_fare | sea_island | ZN2 |  |  | 1 |
| sea_island_ZN3 | skytrain_seabus | sea_island_2_zone_fare | sea_island | ZN3 |  |  | 1 |
| sea_island_sea_island | skytrain_seabus | sea_island_sea_island_fare | sea_island | sea_island |  |  | 3 |
| flat_fare_leg | skytrain_seabus | 1_zone_fare |  |  | weekday_evening |  | 1 |
| flat_fare_leg | skytrain_seabus | 1_zone_fare |  |  | weekend |  | 1 |
| flat_fare_sea_island_leg | skytrain_seabus | sea_island_1_zone_fare | sea_island |  | weekday_evening |  | 2 |
| flat_fare_sea_island_leg | skytrain_seabus | sea_island_1_zone_fare | sea_island |  | weekend |  | 2 |
