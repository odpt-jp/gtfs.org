# データの利用 {: #using-data}

## 消費者向けアプリ {: #consumer-apps}

人々が公共交通を利用する際に使用するアプリ。

### Webアプリ (オープンソース) {: #web-apps-open-source}

- [Catenary Maps](https://catenarymaps.org) - RustとSvelteで書かれた、リアルタイムおよびスケジュール対応の世界的な公共交通マップ・ナビゲーションソフトウェア。
- [Instabus](http://instabus.org) - オースティン（CapMetro）の公共交通のリアルタイムマップ。サーバーやバックエンドに依存せず、完全にGitHub Pages上で動作します。
- [OpenTripPlanner Client GWT](https://github.com/mecatran/OpenTripPlanner-client-gwt) - OpenTripPlanner用のGoogle Web ToolkitベースのWebインターフェース。
- [OpenTripPlanner.js](https://github.com/conveyal/otp.js) - OpenTripPlanner用のJavascriptベースのクライアント（現在は開発終了）。
- [OTP-UI React Component Library](https://github.com/opentripplanner/otp-ui) - 便検索Webアプリを構築するために利用できるReact Javascriptコンポーネントライブラリ。デモは[Storybook](http://www.opentripplanner.org/otp-ui)を参照してください。
- [GTFS-realtime Alerts Producer Web Application](https://github.com/OneBusAway/onebusaway-service-alerts) - GTFS-realtime運行情報(alert)を生成するためのJavaベースのWebアプリケーション。
- [HRT BUS Web app](https://github.com/Code4HR/hrt-bus-api) - Hampton Roads TransitのリアルタイムバスデータをAPIとして公開し、開発者がアプリを作成できるようにするHRT Bus API。
- [Transit-Map](https://github.com/vasile/transit-map) - 公共交通の時刻表を用いて車両（マーカー）の位置をルート（ポリライン）に沿って補間し、地図上でアニメーション表示するWebアプリ。
- [Transitive.js](https://github.com/conveyal/transitive.js) - LeafletまたはD3を使用して、公共交通ルートのカスタマイズ可能なWebマップレイヤーを作成。
- [Google I/O Transport Tracker](https://github.com/googlemaps/transport-tracker) - Google I/Oカンファレンスのシャトル到着時刻を表示するアプリ。オープンソースの[transport-tracker project](https://github.com/googlemaps/transport-tracker)に基づいています。注意: 自分で実装するには[Google Maps APIs Premium Planライセンス](https://developers.google.com/maps/pricing-and-plans/)が必要です。
- [1-Click]([http://camsys.software/products/1-click](https://github.com/camsys/oneclick)) - 公共交通、鉄道、ライドシェア、相乗り、ボランティア輸送、パラトランジット、徒歩や自転車など、幅広い移動手段の情報を集約する仮想「便検索アグリゲーター」。
- [Bustime](https://busti.me) - WebSocketによる更新を用いた公共交通のリアルタイム監視。オープンソースは[GitHub](https://github.com/norn/bustime)で公開。
- [Transit Tracker](https://transittracker.ca/#/) - カナダのモントリオール大都市圏およびトロントのリアルタイム車両位置情報(vehicle position)。
- [GTFS Builder](http://nationalrtap.org/Web-Apps/GTFS-Builder) - GTFSファイル作成を支援する無料のWebアプリケーション。National Rural Transit Assistance Program (RTAP) によって管理されています。
- Dede - リアルタイムの移動をマッピングする独立かつ汎用的な旅客情報システム(PIS)。GTFS-Realtime形式のVehicle Positionエンティティを含むメッセージフィード、または[Dedeアプリ](https://github.com/dancesWithCycles/dede-android)をデータソースとして利用可能。
- [MBTA tile-server](https://github.com/mbta/tile-server) - MBTA.comで使用する地図タイルを開発するために必要な要素をすべて含むDockerコンテナを作成するスクリプト。
- [Cadê Meu Busão](https://tarifazerobh.org/cade-meu-busao/) - ブラジル・ベロオリゾンテのバスをリアルタイム追跡。オープンソースは[GitHub](https://github.com/tarifazero/monitoramento)で公開。
- [Tiramisu Transit](https://github.com/CMU-RERC-APT/tiramisu3-pr) - カーネギーメロン大学が開発・展開した、リアルタイムのバス到着情報を表示する適応型モバイル交通アプリ。現在はメンテナンスされていません。

### Webアプリ（クローズドソース） {: #web-apps-closed-source}

- [TransitScreen](http://transitscreen.com/) - すべての地域交通手段を対象としたカスタムリアルタイム表示
- [Citylines.co](https://www.citylines.co) - 交通システムを共同でマッピングするためのプラットフォームで、その歴史的進化に重点を置いています。
- [Bikeshare Map](http://bikes.oobrien.com/) - 世界中の自転車シェアステーションの稼働状況
- [Bongo](http://ebongo.org) - アイオワシティ、コーラルビル、アイオワ大学向けのリアルタイム交通追跡。3つの異なる交通システムを1つのUIに統合。
- [CityMapper Webapp](https://citymapper.com/nyc) - 30以上の都市に対応した、乗換案内とルート状況を提供する非常に洗練されたWebアプリ。
- [TransSee](https://www.transsee.ca/) - 実際の走行時間、車両位置、時刻表、地図に基づくリアルタイム交通予測。プレミアム版では、時刻表、車両位置、停留所到着、時刻表遵守状況、チャートやグラフの詳細な履歴にアクセス可能。追加料金で、このデータに対してカスタムクエリを実行することも可能。
- [YourStop](http://yourstop.info) - GTFSフィードを利用し、停留所におけるライブおよび予定便を表示するモバイルフレンドリーなWebアプリ。MBTA、YRT/Viva、メリーランドMTAで提供開始。
- [DC MetroHero](https://dcmetrohero.net) - ワシントンD.C.地域のWMATAメトロレールおよびメトロバスシステムにおけるリアルタイム車両位置、到着・出発情報。Webアプリ、Android、iOSアプリを提供。

### ネイティブアプリ (オープンソース) {: #native-apps-open-source}


- [KDE Itinerary](https://apps.kde.org/itinerary/) - 旅程の計画用アプリ（デスクトップおよびAndroid）。公共交通のルート検索、オフライン保存、旅程へのイベント追加、鉄道駅のフロアマップ表示などが可能です。[ソースコード](https://invent.kde.org/pim/itinerary)、[GitHub](https://github.com/KDE/itinerary)
- [MACS Transit Android App](https://github.com/yeSpud/MACSTransitApp) - アラスカ州フェアバンクスのMACS Transitシステム向けのAndroid用バストラッカーアプリ。RouteMatch APIを使用しています。
- [Next Train - Connecticut](https://github.com/data-creative/NextTrainCT) - コネチカット州のShore Line East交通事業者が公開している列車時刻表を検索するためのReact-nativeモバイルアプリ。[Next Train API](https://github.com/data-creative/next-train-api)のデプロイに依存しています。
- [Offi Directions](https://gitlab.com/oeffi/oeffi) - ヨーロッパおよびその他地域の交通事業者向けに、旅程計画、時刻表、リアルタイム出発時刻、運行障害情報を提供するAndroidアプリ。
- OneBusAway アプリ - [Android](https://play.google.com/store/apps/details?id=com.joulespersecond.seattlebusbot) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-android)、[Fire Phone](http://www.amazon.com/gp/mas/dl/android?p=com.joulespersecond.seattlebusbot) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-android)、[iOS](https://itunes.apple.com/us/app/onebusaway/id329380089) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-ios)、[Windows Phone](https://www.microsoft.com/en-us/store/apps/onebusaway/9nblggh0cbd9) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-windows-phone)、[Google Glass GDK](https://github.com/OneBusAway/onebusaway-android/pull/219) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-android/pull/219)、[Alexa skill](https://www.amazon.com/OneBusAway/dp/B01ELVUYCW/) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-alexa)
- [OpenTripPlanner Android](https://github.com/CUTR-at-USF/OpenTripPlanner-for-Android/wiki) - [OpenTripPlanner](http://www.opentripplanner.org/) 用のAndroidアプリ
- [OpenTripPlanner iOS](https://github.com/opentripplanner/OpenTripPlanner-iOS) - [OpenTripPlanner](http://www.opentripplanner.org/) 用のiOSアプリ
- [opentripplanner-client-library](https://github.com/CUTR-at-USF/opentripplanner-client-library) - OpenTripPlanner v2サーバーに対してAPIリクエストを行い、旅程計画、自転車レンタル情報、サーバーメタデータを解析するためのKotlin Multiplatformライブラリ。Android、iOS、Webに対応。
- [Transito](http://git.sr.ht/~mil/transito) - データプロバイダに依存しないFOSS公共交通アプリ。公開されているGTFSフィード（[Mobility Database](https://database.mobilitydata.org/) 由来）を利用して地点間のルート検索が可能です。[Mobroute Go API](http://sr.ht/~mil/mobroute)を利用し、ルーティング計算をスマートフォン上で実行できます。現在はAndroidとLinuxをサポートするクロスプラットフォームアプリ。
- [Tiramisu Transit](https://github.com/CMU-RERC-APT/tiramisu3-pr#mobile-app-client) - カーネギーメロン大学が開発・提供した、リアルタイムのバス到着情報を表示する適応型モバイル交通アプリ。Ionicフレームワークで作成。現在はメンテナンスされていません。
- [Transportr](https://github.com/grote/Transportr) - [public-transport-enabler](https://github.com/schildbach/public-transport-enabler) を利用して世界中の多様な交通ネットワークに接続するAndroidアプリ。
- [Trufi App](https://github.com/trufi-association/trufi-app) - [OpenTripPlanner](http://www.opentripplanner.org/) を利用するクロスプラットフォームFlutterアプリ。

### ネイティブアプリ（クローズドソース） {: #native-apps-closed-source}


- [Transit](http://transitapp.com/)
- [CityMapper](https://citymapper.com/)
- [Moovit](http://moovitapp.com/)
- [Transit Display](http://transitdisplay.com/) - マルチモーダルかつリアルタイムの交通表示ソフトウェア。
- [Ualabee](https://ualabee.com/company/) - ユーザー参加型の旅程プランナー。ユーザーは異常の報告、写真のアップロード、交通データの編集、他の乗客とのチャットが可能。
- [ÖPNV Navigator](https://navigatorapp.net/)
- [TripGo](https://tripgo.com/)

## ハードウェア {: #hardware}


実験用および本番用の交通ハードウェア。

- [Bus Tracking GPS](https://github.com/herrdragon/busTrackingGps) - 交通バスを追跡するための安価なオープンソースソリューションのマイアミ向けプロトタイプ用コード。
- [Train departure Display](https://github.com/chrisys/train-departure-display) - Raspberry Pi Zer0 を基盤とした、英国鉄道駅の列車出発案内板を模した、ほぼリアルタイムのミニチュアレプリカ。

## SDKs {: #sdks}

- [TripKit](https://github.com/alexander-albers/tripkit) - TripKit は、公共交通事業者からデータを取得するための Swift ライブラリです。
- [KPublicTransport](https://invent.kde.org/libraries/kpublictransport) - リアルタイムの公共交通データにアクセスし、公共交通の旅程(journey)検索を行うための C++ ライブラリです。
- [SkedGo's TripKit SDKs](https://developer.tripgo.com) - Android、iOS、React 向けのオープンソース SDK で、[SkedGo](https://skedgo.com) の TripGo API にアクセスでき、便(trip)計画の UI コンポーネントを含みます。

## 可視化 {: #visualizations}

### GTFS ベースの可視化 {: #gtfs-based-visualizations}


- [All Transit](https://all-transit.com) - Mapbox GL JS、Deck.gl、Transitland を使用して、GTFS のルートと時刻表をアニメーションでインタラクティブに表示するツール（米国の都市向け）。Github リポジトリは[こちら](https://github.com/kylebarron/all-transit)。
- [BusGraphs Access Analyzer](https://gitlab.com/publictransitanalytics-pub/readme) - 実際の、または仮想の固定ルート型公共交通ネットワークが提供するアクセスを測定し、さまざまな方法でそのアクセスを可視化・分解するための Web アプリケーション。
- [fastest-bus-analysis-in-the-west](https://github.com/vta/fastest-bus-analysis-in-the-west) - Ridership/APC、Swiftly の速度・停車データ、バス停の在庫、GTFS、地理空間の shape を組み合わせ、停留所ごと、ルートごと、時間帯ごとにフィルタ可能なデータセットを作成する Python Pandas スクリプト。このデータセットは [Tableau](https://public.tableau.com/profile/vivek7797#!/vizhome/stopsandspeedanalyses/Story1) で可視化され、VTA のプランナーが停留所統合や専用レーンといった速度向上手法を通じて、バスや鉄道ネットワークをより速く、信頼性の高いものにするための場所を見つけるのに役立ちます。
- [gtfspy-webviz](https://github.com/CxAalto/gtfspy-webviz) - [gtfspy](https://github.com/CxAalto/gtfspy) を使用して GTFS データをアニメーション・可視化する Web アプリケーション。
- [gtfs-to-geojson](https://www.transit.chat/gtfs-to-geojson) - GTFS を GeoJSON に変換するシンプルなオンラインコンバーター（フィード一覧付き）。
- [gtfs-visualizations](https://github.com/cmichi/gtfs-visualizations) - GTFS データセットのルートを可視化するオープンソースの NodeJS アプリケーション。
- [Mapnificent](https://www.mapnificent.net/) - 公共交通機関を使って指定時間内に到達できるエリアを表示。オープンソース [GitHub](https://github.com/mapnificent/mapnificent) で公開、ライブ版は https://www.mapnificent.net/。
- [MIT COAXS](http://mittransportanalyst.github.io/) - アクセシビリティに基づくステークホルダー参加を用いた交通回廊の共創的計画（[OpenTripPlanner Analyst](http://www.opentripplanner.org/analyst/) を使用してルートシナリオを表示）。
- [MOTIS](https://motis-project.de/) - [可視化](https://europe.motis-project.de/) を含むインターモーダル移動情報システム。
- [MTA Frequency](http://www.tyleragreen.com/maps/new_york/) - [Transitland](https://transit.land/) を使用して構築されたニューヨーク市の地下鉄・バスの運行頻度可視化。
- [SEPTA Rail OTP Report](https://apps.phor.net/septa/) - GTFS を使用したオンラインの定時運行レポート・ドリルダウンツール。
- [Simple Transit Map]([https://transit.chat/simple-transit-map](https://github.com/ioTransit/simple-transit-map)) - Web マップをホスト・更新する方法のオンライン例。
- [Simple Transit Site](https://transit.chat/simple-transit-site) - GTFS からすべて作成する交通サイトのオンライン例 [GitHub](https://github.com/ioTransit/simple-transit-site)。
- [TNExT](https://github.com/ODOT-PTS/TNExT) - Transit Network Explorer Tool (TNExT)。オレゴン州の地域・州全体の交通ネットワークを可視化・分析・報告するために開発された Web ベースのソフトウェアツール。
- [Toronto Transit Explorer](https://github.com/sidewalklabs/totx) - トロント市全体の公共交通、自転車、徒歩のアクセシビリティを可視化する Java アプリケーション。[R5](https://github.com/conveyal/r5) の改良版をルーティングに使用。
- [Transit Vis](https://github.com/zackAemmer/transit_vis) - King County Metro の GTFS-RT フィード（OneBusAway API）から得られるパフォーマンス指標を表示する可視化ツール。[こちら](https://www.transitvis.com/) で閲覧可能。[この論文](https://link.springer.com/article/10.1007/s12469-022-00291-7)で使用。
- [TransitFlow](https://github.com/transitland/transitland-processing-animation) - Processing と Transitland を使用して世界中の GTFS データをアニメーション化。
- [TRAVIC Transit Visualization Client](http://tracker.geops.ch/) - 静的 GTFS データ（場合によってはリアルタイムデータ）に基づいて車両の動きを可視化。260 以上の都市に対応。geOps 組織の Github アカウントは[こちら](https://github.com/geops)。
- [Traze](https://traze.app/) by [Veridict](https://www.veridict.com) - 世界中の公共交通車両を可視化。他のユーザーと協力して、事業者からリアルタイムデータが提供されていない場合でも更新を取得可能。GTFS や GTFS-RT を含む複数のソースに基づく。（以前は Livemap24 として知られていた）。
- [Visualizing MBTA Data](http://mbtaviz.github.io/) - ボストンの地下鉄利用状況を示すインタラクティブなグラフ。
- [GTFS Viz 🚉](https://github.com/gabrielAHN/gtfs-viz) - [duckdb-wasm 🦆](https://duckdb.org/docs/api/wasm/overview.html) を使用し、バックエンドなしでクライアントサイド上で大規模に GTFS データをブラウザで可視化する Web アプリ。

### 交通マップ作成 {: #transit-map-creation}


- [Brand New Subway](https://jpwright.github.io/subway/) - プレイヤーがNYC地下鉄システムを自由に改変できる、インタラクティブな交通計画ゲームです。
- [BENO Metro Mapm Creator](https://beno.uk/metromapcreator/#) - 非常に古風ですが、クラシックな交通マップ作成ツールです。
- [Tennessine Metro Designer](https://tennessine.co.uk/metro/) - モダンで美しいデザインの交通マップ作成ツールです。
- [loom](https://github.com/ad-freiburg/loom) - 地理的に正確または模式的な交通マップを自動生成するためのソフトウェアスイートです。
- [Metro Map Maker](https://metromapmaker.com/)   - オープンソースでシンプルな地下鉄マップ作成ソフトウェアです。
- [MetroDreamin'](https://metrodreamin.com/explore) - ユーザーがインタラクティブな交通マップを作成、保存、いいね、共有できるモダンなオープンソースソフトウェアです。
- [Rail Map Generators](https://wongchito.github.io/RailMapGenerator) - 各都市の公共交通システムのスタイルで鉄道マップや情報パネルを生成するツールです。
- [MetroSets](https://metrosets.ac.tuwien.ac.at/) - メトロマップの比喩を用いて集合システムを可視化する柔軟なウェブツールです。詳細はこの[論文](https://www.computer.org/csdl/journal/tg/2021/02/09224192/1nV7Me0F3Lq)に基づいています。

#### 交通可視化のための一般的な描画アプリケーション {: #general-drawing-applications-for-making-transit-visualizations}

- [Adobe illustrator](https://www.adobe.com/ca/products/illustrator.html) - 業界をリードするベクターグラフィックスソフトウェア（メンバーシッププランが必要）。
- [Inkscape](https://inkscape.org/) - Adobe Illustrator に類似した無料のデザインツール。

#### 公共交通の可視化を行うための一般的なGISアプリケーション {: #general-gis-applications-for-making-transit-visualizations}

 - [Felt](https://felt.com/) - 美しいデザインのモダンなGISソフトウェア。
 - [Google Mymaps](https://www.google.ca/maps/about/mymaps/) - Google My Mapsを使ってカスタムマップを作成・共有できます。
 - [Google Earth](https://www.google.com/earth/about/) - 世界で最も詳細な衛星アプリケーションの1つを使ってカスタムマップを作成・共有できます。

### 交通地図アグリゲーション {: #transit-map-aggregation}

 - [UrbanRail.Net](http://www.urbanrail.net/) - 世界中の都市鉄道（地下鉄、路面電車、通勤鉄道）の参照地図。詳細かつ最新の情報を提供。
 - [OpenRailwayMap](https://www.openrailwaymap.org/) - OpenStreetMap データを使用した世界中の鉄道地図。
 - [AllRailMap](https://www.allrailmap.com/) - OpenStreetMap データを使用したもう1つの世界鉄道地図。
 - [European Railway Atlas](https://europeanrailwayatlas.com/) - ヨーロッパ鉄道地図の参考書籍（購入可能）。
 - [Rail Transit Maps](http://www-personal.umich.edu/~yopopov/rrt/railroadmaps/) - ヨーロッパ（特にロシア）を中心とした鉄道地図のコレクション。
 - [Tramscale](https://alexander.co.tz/tramscale/) - 世界中の路面電車システムの規模を示す地図をまとめたウェブサイト。
 - [Timelines](https://alexander.co.tz/timelines/) - 世界中の高速交通プロジェクトのタイムラインを比較。
 - [Metrolinemap](https://www.metrolinemap.com/) - 世界の地下鉄システムのインタラクティブ地図。
 - [Metrocyclopaedia](https://blog.csaladen.es/metro/ ) - 世界中の地下鉄システムの3D地図（Metrolinemap のデータを使用）。
 - [RailFansCanada](https://map.railfans.ca/) - カナダの都市鉄道システムの現在と将来を示すインタラクティブシステムマップ。
 - [North American Transit](https://www.google.com/maps/d/u/0/viewer?mid=1GAXiiEp8a62LvZNDueYN76NPTCoUxvdx&ll=43.71257881237152%2C-79.385523993394&z=11) - 北米のすべての旅客鉄道（都市間鉄道、地下鉄、路面電車、観光路線を含む）の地図。
 - [Intercity Rail map](https://asm.transitdocs.com/) - Amtrak および Via 列車のリアルタイム位置と時刻表情報の地図。
 - [Indian Railways Map](https://indiarailinfo.com/atlas) - インド本線鉄道網のインタラクティブ地図。
 - [National Rail Network Map](https://www.arcgis.com/apps/mapviewer/index.html?webmap=96ec03e4fc8546bd8a864e39a2c3fc41) - 米国の鉄道路線の範囲と所有権を示す地図（旅客・貨物を含む）。
 - [Ferrocarta](https://ferrocarta.net/) - ブラジル、カナダ、フランスの旅客鉄道網をカバーする一連の地図。
 - [Train Lookout](https://trainlookout.com/) - 列車での旅程を簡単に記録、地図化、共有できるツール。
 - [Australian Rail Maps](http://www.railmaps.com.au/) - オーストラリアの鉄道地図（全国、州、市レベルの詳細）。
 - [Steam Engine "IS"](https://parovoz.com/maps/supermap/) - ソ連の鉄道地図。
 - [Carto.Metro](https://cartometro.com/) - 世界の都市（特にフランス）の地下鉄・路面電車ネットワークの詳細地図。
 - [Railway Stations](https://map.railway-stations.org/) - 世界中の鉄道駅の写真。
 - [INAT](https://www.inat.fr/maps/) - 世界中の地下鉄システムの美しい静的地図。
 - [Transit Maps](https://transitmap.net/) - 世界中の交通地図デザインの批評とレビュー。
 - [Transit Explorer](https://www.thetransportpolitic.com/transitexplorer/) - 世界中の固定軌道交通を含む地図。
 - [Britsh Railways](https://www.merrittcartographic.co.uk/british_railways.html) - グレートブリテンの鉄道網のインタラクティブ地図。
 - [TransitLand Map](https://www.transit.land/map) - GTFS フィードを持つ交通サービスの世界地図。
 - [DB InfraGO](https://geovdbn.deutschebahn.com/pgv/public/map/isr.xhtml) - ドイツ鉄道インフラのインタラクティブ地図。
 - [SNCF Carte interactive](https://www.sncf-reseau.com/fr/carte/carte-interactive-reseau-ferre-francais-0) - フランス鉄道インフラのインタラクティブ地図。
 - [Project Mapping](https://www.projectmapping.co.uk/index.html) - 英国および世界の鉄道網の模式図。
 - [China Railway Map](http://cnrail.geogv.org/enus/about) - 中国の旅客鉄道輸送システムの駅・路線情報を示すオンラインインタラクティブ地図。
 - [Canadian Rail Atlas](https://rac.jmaponline.net/canadianrailatlas/) - カナダの約43,000kmに及ぶ鉄道網のユーザーフレンドリーなインタラクティブ地図。
 - [The Rail Map](https://www.therailmap.com/) - OpenStreetMap データを使用した北米の鉄道路線のインタラクティブ地図。
 - [JR pass](https://www.jrpass.com/map#) - 日本の幹線鉄道のインタラクティブ地図。

## 事業者向けツール {: #agency-tools}

交通事業者向けのツールです。GTFS に特化したツールについては [GTFS Data Collection and Maintenance Tools](../producing-data/#gtfs-data-collection-and-maintenance-tools) も参照してください。

- [Remix](http://getremix.com/) - 交通事業者が簡単にルートを計画できるウェブアプリ。
- [Next Train API](https://github.com/data-creative/next-train-api) - 任意の GTFS フィードを JSON API として提供します。交通事業者や開発者は、オープンソースコードを自分の Heroku サーバーにデプロイすることができます。
- [AC Transit RestroomFinder](https://github.com/actransitorg/ACTransit.RestroomFinder) - バス運転手や現場スタッフ向けに、GPS と画面上の地図を用いて最寄りの認可済みトイレを特定します。
- [AC Transit Training and Education Department (TED) application](https://github.com/actransitorg/ACTransit.Training) - このアプリケーションは、交通および整備従業員の訓練業務を支援します。主にバス運転手や大型バス整備士（見習いおよび熟練工）の職種を対象としていますが、新しいコースや見習いプログラムにも対応しています。
- [AC Transit Customer Relations application (CusRel)](https://github.com/actransitorg/ACTransit.CusRel) - 顧客からの問題やフィードバックに対応する公共交通のチケッティングシステム。部門間ルーティングと通知、部門/担当者の割り当て、シンプルなワークフロー、チケット検索、定型レポート、日次リマインダーなどを備えています。
- [PTV Lines](https://www.ptvgroup.com/en/products/ptv-lines) - 路線計画および公共交通サービスの最適化のためのクラウドベースの公共交通ソフトウェア。
- [TransAM]([http://camsys.software/products/transam](https://github.com/camsys/transam_core)) - 公共交通事業者向けのオープンソース資産管理プラットフォーム。
- [RidePilot](https://github.com/camsys/ridepilot) - 小規模な福祉交通事業者のニーズに対応するための、オープンソースのコンピュータ支援配車・スケジューリング (CASD) ソフトウェアシステム。
- [TNExT](https://github.com/ODOT-PTS/TNExT) - Transit Network Explorer Tool (TNExT) は、オレゴン州の地域および州全体の交通ネットワークの可視化、分析、レポート作成のために開発されたウェブベースのソフトウェアツールです。
- Route Trends ([webapp](https://metrotransitmn.shinyapps.io/route-trends/), [GitHub](https://github.com/metrotransit/route-trends)) - 乗客数の時系列データを取り込み、[STL methodology](https://otexts.com/fpp2/stl.html) に基づいて季節性、トレンド、残差成分を返し、それらの成分に基づいた不確実性を含む予測を行う R Shiny アプリ。[Metro Transit](https://www.metrotransit.org/)（ミネアポリス・セントポール）がスポンサー。
- [TBEST](https://tbest.org/) - TBEST (Transit Boardings Estimation and Simulation Tool) は、多面的な GIS ベースのモデリング、計画、分析ツールの開発を目的とした取り組みです。社会経済、土地利用、交通ネットワークデータを統合し、シナリオベースの交通需要予測と分析を行うプラットフォームを提供します。フロリダ州運輸局が資金提供。無償で利用可能ですが、オープンソースではありません。
- [RideSheet](https://docs.ridesheet.org) – 小規模なデマンド型交通 (DRT) サービス向けのシンプルなスプレッドシートベースのツール。
