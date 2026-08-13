# GTFS Realtime {: #gtfs-realtime}

### 改訂履歴 {: #revision-history}

#### 2026年6月 {: #june-2026}


* Service Alerts に、active_periodをより適切に定義するための2つの新しいフィールド、communication_periodおよびimpact_periodを追加しました。[議論](https://github.com/google/transit/pull/546)を参照してください。
* Service Alerts のCauseリストに新しいSPECIAL_EVENT Causeを追加しました。[議論](https://github.com/google/transit/pull/577)を参照してください。

#### 2026年4月 {: #april-2026}


* gtfs-realtime.proto の誤字を修正しました。[議論](https://github.com/google/transit/pull/541)を参照してください。

#### 2026年2月 {: #february-2026}


* selectedTrips の trip_ids 要件および多重度を修正しました。[議論](https://github.com/google/transit/pull/609)を参照してください。
* 不足していたスペースを追加しました。[議論](https://github.com/google/transit/pull/587)を参照してください。

#### 2025年5月 {: #may-2025}


* schedule_relationship `ADDED` を非推奨とし、`NEW` を採用するとともに `REPLACEMENT` を追加します。[議論](https://github.com/google/transit/pull/504)を参照してください。
* 便の変更(trip modification)についてさらに明確化しました。[議論](https://github.com/google/transit/pull/542)を参照してください。

#### 2024年12月 {: #december-2024}


* リアルタイムデータの基となる GTFS Schedule フィードの feed_info.feed_version に一致する新しい文字列フィールドを追加しました。[議論](https://github.com/google/transit/pull/434)を参照してください。

#### 2024年10月 {: #october-2024}


* 便の変更に関する明確化および小規模な変更。[議論](https://github.com/google/transit/pull/497)を参照してください。

#### 2024年3月 {: #march-2024}


* 便の変更を採用しました。 [議論](https://github.com/google/transit/pull/403)を参照してください。

#### 2022年11月 {: #november-2022}


* DELETED の便(trip)のサポートを追加しました。[議論](https://github.com/google/transit/pull/352)を参照してください。

#### 2022年7月 {: #july-2022}


* cause_detail および effect_detail を追加しました。[議論](https://github.com/google/transit/pull/332)を参照してください。
* TripUpdate.VehicleDescriptor で wheelchair_accessible 値を指定する機能を追加しました。[議論](https://github.com/google/transit/pull/340)を参照してください。

#### 2021年9月 {: #september-2021}


* 運行情報(alert)における機能/画像。[議論](https://github.com/google/transit/pull/283)を参照してください。

#### 2021年8月 {: #august-2021}


* GTFS-NewShapes を実験的機能として追加します。[議論](https://github.com/google/transit/pull/272)を参照してください。

#### 2021年4月 {: #april-2021}


* TripUpdate に departure_occupancy_status を追加します。[議論](https://github.com/google/transit/pull/260)を参照してください。

#### 2021年2月 {: #february-2021}


* GTFS Realtime の occupancy の説明を明確化しました。[議論](https://github.com/google/transit/pull/259)を参照してください。

#### 2020年9月 {: #september-2020}


* 複数車両の混雑度をサポートします。[議論](https://github.com/google/transit/pull/237)を参照してください。

#### 2020年4月 {: #april-2020}


* 停留所等(stop)の割り当てをサポートします。[議論](https://github.com/google/transit/pull/219)を参照してください。

#### 2020年7月 {: #july-2020}


* DUPLICATED 便(trip)をサポートします。[議論](https://github.com/google/transit/pull/221)を参照してください。
* Alert tts_header_text、tts_description_textは、もはや実験的ではありません。[議論](https://github.com/google/transit/pull/229)を参照してください。
* GTFS-RT ADDED 便(trip)を完全には指定されていないものとしてラベル付けします。[議論](https://github.com/google/transit/pull/230)を参照してください。

#### 2020年4月 {: #april-2020}


* SeverityLevel を最終版としてマークします。[議論](https://github.com/google/transit/pull/214)を参照してください。
* occupancy_percentage を追加します。[議論](https://github.com/google/transit/pull/213)を参照してください。

#### 2020年3月12日 {: #march-12-2020}


* ブロック内の次の便(trip)について、TripUpdate の予測を提供することを推奨します。[議論](https://github.com/google/transit/pull/206)を参照してください。

#### 2019年8月 {: #august-2019}


* trip_updates がフィード内でブロック順に発生する必要はないことを文書化しました。[議論](https://github.com/google/transit/pull/176)を参照してください。
* StopTimeUpdate.ScheduleRelationship の UNSCHEDULED 値を追加しました。[議論](https://github.com/google/transit/pull/173)を参照してください。

#### 2019年5月 {: #may-2019}


* アクセシビリティの問題に関する運行情報(alert)の影響を追加します。[議論](https://github.com/google/transit/pull/164)を参照してください。

#### 2019年2月 {: #february-2019}


* GTFS-realtime の運行情報(alert)に NO_EFFECT effect オプションを追加します。[議論](https://github.com/google/transit/pull/137)を参照してください。
* Service Alerts feed に新しい任意フィールド SeverityLevel を追加します。[議論](https://github.com/google/transit/pull/136)を参照してください。
* Service Alerts feed に読み上げ機能のための新しい任意フィールドを追加します。[議論](https://github.com/google/transit/pull/135)を参照してください。

#### 2018年4月 {: #april-2018}


* SCHEDULED の便における stop_time_update の arrival と departure の両方に対する要件を削除します。[議論](https://github.com/google/transit/pull/165)を参照してください。

#### 2017年8月 {: #august-2017}


* GTFS-realtime フィールドの意味的な多重度(cardinality)を定義します。[議論](https://github.com/google/transit/pull/64)を参照してください。

#### 2015年1月30日 {: #january-30-2015}


* まだ Protocol Buffer 拡張 namespace を持っていなかった残りすべての GTFS-realtime メッセージ（`FeedMessage` や `FeedEntity` など）に追加しました。

#### 2015年1月28日 {: #january-28-2015}


* `TripUpdate` に実験的フィールド `delay` を追加しました（[議論](https://groups.google.com/forum/#!topic/gtfs-realtime/NsTIRQdMNN8)）。

#### 2015年1月16日 {: #january-16-2015}


* `TripDescriptor.start_time` の説明を更新しました。

#### 2015年1月8日 {: #january-8-2015}


* 実験的な列挙型 `OccupancyStatus` を定義しました。
* `VehiclePosition` に実験的なフィールド `occupancy_status` を追加しました（[議論](https://groups.google.com/forum/#!topic/gtfs-realtime/_HtNTGp5LxM)）。

#### 2014年5月22日 {: #may-22-2014}


* `StopTimeUpdate` メッセージ内の `ScheduleRelationship` 列挙型の説明を更新しました（[議論](https://groups.google.com/forum/#!topic/gtfs-realtime/77c3WZrGBnI)）。
* `TripDescriptor` メッセージ内の `ScheduleRelationship` 列挙型の値から REPLACEMENT を削除しました（[議論](https://groups.google.com/forum/#!topic/gtfs-realtime/77c3WZrGBnI)）。

#### 2012年10月12日 {: #oct-12-2012}


* `TripUpdate` メッセージに timestamp フィールドを追加しました。

#### 2012年5月30日 {: #may-30-2012}


* 仕様におけるExtensionsに関する具体的な詳細を追加しました。

#### 2011年11月30日 {: #november-30-2011}


* 仕様への拡張機能の記述を容易にするため、主要なGTFS-realtimeメッセージにProtocol Buffer拡張名前空間を追加しました。

#### 2011年10月25日 {: #october-25-2011}


* `alert`、`header_text`、および `description_text` はいずれもプレーンテキスト値であることを明確にするため、ドキュメントを更新しました。

#### 2011年8月20日 {: #august-20-2011}


* `TimeRange` メッセージのセマンティクスを明確にするため、ドキュメントを更新しました。

#### 2011年8月22日 {: #august-22-2011}


* 初版です。
