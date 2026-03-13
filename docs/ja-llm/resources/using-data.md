# データの使用 {: #using-data}

## 消費者向けアプリ {: #consumer-apps}

公共交通を利用する際に人々が使用するアプリです。

### Webアプリ（オープンソース） {: #web-apps-open-source}

- [Catenary Maps](https://catenarymaps.org) - RustとSvelteで書かれた、リアルタイムおよびスケジュール対応の世界的な公共交通マップおよびナビゲーションソフトウェアです。
- [Instabus](http://instabus.org) - オースティン（CapMetro）の公共交通のリアルタイムマップです。サーバーやバックエンドへの依存が一切なく、完全にGitHub Pages上で動作します。
- [OpenTripPlanner Client GWT](https://github.com/mecatran/OpenTripPlanner-client-gwt) - OpenTripPlanner用のGoogle Web ToolkitベースのWebインターフェースです。
- [OpenTripPlanner.js](https://github.com/conveyal/otp.js) - OpenTripPlanner用のJavaScriptベースのクライアントです（現在は開発終了）。
- [OTP-UI React Component Library](https://github.com/opentripplanner/otp-ui) - 便検索Webアプリを構築するために使用できるReact JavaScriptコンポーネントライブラリです。デモは[Storybook](http://www.opentripplanner.org/otp-ui)をご覧ください。
- [GTFS-realtime Alerts Producer Web Application](https://github.com/OneBusAway/onebusaway-service-alerts) - GTFS-realtimeの運行情報(alert)を生成するためのJavaベースのWebアプリケーションです。
- [HRT BUS Web app](https://github.com/Code4HR/hrt-bus-api) - HRT Bus APIは、Hampton Roads Transitのリアルタイムバスデータをアプリケーションプログラミングインターフェースを通じて公開し、開発者がそれを利用してアプリを作成できるようにします。
- [Transit-Map](https://github.com/vasile/transit-map) - 公共交通の時刻表を使用して、ルート（ポリライン）に沿って車両（マーカー）の位置を補間し、地図上でアニメーション表示するWebアプリです。
- [Transitive.js](https://github.com/conveyal/transitive.js) - LeafletまたはD3を使用して、交通ルートのカスタマイズ可能なWebマップレイヤーを作成します。
- [Google I/O Transport Tracker](https://github.com/googlemaps/transport-tracker) - Google I/Oカンファレンスのシャトル到着時刻を表示します。オープンソースの[transport-trackerプロジェクト](https://github.com/googlemaps/transport-tracker)に基づいています。  
  注: ご自身で実装する場合は、[Google Maps APIs Premium Planライセンス](https://developers.google.com/maps/pricing-and-plans/)が必要です。
- [1-Click](https://github.com/camsys/oneclick) - 公共交通、民間、鉄道、ライドシェア、カープール、ボランティア輸送、パラトランジット、徒歩および自転車など、さまざまな交通手段に関する情報を統合する仮想「旅程アグリゲーター」です。
- [Bustime](https://busti.me) - WebSocketによる更新を用いた公共交通のリアルタイム監視ツールです。オープンソース版は[GitHub](https://github.com/norn/bustime)で公開されています。
- [Transit Tracker](https://transittracker.ca/#/) - カナダのモントリオールおよびトロント大都市圏のリアルタイム車両位置情報を提供します。
- [GTFS Builder](http://nationalrtap.org/Web-Apps/GTFS-Builder) - GTFSファイルの作成を支援する無料のWebベースアプリケーションです。National Rural Transit Assistance Program（RTAP）によって管理されています。
- Dede - リアルタイムの移動をマッピングする独立した汎用旅客情報システム（PIS）です。GTFS-Realtime形式のVehicle Positionエンティティを含むメッセージフィード、または[Dedeアプリ](https://github.com/dancesWithCycles/dede-android)をデータソースとして使用することができます。
- [MBTA tile-server](https://github.com/mbta/tile-server) - MBTA.comで使用する地図タイルを開発するために必要なすべての要素を含むDockerコンテナを作成するスクリプトです。
- [Cadê Meu Busão](https://tarifazerobh.org/cade-meu-busao/) - ブラジル・ベロオリゾンテの公共交通バスをリアルタイムで追跡します。オープンソース版は[GitHub](https://github.com/tarifazero/monitoramento)で公開されています。
- [Tiramisu Transit](https://github.com/CMU-RERC-APT/tiramisu3-pr) - カーネギーメロン大学によって開発・運用された、リアルタイムのバス到着情報を表示する適応型モバイル交通アプリです。現在はメンテナンスされていません。

### Webアプリ（クローズドソース） {: #web-apps-closed-source}

- [TransitScreen](http://transitscreen.com/) - すべての地域交通手段をリアルタイムでカスタマイズ表示します。
- [Citylines.co](https://www.citylines.co) - 交通システムを共同でマッピングするためのプラットフォームで、その歴史的な発展に重点を置いています。
- [Bikeshare Map](http://bikes.oobrien.com/) - 世界中の自転車シェアステーションのステータスを表示します。
- [Bongo](http://ebongo.org) - アイオワシティ、コーラルビル、アイオワ大学向けのリアルタイム交通追跡。3つの異なる交通システムを1つのUIに統合しています。
- [CityMapper Webapp](https://citymapper.com/nyc) - 30以上の都市に対応した、旅程計画およびルート状況を提供する非常に洗練されたWebアプリです。
- [TransSee](https://www.transsee.ca/) - 実際の走行時間、車両位置、時刻表、地図に基づくリアルタイム交通予測。プレミアム版では、時刻表、車両位置、停留所到着、時刻表遵守、チャートやグラフの詳細な履歴にアクセスできます。追加料金で、このデータに対してカスタムクエリを実行することもできます。
- [YourStop](http://yourstop.info) - GTFSフィードを利用し、停留所のライブおよび予定便を表示するモバイル対応Webアプリ。MBTA、YRT/Viva、メリーランドMTAで提供開始。
- [DC MetroHero](https://dcmetrohero.net) - ワシントンD.C.地域のWMATAメトロレールおよびメトロバスシステムのリアルタイム車両位置、到着・出発情報を提供。Webアプリ、Android、iOSアプリが利用可能です。

### ネイティブアプリ（オープンソース） {: #native-apps-open-source}


- [KDE Itinerary](https://apps.kde.org/itinerary/) - 旅程を計画するためのアプリ（デスクトップおよび Android）。公共交通のルート検索、オフラインでの保存、旅程へのイベント追加、駅構内のフロアマップ表示などが可能です。[ソースコード](https://invent.kde.org/pim/itinerary)、[GitHub](https://github.com/KDE/itinerary)
- [MACS Transit Android App](https://github.com/yeSpud/MACSTransitApp) - アラスカ州フェアバンクスの MACS Transit システム向けの Android 用バストラッカーアプリ。RouteMatch API を使用しています。
- [Next Train - Connecticut](https://github.com/data-creative/NextTrainCT) - コネチカット州の Shore Line East 交通事業者が公開している列車時刻表を検索するための React Native モバイルアプリ。[Next Train API](https://github.com/data-creative/next-train-api) のデプロイに依存しています。
- [Offi Directions](https://gitlab.com/oeffi/oeffi) - ヨーロッパおよびその他地域の交通当局向けに、旅程計画、時刻表、リアルタイム出発時刻、運行障害情報を提供する Android アプリです。
- OneBusAway アプリ - [Android](https://play.google.com/store/apps/details?id=com.joulespersecond.seattlebusbot) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-android)、[Fire Phone](http://www.amazon.com/gp/mas/dl/android?p=com.joulespersecond.seattlebusbot) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-android)、[iOS](https://itunes.apple.com/us/app/onebusaway/id329380089) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-ios)、[Windows Phone](https://www.microsoft.com/en-us/store/apps/onebusaway/9nblggh0cbd9) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-windows-phone)、[Google Glass GDK](https://github.com/OneBusAway/onebusaway-android/pull/219) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-android/pull/219)、[Alexa スキル](https://www.amazon.com/OneBusAway/dp/B01ELVUYCW/) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-alexa)
- [OpenTripPlanner Android](https://github.com/CUTR-at-USF/OpenTripPlanner-for-Android/wiki) - [OpenTripPlanner](http://www.opentripplanner.org/) 用の Android アプリ。
- [OpenTripPlanner iOS](https://github.com/opentripplanner/OpenTripPlanner-iOS) - [OpenTripPlanner](http://www.opentripplanner.org/) 用の iOS アプリ。
- [opentripplanner-client-library](https://github.com/CUTR-at-USF/opentripplanner-client-library) - Android、iOS、Web 向けに、OpenTripPlanner v2 サーバーから旅程計画、自転車レンタル情報、サーバーメタデータの API リクエストおよびレスポンス解析を行う Kotlin マルチプラットフォームライブラリ。
- [Transito](http://git.sr.ht/~mil/transito) - 公開されている GTFS フィード（[Mobility Database](https://database.mobilitydata.org/) 由来）を利用して、地点間のルート検索を行う FOSS（自由・オープンソースソフトウェア）公共交通アプリ。 [Mobroute Go API](http://sr.ht/~mil/mobroute) を利用し、ルート計算をスマートフォン上で実行します。現在、Android および Linux をサポートするクロスプラットフォームアプリです。
- [Tiramisu Transit](https://github.com/CMU-RERC-APT/tiramisu3-pr#mobile-app-client) - カーネギーメロン大学が開発・運用した、リアルタイムのバス到着情報を表示する適応型モバイル交通アプリ。Ionic フレームワークで作成されています。現在はメンテナンスされていません。
- [Transportr](https://github.com/grote/Transportr) - [public-transport-enabler](https://github.com/schildbach/public-transport-enabler) を使用して、世界中のさまざまな交通ネットワークに接続する Android アプリ。
- [Trufi App](https://github.com/trufi-association/trufi-app) - [OpenTripPlanner](http://www.opentripplanner.org/) を使用するクロスプラットフォーム Flutter アプリ。

### ネイティブアプリ（クローズドソース） {: #native-apps-closed-source}


- [Transit](http://transitapp.com/)
- [CityMapper](https://citymapper.com/)
- [Moovit](http://moovitapp.com/)
- [Transit Display](http://transitdisplay.com/) - 複数の交通モードおよびリアルタイムの交通情報を表示するソフトウェアです。
- [Ualabee](https://ualabee.com/company/) - コミュニティ主導の経路検索アプリで、ユーザーの相互作用に重点を置いています。ユーザーは異常の報告、写真のアップロード、交通データの編集、他の乗客とのチャットができます。
- [ÖPNV Navigator](https://navigatorapp.net/)
- [TripGo](https://tripgo.com/)

## ハードウェア {: #hardware}


実験用および本番用の交通機関ハードウェアです。

- [Bus Tracking GPS](https://github.com/herrdragon/busTrackingGps) - トランジットバスを追跡するための、安価でオープンソースなソリューションの Miami プロトタイプ用コードです。
- [Train departure Display](https://github.com/chrisys/train-departure-display) - Raspberry Pi Zero を基盤とした、英国鉄道駅の列車出発案内表示を模した、ほぼリアルタイムのミニチュア表示装置です。

## SDKs {: #sdks}

- [TripKit](https://github.com/alexander-albers/tripkit) - TripKit は、公共交通機関の事業者からデータを取得するための Swift ライブラリです。
- [KPublicTransport](https://invent.kde.org/libraries/kpublictransport) - リアルタイムの公共交通データへのアクセスおよび公共交通の旅程(journey)検索を行うための C++ ライブラリです。
- [SkedGo の TripKit SDKs](https://developer.tripgo.com) - [SkedGo](https://skedgo.com) の TripGo API にアクセスするための、Android、iOS、React 向けのオープンソース SDK であり、旅程(journey)計画用の UI コンポーネントを含みます。

## 可視化 {: #visualizations}

### GTFS ベースの可視化 {: #gtfs-based-visualizations}


- [All Transit](https://all-transit.com) - Mapbox GL JS、Deck.gl、Transitland を使用して、（米国の都市向けに）GTFS のルートおよび時刻表をインタラクティブにアニメーション表示します。GitHub リポジトリは[こちら](https://github.com/kylebarron/all-transit)です。
- [BusGraphs Access Analyzer](https://gitlab.com/publictransitanalytics-pub/readme) - 実際および仮想の固定ルート型公共交通ネットワークによって提供されるアクセスを測定し、さまざまな方法でこのアクセスを可視化・分解するための Web アプリケーションです。
- [fastest-bus-analysis-in-the-west](https://github.com/vta/fastest-bus-analysis-in-the-west) - Ridership/APC、Swiftly の速度および停車データ、バス停の在庫、GTFS、地理空間の shape を組み合わせて、停留所ごと、ルートごと、時間帯ごとにフィルタリング可能なデータセットを作成する Python Pandas スクリプトです。このデータセットは [Tableau](https://public.tableau.com/profile/vivek7797#!/vizhome/stopsandspeedanalyses/Story1) で可視化され、VTA のプランナーが停留所の統合や専用レーンなどの速度向上手法を通じて、バスおよび鉄道ネットワークをより高速かつ信頼性の高いものにするための場所を特定するのに役立ちます。
- [gtfspy-webviz](https://github.com/CxAalto/gtfspy-webviz) - [gtfspy](https://github.com/CxAalto/gtfspy) を使用して GTFS データをアニメーションおよび可視化する Web アプリケーションです。
- [gtfs-to-geojson](https://www.transit.chat/gtfs-to-geojson) - GTFS を GeoJSON に変換するためのシンプルなオンラインコンバーターで、フィードの一覧も提供します。
- [gtfs-visualizations](https://github.com/cmichi/gtfs-visualizations) - GTFS データセットのルートを可視化するためのオープンソースの NodeJS アプリケーションです。
- [Mapnificent](https://www.mapnificent.net/) - 指定した時間内に公共交通機関で到達可能な範囲を表示します。オープンソース版は [GitHub](https://github.com/mapnificent/mapnificent) にあり、実際のサイトは https://www.mapnificent.net/ です。
- [MIT COAXS](http://mittransportanalyst.github.io/) - アクセシビリティに基づくステークホルダー参加を用いた公共交通回廊の共創的計画ツールです（[OpenTripPlanner Analyst](http://www.opentripplanner.org/analyst/) を使用してルートシナリオを表示します）。
- [MOTIS](https://motis-project.de/) - [可視化機能](https://europe.motis-project.de/) を含む、モビリティ情報の統合システムです。
- [MTA Frequency](http://www.tyleragreen.com/maps/new_york/) - [Transitland](https://transit.land/) を使用して構築された、ニューヨーク市の地下鉄およびバスの運行頻度を可視化するツールです。
- [SEPTA Rail OTP Report](https://apps.phor.net/septa/) - GTFS を使用したオンラインの定時運行パフォーマンス報告およびドリルダウンツールです。
- [Simple Transit Map](https://github.com/ioTransit/simple-transit-map) - Web マップをホストおよび更新する方法を示すオンラインの例です。
- [Simple Transit Site](https://transit.chat/simple-transit-site) - GTFS からすべて生成された交通ウェブサイトを作成する方法を示すオンラインの例です（[GitHub](https://github.com/ioTransit/simple-transit-site) にもあります）。
- [TNExT](https://github.com/ODOT-PTS/TNExT) - Transit Network Explorer Tool (TNExT) は、オレゴン州の地域および州全体の交通ネットワークを可視化、分析、報告するために開発された Web ベースのソフトウェアツールです。
- [Toronto Transit Explorer](https://github.com/sidewalklabs/totx) - トロント市全体の公共交通、自転車、徒歩のアクセシビリティを可視化する Java アプリケーションです。ルーティングには修正版の [R5](https://github.com/conveyal/r5) を使用しています。
- [Transit Vis](https://github.com/zackAemmer/transit_vis) - King County Metro の GTFS-RT フィード（OneBusAway API）から得られたパフォーマンス指標を表示する可視化ツールです。[こちら](https://www.transitvis.com/) で閲覧可能です。[この論文](https://link.springer.com/article/10.1007/s12469-022-00291-7) で使用されています。
- [TransitFlow](https://github.com/transitland/transitland-processing-animation) - Processing および Transitland を使用して、世界中の GTFS データをアニメーション化します。
- [TRAVIC Transit Visualization Client](http://tracker.geops.ch/) - 静的 GTFS データ（および場合によってはリアルタイムデータ）に基づいて車両の動きを可視化します。260 以上の都市をサポートしています。geOps 組織の GitHub アカウントは[こちら](https://github.com/geops)です。
- [Traze](https://traze.app/)（[Veridict](https://www.veridict.com) による） - 世界中の公共交通車両を可視化します。事業者からリアルタイムデータが提供されていない場合でも、他のユーザーと協力してリアルタイム更新を取得できます。GTFS および GTFS-RT を含む複数の情報源に基づいています。（以前は Livemap24 として知られていました。）
- [Visualizing MBTA Data](http://mbtaviz.github.io/) - ボストンの地下鉄システムの利用状況を示すインタラクティブなグラフです。
- [GTFS Viz 🚉](https://github.com/gabrielAHN/gtfs-viz) - [duckdb-wasm 🦆](https://duckdb.org/docs/api/wasm/overview.html) を使用し、バックエンドを持たずにクライアント側で大規模な GTFS データをブラウザ上で可視化する Web アプリです。

### 交通マップ作成 {: #transit-map-creation}


- [Brand New Subway](https://jpwright.github.io/subway/) - プレイヤーがニューヨーク市地下鉄システムを自由に改変できる、インタラクティブな交通計画ゲームです。
- [BENO Metro Map Creator](https://beno.uk/metromapcreator/#) - 非常にクラシックで昔ながらの交通マップ作成ツールです。
- [Tennessine Metro Designer](https://tennessine.co.uk/metro/) - モダンで美しいデザインの交通マップ作成ツールです。
- [loom](https://github.com/ad-freiburg/loom) - 地理的に正確または概略的な交通マップを自動生成するためのソフトウェアスイートです。
- [Metro Map Maker](https://metromapmaker.com/)   - オープンソースでシンプルな地下鉄マップ作成ソフトウェアです。
- [MetroDreamin'](https://metrodreamin.com/explore) - ユーザーがインタラクティブな交通マップを作成、保存、「いいね」、共有できるモダンなオープンソースソフトウェアです。
- [Rail Map Generators](https://wongchito.github.io/RailMapGenerator) - 各都市の公共交通システムのスタイルで鉄道マップや情報パネルを生成するツールです。
- [MetroSets](https://metrosets.ac.tuwien.ac.at/) - メトロマップのメタファーを用いて集合システムを可視化する柔軟なウェブツールです。この[論文](https://www.computer.org/csdl/journal/tg/2021/02/09224192/1nV7Me0F3Lq)に基づいています。

#### 交通可視化を作成するための一般的な描画アプリケーション {: #general-drawing-applications-for-making-transit-visualizations}

- [Adobe illustrator](https://www.adobe.com/ca/products/illustrator.html) - 業界をリードするベクターグラフィックソフトウェア（メンバーシッププランが必要です）。
- [Inkscape](https://inkscape.org/) - Adobe Illustrator に似た無料のデザインツールです。

#### 交通可視化を作成するための一般的な GIS アプリケーション {: #general-gis-applications-for-making-transit-visualizations}

 - [Felt](https://felt.com/) - 美しいデザインのモダンな GIS ソフトウェアです。
 - [Google Mymaps](https://www.google.ca/maps/about/mymaps/) - Google My Maps を使用してカスタムマップを作成・共有できます。
 - [Google Earth](https://www.google.com/earth/about/) - 世界で最も詳細な衛星アプリケーションの1つを使用して、カスタムマップを作成・共有できます。

### 交通地図アグリゲーション {: #transit-map-aggregation}

 - [UrbanRail.Net](http://www.urbanrail.net/) - 世界中の都市鉄道交通（地下鉄、路面電車、通勤鉄道）の詳細かつ最新の情報を提供するリファレンスマップです。
 - [OpenRailwayMap](https://www.openrailwaymap.org/) - OpenStreetMapデータを使用した世界中の鉄道地図です。
 - [AllRailMap](https://www.allrailmap.com/) - OpenStreetMapデータを使用した、もう1つの世界的な鉄道地図です。
 - [European Railway Atlas](https://europeanrailwayatlas.com/) - ヨーロッパの鉄道地図を収録した参考書で、購入可能です。
 - [Rail Transit Maps](http://www-personal.umich.edu/~yopopov/rrt/railroadmaps/) - ヨーロッパ（特にロシア）を中心とした鉄道地図のコレクションです。
 - [Tramscale](https://alexander.co.tz/tramscale/) - 世界中の路面電車システムのスケールを示す地図を掲載したウェブサイトです。
 - [Timelines](https://alexander.co.tz/timelines/) - 世界中の高速交通プロジェクトのタイムラインを比較できます。
 - [Metrolinemap](https://www.metrolinemap.com/) - 世界の地下鉄システムのインタラクティブマップです。
 - [Metrocyclopaedia](https://blog.csaladen.es/metro/ ) - 世界中の地下鉄システムの3Dマップ（Metrolinemapのデータを使用）です。
 - [RailFansCanada](https://map.railfans.ca/) - カナダのさまざまな都市鉄道システムの現在および将来を詳細に示すインタラクティブシステムマップです。
 - [North American Transit](https://www.google.com/maps/d/u/0/viewer?mid=1GAXiiEp8a62LvZNDueYN76NPTCoUxvdx&ll=43.71257881237152%2C-79.385523993394&z=11) - 北米のすべての旅客鉄道（都市間鉄道、地下鉄、路面電車、観光線を含む）の地図です。
 - [Intercity Rail map](https://asm.transitdocs.com/) - AmtrakおよびVia列車のリアルタイム位置と時刻表情報を表示する地図です。
 - [Indian Railways Map](https://indiarailinfo.com/atlas) - インドの主要鉄道ネットワークのインタラクティブマップです。
 - [National Rail Network Map](https://www.arcgis.com/apps/mapviewer/index.html?webmap=96ec03e4fc8546bd8a864e39a2c3fc41) - 米国の鉄道路線の範囲と所有権（旅客および貨物線を含む）を示す地図です。
 - [Ferrocarta](https://ferrocarta.net/) - ブラジル、カナダ、フランスのすべての旅客鉄道ネットワークをカバーする一連の地図です。
 - [Train Lookout](https://trainlookout.com/) - 列車での旅を簡単に記録、地図化、共有できるツールです。
 - [Australian Rail Maps](http://www.railmaps.com.au/) - 国、州、都市レベルの詳細なオーストラリア鉄道地図です。
 - [Steam Engine "IS"](https://parovoz.com/maps/supermap/) - ソビエト連邦の鉄道地図です。
 - [Carto.Metro](https://cartometro.com/) - 世界の都市（特にフランス）の地下鉄および路面電車ネットワークの詳細地図です。
 - [Railway Stations](https://map.railway-stations.org/) - 世界中の鉄道駅の写真を掲載しています。
 - [INAT](https://www.inat.fr/maps/) - 世界中の地下鉄システムの美しい静的地図です。
 - [Transit Maps](https://transitmap.net/) - 世界中の交通地図デザインに関する批評とレビューです。
 - [Transit Explorer](https://www.thetransportpolitic.com/transitexplorer/) - 世界中の固定軌道交通を含む地図です。
- [Britsh Railways](https://www.merrittcartographic.co.uk/british_railways.html) - グレートブリテンの鉄道ネットワークのインタラクティブマップです。
- [TransitLand Map](https://www.transit.land/map)  - GTFSフィードを持つ交通サービスの世界地図です。
 - [DB InfraGO](https://geovdbn.deutschebahn.com/pgv/public/map/isr.xhtml)  - ドイツ鉄道インフラのインタラクティブマップです。
 - [SNCF Carte interactive](https://www.sncf-reseau.com/fr/carte/carte-interactive-reseau-ferre-francais-0) - フランス鉄道インフラのインタラクティブマップです。
 - [Project Mapping](https://www.projectmapping.co.uk/index.html) - 英国および世界の鉄道ネットワークの模式図です。
 - [China Railway Map](http://cnrail.geogv.org/enus/about) - 中国の旅客鉄道輸送システムの駅および路線情報を表示するオンラインインタラクティブマップです。
 - [Canadian Rail Atlas](https://rac.jmaponline.net/canadianrailatlas/) - カナダの約43,000キロメートルに及ぶ鉄道ネットワークを示す、使いやすいインタラクティブマップです。
 - [The Rail Map](https://www.therailmap.com/) - OpenStreetMapデータを使用した北米の鉄道路線のインタラクティブマップです。
 - [JR pass](https://www.jrpass.com/map#) - 日本の幹線鉄道のインタラクティブマップです。

## 事業者向けツール {: #agency-tools}

交通事業者向けのツールです。GTFS に特化したツールについては、[GTFS データ収集および保守ツール](../producing-data/#gtfs-data-collection-and-maintenance-tools)も参照してください。

- [Remix](http://getremix.com/) - 交通事業者が簡単にルートを計画できるウェブアプリです。
- [Next Train API](https://github.com/data-creative/next-train-api) - 任意の GTFS フィードを JSON API として提供します。交通事業者および開発者は、オープンソースコードを自分の Heroku サーバーにデプロイすることができます。
- [AC Transit RestroomFinder](https://github.com/actransitorg/ACTransit.RestroomFinder) - バス運転手および現場スタッフ向けに、GPS と画面上の地図を使用して最寄りの認可トイレを特定します。
- [AC Transit Training and Education Department (TED) application](https://github.com/actransitorg/ACTransit.Training) - このアプリケーションは、交通および整備職員（主にバス運転手および大型バス整備士（見習いおよび熟練工））の訓練業務を支援します。新しいコースや見習いプログラムにも対応しています。
- [AC Transit Customer Relations application (CusRel)](https://github.com/actransitorg/ACTransit.CusRel) - 顧客からの問題やフィードバックを処理する公共交通向けチケットシステムです。部門間ルーティングと通知、部門・担当者の割り当て、シンプルなワークフロー、チケット検索、定型レポート、日次リマインダーなどの機能を備えています。
- [PTV Lines](https://www.ptvgroup.com/en/products/ptv-lines) - 路線計画および公共交通サービスの最適化のためのクラウドベースの公共交通ソフトウェアです。
- [TransAM](https://github.com/camsys/transam_core) - 公共交通事業者向けのオープンソース資産管理プラットフォームです。
- [RidePilot](https://github.com/camsys/ridepilot) - 小規模な福祉交通事業者のニーズに対応する、オープンソースのコンピュータ支援配車・スケジューリング（CASD）システムです。
- [TNExT](https://github.com/ODOT-PTS/TNExT) - Transit Network Explorer Tool (TNExT) は、オレゴン州の地域および州全体の交通ネットワークの可視化、分析、レポート作成のために開発されたウェブベースのソフトウェアツールです。
- Route Trends ([webapp](https://metrotransitmn.shinyapps.io/route-trends/)、[GitHub](https://github.com/metrotransit/route-trends)) - 乗車数の時系列データを取り込み、[STL 手法](https://otexts.com/fpp2/stl.html)に基づいて季節成分、トレンド成分、残差成分を算出し、それらに基づく不確実性を含む予測を返す R Shiny アプリです。[Metro Transit](https://www.metrotransit.org/)（ミネアポリス・セントポール）がスポンサーです。
- [TBEST](https://tbest.org/) - TBEST（Transit Boardings Estimation and Simulation Tool）は、社会経済データ、土地利用データ、交通ネットワークデータを統合し、シナリオベースの交通需要推計および分析を行う多面的な GIS ベースのモデリング・計画・分析ツールを開発する取り組みです。フロリダ州運輸局が資金提供しています。無料で利用できますが、オープンソースではありません。
- [RideSheet](https://docs.ridesheet.org) – 小規模なデマンド型交通（DRT）サービス向けの、シンプルなスプレッドシートベースのツールです。
