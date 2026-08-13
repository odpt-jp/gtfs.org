# 便の変更(trip modification) {: #trip-modifications}


`TripModifications` メッセージは、迂回などの特定の変更の影響を受ける、(CSV) GTFS 内の類似した `trip_ids` のリストを識別します。

<br><br>**注意:** このエンティティは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される場合があります。

## SLO: サービスレベル目標 {: #slo-service-level-objective}


データ更新の頻度は、おおよそ毎時（約24回/日）であることが想定されています。取り込み時間は、影響を受ける便(trip)の総数に依存する場合があります。コンシューマーは、単一のTripModificationを5分以内に取り込み、数百件の迂回を含むフィードを20分以内に取り込むことが想定されています。

## TripModifications {: #tripmodifications}


`TripModifications` は、フィードから削除されるまで、列挙されたすべての service_dates において有効です。任意の運行日において、1つの便(trip)を複数の `TripModifications` オブジェクトに割り当ててはいけません。

特定の停留所等(stop)パターンに対して複数の `TripModifications` が存在することがあります。例えば、迂回の途中で `propagated_modification_delay` が大きく変化する場合、便(trip)を複数の変更に分割することが望ましい場合があります。

GTFS-TripModifications を通じて作成される便(trip)は、指定された各 `trip_id` を変更して置き換えるものであり、コピーや追加運行を作成するものではありません。変更は、静的 GTFS（CSV）が変更された場合と同様に、スケジュール情報に適用されます。 

各置換便(trip)の予定停車時刻(stop_time)は、影響を受ける便(trip)の予定停車時刻(stop_time)から、変更に列挙された変更を実行することで作成されます。すべての停車時刻(stop_time)の `stop_sequence` は、最初の stop_time を 1 とし、便(trip)内の停留所等(stop)ごとに 1 ずつ増加する、1 から n までの新しい値に置き換えられます。置換便(trip)のリアルタイムの到着時刻・出発時刻を公開するには、`TripUpdate` メッセージを提供しなければなりません。

## TripUpdates へのリンク {: #linkage-to-tripupdates}


* TripUpdate は、TripUpdate の `TripDescriptor` 内で `ModifiedTripSelector` を使用して提供するべきです。
    * TripUpdate が代替便を参照する場合、コンシューマーは、静的 GTFS が TripModifications によって変更されたかのように動作するべきです（例: 代替停留所における `arrival_time`、`departure_time`、`stop_sequence`、`stop_id`）。
    * `ModifiedTripSelector` を提供する場合、`ModifiedTripSelector` の値を探していないコンシューマーの混乱を避けるため、`TripDescriptor` の `trip_id`、`route_id`、`direction_id`、`start_time`、`start_date` フィールドは空のままにしなければなりません。
    * `ModifiedTripSelector` を含む更新を提供する TripUpdate フィードは、TripModifications をサポートしないクライアントを対象とする TripUpdate も含めるべきです。言い換えると、2つの TripUpdate が存在するべきです。1つは変更された便を使用するクライアント向け（`TripModifications` あり）、もう1つは元の変更されていない GTFS を使用するクライアント向け（`TripModifications` なし）です。
    * `ModifiedTripSelector` を含む TripUpdate を提供することが、代替停留所で予測を作成する唯一の方法です。
* そのような TripUpdate が見つからない場合、元の `trip_id` の TripUpdate が変更された便に適用されます。
    * この場合、使用する静的 GTFS 情報は、TripModifications が適用される前の静的 GTFS のものであるべきです。
    * 前の便と新しい変更済み便の共通停留所等(stop)ではリアルタイム情報を利用できることがありますが、代替停留所では到着予定時刻を利用できません。

## 変更 {: #modification}


`Modification` メッセージは、`start_stop_selector` から開始する影響を受ける各便(trip)への変更を記述します。`Modification` によって置き換えられる停車時刻(stop_time)は、0件、1件、または複数件存在することができます。変更の範囲は重複してはいけません。範囲は連続していてはいけません。この場合、2つの変更を1つに統合しなければなりません。これらの停車時刻(stop_time)は、`replacement_stops` で記述される各置換停留所等(stop)の新しい停車時刻(stop_time)に置き換えられます。

`replacement_stops` のシーケンスは任意の長さにすることができます。例えば、状況に応じて、3つの停留所等(stop)を2つ、4つ、または0個の停留所等(stop)に置き換えることができます。

![](/../assets/trip-modification.png)

_特定の便(trip)に対する変更の影響を示す例です。この変更は、他の複数の便(trip)にも適用されることがあります。_

![](/../assets/propagated-delay.png)

_迂回による伝播遅延は、変更の終了後に続くすべての停留所等(stop)に影響します。便(trip)に複数の変更がある場合、遅延は累積されます。_

## 代替停留所等(ReplacementStop) {: #replacementstop}


各 `ReplacementStop` メッセージは、便(trip)が新たに訪問する停留所等(stop)を定義し、任意でその停留所等(stop)までの推定所要時間を指定します。`ReplacementStop` メッセージは、その停留所等(stop)の予定 `stop_time` を構築するために使用されます。

`travel_time_to_stop` が指定されている場合、`arrival_time` は元の便(trip)内の基準停留所等(stop)から、`travel_time_to_stop` のオフセットを加えて計算されます。それ以外の場合、`arrival_time` は元の便(trip)における変更の総所要時間に基づいて補間することができます。

`departure_time` は常に `arrival_time` と等しくなります。

(CSV) GTFS 仕様における [`stop_times.txt`](../../../schedule/reference/#stop_timestxt) の任意フィールドはすべて、デフォルト値に設定されます。

![](/../assets/first-stop-reference.png)

_変更が便(trip)の最初の停留所等(stop)に影響する場合、その停留所等(stop)は変更の基準停留所等(stop)としても機能します。_
