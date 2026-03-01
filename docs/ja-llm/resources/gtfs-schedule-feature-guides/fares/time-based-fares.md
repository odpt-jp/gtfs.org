# 時間帯別運賃 {: #time-based-fares}

*主なファイル: fare_leg_rules.txt, timeframes.txt*  
*例: [Translink (バンクーバー)](../intro/#translink-vancouver)*

!!! info "リマインダー"

    時間帯別運賃(Time-Based Fares)は、特定の時間帯や曜日に応じて運賃を設定するもので、ピーク時運賃、オフピーク時運賃、週末運賃などが含まれます。詳細については、イントロダクションページの[機能セクション](../intro/#fares-features-and-their-files)を参照してください。

## チケット商品と有効運賃区間ルールの作成 {: #create-fare-products-and-fare-leg-rules}

チケット商品と有効運賃区間ルールを作成します。運賃体系に基づいて、該当するチケット商品と有効運賃区間ルールを追加してください。

* ルート・路線系統(route)-ベースの運賃については、[Route-Based Fares](../route-based-fares) セクションをご覧ください。  
* ゾーンベースの運賃については、[Zone-Based Fares](../zone-based-fares) セクションをご覧ください。

この例では、Translink に対して、Route-Based Fares セクションおよび Zone-Based Fares セクションの両方で、ルート・路線系統ベースおよびゾーンベースの運賃が定義されています。

## 時間枠の作成 {: #create-timeframes}

時間枠(timeframe)とは、異なる運賃が適用される期間を囲む時間帯のことです。  
例: ピーク運賃のための平日朝のラッシュ時間帯を囲む時間枠、割引運賃のための週末夜間を囲む時間枠など。

時間枠には、運賃が有効となる曜日と時間帯の両方が含まれます。これらは `timeframes.txt` に次のように作成します。

1. **timeframe_group_id** に時間枠グループのIDを入力します。  
2. **start_time** に特別運賃の時間枠の開始時刻を入力します。  
3. **end_time** に特別運賃の時間枠の終了時刻を入力します。  
4. **service_id** に、`calendar.txt` または `calendar_dates.txt` の **service_id** を参照するIDを入力します。これにより、時間ベースの運賃を特定の運行日または運行日の範囲に関連付けることができます。

`timeframes.txt` の詳細および設定方法については、[ドキュメントを参照してください](../../../reference/#timeframestxt)。

!!! Note  

	時間枠は end_time の1秒前に終了します。  
	例: `end_time=11:00:00` の場合、時間枠は 10:59:59 に終了します。  
	時間枠が深夜をまたぐ場合は、同じ `timeframe_group_id` を持つ2つの時間枠行に分割しなければなりません。24:00:00 を超える値は使用してはいけません。

!!! info "リマインダー"

    Translink では、夜間（午後6時30分から午前3時）および週末に割引運賃を提供しています。すべての SkyTrain および Seabus の運賃は1ゾーン運賃（CAD 3.20）になります。Sea Island からの夜間および週末の移動は CAD 8.20（CAD 5.00 の追加料金 + 1ゾーン運賃 CAD 3.20）です。

この設定は `timeframes.txt` ファイルを使用してモデル化することができます。  
時間ベースの割引運賃（チケット商品および運賃区間ルール）は、前のセクション（[ルートベース運賃](../route-based-fares) セクション、[ゾーンベース運賃](../zone-based-fares) セクション）で作成したすべてのチケット商品に追加されます。

この例では、3つの時間枠グループを作成します。

まず、`weekday_daytime` 時間枠を午前3時から午後6時30分までとして定義します。

次に、`weekday_evening` 時間枠は深夜をまたぐため、2つの部分に分割します。午後6時30分から深夜まで、そして深夜から午前3時までです。両方の部分は平日運行サービスに関連付けられます。

最後に、週末全体をカバーする `weekend` 時間枠を作成します。この時間枠では、`start_time` および `end_time` を空欄にしておくことで、`weekend_service` の全期間に適用されることを意味します。

[**timeframes.txt**](../../../reference/#timeframestxt)

| timeframe_group_id | start_time | end_time | service_id |
| :---- | :---- | :---- | :---- |
| weekday_daytime | 03:00:00 | 18:30:00 | weekday_service |
| weekday_evening | 18:30:00 | 24:00:00 | weekday_service |
| weekday_evening | 00:00:00 | 03:00:00 | weekday_service |
| weekend |  |  | weekend_service |

以下は、`timeframes.txt` に登場する service_id を示す `calendar.txt` の簡単な例です。

[**calendar.txt**](../../../reference/#calendartxt)

| service_id | monday | tuesday  | wednesday | thursday | friday | saturday | sunday | start_date | end_date |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| weekday_service | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 20250101 | 20251231 |
| weekend_service | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 20250101 | 20251231 |

## 運賃区間ルールを時間枠に関連付ける {: #associate-fare-leg-rules-to-timeframes}

運賃区間ルールは、運賃が適用される時間帯においてのみ区間のマッチングが行われるように、異なる時間枠に関連付けられます。これは `fare_leg_rules.txt` において次のように行われます。

1. 運賃区間ルールの **from_timeframe_group_id** および **to_timeframe_group_id** に、運賃が適用される時間枠（または時間枠グループ）の ID を入力します。  
    * これは `timeframes.txt` の **timeframe_group_id** を参照する外部キーです。  
2. 同じ **leg_group_id** を複製し、異なる **from_timeframe_group_id**、**to_timeframe_group_id**、および **fare_product_id** を設定して、同じマッチングルール（**network_id**、**from_area_id**、**to_area_id**）を持ちながら、異なる時間枠および運賃を表現します。

!!! Note

    Translink では、区間の終了時刻が運賃に影響するかどうかの情報がないため、影響しないものと仮定します。つまり、区間が `weekday_daytime` の時間枠内で開始した場合、たとえ異なる時間枠で終了しても、その区間は `weekday_daytime` の一部として扱われます。

この例では、`flat_fare_leg` が 2 回繰り返されており、1 回は `weekday_evening` の時間枠、もう 1 回は `weekend` の時間枠に対応しています。これにより、平日夜間および週末において、SkyTrain および Seabus に対して 1ゾーン／均一運賃を適用できるようになります。

さらに、`flat_fare_sea_island_leg` は、`sea_island` から出発する Sea Island 区間を、`daytime_evening` および `weekend` の時間枠において、任意のゾーンに対して `sea_island_1_zone_fare` に関連付けるために作成されました。

Sea Island 区間に対して `rule_priority=1` を設定することで、追加の 5.00 カナダドル運賃を適用する優先順位を維持します。平日昼間以外のすべての運賃が 1ゾーン運賃であるため、追加の 5.00 カナダドルは 1ゾーン運賃に加算され、Sea Island 発の便の新しい運賃は `sea_island_1_zone_fare`（5.00 + 3.20 = 8.20 カナダドル）となります。

!!! Note

    `rule_priority` 列が存在する場合、`from_area_id`（または `to_area_id`）が空欄であるときは、出発ゾーン `from_area_id`（または到着ゾーン `to_area_id`）が区間のマッチングに影響しないことを意味します。同様に、`from_timeframe_group_id` または `to_timeframe_group_id` のいずれかが空欄である場合、そのフィールドはマッチング処理において無関係であることを意味します。

**[fare_leg_rules.txt](../../../reference/#fare-leg-rulestxt)（ファイル全体）**	

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

## rule_priority を使用した簡略化 {: #simplify-using-rule_priority}


!!! info "リマインダー"

    `rule_priority` フィールドは、一致するルールが適用される順序を決定します。同じ条件に一致するルールの集合において、`rule_priority` の値が高いルールは、値が低いまたは空のルールよりも優先されます。

平日夜間および週末の運賃が均一運賃または1ゾーン運賃と同じであるため、**rule_priority** を使用してさらに簡略化することができます。これは `fare_leg_rules.txt` において次のように行います。

1. 夜間／週末の乗車区間(leg)に高い **rule_priority** を割り当てます。  
2. 平日日中の乗車区間から時間帯の関連付けを削除します。

この例では、平日夜間および週末の乗車区間に高い `rule_priority` を設定し、平日日中の乗車区間の `rule_priority` フィールドを空のままにしておく（これは 0 を設定するのと同じです）ことで、夜間および週末の乗車区間がその時間帯において平日日中の乗車区間よりも優先されます。これにより、正しい運賃が計算されます。

* 平日夜間および週末の `flat_fare_leg` には `rule_priority=1` が割り当てられ、他のすべての均一運賃区間やゾーンベースの区間よりも優先されます。したがって、旅程(journey)が `weekday_evening` または `weekend` の時間帯内で行われる場合、均一運賃の乗車区間が他のすべての区間（Sea Island 区間を除く）よりも優先的に選択されます。  
* 夜間および週末の `flat_fare_leg_sea_island` には `rule_priority=2` が割り当てられ、これにより、これらの時間帯において Sea Island から出発する他の区間（前の [Zone-Based Fares](../zone-based-fares) セクションで `rule_priority=1` が割り当てられていたもの）よりも優先されます。  
* `sea_island_sea_island_leg` には `rule_priority=3` が割り当てられ、常に `from_area_it=sea_island` かつ `to_area_it=sea_island` に一致する他のすべての区間よりも優先されます。これにより、時間帯に関係なく、Sea Island 内での無料運賃が常に保証されます。

**[fare_leg_rules.txt](../../../reference/#fare_leg_rulestxt)（全ファイル）**	

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
