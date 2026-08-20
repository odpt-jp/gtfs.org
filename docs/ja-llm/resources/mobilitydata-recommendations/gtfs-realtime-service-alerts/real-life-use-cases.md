# 実際のユースケース {: #real-life-use-cases}


このセクションでは、実際の運行情報(alert)に関するすべての可能なユースケースを検討します。各ユースケースについて、最適な運行情報(alert)の実践方法を含めます。

## ルート・路線系統(route)の運休 {: #route-closure}


サービス変更の例:

* ルート・路線系統(route)が一定期間運休します。

影響:

* `NO_SERVICE`

通知対象エンティティ:

* `route_id`  
* `direction_id`（ルート・路線系統(route)がある方向に沿って運行していない場合）。

提案:

* 以下を明確に記載した運行情報(alert)を追加してください:  
    * ルート・路線系統(route)の運休があること（ヘッダー内）。  
    * 影響を受けるルート・路線系統(route)の名称（ヘッダー内）。  
    * 該当する場合、影響を受けるルート・路線系統(route)の方向（ヘッダー内）。  
    * サービス中断が及ぶ期間（説明内。ヘッダーが長くなりすぎない場合は、追加でヘッダーに含めることもできます）。  
    * 代替サービスがある場合は、その情報（説明内）。

* `tripUpdates`が提供される場合、可能であれば、影響を受ける便(trip)の`TripDescriptor`内の`ScheduleRelationship`をCANCELEDに変更してください。  
* 代替サービスがあり、作成可能な場合は、次のいずれかを行うことができます:  
    * 運休が計画されている場合、または運休が長期化し運休に関する確実性が高まった場合は、GTFS Schedule feed内に作成してください。  
    * その代替サービスがGTFS内のルート・路線系統(route)に対応しており、追加の便(trip)をGTFS Schedule feed内で定義できない場合は、`TripDescriptor = NEW`を使用して`tripUpdates`内に作成してください。

!!! example "推奨テンプレート"

    ヘッダー: 「ルート・路線系統(route) `{route name}` のサービス中断」

    説明: 「{cause}により、ルート・路線系統(route)／路線`{route name}`のサービスは、`{start time}`から`{end time if applicable}`まで運休します。代わりに`{replacement suggestion}`をご利用ください。」

---

## 便の運休 {: #trip-cancellation}


サービス変更の例:

* 特定の便(trip)が運休します。

影響:

* `NO_SERVICE`

通知対象エンティティ:

* `route_id`  
* `direction_id`（ルート・路線系統(route)がある方向に沿って運行していない場合）。  
* TripDescriptor を使用した `trip_id`。  
    * 便(trip)が頻度ベースの場合、単一の便(trip)に一致させるため、TripDescriptor の下に `start_time` および `start_date` を追加します。

提案:

* 以下を明確に記載した運行情報(alert)を追加します。  
    * 便(trip)の運休があること（ヘッダー内）。  
    * 影響を受けるルート・路線系統(route)の名称（ヘッダー内）。  
    * 該当する場合、影響を受けるルート・路線系統(route)の方向（ヘッダー内）。  
    * 可能であれば、開始時刻など、運休する便(trip)の人間が読める識別子（説明内）。  
    * 代替サービスがある場合、その情報（説明内）。

* `tripUpdates` が提供される場合、可能であれば、影響を受ける便(trip)の `TripDescriptor` 内の `ScheduleRelationship` を CANCELED に変更します。  
* 代替サービスがあり、作成可能な場合は、次のいずれかを行うことができます。  
    * 運休が計画されている場合、または運休が長期化し運休に関する確実性が高まった場合は、GTFS Schedule フィード内に作成します。  
    * その代替サービスが GTFS 内のルート・路線系統(route)に対応し、追加の便(trip)を GTFS Schedule フィード内で定義できない場合は、`TripDescriptor \= NEW` を使用して `tripUpdates` 内に作成します。

!!! example "推奨テンプレート"

    ヘッダー: 「ルート・路線系統(route) `{route name}` の便(trip)が運休」

    説明: 「{cause}により、`{direction}` 方向のルート・路線系統(route) `{route name}` では、一部の便(trip)が運休します。現在、`{human-readable trip identifier, e.g. start time}` の便(trip)が影響を受けています。代わりに `{replacement suggestion}` をご利用ください。」

---

## ルート区間の閉鎖 {: #route-segment-closure}


サービス変更の例:

* バスが2つの停留所等(stop)間を運行しません。

影響:

* `NO_SERVICE`  
* 消費者が運行情報(alert)を使用して旅程(journey)の提案または経路探索を調整することが想定され、かつ `NO_SERVICE` の影響を設定すると乗客に不正確な情報が提示される可能性がある場合は、`MODIFIED_SERVICE` を使用することもできます。

通知対象エンティティ:

* `route_id`  
* `direction_id`（ルート・路線系統(route)がある方向に沿って運行していない場合）。  
* `stop_id`  
    * バスルート・路線系統(route)が `stop_A` から `stop_B` まで運行していない場合、`route_id`、`direction_id`、およびその方向における `stop_A` と `stop_B` の間のすべての `stop_ids` を含めてください。  
    * 区間の最初と最後の停留所等(stop)が障害の影響を受けない場合は、通知対象エンティティに含めてはいけません。実際に閉鎖されている区間の停留所等(stop)のみを含めてください。

提案:

* 以下を明確に記載した運行情報(alert)を追加してください。  
    * ルート区間の閉鎖があること（ヘッダー内）。  
    * 影響を受けるルート・路線系統(route)の名称（ヘッダー内）。  
    * 方向（ヘッダー内）および影響を受ける区間の両端の停留所等(stop)（ヘッダー内および/または説明内）。  
    * サービス障害が継続する期間（説明内。ヘッダーが長くなりすぎない場合は、追加でヘッダーに含めることもできます）。  
    * 代替サービスがある場合は、その情報（説明内）。

* `tripUpdates` が提供される場合、可能であれば、関連する各便(trip)の影響を受ける `stop_times` の `StopTimeUpdate` における `ScheduleRelationship` を SKIPPED に変更してください。  
* 代替サービスがあり、作成可能な場合は、以下のいずれかを行うことができます。  
    * 閉鎖が計画されている場合、または閉鎖が長期間に及び閉鎖に関する確実性が高まった場合は、GTFS Schedule フィード内に作成してください。  
    * その代替サービスが既存のルート・路線系統(route)によって運行されており、GTFS Schedule フィード内で定義できない場合は、`TripDescriptor \= NEW` を使用して `tripUpdates` 内に作成してください。

!!! example "推奨テンプレート"

    ヘッダー: 「ルート・路線系統(route) `{route name}` は `{start stop}` と `{end stop}` の間で運行しません」

    説明: 「{cause}のため、ルート・路線系統(route) `{route name}` は、`{start time}` から `{end time if applicable}` まで、`{start stop}` と `{end stop}` の間の停留所等(stop)には停車しません。代わりに `{replacement suggestion}` をご利用ください。」

---

## 停留所等(stop)の閉鎖 {: #stop-closure}


サービス変更の例:

* 停留所等(stop)が乗車および降車のために閉鎖されます。  
* 駅のプラットフォームが乗車および降車のために閉鎖されます。

影響:

* `NO_SERVICE`

通知対象エンティティ:

* `stop_id`

提案:

* 以下を明確に記載した運行情報(alert)を追加してください。  
    * 停留所等(stop)の閉鎖があること（ヘッダー内）。  
    * 影響を受ける停留所等(stop)またはプラットフォームの名称（ヘッダー内）。  
    * サービス中断が及ぶ期間（説明内。ヘッダーが長くなりすぎない場合は、ヘッダーにも追加で含めることができます）。

* `tripUpdates`が提供される場合、可能であれば、影響を受ける`stop_times`の`StopTimeUpdate`におけるScheduleRelationshipをSKIPPEDに変更してください。  
* 閉鎖された停留所等(stop)に停車する便(trip)について、[`stop_times.txt`](https://gtfs.org/documentation/schedule/reference/#stop_timestxt)の停車時刻(stop_time)エントリから`stop_id`を削除してください。これは、運行情報(alert)が長期間（数週間から数か月）に及ぶ場合により重要です。  
* 停留所等(stop)が恒久的に閉鎖されることが確実な場合は、GTFS Scheduleフィードから削除してください。それ以外の場合、[`stops.txt`](https://gtfs.org/documentation/schedule/reference/#stopstxt)から停留所等(stop)を削除する必要はありません。 

!!! example "推奨テンプレート"

    ヘッダー: 「停留所等(stop) `{stop name and location}` は運休中です」

    説明: 「{cause}により、停留所等(stop) `{stop name and location}` は`{start time}`から`{end time if applicable}`まで運休します。代わりに`{replacement stop}`をご利用ください。」

---

## 駅閉鎖 {: #station-closure}


サービス変更の例:

* 駅が、すべてのプラットフォーム、入口および出口を含めて工事のため閉鎖されています。列車は当該駅を通過し続けますが、乗客の乗車または降車はできません。

影響:

* `NO_SERVICE`

通知対象エンティティ:

* 駅の `stop_id`。

提案:

* 以下を明確に記載した運行情報(alert)を追加してください。  
    * 駅閉鎖があること（ヘッダー内）。  
    * 影響を受ける駅の名称（ヘッダー内）。  
    * サービス中断が及ぶ期間（説明内。ヘッダーが長くなりすぎない場合は、ヘッダーにも追加で含めることができます）。

* `tripUpdates` が提供される場合、可能であれば、影響を受ける `stop_times` の `StopTimeUpdate` における `ScheduleRelationship` を SKIPPED に変更してください。  
* 閉鎖された駅に停車する便(trip)について、[`stop_times.txt`](https://gtfs.org/documentation/schedule/reference/#stop_timestxt) の停車時刻(stop_time)エントリから駅プラットフォームの `stop_ids` を削除してください。これは、事象が長期間（数週間から数か月）にわたる場合により重要です。  
* 駅が恒久的に閉鎖されることが確実な場合は、GTFS Schedule フィードから駅およびその子停留所等(stop)を削除してください。それ以外の場合、[`stops.txt`](https://gtfs.org/documentation/schedule/reference/#stopstxt) から駅およびその停留所等(stop)を削除する必要はありません。

!!! example "推奨テンプレート"

    ヘッダー: 「駅 `{station name}` は運休中です」

    説明: 「{cause}により、駅 `{station name}` は`{start time}`から`{end time if applicable}`まで運休となります。代わりに `{bus replacement service or nearest in-service station}` をご利用ください。」

---

## 迂回 {: #detour}


サービス変更の例:

* バス路線が経路から逸脱し、それにより一部の停留所等(stop)の位置が移動します。  
* NYC地下鉄の路線が、別の路線の線路を経由して経路変更されます。

影響:

* `DETOUR`

通知対象エンティティ:

* `route_id`  
* `direction_id`（迂回の方向）。  
* `stop_id`（迂回される区間内の停留所等(stop)の`stop_ids`）を含めます。  
* インシデント中に一部の便(trip)のみが迂回する場合は、可能であればTripDescriptorを使用して`trip_id`を含めます。

提案:

* 迂回によって停留所等(stop)が移動せず、ルート・路線系統(route)のルート形状(shape)のみが変更される場合は、`DETOUR`運行情報(alert)を作成しないでください。その場合、迂回によって大幅な遅延が発生する場合は、インシデントを[`SIGNIFICANT_DELAYS`運行情報(alert)](#strong-delays)として扱ってください。

* 以下を明確に記載した運行情報(alert)を追加してください。  
    * 迂回があること（ヘッダー内）。  
    * 影響を受けるルート・路線系統(route)の名称（ヘッダー内）。  
    * 方向（ヘッダー内）  
    * サービス中断が及ぶ期間（説明内。ヘッダーが長くなりすぎない場合は、追加でヘッダーに含めることもできます）。  
    * 迂回によって一部の停留所等(stop)の位置が変更される場合は、影響を受ける停留所等(stop)、または影響を受ける区間の開始および終了の停留所等(stop)（ヘッダーおよび/または説明内）。  
    * 迂回によってルート・路線系統(route)のルート形状(shape)のみが変更され、すべての停留所等(stop)が元の位置に維持される場合は、停留所等(stop)が影響を受けないこと（ヘッダーおよび/または説明内）。

* `tripModifications`が提供される場合は、`tripModifications`を使用して、新しい停留所等(stop)の`stop_ids`、新しい`stop_times`、および伝播した遅延を示してください。詳細については、[`tripModifications`リファレンス](https://gtfs.org/documentation/realtime/feed-entities/trip-modifications/)を参照してください。  
* 迂回が計画されている場合は、GTFS Scheduleフィードに反映されていることを確認してください。迂回が予定外で長期間に及ぶ場合は、より確実になった時点でGTFS Scheduleフィードに追加することを検討してください。

!!! example "推奨テンプレート"

    ヘッダー: 「ルート・路線系統(route) `{route name}` は `{start stop}` と `{end stop}` の間で迂回します」

    説明: 「`{cause}`により、ルート・路線系統(route) `{route name}` は迂回します。`{start stop}` と `{end stop}` の間の停留所等(stop)には停車せず、`{start time}`から`{end time if applicable}`まで`{replacement streets/roads}`を経由して運行します。代わりに`{replacement stops}`をご利用ください。」

---

## 短縮運行 {: #short-turn}


サービス変更の例:

* TTCのストリートカーが短縮運行を行い、運行の均衡を保つため、すべての停留所等(stop)を完了する前に反対方向へ折り返します。

影響:

* `NO_SERVICE`  
* 消費者が運行情報(alert)を使用して旅程(journey)の提案または経路探索を調整することが想定され、それを行わない場合に乗客へ不正確な情報が提示される可能性があるときは、`MODIFIED_SERVICE` を使用することもできます。

通知対象エンティティ:

* `route_id`  
* `direction_id`（ルート・路線系統(route)の方向）。  
* 短縮運行が発生する便(trip)の `trip_id`（TripDescriptor を使用）。  
* `stop_id`（短縮運行により通過される `stop_ids`）

提案:

* 以下を明確に記載した運行情報(alert)を追加してください。  
    * 短縮運行があること（ヘッダー内）。  
    * 影響を受けるルート・路線系統(route)の名称（ヘッダー内）。  
    * 方向（ヘッダー内）。  
    * サービス中断が及ぶ期間（説明内。ヘッダーが長くなりすぎない場合は、追加でヘッダーに含めることもできます）。  
    * 影響を受ける便(trip)（説明内）。  
    * 短縮運行により通過される停留所等(stop)（ヘッダーおよび/または説明内）。

* `tripUpdates` が提供される場合、可能であれば、影響を受ける `stop_times` の `StopTimeUpdate` における ScheduleRelationship を SKIPPED に変更してください。

!!! example "推奨テンプレート"

    ヘッダー: 「ルート・路線系統(route) `{route name}` は `{stop name}` の手前で短縮運行します」

    説明: 「ルート・路線系統(route) `{route name}` は `{stop name}` の手前で短縮運行します。`{start stop}` から `{end stop}` までの停留所等(stop)には停車しません。影響を受ける便(trip)は `{human-readable trip identifier. e.g. Trip start time}` です」

---

## 停留所等(stop)の移設 {: #stop-moved}


サービス変更の例:

* 工事により、停留所等(stop)の場所が一時的に変更されます。  
* 停留所等(stop)が恒久的に移設されます（情報提供の目的で、運行情報(alert)はしばらく残ることがあります）。

影響:

* `STOP_MOVED`

通知対象エンティティ:

* `stop_id`（場所が変更された停留所等(stop)のもの）。

提案:

* 以下を明確に記載した運行情報(alert)を追加してください。  
    * 停留所等(stop)が移設されたこと（ヘッダー内）。  
    * 停留所等(stop)の名称（ヘッダー内）。  
    * 停留所等(stop)の新しい場所（説明内）。  
    * サービス変更が適用される期間（説明内。ヘッダーが長くなりすぎない場合は、ヘッダーにも追加で含めることができます）。

* 停留所等(stop)が GTFS Schedule フィード内で変更された場合でも、運行情報(alert)が存在することがあります。（情報提供のみを目的とするためです。GTFS 内に `stop_id` がないため、不一致の問題につながる可能性があります）。  
* 運行情報(alert)が予定されている、または長期間（数週間から数か月）にわたる場合は、新しい停留所等(stop)を反映するために [`stops.txt`](https://gtfs.org/documentation/schedule/reference/#stopstxt) および [`stop_times.txt`](https://gtfs.org/documentation/schedule/reference/#stop_timestxt) を更新することを検討してください。

!!! example "推奨テンプレート"

    ヘッダー: 「停留所等(stop) `{stop name and location}` は移設されました」

    説明: 「{cause} により、停留所等(stop) `{stop name and location}` は `{definitely or indefinitely until {end time if applicable}}`、`{new location}` に移設されます。」

---

## 軽微な遅延 {: #light-delays}


サービス変更の例:

* バスのルート・路線系統(route)で小規模な遅延が発生しています。

提案:

* `tripUpdates` では、影響を受ける `stop_times` の `StopTimeUpdate` が調整されていることを確認してください。  
* 重要でない遅延は `tripUpdates` に含めるべきです。運行情報(alert)を作成してはいけません。

---

## 大幅な遅延 {: #strong-delays}


サービス変更の例:

* バス路線が交通渋滞により大幅な遅延を経験しています。  
* 迂回運行により、あるルート・路線系統(route)で大幅な遅延が発生しています。

影響:

* `SIGNIFICANT_DELAYS`

対象エンティティ:

* `route_id`  
* `direction_id`（ルート・路線系統(route)の遅延が一方向のみの場合）。

提案:

* 以下を明確に記載した運行情報(alert)を追加してください。  
    * 遅延が発生していること（ヘッダー内）。  
    * 影響を受けるルート・路線系統(route)の名称（ヘッダーおよび/または説明内）。  
    * サービス中断が及ぶ期間（説明内。ヘッダーが長くなりすぎない場合は、追加でヘッダーに含めることもできます）。

* `tripUpdates` が提供されている場合は、影響を受ける `stop_times` の `StopTimeUpdate` が調整されていることを確認してください。

!!! example "推奨テンプレート"

    ヘッダー: 「ルート・路線系統(route) `{route name}` の遅延運行」

    説明: 「{cause} により、ルート・路線系統(route) `{route name}` では最大 `{estimated current delay}` の遅延が発生しています。」

---

## 特別ダイヤ／ダイヤ変更 {: #special-scheduleschedule-modification}


運行変更の例:

* 夏季／冬季ダイヤが開始されます。  
* 運行間隔が変更される短期間の催事向け特別ダイヤ。

影響:

* `MODIFIED_SERVICE`

通知対象エンティティ:

* `route_id`（変更された運行がルート・路線系統(route)に影響する場合）。  
* `direction_id`（変更された運行が一方向にのみ沿う場合）。

提案:

* 以下を明確に記載した運行情報(alert)を追加してください。  
    * 特別運行または変更された運行であること（ヘッダー内）。  
    * 影響を受けるルート・路線系統(route)の名称（ヘッダーおよび／または説明内）。  
    * 運行変更が適用される期間（説明内。ヘッダーが長くなりすぎない場合は、ヘッダーにも追加で含めることができます）。

!!! example "推奨テンプレート"

    ヘッダー: 「`{route names}`のダイヤ調整：`{start time}`から`{end time if applicable}`まで」

    説明: 「{原因: 新サービス、短期的な変更}により、`{routes names}`の運行を`{start time}`から`{end time if applicable}`まで調整します。詳細については、当社ウェブサイト`{URL (also included in the URL field)}`をご参照ください。」

    または

    説明: 「{原因: 新サービス、短期的な変更}により、`{routes names}`は`{start time}`から`{end time if applicable}`まで、`{special schedule: holiday, weekend, etc}`ダイヤで運行します。詳細については、当社ウェブサイト`{URL (also included in the URL field)}`をご参照ください。」

---

## 運行本数の増加 {: #service-increase}


サービス変更の例:

* 特別な催事または不十分なサービスを補うため、短期間に追加の列車が運行されます。  
* 列車の便(trip)の運休を補うため、バスの運行スケジュールが増便されます。

影響:

* `ADDITIONAL_SERVICE`

通知対象エンティティ:

* `route_id`（増便がルート・路線系統(route)に影響する場合）。  
* `direction_id`（変更されたサービスが一方向に沿う場合のみ）。

提案:

* 以下を明確に記載した運行情報(alert)を追加してください。  
    * サービスが増加していること（ヘッダー内）。  
    * 影響を受けるルート・路線系統(route)の名称（ヘッダーおよび/または説明内）。  
    * サービス変更が適用される期間（説明内。ヘッダーが長くなりすぎない場合は、追加でヘッダーに含めることもできます）。

* 追加の便(trip)が GTFS Schedule フィードに追加されない場合:

    * `tripUpdates` が提供されており、かつサービス増加が予定外で、サービス変更の7日前までに GTFS Schedule フィードへ追加できない場合: `tripUpdates` において、`TripDescriptor` の `ScheduleRelationship` を `NEW` に設定して新しい便(trip)を追加してください。  
    * GTFS Schedule フィードで便(trip)を追加できる場合は、便(trip)の追加に GTFS realtime TripUpdates を過度に依存してはいけません。

!!! example "推奨テンプレート"

    ヘッダー: 「`{route names}` の運行本数を増加」

    説明: 「`{cause: new service, short term change}` により、`{start time}` から `{end time if applicable}` まで、`{routes names}` の出発便が増加します。詳細については、当社ウェブサイト `{URL (also included in the URL field)}` をご確認ください。」

---

## 運行本数の削減 {: #service-cuts}


サービス変更の例:

* 短期間のバス運行本数の削減。

影響:

* `REDUCED_SERVICE`

通知対象エンティティ:

* `route_id`（運行本数の削減がルート・路線系統(route)に影響する場合）。  
* `direction_id`（運行本数の削減が一方向のみに沿っている場合）。

提案:

* 以下を明確に記載した運行情報(alert)を追加してください:  
    * サービスが削減されていること（ヘッダー内）。  
    * 影響を受けるルート・路線系統(route)の名称（ヘッダーおよび/または説明内）。  
    * サービス変更の対象期間（説明内。ヘッダーが長くなりすぎない場合は、追加でヘッダーに含めることもできます）。

* 削除された便(trip)がGTFS Scheduleフィードから削除されていない場合:

    * `tripUpdates`が提供されており、かつ運行本数の削減が予定外で、サービス変更の7日前までにGTFS Scheduleフィードから削除できない場合: 可能であれば、`tripUpdates`内で、影響を受ける便(trip)の`TripDescriptor`にある`ScheduleRelationship`をCANCELEDに変更してください。  
    * GTFS Scheduleフィードで便(trip)を削除できる場合は、便(trip)の削除についてGTFS realtime TripUpdatesに過度に依存してはいけません。

!!! example "推奨テンプレート"

    ヘッダー: 「`{route names}`の運行便数を削減」

    説明: 「{原因: 新サービス、短期的な変更}により、`{start time}`から{該当する場合はend time}まで、`{routes names}`の出発便数を削減します。詳細については、当社ウェブサイト`{URL (URLフィールドにも含めます)}`をご参照ください。」

---

## アクセシビリティの問題 {: #accessibility-issue}


サービス変更の例:

* 駅のエレベーターが故障しています。  
* 駅内のアクセシビリティ用スロープが通行不能です。  
* バスのアクセシビリティ用スロープが通行不能です。

影響:

* `ACCESSIBILITY_ISSUE`

通知対象エンティティ:

* 必要に応じて `route_id`、`trip_id`（TripDescriptor を使用）、および `direction_id`（アクセシビリティの問題がルート・路線系統(route)の車両で発生している場合）。  
* 影響を受けるプラットフォームおよび出入口の `stop_id`、ならびに必要に応じて `direction_id`。  
* 駅の `stop_id` は必須ではありません。駅は GTFS Schedule feed を使用して特定できます。

提案:

* 以下を明確に記載した運行情報(alert)を追加してください。  
    * アクセシビリティの問題があること（ヘッダー内）。  
    * サービス中断が継続する期間（説明内。ヘッダーが長くなりすぎない場合は、追加でヘッダーに含めることもできます）。  
    * アクセシビリティの問題が車両で発生している場合  
        * 影響を受けるルート・路線系統(route)の名称（ヘッダーおよび/または説明内）。  
        * 影響を受けるルート・路線系統(route)の方向（ヘッダーおよび/または説明内）。  
        * 該当する場合、アクセシビリティの問題が発生している正確な便(trip)の識別子（説明内）。  
    * アクセシビリティの問題が駅で発生している場合  
        * 影響を受ける駅の名称（ヘッダーおよび/または説明内）。  
        * アクセスに影響を受けるプラットフォーム（説明内）。  
        * 影響を受けた正確な構内通路(pathway)（エレベーター、スロープなど）（説明内）。

!!! example "推奨テンプレート"

    ヘッダー: “`{station name}`: {pathway. e.g. Southbound platform elevator}` は故障しています。”

    説明: “`{cause: maintenance, mechanical error, etc}` により、`{station}` の `{platform}` への `{pathway}` は、`{start_time}` から `{end_time}` まで利用できません。代わりに `{replacement pathway}` をご利用ください。”

!!! example "推奨テンプレート"

    ヘッダー: “`{route names}`: {human-readable trip identifier. e.g. trip start time} の車両は利用できません”

    説明: “`{cause: e.g. mechanical error}` により、`{route names}` の便(trip) `{human-readable trip identifier}` を運行する車両は利用できません。”
