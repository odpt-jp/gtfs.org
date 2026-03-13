# GTFS Realtime {: #gtfs-realtime}

### 改訂履歴 {: #revision-history}

#### 2026年2月 {: #february-2026}


* selectedTrips における trip_ids の要件および多重度(cardinality)を修正しました。詳細は[ディスカッション](https://github.com/google/transit/pull/609)を参照してください。
* 欠落していたスペースを追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/587)を参照してください。

#### 2025年5月 {: #may-2025}


* schedule_relationship の `ADDED` を非推奨とし、代わりに `NEW` を使用し、`REPLACEMENT` を追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/504)を参照してください。
* 便の変更(trip modification)に関するさらなる明確化を行いました。詳細は[ディスカッション](https://github.com/google/transit/pull/542)を参照してください。

#### 2024年12月 {: #december-2024}


* リアルタイムデータの基となる GTFS Schedule フィードの feed_info.feed_version に対応する新しい文字列フィールドを追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/434)を参照してください。

#### 2024年10月 {: #october-2024}


* 便の変更(Trip Modifications)に関する明確化および小規模な修正を行いました。詳細は[ディスカッション](https://github.com/google/transit/pull/497)をご覧ください。

#### 2024年3月 {: #march-2024}


* 便の変更(Trip-Modifications)を採用しました。詳細は[ディスカッション](https://github.com/google/transit/pull/403)をご覧ください。

#### 2022年11月 {: #november-2022}


* 削除された便(DELETED trips)のサポートを追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/352)をご覧ください。

#### 2022年7月 {: #july-2022}


* cause_detail および effect_detail を追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/332)を参照してください。
* TripUpdate.VehicleDescriptor に wheelchair_accessible の値を指定できるようにしました。詳細は[ディスカッション](https://github.com/google/transit/pull/340)を参照してください。

#### 2021年9月 {: #september-2021}


* 運行情報(alert)における画像機能。詳細は[ディスカッション](https://github.com/google/transit/pull/283)を参照してください。

#### 2021年8月 {: #august-2021}


* GTFS-NewShapes を実験的機能として追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/272)をご覧ください。

#### 2021年4月 {: #april-2021}


* TripUpdate に departure_occupancy_status を追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/260)をご覧ください。

#### 2021年2月 {: #february-2021}


* GTFS Realtime の乗車率(occupancy)の説明を明確化しました。詳細は[ディスカッション](https://github.com/google/transit/pull/259)をご覧ください。

#### 2020年9月 {: #september-2020}


* 複数車両の混雑度をサポートしました。詳細は[ディスカッション](https://github.com/google/transit/pull/237)をご覧ください。

#### 2020年4月 {: #april-2020}


* 停留所割り当て(stop assignments)をサポートしました。詳細は[ディスカッション](https://github.com/google/transit/pull/219)をご覧ください。

#### 2020年7月 {: #july-2020}


* DUPLICATED 便(trip)をサポートしました。詳細は[ディスカッション](https://github.com/google/transit/pull/221)を参照してください。
* Alert の tts_header_text および tts_description_text が実験的機能ではなくなりました。詳細は[ディスカッション](https://github.com/google/transit/pull/229)を参照してください。
* GTFS-RT の ADDED 便(trip)を「完全には指定されていない」とラベル付けしました。詳細は[ディスカッション](https://github.com/google/transit/pull/230)を参照してください。

#### 2020年4月 {: #april-2020}


* SeverityLevel を final としてマークしました。詳細は[ディスカッション](https://github.com/google/transit/pull/214)を参照してください。
* occupancy_percentage を追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/213)を参照してください。

#### 2020年3月12日 {: #march-12-2020}


* ブロック内の次の便(trip)に対して TripUpdate の予測を提供することを推奨します。詳細は[ディスカッション](https://github.com/google/transit/pull/206)を参照してください。

#### 2019年8月 {: #august-2019}


* trip_updates がフィード内でブロック順に出現する必要はないことを文書化しました。詳細は[ディスカッション](https://github.com/google/transit/pull/176)を参照してください。
* StopTimeUpdate.ScheduleRelationship に UNSCHEDULED 値を追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/173)を参照してください。

#### 2019年5月 {: #may-2019}


* アクセシビリティに関する問題の運行情報(alert)効果を追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/164)をご覧ください。

#### 2019年2月 {: #february-2019}


* GTFS Realtime の運行情報(alert)に NO_EFFECT の effect オプションを追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/137)を参照してください。
* 運行情報フィードに新しい任意フィールド SeverityLevel を追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/136)を参照してください。
* 運行情報フィードに読み上げ用機能(Text-to-Speech)のための新しい任意フィールドを追加しました。詳細は[ディスカッション](https://github.com/google/transit/pull/135)を参照してください。

#### 2018年4月 {: #april-2018}


* SCHEDULED の便(trip)における stop_time_update の到着および出発の両方の必須要件を削除しました。詳細は[ディスカッション](https://github.com/google/transit/pull/165)を参照してください。

#### 2017年8月 {: #august-2017}


* GTFS Realtime フィールドの意味的多重度(semantic cardinality)を定義しました。詳細は[ディスカッション](https://github.com/google/transit/pull/64)をご覧ください。

#### 2015年1月30日 {: #january-30-2015}


* これまでに存在していなかったすべての残りの GTFS-realtime メッセージ（`FeedMessage` や `FeedEntity` など）に、Protocol Buffer 拡張名前空間を追加しました。

#### 2015年1月28日 {: #january-28-2015}

* `TripUpdate` に実験的フィールド `delay` を追加しました（[ディスカッション](https://groups.google.com/forum/#!topic/gtfs-realtime/NsTIRQdMNN8)）。

#### 2015年1月16日 {: #january-16-2015}


* `TripDescriptor.start_time` の説明を更新しました。

#### 2015年1月8日 {: #january-8-2015}


* 実験的な列挙型 `OccupancyStatus` を定義しました。
* 実験的なフィールド `occupancy_status` を `VehiclePosition` に追加しました（[議論](https://groups.google.com/forum/#!topic/gtfs-realtime/_HtNTGp5LxM)）。

#### 2014年5月22日 {: #may-22-2014}

* `StopTimeUpdate` メッセージ内の `ScheduleRelationship` 列挙型の説明を更新しました（[議論](https://groups.google.com/forum/#!topic/gtfs-realtime/77c3WZrGBnI)）。
* `TripDescriptor` メッセージ内の `ScheduleRelationship` 列挙型の値から REPLACEMENT を削除しました（[議論](https://groups.google.com/forum/#!topic/gtfs-realtime/77c3WZrGBnI)）。

#### 2012年10月12日 {: #oct-12-2012}


* `TripUpdate` メッセージに timestamp フィールドを追加しました。

#### 2012年5月30日 {: #may-30-2012}

* 仕様に拡張(Extensions)に関する具体的な詳細を追加しました。

#### 2011年11月30日 {: #november-30-2011}

* 仕様への拡張を容易にするために、主要な GTFS-realtime メッセージに Protocol Buffer 拡張名前空間を追加しました。

#### 2011年10月25日 {: #october-25-2011}

* `alert`、`header_text`、および `description_text` がいずれもプレーンテキスト値であることを明確にするために、ドキュメントを更新しました。

#### 2011年8月20日 {: #august-20-2011}

* `TimeRange` メッセージの意味を明確にするためにドキュメントを更新しました。

#### 2011年8月22日 {: #august-22-2011}


* 初版。
