# データの作成 {: #producing-data}

## GTFS {: #gtfs}


- General Transit Feed Specification の公式ドキュメントサイト：[GTFS.org](https://gtfs.org)

### GTFS コース {: #gtfs-courses}


- [MobilityData - 「Understanding GTFS: An intro and overivew」](https://www.youtube.com/watch?v=SDz2460AjNo) - この動画では、General Transit Feed Specification (GTFS) の概要と、それが交通事業者、乗客、政策立案者にとって有用である理由を説明しています。 
- [World Bank - 「Intro. to GTFS」オンラインコース](https://olc.worldbank.org/content/introduction-general-transit-feed-specification-gtfs-and-informal-transit-system-mapping) - GTFS および GTFS-realtime について学ぶための、無料のオンライン自習型コースです。
- [Open Transit Data Toolkit](http://transitdatatoolkit.com/) - オープンな交通データを活用するための一連のレッスンです。
- [ArcGIS - GTFS 入門](https://www.youtube.com/watch?v=8OQKHhu1VgQ&t=148s)
- [GTFS-books](https://github.com/MobilityData/GTFS-books) - GTFS および GTFS Realtime の包括的なガイドです。これらの書籍は [Quentin Zervaas](https://github.com/HendX) によって執筆され、[MobilityData](https://mobilitydata.org/) に寄贈され、オープンアクセスとして公開されています。
- [MBTA GTFS Onboarding](https://mybinder.org/v2/gh/mbta/gtfs_onboarding/main?urlpath=lab/tree/GTFS_Onboarding.ipynb) - MBTA が GTFS static 向けに作成したインタラクティブなチュートリアルです。[スタンドアロンの Docker image](https://github.com/mbta/gtfs_onboarding) は GitHub でも利用でき、Jupyter notebook の[ホスト型・インストール不要版](https://mybinder.org/v2/gh/mbta/gtfs_onboarding/main?urlpath=lab/tree/GTFS_Onboarding.ipynb)も利用できます。
- [Planetizen「Building a Transit Map Web App」コース](https://courses.planetizen.com/course/building-transit-map-app) - コーディング経験を必要とせず、独自の Web ベースの地図アプリケーションを設定するための動画チュートリアルです。

### GTFS コンシューマーアプリのガイダンス {: #gtfs-consumer-app-guidance}


- [Google Transit Developers](https://developers.google.com/transit/gtfs/) - GTFS に関する Google 固有の追加ドキュメントです。
- [Transit app Guidelines for Producing GTFS Static Data](https://resources.transitapp.com/article/458-guidelines-for-producing-gtfs-static-data-for-transit) - GTFS に関する Transit app 固有の追加ドキュメントです。
- [Bing Maps Transit - Add your transit data to Bing Maps](https://www.bing.com/maps/transitcontentproviders) - GTFS に関する Bing 固有の追加ドキュメントです。
- [Yandex Maps - Transport integration](https://yandex.ru/support/m-maps/transport.html?lang=en#connect-display) - GTFS に関する Yandex 固有の追加ドキュメントです。

### GTFS ライブラリ {: #gtfs-libraries}


さまざまな言語で GTFS データを容易に利用できるようにするソフトウェア。

#### C {: #c}

- [CGTFS](https://github.com/rakhack/cgtfs) - 静的GTFSフィードを読み取るためのCライブラリです。展開済みフィードをアプリケーションメモリまたはSQLiteデータベースに読み込むことをサポートしています。
- [RRRR Rapid Real-time Routing](https://github.com/bliksemlabs/rrrr) - RRRR（通常はR4と発音されます）は、RAPTOR公共交通ルーティングアルゴリズムのC言語実装です。

#### C++ {: #c}

-  [just_gtfs](https://github.com/mesozoic-drones/just_gtfs) - GTFS の読み書きのための C++17 ヘッダーオンリーライブラリ（[Valhalla](https://github.com/valhalla/valhalla) で使用されています）。主な機能: GTFS フィードの高速な読み書き、[拡張 GTFS route types](https://developers.google.com/transit/gtfs/reference/extended-route-types) のサポート、GTFS の Date および Time 形式の簡単な操作。

#### C# {: #c}

- [ESRI public-transit-tools](https://github.com/Esri/public-transit-tools) - ArcGIS で公共交通データを扱うためのツール（ArcGIS のライセンスが必要です）。
- [GTFS Feed Parser](https://github.com/OsmSharp/GTFS) - GTFS パーサーの .Net/Mono 実装です。

#### Go {: #go}

- [Go GTFS Parser](https://github.com/geops/gtfsparser) - Go 用の GTFS 解析ライブラリです。

#### Java {: #java}

- [OneBusAway GTFS Modules](https://github.com/OneBusAway/onebusaway-gtfs-modules/wiki) - データベースサポートを含む、GTFS形式の公共交通データの読み取り、書き込み、変換を行うためのJavaベースのライブラリです。

#### JavaScript {: #javascript}

- [gtfs-sequelize](https://github.com/evansiroky/gtfs-sequelize) - sequelize.js を使用して静的 GTFS をモデル化する Node.js ライブラリです。
- [gtfs-utils](https://github.com/public-transport/gtfs-utils) – GTFS データセットを処理するためのユーティリティです（例: `calendar.txt` と `calendar_dates.txt` の「フラット化」、便(trip)の到着時刻・出発時刻の計算）。
- [gtfs-via-postgres](https://github.com/derhuerst/gtfs-via-postgres) – PostgreSQL を使用して GTFS を処理する、もう1つのツールです。
- [Node-GTFS](https://github.com/BlinkTagInc/node-gtfs) - GTFS ファイルから交通データを読み込み、解凍して SQLite データベースに保存します。事業者、ルート・路線系統(route)、停留所等(stop)、時刻を照会するためのいくつかのメソッドを提供します。

#### PostgreSQL {: #postgresql}

- [gtfs-schema](https://github.com/tyleragreen/gtfs-schema) - GTFSフィード用のPostgreSQLスキーマです。
- [gtfs-via-postgres](https://github.com/derhuerst/gtfs-via-postgres) – PostgreSQLを使用してGTFSを処理する、もう1つのツールです。

#### Python {: #python}

- [ESRI public-transit-tools](https://github.com/Esri/public-transit-tools) - ArcGISで公共交通データを扱うためのツールです（ArcGISのライセンスが必要です）。
- [gtfsdb](https://github.com/OpenTransitTools/gtfsdb) - GTFSファイルをリレーショナルデータベースに変換するためのPythonライブラリです。
- [gtfs_functions](https://github.com/Bondify/gtfs_functions) - GTFSフィードから地理空間可視化を作成するための便利な関数を含むPythonパッケージです。
- [gtfs-segments](https://github.com/UTEL-UIUC/gtfs_segments) - セグメントを使用して、バスのGTFSデータを簡潔な表形式で表現するPythonパッケージです。
- [gtfslib-python](https://github.com/afimb/gtfslib-python) - GTFSファイルを読み取り、公共交通ネットワークに関するさまざまな統計および指標を計算するためのPythonによるオープンソースライブラリです。
- [gtfsman](https://github.com/geops/gtfsman) - 大量のGTFSフィードを管理および更新するための、Pythonによるリポジトリのようなツールです。
- [gtfspy](https://github.com/CxAalto/gtfspy) - Python3を使用した公共交通ネットワーク分析および移動時間計算です。Postgres/PostGIS、Oracle、MySQL、およびSQLiteと互換性があります。[gtfspy-webviz](https://github.com/CxAalto/gtfspy-webviz)で使用されています。
- [GTFS Kit](https://github.com/mrcagney/gtfs_kit) - General Transit Feed Specification (GTFS) データを分析するためのPython 3.8+ツールキットです。GTFSTKの後継です。
- [Make GTFS](https://github.com/mrcagney/make_gtfs) - 基本的なルート・路線系統(route)情報からGTFSフィードを作成するためのPythonライブラリです。
- [Mapzen GTFS](https://github.com/transitland/mapzen-gtfs) - 個別のGTFSテーブルの読み取り、またはフィード内の各事業者を表すグラフの構築をサポートするPython GTFSライブラリです。
- [multigtfs](https://github.com/tulsawebdevs/django-multi-gtfs) - GTFSをインポートおよびエクスポートするためのDjangoアプリケーションです。
- [partridge](https://github.com/remix/partridge) - pandas DataFrames上に構築された、高速で寛容なPython GTFSリーダーです。
- [transit_service_analyst](https://github.com/psrc/transit_service_analyst) - 交通サービス分析を支援するPythonライブラリです。
- [TransitGPT](https://github.com/UTEL-UIUC/TransitGPT) - TransitGPTは、公共交通愛好家が自然言語による指示を通じてGeneral Transit Feed Specification (GTFS) データにアクセスし、分析できるようにする、生成AI搭載のチャットボットです。

#### R {: #r}

- [r-transit](https://github.com/r-transit) - R における GTFS 用ツールのコレクションです。
- [gtfsio](https://github.com/r-transit/gtfsio) - R で GTFS を読み書きするための高速かつ柔軟な関数です。
- [tidytransit](https://github.com/r-transit/tidytransit) - tidytransit を使用して、公共交通の停留所等(stop)とルート・路線系統(route)を地図化し、移動時間と公共交通の運行頻度を計算し、公共交通フィードを検証します。tidytransit は、General Transit Feed Specification を tidyverse および simple features のデータフレームに読み込みます。

#### Ruby {: #ruby}

- [GTFS-viz](https://github.com/vasile/GTFS-viz) - GTFSファイル一式をSQLiteデータベースとGeoJSONに変換するRubyスクリプト（[Transit Map](https://github.com/vasile/transit-map) Webアプリケーションで必要です）

#### Rust {: #rust}

- [gtfs-structure](https://github.com/rust-transit/gtfs-structure) - このクレートは、GTFS構造体およびGTFSアーカイブを読み取るためのヘルパーを提供します。

### GTFS コンバーター {: #gtfs-converters}


さまざまな静的時刻表形式と GTFS の間で変換するコンバーターです。

- [Chouette](https://enroute.atlassian.net/wiki/spaces/PUBLIC/pages/539426886/Chouette+Convert) - French-Transmodel [NeTEX](https://transmodel-cen.eu/index.php/netex/) と GTFS の間で変換します。
- [extract-gtfs-pathways](https://github.com/derhuerst/extract-gtfs-pathways) – GTFS データセットから構内通路(pathway)を GeoJSON として抽出するコマンドラインツールです。
- [extract-gtfs-shapes](https://github.com/derhuerst/extract-gtfs-shapes) – GTFS データセットからルート形状(shape)を GeoJSON として抽出するコマンドラインツールです。
- [GTFS-OSM-Sync](https://github.com/CUTR-at-USF/gtfs-osm-sync) - GTFS 形式のデータを [OpenStreetMap.org](http://www.openstreetmap.org/) と同期するための Java ツールです。
- [gtfs-parser](https://github.com/ioTransit/gtfs-parser) - GTFS-PARSER ライブラリは、javascript で gtfs を解析し、クライアントまたはサーバー上で geojson を作成できるようにするライブラリです。
- [gtfs-service-area](https://github.com/cal-itp/gtfs-service-area) - 静的 GTFS から公共交通サービスエリアを計算します。結果は単一レイヤーの .geojson ファイルとして出力されます。[gtfs-to-geojson](https://github.com/BlinkTagInc/gtfs-to-geojson) の Docker 化されたバージョンです。
- [GTFS-route-shapes](https://github.com/kotrc/GTFS-route-shapes) - GTFS アーカイブ内の各公共交通ルート・路線系統(route)について、単一の geoJSON ルート形状(shape)を生成する Python スクリプトです。
- [gtfs-to-geojson](https://github.com/BlinkTagInc/gtfs-to-geojson) - GTFS のルート形状(shape)および停留所等(stop)内の公共交通データを geoJSON に変換する Javascript ツールです。これは公共交通ルート・路線系統(route)の地図を作成する際に役立ちます。
- [gtfs2gps](https://github.com/ipeaGIT/gtfs2gps) - GTFS 形式の公共交通データを `data.table` 内の GPS に類似したレコードに変換する R パッケージです。各行は、指定された空間解像度における各車両のタイムスタンプを表します。
- [gtfs2emis](https://github.com/ipeaGIT/gtfs2emis) - General Transit Feed Specification (GTFS) データに基づいて公共交通車両の排出レベルを推定する R パッケージです。
- [gtsf](https://github.com/r-gtfs/gtsf) - R における general transit (GTFS) simple (geographic) features (sf) です。GDAL を通じて、GTFS から Shapefile、GeoJSON、およびその他の形式への変換に使用できます。
- [hafas-generate-gtfs](https://github.com/derhuerst/hafas-generate-gtfs) *(進行中)* – HAFAS エンドポイントから GTFS ダンプを生成する Javascript ツールです。
- [Hafas2GTFS](https://github.com/geops/hafas2gtfs) - Python で記述された Hafas2GTFS コンバーターであり、SBB HAFAS フィード向けに最適化されています。
- [kml-to-gtfs-shapes](https://github.com/bdferris/kml-to-gtfs-shapes/tree/gh-pages) - KML ファイルのポリラインを GTFS shapes.txt ファイルに変換する Javascript ツールです。GitHub の[こちら](http://bdferris.github.io/kml-to-gtfs-shapes/)でホストされています。
- [NeTEx-to-GTFS Converter Java](https://github.com/entur/netex-gtfs-converter-java) - NeTEX データセットを GTFS データセットに変換します。入力 NeTEx データセットは Nordic NeTEx Profile に従うことが必須です。
- [o2g](https://github.com/hiposfer/o2g) - OpenStreetMap から GTFS フィードを抽出するシンプルなツールです。
- [Open-Transport SYNTHESE Convertors](https://github.com/Open-Transport/synthese/wiki) - French-Transmodel、SIRI、NETeX、HAFAS、HASTUS、VDV452 などを変換します。
- [onebusaway-gtfs-to-barefoot](https://github.com/OneBusAway/onebusaway-gtfs-to-barefoot) - GTFS ファイルから [Barefoot](https://github.com/bmwcarit/barefoot) mapfile を作成する Java ツールです。
- [onebusaway-vdv-modules](https://github.com/OneBusAway/onebusaway-vdv-modules) - VDV-452 時刻表データを GTFS に変換することを含む、VDV 形式の公共交通データを扱うための Java ライブラリです。
- [osm2gtfs](https://github.com/grote/osm2gtfs) - OpenStreetMap データおよび時刻表情報を GTFS に変換します。
- [transit_model](https://github.com/hove-io/transit_model) - 次の形式との相互変換を行う Rust ライブラリです: GTFS、NTFS（Navitia 用、[API 作成用ソフトウェア](#software-for-creating-apis)を参照）、TransXChange（英国仕様）、KV1（オランダ仕様）、NeTEx（EU 仕様）。
- [transloc-gtfs-rectifier](https://github.com/laidig/transloc-gtfs-rectifier) - [TransLoc](http://transloc.com/) の API が GTFS `stop_ids` を提供していないため、[TransLoc](http://transloc.com/) ID に GTFS stop_ids を割り当てようとする Python アプリケーションです。[TransLoc's API](https://market.mashape.com/transloc/openapi-1-2) を使用します。
- [Transmodel and IFF to GTFS](https://github.com/bliksemlabs/bliksemintegration) - 鉄道ネットワークの時刻表をインポートするために、(Transmodel) BISON Koppelvlak1、IFF（HP/EDS により記述された形式で、ATCO CIF にやや類似）をインポートおよび同期します。内部の疑似 NETeX データ構造により GTFS へのエクスポートが可能であり、NETeX、GTFS、IFF などの他形式へエクスポートする概念実証もあります。
- [Transporter-Project transxchange-to-gtfs](https://github.com/Transporter-Project/transxchange-to-gtfs) Objective-C で記述された TransXChange から GTFS へのコンバーターです。
- [TXC TransXChange publisher (UK Department for Transport)](https://www.gov.uk/government/publications/transxchange-publisher) - TXC TransXChange publisher は、TransXChange 準拠の XML ドキュメントを読みやすく印刷しやすい形式で公開するために使用できる、スタンドアロンのソフトウェアツールです。
- [UK2GTFS](https://itsleeds.github.io/UK2GTFS/) - 英国形式の TransXchange（バス、地下鉄、路面電車、フェリー）および CIF（鉄道）時刻表を GTFS に変換する R パッケージです。

### GTFS データ収集および保守ツール {: #gtfs-data-collection-and-maintenance-tools}


- [AddTransit](https://addtransit.com/gtfs-transit-file.php) - GTFS形式のスケジュールを作成、編集、公開するためのSaaS（Software as a Service）プラットフォームです。
- [bus-router](https://github.com/atlregional/bus-router) - [Google Maps Directions API](https://developers.google.com/maps/documentation/directions/) または [OSRM](https://github.com/Project-OSRM/osrm-backend/wiki/Server-api) によるルーティングを使用して、GTFS用の不足しているshapes.txtを生成するPythonスクリプトです。
- [gtfs-blocks-to-transfers](https://github.com/TransitApp/GTFS-blocks-to-transfers) - [trip.block\_id](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#example-blocks-and-service-day) を設定することで定義されるGTFSブロックを、一連の[便から便への乗換（提案）](https://github.com/google/transit/pull/303)に変換するPythonツールです。
- [GTFS Diff](https://transport.data.gouv.fr/tools/gtfs_diff) - GTFS Diffはtransport.data.gouv.frによって作成された仕様であり、GTFSファイル間の差分を表現するためのシンプルで統一された方法を提供することを目的としています。
- [GTFS Editor](https://github.com/conveyal/gtfs-editor) - （セルフホスト型の）WebベースGTFS編集フレームワークです。（注：このプロジェクトは非推奨となっており、[IBI Data Tools](https://github.com/ibi-group/datatools-ui)に置き換えられています。）
- [GTFS Editor for Vagrant](https://github.com/laidig/vagrant-gtfs-editor) - [Vagrant](https://www.vagrantup.com/)を使用して、上記のGTFS Editorを迅速にセットアップします。
- [static-GTFS-manager](https://github.com/WRI-Cities/static-GTFS-manager) - static GTFSを作成、編集、エクスポートするための（セルフホスト型の）ブラウザベースのユーザーインターフェースです（[関連投稿](https://groups.google.com/forum/#!topic/transit-developers/GFz5rTJTB0I)を参照してください）。 
- [TransitWand](https://github.com/conveyal/transit-wand) - 交通データを収集するためのオープンソースのWebおよびモバイルアプリケーションです。GTFSフィードの作成、乗客数の記録、またはGISデータセットの生成に使用できます。
- [IBI Data Tools](https://github.com/ibi-group/datatools-ui) - GTFSの編集、検証、品質チェック、およびOpenTripPlannerへのデプロイを扱うWebアプリケーションです。（非推奨となった[Gtfs Data Manager](https://github.com/conveyal/gtfs-data-manager)および[GTFS Editor](https://github.com/conveyal/gtfs-editor)の機能を統合し、その機能を基盤としています。）
- [IBI Data Tools Infra](https://github.com/cal-itp/ibi-datatools-infra) - 上記のIBI Data Toolsプロジェクトのローカルインスタンスを迅速にセットアップして実行するためのツールです。  
- [GTFS.html](https://gtfs.pleasantprogrammer.com) - GTFSフィードを表示する完全にブラウザベースのツールです。ルート・路線系統(route)、停留所等(stop)、時刻表などの表示に使用できます。
- [pfaedle](https://github.com/ad-freiburg/pfaedle) - OpenStreetMapデータを使用したGTFSの高精度なマップマッチングです。
- [GTFS shape mapfit](https://github.com/HSLdevcom/gtfs_shape_mapfit) - GTFS shapeファイルおよび停留所等(stop)を指定されたOSMマップファイルに適合させるPythonツールです。マッチングには[pymapmatch](https://github.com/tru-hy/pymapmatch)を使用します。
- [GTFS Builder](http://nationalrtap.org/Web-Apps/GTFS-Builder) - GTFSファイルの作成を支援する無料のWebベースアプリケーションです。National Rural Transit Assistance Program（RTAP）によって保守されています。
- [gtfs-station-builder](https://github.com/kostjerry/gtfs-station-builder) - 駅の内部構造（pathways.txtを含む）の構築を支援するUIツールです。
- [GTFS Text-to-Speech Tester](https://github.com/BlinkTagInc/node-gtfs-tts) - stops.txt内のtts_stop_nameに読み上げ用フィールド(text-to-speech field)値が必要なものを判定するため、Text-to-Speechを使用してGTFSの停留所等(stop)名を読み上げるコマンドラインツールです。
- [Spare GTFS-Flex Builder](https://sparelabs.com/en/spare-gtfs-flex-builder) - 交通事業者がGTFS-Flex形式の交通データを容易に作成、管理、エクスポートできるよう支援する無料ツールです。 
- [Swiftly](https://goswift.ly/) - リアルタイム交通データを生成するツールです。
- [Chouette SaaS](https://bitbucket.org/enroute-mobi/chouette-core) - GTFS Scheduleデータを生成するツールです。
- [Ara SaaS](https://bitbucket.org/enroute-mobi/ara) - GTFS Realtimeデータを生成するツールです。
- [Amarillo](https://github.com/mfdz/amarillo) - カープーリングのオファーを集約・拡充し、GTFS(-RT)として公開します。

### GTFS マージツール {: #gtfs-merge-tools}

- [combine_gtfs_feeds](https://github.com/psrc/combine_gtfs_feeds) - 複数のgtfs feedを1つのfeed/datasetに統合するPythonツールです。
- [GTFS Kit](https://github.com/mrcagney/gtfs_kit) - General Transit Feed Specification (GTFS) データを分析および統合するためのPython 3.8+ツールキットです。[feedを集約およびクリーンアップする方法に関する情報はこちらで提供されています](https://mrcagney.github.io/gtfs_kit_docs/index.html#module-gtfs_kit.cleaners)。
- [Transitfeed merge function](https://github.com/google/transitfeed/wiki/Merge) - 2つの異なるGTFS feedを統合する関数を備えたPythonライブラリです。
- [gtfsmerge](https://github.com/now8-org/gtfsmerge) - GTFS ZIPアーカイブを1つに統合するPythonスクリプトです。

### GTFS 分析ツール {: #gtfs-analysis-tools}


- [GTFS Kit](https://github.com/mrcagney/gtfs_kit) - General Transit Feed Specification (GTFS) データを分析するための Python 3.6+ ツールキットです。[GTFSTK](https://github.com/araichev/gtfstk) の後継です。
- [gtfstools](https://github.com/ipeaGIT/gtfstools) - R で GTFS 形式の交通フィードを編集・分析するための便利なツールセットです。
- [transit_service_analyst](https://github.com/psrc/transit_service_analyst) - 交通サービス分析を支援する Python ライブラリです。
- [Peartree](https://github.com/kuanb/peartree) - ネットワーク分析のために交通データを有向グラフへ変換する Python ライブラリです。
- [R5: Rapid Realistic Routing on Real-world and Reimagined networks](https://github.com/conveyal/r5) - Conveyal が開発した、マルチモーダル（交通機関/自転車/徒歩/自動車）ネットワーク向けの Java ベースの経路探索エンジンです。現在、シナリオ計画および分析を目的として、時間枠にわたる複数の旅程を計画します。関連する R ラッパーパッケージ（[r5r](https://github.com/ipeaGIT/r5r/)）は、IPEA により独立して開発されています。以下にリンクされている Higgins et al. (2022) による性能比較も参照してください。
- [tidytransit](https://github.com/r-transit/tidytransit) - GTFS データを tibbles および simple features dataframes に読み込み、交通機関の停留所等(stop)とルート・路線系統(route)を地図化し、移動時間と交通機関の運行頻度を計算し、交通フィードを検証するための R パッケージです。
- [transitr](https://github.com/tmelliott/transitr) - 車両 ETA を取得するために、リアルタイムで交通ネットワークを構築・モデル化する R パッケージです。
- [transit-intensity](https://github.com/ioTransit/transit-intensity) - Go で記述された、交通機関の運行密度を測定するためのシンプルなプロジェクトです。
- [Busbuzzard](https://github.com/bmander/busbuzzard) - 交通車両に関する実測データから確率的な時刻表を推定します。
- [ESRI ArcGIS Public Transit Tools (GTFS)](https://github.com/Esri/public-transit-tools) - ArcGIS で公共交通データを扱うためのツールです。
- [GTFS-to-Chart](https://github.com/BlinkTagInc/gtfs-to-chart) - GTFS データから、交通ルート上のすべての車両を示す列車ダイヤグラムを作成します。
- [GTFS Display](https://codeberg.org/dancingCycle/gtfs-display) - GTFS データを分析、監視、維持します（[インスタンス例](https://www.swingbe.de/activity/gtfs-display/)）。
- [PTNA](https://wiki.openstreetmap.org/wiki/Public_Transport_Network_Analysis) - Public Transit Nework Analysis は、OSM にマッピングされた公共交通路線に関する情報を検索・集約するためのオープンソースシステムです。

### GTFS 時刻表公開ツール {: #gtfs-timetable-publishing-tools}


- [GTFS-to-HTML](https://gtfstohtml.com) - GTFS から直接、HTML または PDF 形式の人間が読みやすい時刻表を生成します。
- [Timetable Kit](https://github.com/neroden/timetable_kit) - [GTFS Kit](https://github.com/mrcagney/gtfs_kit) に依存するオープンソースの Python 3.10 モジュールおよびスクリプトであり、柔軟なレイアウトで複雑な印刷用/PDF 時刻表を作成するよう設計されています。現在は Amtrak の GTFS でのみそのまま動作しますが、活発に開発されています。 
- [TimeTablePublisher (TTPUB)](https://github.com/OpenTransitTools/ttpub) - TriMet によって開発された Web 公開システムであり、交通事業者が生のスケジューリングデータを確認、変更、および変換して、顧客案内を目的とした読みやすい時刻表にすることを可能にします。

### GTFS バリデーター {: #gtfs-validators}


- [Conveyal の gtfs-validator](https://github.com/conveyal/gtfs-validator) - OneBusAway GTFS Modules に基づく Java ベースの GTFS バリデーターです。Java で実行され、Google 提供のものより高速です。
- [Conveyal の gtfs-lib](https://github.com/conveyal/gtfs-lib/) - Conveyal 独自の [gtfs-validator](https://github.com/conveyal/gtfs-validator) の後継です。任意のサイズの GTFS フィードをディスクバックドストレージで読み込み・保存するための Java ベースのライブラリです。
- [Google の feedValidator](https://github.com/google/transitfeed/wiki/FeedValidator) - Google がサポートする Python ベースの GTFS バリデーターです。
- [GTFS Data Package Specification](https://github.com/Stephen-Gates/GTFS) - Good Tables による検証を実現する Data Package 仕様です。データパッケージ、スキーマ、テストを含み、South East Queensland の GTFS データを例として使用しています。
- [gtfstidy](https://github.com/patrickbr/gtfstidy) - GTFS フィードを整理および検証するための Go ベースのツールです。
- [gtfsclean](https://github.com/public-transport/gtfsclean) - GTFS フィードを確認、サニタイズ、および最小化するためのツールです。gtfstidy のフォークであり、まだ上流にマージされていない追加の修正が含まれています。
- [gtfs-validator-api](https://github.com/cal-itp/gtfs-validator-api) - [MobilityData/gtfs-validator](https://github.com/MobilityData/gtfs-validator) の薄いラッパーである Python パッケージです。生成された中間ファイルを処理し、gtfs-validator の出力ファイルを見つけて、特定の名前を付けたり文字列として返したりできるようにします。
- [GTFSVTOR](https://github.com/mecatran/gtfsvtor) - [Mecatran](https://www.mecatran.com/) が保守する、GPLv3 の下でライセンスされた Java 実装のオープンソース GTFS バリデーターです。
- [MobilityData の gtfs-validator](https://github.com/MobilityData/gtfs-validator) - [MobilityData](https://mobilitydata.org/) が保守する、Apache v2.0 の下でライセンスされた Java 実装の、GTFS 仕様に正統に従うオープンソース GTFS バリデーターです。
- [Reflect GTFS Validator（Foursquare ITP がホスト）](https://reflect.foursquareitp.com) - [Foursquare ITP](https://www.foursquareitp.com) による交通スケジュールおよび GTFS 検証プラットフォームです。[gtfs-lib](https://github.com/conveyal/gtfs-lib/) に基づく無料の Web ベース GTFS バリデーターを含みます。
- [Transit App の gtfs-fares-v2-validator](https://github.com/TransitApp/gtfs-fares-v2-validator) - [ドラフト仕様](https://docs.google.com/document/d/19j-f-wZ5C_kYXmkLBye1g42U-kvfSVgYLkkG5oyBauY/edit#) に基づいて GTFS-Fares-v2 データを検証する Python ツールです。
- [Transport Validator](https://github.com/etalab/transport-validator/) - [Rust](https://www.rust-lang.org/) で実装されたオープンソースのバリデーターです。[French National Access Point](https://transport.data.gouv.fr/validation/) で使用されています。
- [gtfs-accessiblity-validator](https://github.com/BlinkTagInc/gtfs-accessibility-validator) - GTFS ファイル内のアクセシビリティ関連フィールドおよびファイルの存在を検証します。コマンドラインツールまたは node.js パッケージとして使用できます。

## GTFS Realtime {: #gtfs-realtime}


- [GTFS-realtime ドキュメント](https://github.com/google/transit/tree/master/gtfs-realtime)。
- [GTFS-realtime Autodoc](https://laidig.github.io/gtfs-rt-autodoc/index.html) - 公式の[GTFS-realtime protocol buffer specification](https://github.com/google/transit/blob/master/gtfs-realtime/proto/gtfs-realtime.proto)から生成され、一部の拡張を含む、GTFS-realtime のために自動生成されたドキュメントです。

### GTFS Realtime ライブラリとデモアプリ {: #gtfs-realtime-libraries-demo-apps}


- [gtfs-realtime-bindings](https://github.com/google/gtfs-realtime-bindings) - 公式の [GTFS-realtime protocol buffer specification](https://github.com/google/transit/blob/master/gtfs-realtime/proto/gtfs-realtime.proto) から生成された、Java、.NET、Node.js、Python、Ruby 向けの公式バインディングです。
- [gtfs-rt](https://crates.io/crates/gtfs-rt) - GTFS-Realtime データの読み取り、書き込み、操作を行うための Rust crate です。
- [GTFS-realtime Exporter](https://github.com/OneBusAway/onebusaway-gtfs-realtime-exporter/wiki) - GTFS-relatime feed の生成および共有を支援する Java ベースのツールです。
- [GTFS-realtime Alerts Producer Demo](https://github.com/OneBusAway/onebusaway-gtfs-realtime-alerts-producer-demo/wiki) - GTFS-realtime Service Alerts を生成するための Java ベースのデモプロジェクトです。
- [GTFS-realtime Alerts Producer Web Application](https://github.com/OneBusAway/onebusaway-service-alerts) - GTFS-realtime Service Alerts を生成するための Java ベースの Web アプリケーションです。
- [GTFS-realtime TripUpdates & VehiclePositions Producer Demo](https://github.com/OneBusAway/onebusaway-gtfs-realtime-trip-updates-producer-demo/wiki) - GTFS-realtime TripUpdates（推定到着時刻）および Vehicle Positions を生成するための Java ベースのデモプロジェクトです。
- [GTFS-realtime Vehicle Positions Consumer/Visualizer Demo](https://github.com/OneBusAway/onebusaway-gtfs-realtime-visualizer) - GTFS-realtime Vehicle Positions feed を利用し、この情報を地図上に表示するための Java ベースのデモプロジェクトです。

### GTFS Realtime バリデーター {: #gtfs-realtime-validators}


- [gtfs-realtime-validator](https://github.com/MobilityData/gtfs-realtime-validator) - 当初はUniversity of South Floridaの[Center for Urban Transportation Research](https://www.cutr.usf.edu/)によって開発され、現在は[MobilityData](https://mobilitydata.org/)によって保守されているGTFS Realtime検証ツールです。

### GTFS Realtime（およびその他のリアルタイム API）アーカイブツール {: #gtfs-realtime-and-other-real-time-api-archival-tools}


- [GTFS-realtime to SQL](https://github.com/OpenMobilityData/GtfsRealTimeToSql) - GTFS-RealTime feed を SQL データベースに解析します（[OpenMobilityData.org](https://openmobilitydata.org) で使用されています）
- [gtfsrdb](https://github.com/CUTR-at-USF/gtfsrdb) - GTFS-realtime feed の読み取りおよびデータベースへのアーカイブをサポートする Python ツールです
- [retro-gtfs](https://github.com/SAUSy-Lab/retro-gtfs) - Nextbus API からリアルタイムデータを収集し、GTFS 形式（すなわち、遡及的 GTFS）でアーカイブする Python アプリケーションです。
- [Transi](https://gitlab.com/cutr-at-usf/transi) - Cloud-native GTFS-RT/GTFS アーカイブシステムです。
- [GTFS-Realtime-Capsule](https://github.com/tsdataclinic/gtfs-realtime-capsule) - リアルタイム公共交通データをスクレイピング、正規化、およびアーカイブするコマンドラインツールです。
- [gtfsdb_realtime](https://github.com/OpenTransitTools/gtfsdb_realtime) - リアルタイム GTFS データベースローダーおよび ORM ライブラリです

### GTFS Realtime コンバーター {: #gtfs-realtime-convertors}


- [SIRI から GTFS-realtime](https://github.com/OneBusAway/onebusaway-gtfs-realtime-from-siri-cli) - [SIRI format](https://www.siri.org.uk/) から GTFS-realtime へ変換する Java ベースのコマンドラインユーティリティです。
- [OrbCAD SQL Server から GTFS-realtime](https://github.com/CUTR-at-USF/HART-GTFS-realtimeGenerator/) - OrbCAD SQL Server から車両位置情報(vehicle position)および便の更新(trip update)情報を抽出し、GTFS-realtime の TripUpdates および VehiclePositions 形式へエクスポートする Java ベースのコマンドラインユーティリティです。
- [NextBus API から GTFS-realtime](https://github.com/OneBusAway/onebusaway-gtfs-realtime-from-nextbus-cli/wiki) - [NextBus API format](http://www.nextbus.com/xmlFeedDocs/NextBusXMLFeed.pdf) から GTFS-realtime へ変換する Java ベースのコマンドラインユーティリティです。NextBus は現在、自社製品向けに GTFS-realtime API を直接提供していることに注意してください。[Cubic site](http://nextbus.cubic.com/Products/Real-Time-Rider-Information) および [this FAQ](https://medium.com/omnimodal/want-more-riders-open-up-your-nextbus-api-with-gtfs-realtime-7387c80f31e1#.pkuzizhl5) を参照してください。
- [Syncromatics API から GTFS-realtime](https://github.com/CUTR-at-USF/bullrunner-gtfs-realtime-generator) - [Syncromatics API](http://www.syncromatics.com/) 形式から GTFS-realtime TripUpdates および VehiclePositons へ変換する Java ベースのコマンドラインユーティリティです。
- [KV6、15、17、および ARNU から GTFS-realtime](https://github.com/bliksemlabs/bliksemintegration-realtime) - 受信した KV6、15、17、および ARNU を処理し、RID 統合データベースに存在する静的な交通データと照合する Java ベースのツールです。その後、このデータを ARNU RITinfo、GTFS(realtime)、および KV78turbo としてエクスポートします。
- [WMATA BusPositions API から GTFS-realtime](https://github.com/kurtraschke/wmata-gtfsrealtime) - WMATA の [BusPositions API](https://developer.wmata.com/docs/services/54763629281d83086473f231/operations/5476362a281d830c946a3d68) および [MetroAlerts](http://www.wmata.com/rider_tools/metro_service_status/rail_bus.cfm?) の運行情報(alert) RSS フィードから、GTFS-realtime TripUpdates、VehiclePositions、および Alerts フィードへ変換する Java ベースのツールです。
- [SEPTA API から GTFS-realtime](https://github.com/kurtraschke/septa-gtfsrealtime) - [SEPTA](http://www.septa.org/) の[リアルタイムバスおよび鉄道データ](http://www3.septa.org/hackathon/)を GTFS-realtime へ変換する Java ベースのツールです。
- [CTA API から GTFS-realtime](https://github.com/kurtraschke/ctatt-gtfsrealtime) - [CTA](http://www.transitchicago.com/) の [Train Tracker data](http://www.transitchicago.com/developers/traintracker.aspx) を GTFS-realtime へ変換する Java ベースのツールです。
- [Detroit DOT から GTFS-realtime](https://github.com/prashtx/ddot-avl) - [DDOT](http://www.detroitmi.gov/How-Do-I/Locate-Transportation/Bus-Schedules) の TransitMaster インストール環境（データベース）からリアルタイム情報を抽出し、GTFS-realtime へ変換します。
- [Live Transit Event Trigger](https://github.com/ipublic/live_transit_event_trigger) - [Ride On](http://www.montgomerycountymd.gov/dot-transit/) の OrbCAD データベースからデータを抽出し、GTFS-realtime としてエクスポートします。
- [SoundTransit から GTFS-realtime](https://github.com/bdferris/onebusaway-sound-transit-realtime) - [Sound Transit](http://www.soundtransit.org/) からのテキストファイルフィードを GTFS-realtime へ変換します。
- [Civic Transit](https://github.com/jestin/CivicTransit) - [KCATA](http://www.kcata.org/) の TransitMaster WebWatch インストール環境をスクリーンスクレイピングし、GTFS-realtime フィードを生成します。
- [GTFS-realtime VehiclePositions から GTFS-realtime TripUpdates（TransitClock）](https://thetransitclock.github.io/) - 生の車両位置情報(vehicle position)を取り込み、GTFS-realtime などの形式で予測時刻を生成できる Java アプリケーションです。以前は「Transitime」として知られていました。
- [gtfs-realtime-translators](https://github.com/Intersection/gtfs-realtime-translators) - カスタム到着 API 形式を GTFS-realtime へ変換する Python ベースのツールです。2019年7月時点で、LA Metro および SEPTA をサポートしています。
- [Transloc API から GTFS-realtime](https://github.com/jonathonwpowell/transloc-to-gtfs-real-time) - Transloc API を GTFS-realtime へ変換する Node.js ベースのツールです。
- [hafas-gtfs-rt-feed](https://github.com/derhuerst/hafas-gtfs-rt-feed) – HAFAS endpoint から GTFS Realtime フィードを生成する Javascript ツールです。
- [GTFS-realtime から SIRI-Lite](https://github.com/etalab/transpo-rt/) - 複数の GTFS-RT フィードを SIRI-Lite API へ変換する [Rust](https://www.rust-lang.org/) Web サーバーです。

### GTFS Realtime ユーティリティ {: #gtfs-realtime-utilities}


- [bus_kalman](https://github.com/cmoscardi/bus_kalman) - NYC MTAのリアルタイムデータを使用してバスの移動時間を補間するために使用されるKalman Filterです。
- [Concentrate](https://github.com/mbta/concentrate) - 複数のソースからのリアルタイム交通情報を単一の出力ファイルに統合します。[Massachusetts Bay Transportation Authority (MBTA)](https://github.com/mbta)によって保守されています。
- [gtfs-realtime-test-service](https://github.com/CUTR-at-USF/gtfs-realtime-test-service) - GTFS-realtimeフィードのコンテンツをモックするためのツールです（例: GTFS-realtimeを利用するアプリケーションのテストで使用します）。
- [GTFS-realtime Munin Plugin](https://github.com/OneBusAway/onebusaway-gtfs-realtime-munin-plugin) - GTFS-realtimeフィードに関する情報を記録するための[Munin](http://munin-monitoring.org/)プラグインを提供します。
- [GTFS-realtime Nagio Plugin](https://github.com/OneBusAway/onebusaway-gtfs-realtime-nagios-plugin) - GTFS-realtimeフィードを監視するための[Nagios](https://www.nagios.org/)プラグインを提供します。
- [GTFS-realtime Printer](https://github.com/laidig/gtfs-rt-printer) - GTFS-realtimeファイルまたはURLから情報を出力するJavaベースのユーティリティです。
- [gtfs-rt-admin](https://github.com/conveyal/gtfs-rt-admin) - GTFS-RTの運行情報(alert)を管理するための管理ツールです（JavaScriptおよびJava）。
- [gtfs-rt-differential-to-full-dataset](https://github.com/derhuerst/gtfs-rt-differential-to-full-dataset) – 継続的なGTFS Realtimeストリームの`DIFFERENTIAL`増分データを`FULL_DATASET`ダンプに変換するJavascriptツールです。
- [gtfs-rt-dump](https://github.com/kurtraschke/gtfs-rt-dump) - GTFS-realtimeフィードをプレーンテキストで容易に閲覧できるよう、protocol buffer形式をプレーンテキストに変換します（デバッグ目的）。
- [gtfs-rt-inspector](https://public-transport.github.io/gtfs-rt-inspector/) – 任意の（CORS対応）GTFS Realtimeフィードを検査・分析するWebアプリです。[GitHub](https://github.com/public-transport/gtfs-rt-inspector)でオープンソースとして公開されています。
- [GTFS Data Pipeline for TfNSW Bus Datasets](https://github.com/teckkean/GTFS-Data-Pipeline-TfNSW-Bus) - TfNSWのGTFS StaticおよびRealtimeデータセット向けに開発されたデータパイプラインです。このパイプラインを使用して生成されたデータセットは、Public Transport Information and Priority System (PTIPS)を介したTfNSWのTransit Signal Priority Requestの性能検証に使用されています。
- [manual-gtfsrt](https://github.com/pailakka/manual-gtfsrt) - 編集可能なJSONから作成されたGTFS-RTフィードを提供するGoベースのツールです。
- [print-gtfs-rt-cli](https://github.com/derhuerst/print-gtfs-rt-cli) – stdinからGTFS Realtimeフィードを読み取り、人間が読める形式またはJSONとして出力するJavascriptツールです。
- [transitcast](https://github.com/OpenTransitTools/transitcast) - GTFSおよびGTFS-RTの車両位置情報(vehicle position)フィードを使用し、各車両が予定された停留所等(stop)から予定された停留所等(stop)へ移動するのに要する推定移動時間を生成して、これらを"observed_stop_time"テーブルに記録します。これらのレコードは後で、車両移動予測を行う機械学習モデルの訓練に使用できます。[FTA IMIプロジェクト](https://trimet.org/imi/program.htm)の一環としてTriMetが作成しました。
- [transit-feed-quality-calculator](https://github.com/CUTR-at-USF/transit-feed-quality-calculator) - [gtfs-realtime-validator](https://github.com/CUTR-at-USF/gtfs-realtime-validator)を使用して多数の交通フィードの品質を評価するJavaプロジェクトです。フィードURLはグローバルディレクトリ（[TransitFeeds.com/OpenMobilityData.org](https://openmobilitydata.org/)）から取得します。
- [Transit Network Model](https://github.com/tmelliott/TransitNetworkModel) - GTFS-realtimeのVehiclePositions、particle filter、およびKalman Filterを使用して予測を生成するツールです。 
- [GTFS Realtime Display](https://codeberg.org/dancingCycle/gtfs-rt-display) - GTFS Realtimeデータを分析、監視、保守します。[インスタンス例](https://www.swingbe.de/activity/gtfs-rt-display/)
- [GTFS Realtime Prediction Accuracy metrics](https://docs.google.com/document/d/1-AOtPaEViMcY6B5uTAYj7oVkwry3LfAQJg3ihSRTVoU/edit#heading=h.j27shba7rlk6) - GTFS-Realtimeに有用な性能指標です。

## SIRI {: #siri}


- [SIRI API](https://github.com/OneBusAway/onebusaway/wiki/SIRI-Resources) - v1.0およびv1.3の[SIRI](https://www.siri.org.uk/)スキーマから生成されたJavaクラスです。
- [SIRI 2.0 API](https://github.com/laidig/siri-20-java) - v2.0の[SIRI](https://www.siri.org.uk/)スキーマから生成されたJavaクラスです。
- [SIRI to GTFS-realtime](https://github.com/OneBusAway/onebusaway-gtfs-realtime-from-siri-cli/wiki) - [SIRI format](https://www.siri.org.uk/)からGTFS-realtimeへ変換するためのJavaベースのコマンドラインユーティリティです。
- [SIRI 2.0 Autodoc](https://laidig.github.io/siri-20-java/doc/) - （非常に優れた）注釈付きSIRI 2.0 Schema Definitionから自動生成されたドキュメントです。
- [King County Metro Legacy AVL to SIRI](https://github.com/bdferris/onebusaway-king-county-metro/tree/master/onebusaway-king-county-metro-legacy-avl-to-siri) - [King County Metro](http://metro.kingcounty.gov/)のLegacy AVL形式をSIRIへ変換するためのJavaベースのツールです。
- [SIRI REST Client](https://github.com/CUTR-at-USF/SiriRestClient/wiki) - 現在[MTA Bus Time API](http://bustime.mta.info/wiki/Developers/SIRIIntro)で使用されているものなど、リアルタイム交通データ向けのRESTful SIRIインターフェースと連携するためのオープンソースAndroidライブラリです。
- [SIRI 1.3 POJOs (Android-compatible)](https://github.com/CUTR-at-USF/onebusaway-siri-api-v13-pojos/wiki) - SIRI v1.3 APIのレスポンスをデータバインディング（XML/JSONのデシリアライズ）するために使用される、Android互換のPlain Old Java Objects（POJOSs）です。[SIRI REST Client](https://github.com/CUTR-at-USF/SiriRestClient/wiki)で使用されています。
- [pysiri2validator](https://github.com/laidig/pysiri2validator) - Python 3で記述されたSIRI 2.0用のシンプルなバリデーターです。
- [Edwig](https://github.com/af83/edwig) - SIRIプロトコルを使用する、リアルタイム公共交通データ交換のためのgolangサーバーです。
- [BISON](https://bison.dova.nu/standaarden/nederlands-siri-profiel) - オランダにおけるSIRIの実装です。

## その他のマルチモーダルデータ形式 {: #other-multimodal-data-formats}

### 広く採用されている {: #widely-adopted}

- [APDS](https://www.allianceforparkingdatastandards.org/) - Alliance for Parking Data Standards: [International Parking Institute (IPI)](https://www.parking.org/)、[British Parking Association (BPA)](http://www.britishparking.co.uk/)、および [European Parking Association (EPA)](http://www.europeanparking.eu/) によって設立されました。APDS は、組織が世界中のプラットフォーム間で駐車データを共有できるようにする、統一されたグローバル標準を開発、推進、管理、および維持することを使命とする非営利組織です。
- [DATEX](https://datex2.eu/) - 道路交通および旅行情報のための EU データ標準です。
- [GBFS](https://gbfs.org/) - General Bikeshare Feed Specification: bikeshare、scootershare、mopedshare、および carshare に関するリアルタイム情報のためのオープンデータ標準です。
    - [gbfs R package](https://github.com/simonpcouch/gbfs) - R で GBFS feeds と連携するための関数であり、ユーザーは指定した都市/bikeshare プログラムについて、整然とした .rds データセットを保存および蓄積できます。
- [MDS](https://github.com/openmobilityfoundation/mobility-data-specification) - Mobility Data Specification: 自治体および mobility as a service providers のためのリアルタイムデータ共有、測定、および規制を実装するための形式です。政府がプロバイダーを施行、評価、および管理する能力を確保することを目的としています。[Open Mobility Foundation](https://www.openmobilityfoundation.org/) によって維持されています。
- [NeTex](http://netex-cen.eu/) - [CEN standards process](https://www.cencenelec.eu/european-standardization/european-standards/) によって管理される分散システム間で、複雑な静的交通データを交換するために設計された汎用 XML 形式です。
- [TODS](https://ods.calitp.org/) - Transit Operational Data Standard: 運転士、運行管理者、および計画担当者が交通運行を実施するために使用する交通時刻表を表現するための標準形式です。 
- [TOMP](https://github.com/TOMP-WG/TOMP-API) - Transport Operator Mobility-as-a-service Provider API: 事業者の発見、旅程計画、エンドユーザーとの対話、予約、および支払いのために、交通事業者および mobility-as-a-service providers が使用する API 標準です。

### パイロットまたは開発段階 {: #pilot-or-development-stage}

- [CurbLR](https://github.com/curblr/curblr-spec) - 縁石規制に関する仕様です。
- [Dyno-Demand](https://github.com/osplanning-data-standards/dyno-demand) - San Francisco County Transportation Authority、LMZ LLC、およびUrbanLabs LLCによって開発された、動的ネットワークモデリングに適した個々の乗客の*需要*に焦点を当てたGTFSベースの移動需要データ形式です。
- [Dyno-Path](https://github.com/osplanning-data-standards/dyno-path) - （開発中 - [この投稿](https://github.com/osplanning-data-standards/GTFS-PLUS/pull/52#issuecomment-331231000)を参照）個々の乗客の*軌跡*に関するデータです。
- [GTFS-plus](https://github.com/osplanning-data-standards/GTFS-PLUS) - Puget Sound Regional Council、UrbanLabs LLC、LMZ LLC、およびSan Francisco County Transportation Authorityによって開発された、動的公共交通モデリングに適した*車両および容量データ*のためのGTFSベースの公共交通ネットワーク形式です。
- [GTFS-ride](https://github.com/ODOT-PTS/GTFS-ride) - Oregon Department of TransportationとOregon State Universityのパートナーシップを通じて開発された、オープンな固定ルート公共交通の利用者数データ標準です。
- [GTFS-stat](https://github.com/osplanning-data-standards/GTFS-STAT) - UrbanLabs LLCおよびSan Francisco County Transportation Authorityによって開発された、パフォーマンスデータを含む追加ファイルを備えたGTFS公共交通ネットワークの拡張です。
- [GMNS](https://github.com/zephyr-data-specs/GMNS) - General Modeling Network Specification: 複数モードの静的および動的な交通計画・運用モデルで使用するよう設計された、経路探索可能な道路ネットワークファイルを共有するための形式です。Volpe/FHWAとZephyr Foundationのパートナーシップです。
- [GTNS](https://zephyrtransport.org/trb17projects/7-general-travel-network-specification/) - General Travel Network Specification: 移動需要モデルネットワークを共有するために計画されているデータ仕様です。
- [IXSI](https://github.com/RWTH-i5-IDSG/ixsi) - 移動情報システムとシェアリングシステム（カーシェア、バイクシェア）との間で情報を交換するためのインターフェースです。
- [MTLFS](https://github.com/vta/Managed-and-Tolled-Lanes-Feed-Specification) - Managed and Tolled Lanes Feed Specification: Managed and Tolled Lanes Tolling Feed Specification（MTLFS）を構成し、それらすべてのファイルで使用されるフィールドを定義するスキーマの提案であり、[Santa Clara Valley Transportation Authority](http://www.vta.org/)によって開発されました。
- [MaaS API](https://github.com/maasglobal/maas-tsp-api/blob/master/specs/Booking.md) - MaaS互換APIを定義するオープンドキュメントおよびテストスイートのセットです。
- [NCHRP 08-119 Developing Data Standards and Guidance for Transportation Planning and Traffic Operations - Phase 1 (Anticipated)](http://apps.trb.org/cmsfeed/TRBNetProjectDisplay.asp?ProjectID=4543) - この研究の目的は、交通計画および運用のための静的データとリアルタイムデータの収集、管理、共有において交通コミュニティが使用・採用する標準および／またはガイダンスを開発することです。
- [OMX: The Open Matrix data file format](https://github.com/osPlanning/omx) - 交通モデリング業界で使用される可能性がある、2次元配列オブジェクトおよび関連メタデータの構造化されたコレクションです。
- [OJP](https://github.com/VDVde/OJP) - Open Journey Plannerです。
- [OSDM](https://github.com/UnionInternationalCheminsdeFer/OSDM) - Open Sales and Distribution Model: 鉄道旅行の顧客にとって予約プロセスを大幅に簡素化し、販売事業者および鉄道事業者の複雑性と流通コストを削減することを目指しています。オフラインモデルおよびオンラインAPIの仕様を含みます。[International Union of Railways (UIC)](https://github.com/UnionInternationalCheminsdeFer)によって維持されています。
- [SAE Shared and Digital Mobility Committee](http://articles.sae.org/15799/) - カーシェアおよび交通ネットワーク企業（TNC）／ライドシェア向けのデータ標準に取り組んでいるようです。
- [shared-row](https://github.com/d-wasserman/shared-row) - SharedStreets Referenceのための通行権（ROW）に関する仕様です。
- [TCRP G-16 Development of Transactional Data Specifications for Demand-Responsive Transportation (In progress)](http://apps.trb.org/cmsfeed/TRBNetProjectDisplay.asp?ProjectID=4120) - この研究の目的は、デマンド型交通の提供に関与する事業体向けのトランザクションデータに関する技術仕様を開発することです。完了予定日は2018年後半です。
- [TIDES](https://github.com/TIDES-transit/TIDES) - Transit ITS Data Exchange Specification（TIDES）は、AVL、APC、およびAFCデータを含む過去の公共交通ITSデータのための標準データ構造、API、およびデータ管理ツールを作成するための提案された取り組みです。

## API 作成用ソフトウェア {: #software-for-creating-apis}


交通およびマルチモーダルデータ向けの API を提供するためにセットアップできるソフトウェアです。

- [GraphHopper Routing Engine](https://github.com/graphhopper/graphhopper/#public-transit) OpenStreetMap 向けのオープンソースルーティングエンジンです。Java ライブラリまたはサーバーとして使用できます。
- [gtfs-server](https://github.com/denysvitali/gtfs-server) - PostGIS をバックエンドとして使用し、HTTP endpoint 経由で GTFS データを提供する、Rust で記述された web server です。
- [hafas-rest-api](https://github.com/public-transport/hafas-rest-api) – [HAFAS](https://de.wikipedia.org/wiki/HAFAS) endpoint を REST API として公開します。
- [Linked Connections](http://linkedconnections.org/) - クライアントが（サーバーではなく）ルート計画アルゴリズムを実行できるようにする、オープンソースでスケーラブルなインターモーダルルート計画エンジンです。GTFS データを使用します。
- [Mobroute](http://sr.ht/~mil/mobroute) - Mobroute は、交通事業者自身から時刻表（GTFS）データを直接取り込むことで動作する、汎用 FOSS 公共交通ルーター（例: trip planner）用の Go ライブラリおよび CLI です（データは [Mobility Database](https://database.mobilitydata.org/) から取得されます）。デバイス上で GTFS データに基づくルーティングリクエストを迅速に実行・テストするために（CLI 経由で）使用でき、既存のナビゲーションアプリに GTFS ルーティングを追加するためのライブラリとして組み込むこともできます。
- [MOTIS](https://github.com/motis-project/motis) - C++ および Java で記述された Multi Objective Travel Information System です。GTFS または HAFAS 形式のスケジュール時刻表、および GTFS-RT（ならびに Deutsche Bahn の proprietary format である RISML）形式のリアルタイム情報を入力データとして利用できます。歩行者ルーティング（Per Pedes Routing が処理）および自動車ルーティング（OSRM が処理）には OpenStreetMap データが使用されます。
- [Navitia](https://github.com/hove-io/navitia) は、[Navitia.io](http://www.navitia.io/) live API の背後にある opensource engine です。
- [OneBusAway](http://onebusaway.org/) - GTFS および GTFS-Realtime（[other formats](https://github.com/OneBusAway/onebusaway-application-modules/wiki/Real-Time-Data-Configuration-Guide) とともに）を利用し、それらを使いやすい REST API に変換する Java app です。
- [OpenTripPlanner](http://www.opentripplanner.org/) - マルチモーダルかつ複数事業者に対応した旅程(journey)計画、およびマルチモーダルグラフに関する情報の返却を行うオープンソースプラットフォームです（GTFS や [OpenStreetMap](http://www.openstreetmap.org/) などのデータソースを使用します）。
- [pyBikes](https://github.com/eskerda/pybikes) - 世界中の bikeshare system 情報向け [CityBikes](http://api.citybik.es) を支えるソフトウェアです。
- [Simple Transit Api](https://github.com/ioTransit/simple-transit-api) - Golang で GTFS api を始めるための簡単な方法です。
- [TransitClock](https://thetransitclock.github.io/) - 生の車両位置情報(vehicle position)を利用し、GTFS-realtime などの形式で予測時刻を生成できる Java application です。以前は「Transitime」として知られていました。
- [Transitous](https://transitous.org) - コミュニティ運営の、無料かつオープンな公共交通ルーティングサービスです。
