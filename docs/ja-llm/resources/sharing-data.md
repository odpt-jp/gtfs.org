# データの共有 {: #sharing-data}


GTFS およびその他の交通・マルチモーダルデータのコレクションにアクセスできる場所。

### サードパーティのGTFS URLディレクトリ {: #3rd-party-gtfs-url-directories}

- [The Mobility Database](https://mobilitydatabase.org/) - 世界中の2,000以上のモビリティデータセットのリポジトリである、[GitHub](https://github.com/MobilityData/mobility-database-catalogs)上のJSONおよびCSVファイルです。OpenMobilityData/TransitFeeds.comのコンテンツを含みます。
- [Transitland](https://transit.land/) - 多数の交通事業者のGTFSデータセットについて、コミュニティが編集可能なリストです。また、JSON/GeoJSONとしてデータにアクセスするためのAPIと、データを試すためのplaygroundも提供しています。
- [TransitData.io](https://transitdata.io/) - ラテンアメリカの一部地域におけるGTFSデータのリストです。フィードは公開されていないため、アクセスするにはWebサイトの管理者に直接連絡しなければなりません。
- [~~OpenMobilityData~~（非推奨）](https://openmobilitydata.org/) - GTFSおよび[GTFS-RT](https://openmobilitydata.org/search?q=gtfsrt)フィードのリストです。GTFSフィードを[アーカイブおよび検証](https://openmobilitydata.org/p/capital-metro/24)し、ブラウザを通じて[GTFS](https://openmobilitydata.org/p/capital-metro/24/latest)と[GTFS-RT](https://openmobilitydata.org/p/capital-metro/495)の両方をプレビューできます。以前はTransitFeeds.comでした。[MobilityDataは](https://database.mobilitydata.org/#h.u71vp6xgkckf)、2022年初頭時点でサービス終了となっており、停止日は未定であると発表しました。

### 交通事業者データアーカイブ {: #transit-agency-data-archives}

- [CapMetrics](https://github.com/scascketta/CapMetrics) - オースティンの交通事業者（CapMetro）の過去の車両位置情報です。データは Go デーモンである [capmetricsd](https://github.com/scascketta/capmetricsd) によって収集されます。
- [Bus Observatory API](https://api.busobservatory.org/) - 世界中の交通システムから収集された、車両の移動および状態に関するリアルタイムデータの公開アーカイブです。

### 国の政府データセット {: #national-government-datasets}

- [National Transit Database（米国）](https://www.transit.dot.gov/ntd) - 連邦公共交通局が運営する、米国の公共交通システムに関する情報および統計です。
- [transport.data.gouv（フランス）](https://transport.data.gouv.fr/) - フランスの交通エコシステムのためのデータプラットフォームです。
- [European long-distance transport operators（EU）*(非公式)*](https://github.com/public-transport/european-transport-operators) - 利用可能な API endpoint、GTFS feed、および client library の非公式リストです。

### プロプライエタリ（非標準）のベンダー API {: #proprietary-non-standard-vendor-apis}

- [Transport API](https://www.transportapi.com/) - 英国の集約された交通データ向け REST API。有料アクセスです。
- [NextBus API](http://www.nextbus.com/xmlFeedDocs/NextBusXMLFeed.pdf) - NextBus のハードウェアおよび／またはソフトウェアを購入した事業者向けの、リアルタイム車両、ルート・路線系統(route)、停留所等(stop)、到着データの REST API。
- [Navitia.io](http://www.navitia.io/) - 米国および EU における旅程(journey)計画、停留所等(stop)時刻表、等時線などのための REST API。[Navitia](https://github.com/hove-io/navitia) は、ライブ API を支えるオープンソースエンジンです。
- [CityBikes](http://api.citybik.es) - 世界各地の集約されたシェアサイクルデータ向け REST API。[pyBikes](https://github.com/eskerda/pybikes) により提供されています。
- [HAFAS](https://de.wikipedia.org/wiki/HAFAS) – [HaCon](https://www.hacon.de) によるプロプライエタリな公共交通管理ソフトウェア（[エンドポイント一覧](https://gist.github.com/derhuerst/2b7ed83bfa5f115125a5)）
- [Citymapper API](https://docs.external.citymapper.com/api/) - 交通機関の旅程(journey)計画、リアルタイム交通データ、および徒歩、自転車、スクーターの移動時間向け REST API。
- [TripGo API](https://developer.tripgo.com) - [SkedGo](https://skedgo.com) によるマルチモーダルな旅程(journey)計画およびリアルタイムデータ向け REST API。

### クラウドソーシングによる交通データ {: #crowdsourced-transit-data}

- [Citylines.co](https://www.citylines.co) - 交通システムの歴史的な変遷に重点を置いた、交通システムをマッピングするための共同プラットフォームです。データは [citylines.co/data](https://www.citylines.co/data) から GeoJSON または CSV としてダウンロードできます。
- [OpenStreetMap (OSM)](https://www.openstreetmap.org) - 交通、公共交通、および経路探索データを含む、世界をマッピングするための共同プラットフォームです。
- [GTFS-Hub](https://github.com/mfdz/gtfs-hub) - （現在はドイツの）交通事業者の、コミュニティによるテスト済みで、おそらく品質・内容が強化され、部分的に統合またはフィルタリングされた GTFS-feeds です。[MITFAHR|DE|ZENTRALE](https://github.com/mfdz) により維持されています。

### ソフトウェアテストに使用されるサンプル GTFS および GTFS Realtime データセット {: #sample-gtfs-and-gtfs-realtime-datasets-used-for-software-testing}

- [sample-gtfs-feed](https://github.com/public-transport/sample-gtfs-feed) - テストに使用される架空の GTFS データセットです。
- [transitfeed unit tests](https://github.com/google/transitfeed/tree/master/tests/data) - 元の Google [Python GTFS validator](https://github.com/google/transitfeed/wiki/FeedValidator) 用に作成されたテストデータです。
- [Transitland GTFS and GTFS Realtime unit tests](https://github.com/interline-io/transitland-lib) - Transitland 向けの GTFS および GTFS Realtime の解析と検証を処理する [transitland-lib](https://github.com/interline-io/transitland-lib) ライブラリをテストするためのものです。
    - [GTFS - 単一行レベルの「不正なエンティティ」](https://github.com/interline-io/transitland-lib/tree/master/test/data/bad-entities)
    - [GTFS - 1つ以上のファイル内のエンティティに関係する検証エラー](https://github.com/interline-io/transitland-lib/tree/master/test/data/validator/errors)
    - [GTFS - ベストプラクティス](https://github.com/interline-io/transitland-lib/tree/master/test/data/validator/best-practices)
- [gtfs-realtime-validator unit tests](https://github.com/MobilityData/gtfs-realtime-validator/tree/master/gtfs-realtime-validator-lib/src/test/) - 一部の [GTFS データセット（zip ファイル）](https://github.com/MobilityData/gtfs-realtime-validator/tree/master/gtfs-realtime-validator-lib/src/test/resources)が含まれており、多数の GTFS RT メッセージが、gtfs-realtime-bindings ライブラリを介して [Java でプログラムにより](https://github.com/MobilityData/gtfs-realtime-validator/tree/master/gtfs-realtime-validator-lib/src/test/java/edu/usf/cutr/gtfsrtvalidator/lib/test/rules)定義されています。
- [OpenTripPlanner unit tests](https://github.com/opentripplanner/OpenTripPlanner/tree/dev-2.x/src/test) - 一部の [GTFS データセット](https://github.com/opentripplanner/OpenTripPlanner/tree/dev-2.x/src/test/resources/gtfs)が、ユニットテスト（[GtfsTest](https://github.com/opentripplanner/OpenTripPlanner/blob/dev-2.x/src/test/java/org/opentripplanner/GtfsTest.java) および [mmri folder](https://github.com/opentripplanner/OpenTripPlanner/tree/dev-2.x/src/test/java/org/opentripplanner/mmri)）用に定義されています。
