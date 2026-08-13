# GTFS Schedule {: #gtfs-schedule}

### 改訂履歴 {: #revision-history}

#### 2026年4月 {: #april-2026}

* より適切な柔軟な便の時間見積もりを提供するため、`trips.txt` に安全な所要時間フィールドを追加しました。[議論](https://github.com/google/transit/pull/598)を参照してください。

#### 2026年2月 {: #february-2026}

* transfer_type = 0 または空の場合（推奨乗換地点）、`from_stop_id` および `to_stop_id` を必須としました。[議論](https://github.com/google/transit/pull/591)を参照してください。
* 通貨金額の説明を改善しました。[議論](https://github.com/google/transit/pull/615)を参照してください。
* 運賃乗換ルールが方向性を持つことを明確化しました。[議論](https://github.com/google/transit/pull/602)を参照してください。
* `fare_leg_rules.txt` の参照先へのリンクを追加しました。[議論](https://github.com/google/transit/pull/601)を参照してください。
* 不足していたスペースを追加しました。[議論](https://github.com/google/transit/pull/587)を参照してください。

#### 2025年10月 {: #october-2025}

* `fare_transfer_rules.txt` に関する明確化を追加し、新しい `Local Time` フィールド型を導入しました。[議論](https://github.com/google/transit/pull/561)を参照してください。

#### 2025年9月 {: #september-2025}

* `agency.txt` および `routes.txt` に `cemv_support` フィールドを追加しました。[議論](https://github.com/google/transit/pull/545)を参照してください。
* `stops.txt` に `stop_access` フィールドを追加しました。[議論](https://github.com/google/transit/pull/515)を参照してください。

#### 2025年6月 {: #june-2025}

* `trips.txt` に `cars_allowed` フィールドを追加しました。[議論](https://github.com/google/transit/pull/547)を参照してください。

#### 2025年4月 {: #april-2025}

* DRT services において `continuous_pickup/continuous_drop_off` に値 1 を許可する明確化を追加しました。[議論](https://github.com/google/transit/pull/558)を参照してください。

#### 2025年2月 {: #february-2025}

* rider_categories.txt を追加しました。[議論](https://github.com/google/transit/pull/511)を参照してください。

#### 2025年1月 {: #january-2025}

* agency_fare_url を更新し、その説明を拡充して運賃情報のみを含めるようにしました。[議論](https://github.com/google/transit/pull/524)を参照してください。

#### 2024年12月 {: #december-2024}

* `fare_leg_join_rules.txt` を追加し、有効運賃区間(effective fare leg)の概念を導入しました。[議論](https://github.com/google/transit/pull/439)を参照してください。

#### 2024年9月 {: #september-2024}

* transfers.txt における from/to_stop_id および from/to_trip_id フィールドの存在と使用を明確化しました。[議論](https://github.com/google/transit/pull/455)を参照してください。
* GeoJSON ファイル内のポリゴンに対する有効性ルールを追加しました。[議論](https://github.com/google/transit/pull/476)を参照してください。

#### 2024年8月 {: #august-2024}

* デマンド型サービスにより stops.txt の存在要件を変更しました。[議論](https://github.com/google/transit/pull/472)を参照してください。
* stop_times.txt の timepoint の意図された使用方法を明確化しました。[議論](https://github.com/google/transit/pull/474)を参照してください。
* 行先表示(headsign)が推奨であることを追加しました。[議論](https://github.com/google/transit/pull/485)を参照してください。

#### 2024年7月 {: #july-2024}

* feed_info.txt の要件を更新します。[議論](https://github.com/google/transit/pull/460)を参照してください。
* shape を含めるべきであることを追加します。[議論](https://github.com/google/transit/pull/470)を参照してください。

#### 2024年5月 {: #may-2024}

* `fare_leg_rules.txt` に `rule_priority` フィールドを追加しました。[議論](https://github.com/google/transit/pull/418)を参照してください。
* `stops.zone_id` の存在を明確化しました。[議論](https://github.com/google/transit/pull/432)を参照してください。

#### 2024年4月 {: #april-2024}

* チケット商品の定義を明確化しました。[議論](https://github.com/google/transit/pull/426)を参照してください。

#### 2024年3月 {: #march-2024}

* GTFS Flex を追加しました。[議論](https://github.com/google/transit/pull/433)を参照してください。

#### 2023年11月 {: #november-2023}

* ベストプラクティス: すべてのファイルに対するデータセット公開ガイドラインおよび実践に関する推奨事項を追加します。[議論](https://github.com/google/transit/pull/406)を参照してください。
* networks.txt および route_networks.txt を追加します。[議論](https://github.com/google/transit/pull/405)を参照してください。

#### 2023年8月 {: #august-2023}

* fare_media_type=1 を追加します。[議論](https://github.com/google/transit/pull/385)を参照してください。

#### 2023年7月 {: #july-2023}

* GTFSファイル内のサブフォルダを禁止しました。[議論](https://github.com/google/transit/pull/379)を参照してください。
* 時刻または日による変動運賃を追加しました。[議論](https://github.com/google/transit/pull/357)を参照してください。
* stop_times.txt における暗黙のタイムゾーンを明確化しました。[議論](https://github.com/google/transit/pull/378)を参照してください。
* 停車時刻(stop_time)の shape_dist_traveled は、便(trip)の shape の最大距離を超えてはいけないことを規定しました。[議論](https://github.com/google/transit/pull/380)を参照してください。
* ベストプラクティス: 推奨される存在を追加しました。[議論](https://github.com/google/transit/pull/386)を参照してください。

#### 2023年3月14日 {: #march-14-2023}


* チケットメディアを追加しました。[議論](https://github.com/google/transit/pull/355)を参照してください。

#### 2022年7月26日 {: #july-26-2022}


* 座席に座ったまま利用できるオプションを備えた便(trip)間の乗換を追加しました。[議論](https://github.com/google/transit/pull/303)を参照してください。

#### 2022年5月17日 {: #may-17-2022}


* GTFS-Fares v2 の基本実装。[議論](https://github.com/google/transit/pull/286)を参照してください。

#### 2021年10月22日 {: #oct-22-2021}


* Primary ID および Foreign ID フィールドを追加しました。[議論](https://github.com/google/transit/pull/278)を参照してください。

#### 2021年10月05日 {: #oct-05-2021}


* 便から便への乗換およびルート・路線系統(route)からルート・路線系統(route)への乗換を追加しました。[議論](https://github.com/google/transit/pull/284)を参照してください。

#### 2021年9月15日 {: #september-15-2021}


* 運賃ゲート(pathway_mode=6)を双方向にすることを許可しました。[議論](https://github.com/google/transit/pull/276)を参照してください。

#### 2021年9月13日 {: #september-13-2021}


* `stop_name` のベストプラクティスを更新しました。[議論](https://github.com/google/transit/pull/282)を参照してください。

#### 2021年8月27日 {: #august-27-2021}


* GTFS Schedule を [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119) に更新しました。[議論](https://github.com/google/transit/pull/277)を参照してください。

#### 2021年1月4日 {: #january-4-2021}


* `stop_times.stop_id` の説明を明確化しました。[議論](https://github.com/google/transit/pull/258)を参照してください。
* 正およびゼロ以外のフィールドの符号を定義しました。[議論](https://github.com/google/transit/pull/251)を参照してください。

#### 2020年10月2日 {: #october-2-2020}


* `frequencies.headway_secs` のフィールド型を非負整数から正の整数に変更しました。[議論](https://github.com/google/transit/pull/249)を参照してください。

#### 2020年5月25日 {: #may-25-2020}


* `pathways.txt`、`levels.txt`、および`attributions.txt`を翻訳可能なテーブルとして定義しました。多言語の`signposted_as`値を翻訳するための推奨事項を追加しました。[議論](https://github.com/google/transit/pull/220)を参照してください。

#### 2020年5月13日 {: #may-13-2020}


* `routes.txt` および `stop_times.txt` に `continuous_pickup` と `continuous_drop_off` を追加しました。`shape_id` を「任意」から「条件付き必須」に変更しました。[議論](https://github.com/google/transit/pull/208)を参照してください。

#### 2020年3月24日 {: #march-24-2020}


* 読み上げ用フィールド(text-to-speech field)を定義し、`stops.txt` に `tts_stop_name` を追加しました。[議論](https://github.com/google/transit/pull/49)を参照してください。

#### 2020年2月5日 {: #february-5-2020}


* トロリーバスおよびモノレールの`route_types`を追加しました。[議論](https://github.com/google/transit/pull/174)を参照してください。

#### 2020年1月9日 {: #january-9-2020}


* `translations.txt` を追加しました。[議論](https://github.com/google/transit/pull/180)を参照してください。

#### 2019年12月26日 {: #december-26-2019}


* `route_type` のケーブルカーおよび索道の定義を更新しました。[議論](https://github.com/google/transit/pull/186)を参照してください。

#### 2019年12月20日 {: #december-20-2019}


* `attributions.txt` を追加しました。[議論](https://github.com/google/transit/pull/192)を参照してください。

#### 2019年8月26日 {: #august-26-2019}


* `stop_lat` および `stop_lon` は、乗客が車両への乗車を待つ位置に配置しなければならないことを明記しました。[議論](https://github.com/google/transit/pull/179)を参照してください。

#### 2019年7月9日 {: #july-9-2019}


* 到着時刻および出発時刻のベストプラクティスを追加しました。[議論](https://github.com/google/transit/pull/165)を参照してください。
* 行先表示(headsign)のベストプラクティスを追加しました。[議論](https://github.com/google/transit/pull/167)を参照してください。
* `stop_id` のベストプラクティスを追加しました。[議論](https://github.com/google/transit/pull/169)を参照してください。

#### 2019年6月25日 {: #june-25-2019}


* ルート形状(shape)のポイントと停留所等(stop)の関係を明確化しました。[議論](https://github.com/google/transit/pull/39)を参照してください。

#### 2019年4月4日 {: #april-4-2019}


* `stops.txt` に `platform_code` フィールドを追加しました。[議論](https://github.com/google/transit/pull/146)を参照してください。

#### 2019年3月27日 {: #march-27-2019}


* `pathways.txt` および `levels.txt` を追加しました。[議論](https://github.com/google/transit/pull/143)を参照してください。

#### 2019年2月6日 {: #february-6-2019}


* 明確化のための編集および書式変更。 [議論](https://github.com/google/transit/pull/120)を参照してください。

#### 2018年10月2日 {: #october-2-2018}


* フィールド型を因数分解しました。[議論](https://github.com/google/transit/pull/104)を参照してください。

#### 2018年9月14日 {: #september-14-2018}


* 「条件付き必須」の概念を追加しました。[議論](https://github.com/google/transit/pull/100)を参照してください。

#### 2018年9月4日 {: #september-4-2018}


* `agency_lang` と `feed_lang` の定義を統一しました。[議論](https://github.com/google/transit/pull/98)を参照してください。

#### 2018年8月27日 {: #august-27-2018}


* `CHANGES.md` および最終改訂日を更新しました。[議論](https://github.com/google/transit/pull/99)を参照してください。

#### 2018年8月22日 {: #august-22-2018}


* `feed_info.txt` ファイルに `feed_contact_email` および `feed_contact_url` フィールドを追加しました。[議論](https://github.com/google/transit/pull/31)を参照してください。

#### 2017年12月11日 {: #december-11-2017}


* `routes.txt` に `route_sort_order` を追加しました。[議論](https://github.com/google/transit/pull/83)を参照してください。

#### 2017年3月15日 {: #march-15-2017}


* 提案者の投票は合計に含まれないことを明確化しました。[議論](https://github.com/google/transit/pull/50)を参照してください。
* 投票を呼びかける前に、少なくとも1つのGTFS producerと1つのGTFS consumerが提案された変更を実装するべきであることを規定しました。[議論](https://github.com/google/transit/pull/46)を参照してください。

#### 2017年2月7日 {: #february-7-2017}


* `block_id` と `service_id` の関係を明確化しました。[議論](https://github.com/google/transit/pull/44)を参照してください。
* 頻度ベースのサービスは車両の出発時に開始することを明確化しました。[議論](https://github.com/google/transit/pull/42)を参照してください。
* `stop_id` および `stop_code` の説明を明確化しました。[議論](https://github.com/google/transit/pull/40)を参照してください。

#### 2017年12月11日 {: #december-11-2017}


* `routes.txt` ファイルに `route_sort_order` フィールドを追加しました。[議論](https://github.com/google/transit/pull/83)を参照してください。

#### 2016年11月27日 {: #november-27-2016}


* 駅入口を `stops.location_type` として追加しました。[議論](https://github.com/google/transit/pull/30)を参照してください。

#### 2016年9月2日 {: #september-2-2016}


* `fare_attributes.txt` に `agency_id` を追加するため、ドキュメントを更新しました。[議論](https://github.com/google/transit/pull/27)を参照してください。

#### 2016年3月16日 {: #march-16-2016}


* GTFS ドキュメントを https://github.com/google/transit の Github へ移行しました

#### 2016年2月3日 {: #february-3-2016}


* `agency.txt` に `agency_email` を追加する提案を仕様に追加しました: [議論](https://groups.google.com/forum/?fromgroups#!topic/gtfs-changes/aezjQsriLYA)

#### 2015年2月2日 {: #february-2-2015}


* stop_times.txt の 'timepoint' 提案を仕様に追加しました: [議論](https://groups.google.com/forum/?fromgroups#!topic/gtfs-changes/Ah-J9JP2rJY)

#### 2014年2月17日 {: #february-17-2014}


* trips.txt の 'bikes_allowed' 提案を仕様に追加しました: [議論](https://groups.google.com/forum/?fromgroups#!topic/gtfs-changes/rEiSeKNc4cs)

#### 2012年10月15日 {: #october-15-2012}


trips.txt の 'wheelchair_accessible' 提案を仕様に追加しました: [議論](https://groups.google.com/forum/?fromgroups#!topic/gtfs-changes/ASxItgsQlh4)

#### 2012年6月20日 {: #june-20-2012}


* 'wheelchair_boarding' の提案を仕様に追加しました: [議論](https://groups.google.com/forum/?fromgroups#!topic/gtfs-changes/ASxItgsQlh4)

#### 2012年2月2日 {: #february-2-2012}


* 仕様に「stop_timezone」の提案を追加しました: [議論](https://groups.google.com/forum/#!topic/gtfs-changes/2Il0Q9OXqu4)

#### 2012年1月18日 {: #january-18-2012}


* ドキュメントを旧 code.google.com から developers.google.com の新しい場所へ移行しました。

#### 2011年9月26日 {: #september-26-2011}


* 仕様に「feed_info」の提案を追加しました: [議論](https://groups.google.com/forum/#!topic/gtfs-changes/Sh0e4o9o2Gw)

#### 2011年9月6日 {: #september-6-2011}


* 仕様に「agency_fare_url」の提案を追加しました: [議論](https://groups.google.com/forum/#!topic/gtfs-changes/Zp9rPG07CgE)
* 仕様に「exact_times」の提案を追加しました: [議論](https://groups.google.com/forum/#!topic/gtfs-changes/nZF9lbQ7TQs)

#### 2009年3月30日 {: #march-30-2009}


* 交通フィードを一般公開することに関する新しいセクション。これは、データの解釈方法または記述方法の変更では厳密にはなかったため、以前はグループで議論されていませんでした。しかし、GTFS形式のデータを利用できるアプリケーションが増加していることから、Googleの一部の関係者は、GTFSのGoogle以外での利用に関する議論を含めることが有益であると考えました。
* CSV形式の明確化: [議論](https://groups.google.com/forum/#!topic/gtfs-changes/03qz5aTA2mk)。
* route_color および route_text_color フィールドの説明において、対照的な色を選択する方法に関する追加のガイダンス。
* これらのスレッドで提案およびテストされた trip_short_name: a および b。
* 文書末尾に含まれるサンプルデータの軽微な誤りを修正しました（停留所等(stop) S7 の parent_station を S8 としていました）。
* コメント期間中にMarcyが提案したとおり、文書末尾のサンプルデータに「agency_lang」情報を追加しました: [議論](https://groups.google.com/forum/#!topic/gtfs-changes/5qP1kDUFqx0)。
* サイドバー内のOCTAのGTFSフィードへのリンクを更新しました。
* [元の要約](https://groups.google.com/forum/#!topic/gtfs-changes/cL1E4oKKpKw)を参照してください。

#### 2009年2月26日 {: #february-26-2009}


* 現時点ではGTFSデータを利用する他のアプリケーションが多数あるため、Google固有のフィード提出手順の大部分を削除しました。
* サイドバーにあるOrange County OCTAの公開フィードへの壊れたリンクを修正しました。

#### 2008年8月7日 {: #august-7-2008}


* 8月6日版で誤って省略されていた stop_url フィールドを復元しました
* サンプルデータに agency_phone を追加しました
* Google にフィードを提出する際のデータ利用契約への言及を追加しました

#### 2008年8月6日 {: #august-6-2008}


* フィード公開者が推奨される乗換動作に関するヒントを提供できるようにする transfers.txt ファイルを追加しました（[original proposal](https://groups.google.com/forum/#!topic/gtfs-changes/cL1E4oKKpKw)）
* 停留所等(stop)ポイントを駅にグループ化できるようにする location_type および parent_station フィールドを stops.txt に追加しました（[original proposal](https://groups.google.com/forum/#!topic/gtfs-changes/ScGAyZ9a_yw)）
* 事業者の音声電話番号を提供するための agency_phone フィールドを追加しました（[original proposal](https://groups.google.com/forum/#!topic/gtfs-changes/8Itt58ueyqA)）
* オープンソースのテストツールに言及する「フィードのテスト」セクションを追加しました
* CSV形式、agency_timezone、agency_lang、route_color、route_text_color、arrival_time、departure_time、calendar.txt と calendar_dates.txt の比較、運賃テーブル、および frequencies.txt に関する明確化を追加しました
* フィード履歴ドキュメントへのリンクを追加し、一部の公開フィードリンクを修正しました
* 現在の Google Maps UI を描写するようにサンプル画像を更新しました
* ドキュメント内のサンプルデータを更新・修正しました

#### 2008年2月29日 {: #february-29-2008}


* 乗客向けの停留所等(stop)コードを指定できるように、stops.txt に stop_code フィールドを追加しました（[元の提案](https://groups.google.com/forum/#!topic/gtfs-changes/k9A95fYZexc)）
* routes.txt の route_short_name および route_long_name の説明を明確化しました
* stop_times.txt の arrival_time および departure_time の説明を明確化しました
* サンプルデータのセクションにある誤字を修正しました

#### 2007年11月20日 {: #november-20-2007}


* block_id の説明を明確化しました
* Google Transit を強調しない表現に変更し（Google 以外のアプリケーションも GTFS を使用しており、現在では公共交通機関の経路検索が Google Maps の統合機能となっているため）、各種の誤字を修正しました
* 現在の Google Maps UI における GTFS フィールドの表示を反映するよう、スクリーンショットの例を更新しました
* 公共交通データ提供者向けの Google の連絡先メールアドレスを更新しました
* 書式を更新しました

#### 2007年10月5日 {: #october-5-2007}


* stop_sequence および shape_pt_sequence を、任意の増加する非負整数を許可するよう変更しました
* 説明を明確化し、誤字を修正しました

#### 2007年5月31日 {: #may-31-2007}


* ページスタイルを更新し、HTMLをよりクリーンでアクセシブルなものにしました
* 公開フィードの例およびその他の有用なサイトへのリンクを追加しました
* 個々のフィールド説明から例を削除しました

#### 2007年4月9日 {: #april-9-2007}


* [フィードの送信](https://developers.google.com/transit/google-transit#SubmitFeedToGoogle)に関するセクションを追加しました。
* [Example Demo Transit Agency feed](https://developers.google.com/transit/gtfs/examples/gtfs-feed)を追加しました。
* すべての運行日がcalendar_dates.txtで定義されている場合、calendar.txtを省略できる旨の注記を追加しました。
* 1つの事業者のみを含むフィードでは、agency_idフィールドを任意にしました。これにより、agency_idを含まない既存のフィードも有効なままとなります。
* agency_url、stop_url、およびroute_urlのより詳細な仕様と、それらのフィールドの追加の値の例を追加しました。
* 有効なroute_type値として、6（Gondola）および7（Funicular）を追加しました。

#### 2007年3月8日 {: #march-8-2007}


* stop_url フィールドを、2007年2月28日の更新で誤って指定されていた stop_times.txt から、本来属する stops.txt へ移動する軽微な編集を行いました。

#### 2007年3月5日 {: #march-5-2007}


* route_long_name フィールドの説明を明確にするための軽微な編集。

#### 2007年2月28日 {: #february-28-2007}


* 運行間隔ベースのスケジュールをサポートするため、frequencies.txt を追加しました。
* 同一フィード内で複数の事業者が許可されるようになりました。また、どのルート・路線系統(route)をどの事業者が運行するかを指定できる、新しい agency_id フィールドを agencies.txt と routes.txt の両方に追加しました。
* ルート・路線系統(route)ごとおよび停留所等(stop)ごとの URL を追加しました。
* trips.txt に direction_id フィールドを追加しました。
* stop_times.txt に stop_headsign フィールドを追加し、便(trip)の途中での行先表示(headsign)変更をサポートしました。
* routes.txt に任意の route_color および route_text_color を追加し、ルート・路線系統(route)の色をサポートしました。
* 住所を使用して停留所等(stop)を指定する機能を削除しました。仕様の以前のバージョンでは、stop_street、stop_city、stop_region、stop_postcode、および stop_country フィールドに住所を指定して、交通機関の停留所等(stop)の位置を示すことができました。現在は、ほとんどのアプリケーションにとってより有用な、緯度を示す stop_lat と経度を示す stop_lon を使用して停留所等(stop)の位置を指定しなければなりません。
* routes.txt の route_type フィールドにケーブルカーの車両タイプを追加しました。
* 変更の概要については、元の[Headway ブログ投稿](http://headwayblog.com/2007/03/02/google-feed-spec-update-2007-02/)を参照してください。

#### 2006年11月29日 {: #november-29-2006}


* shapes.txt を介した便(trip)のルート形状(shape)情報のサポートを追加しました
* stop_sequence の定義を明確化しました
* pickup_type および drop_off_type を任意として明記しました

#### 2006年10月31日 {: #october-31-2006}


* 運賃情報のサポートを追加しました
* 個別ファイル名から日付を削除しました
* route_type の値の定義を変更しました
* 運行期間が重複しない限り、複数のフィードファイルを同時に投稿できるようにしました
* trips.txt の block_id を修正し、任意として正しく記載しました
* 列ヘッダーを含めなければならないことを明記しました

#### 2006年9月29日 {: #september-29-2006}


* 例にあるいくつかの誤りを修正するための軽微な編集。

#### 2006年9月25日 {: #september-25-2006}


* 初版。
