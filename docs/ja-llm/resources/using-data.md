# データの利用 {: #using-data}

## 利用者向けアプリ {: #consumer-apps}


人々が公共交通機関を利用する際に使用するアプリです。

### Web Apps（オープンソース） {: #web-apps-open-source}

- [Catenary Maps](https://catenarymaps.org) - Rust と Svelte で記述された、Realtime および Schedule の世界規模の公共交通機関マップ・ナビゲーションソフトウェアです。
- [Instabus](http://instabus.org) - オースティン（CapMetro）の公共交通機関の Realtime マップです。サーバー／バックエンドへの依存関係が一切なく、完全に GitHub pages 上で動作します。
- [OpenTripPlanner Client GWT](https://github.com/mecatran/OpenTripPlanner-client-gwt) - OpenTripPlanner 用の Google Web Toolkit ベースの Web インターフェースです。
- [OpenTripPlanner.js](https://github.com/conveyal/otp.js) - OpenTripPlanner 用の Javascript ベースのクライアントです（開発は終了しています）。
- [OTP-UI React Component Library](https://github.com/opentripplanner/otp-ui) - 旅程プランナー Web アプリを構築するために使用できる React Javascript コンポーネントライブラリです。デモについては、[Storybook](http://www.opentripplanner.org/otp-ui) を参照してください。
- [GTFS-realtime Alerts Producer Web Application](https://github.com/OneBusAway/onebusaway-service-alerts) - GTFS-realtime Service Alerts を生成するための Java ベースの Web アプリケーションです。
- [HRT BUS Web app](https://github.com/Code4HR/hrt-bus-api) - HRT Bus API は、開発者がアプリを作成できるよう、Hampton Roads Transit のリアルタイムバスデータを application programming interface を通じて公開します。
- [Transit-Map](https://github.com/vasile/transit-map) - 公共交通機関の時刻表を使用してルート（ポリライン）上の位置を補間し、地図上で車両（マーカー）をアニメーション表示する Web アプリです。
- [Transitive.js](https://github.com/conveyal/transitive.js) - Leaflet または D3 を使用して、カスタマイズ可能な公共交通機関ルートの Web マップレイヤーを作成します。
- [Google I/O Transport Tracker](https://github.com/googlemaps/transport-tracker) - オープンソースの [transport-tracker project](https://github.com/googlemaps/transport-tracker) に基づき、Google I/O カンファレンスのシャトル到着時刻を表示します。注記: これを自分で実装するには、[Google Maps APIs Premium Plan license](https://developers.google.com/maps/pricing-and-plans/) が必要です。
- [1-Click](https://github.com/camsys/oneclick) - 公共交通機関、私有交通、鉄道、ライドシェア、カープール、ボランティア、パラトランジット、徒歩および自転車など、利用可能な多種多様な交通手段に関する情報を集約する仮想的な「旅程アグリゲーター」です。
- [Bustime](https://busti.me) - WebSocket 更新による公共交通機関のリアルタイム監視です。[GitHub](https://github.com/norn/bustime) でオープンソースとして公開されています。
- [Transit Tracker](https://transittracker.ca/#/) - カナダの Greater Montreal および Toronto における Realtime 車両位置情報です。
- [GTFS Builder](http://nationalrtap.org/Web-Apps/GTFS-Builder) - GTFS ファイルの作成を支援する無料の Web ベースアプリケーションです。National Rural Transit Assistance Program（RTAP）によって保守されています。
- Dede - Realtime の移動を地図表示する、独立した汎用的な乗客情報システム（PIS）です。GTFS-Realtime 形式の Vehicle Position エンティティを含むメッセージフィード、または [Dede app](https://github.com/dancesWithCycles/dede-android) をデータソースとして使用できます。
- [MBTA tile-server](https://github.com/mbta/tile-server) - MBTA.com で使用する地図タイルを開発するために必要なすべての要素を包含する Docker コンテナを作成するスクリプトです。
- [Cadê Meu Busão](https://tarifazerobh.org/cade-meu-busao/) - ブラジルの Belo Horizonte における公共交通バスの Realtime 追跡です。[GitHub](https://github.com/tarifazero/monitoramento) でオープンソースとして公開されています。
- [Tiramisu Transit](https://github.com/CMU-RERC-APT/tiramisu3-pr) - Carnegie Mellon University により開発・展開された、リアルタイムのバス到着情報を表示する適応型モバイル公共交通アプリです。現在は保守されていません。

### Web Apps（クローズドソース） {: #web-apps-closed-source}

- [TransitScreen](http://transitscreen.com/) - 地域内のすべての交通手段を対象としたカスタムリアルタイム表示
- [Citylines.co](https://www.citylines.co) - 交通システムの歴史的な変遷に重点を置いてマッピングするための、共同作業プラットフォームです。
- [Bikeshare Map](http://bikes.oobrien.com/) - 世界中のすべてのシェアサイクルステーションの状況
- [Bongo](http://ebongo.org) - Iowa City、Coralville、およびUniversity of Iowa向けのリアルタイム交通追跡です。3つの異なる交通システムを1つのUIに統合しています。
- [CityMapper Webapp](https://citymapper.com/nyc) - 30以上の都市向けに、非常に洗練された旅程プランナーおよびルート状況を備えたWebアプリです。
- [TransSee](https://www.transsee.ca/) - 実際の移動時間、車両位置、時刻表、および地図に基づくリアルタイム交通予測です。Premiumでは、時刻表、車両位置、停留所等(stop)への到着、時刻表遵守、チャート、およびグラフの詳細な履歴にアクセスできます。追加料金により、このデータに対してカスタムクエリを実行できます。
- [YourStop](http://yourstop.info) - GTFSフィードを利用し、停留所等(stop)についてライブおよび予定された便(trip)の両方を表示する、モバイル対応Webアプリです。MBTA、YRT/Viva、およびMaryland MTAで開始されました。
- [DC MetroHero](https://dcmetrohero.net) - Washington, D.C.地域のWMATA MetrorailおよびMetrobusシステム向けの、リアルタイム車両位置、到着、および出発情報です。WebApp、Android、およびiOSアプリを利用できます。

### ネイティブアプリ（オープンソース） {: #native-apps-open-source}


- [KDE Itinerary](https://apps.kde.org/itinerary/) - 旅程を計画するためのアプリ（デスクトップおよびAndroid）です。公共交通機関のルートを検索し、オフラインで保存し、旅程にイベントを追加し、鉄道駅のフロアプランを確認するなど、さまざまなことができます。[ソースコード](https://invent.kde.org/pim/itinerary)、[GitHub](https://github.com/KDE/itinerary)
- [MACS Transit Android App](https://github.com/yeSpud/MACSTransitApp) - アラスカ州フェアバンクスのMACS Transitシステム向けの、Androidデバイス用バス追跡アプリです。RouteMatch APIsを使用します。
- [Next Train - Connecticut](https://github.com/data-creative/NextTrainCT) - コネチカット州のShore Line East交通事業者が公開する列車時刻表を検索するためのReact-nativeモバイルアプリです。[Next Train API](https://github.com/data-creative/next-train-api)のデプロイメントに依存しています。
- [Offi Directions](https://gitlab.com/oeffi/oeffi) - 欧州およびその他の地域の交通事業者向けに、旅程計画、時刻表、リアルタイム出発時刻、運行障害情報を提供するAndroidアプリです。
- OneBusAwayアプリ - [Android](https://play.google.com/store/apps/details?id=com.joulespersecond.seattlebusbot) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-android)、[Fire Phone](http://www.amazon.com/gp/mas/dl/android?p=com.joulespersecond.seattlebusbot) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-android)、[iOS](https://itunes.apple.com/us/app/onebusaway/id329380089)  [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-ios)、[Windows Phone](https://www.microsoft.com/en-us/store/apps/onebusaway/9nblggh0cbd9) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-windows-phone)、[Google Glass GDK](https://github.com/OneBusAway/onebusaway-android/pull/219) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-android/pull/219)、[Alexa skill](https://www.amazon.com/OneBusAway/dp/B01ELVUYCW/) [*(ソースコード)*](https://github.com/OneBusAway/onebusaway-alexa)
- [OpenTripPlanner Android](https://github.com/CUTR-at-USF/OpenTripPlanner-for-Android/wiki) - [OpenTripPlanner](http://www.opentripplanner.org/)向けのAndroidアプリです。
- [OpenTripPlanner iOS](https://github.com/opentripplanner/OpenTripPlanner-iOS) - [OpenTripPlanner](http://www.opentripplanner.org/)向けのiOSアプリです。
- [opentripplanner-client-library](https://github.com/CUTR-at-USF/opentripplanner-client-library) - Android、iOS、Web向けに、旅程計画、自転車レンタル情報、サーバーメタデータについてOpenTripPlanner v2サーバーへのAPIリクエストを行い、レスポンスを解析するためのKotlin Multiplatformライブラリです。
- [Transito](http://git.sr.ht/~mil/transito) - 公開されている公共GTFSフィード（[Mobility Database](https://database.mobilitydata.org/)から取得）を使用して、地点間の経路検索を行える、FOSSのデータプロバイダー非依存型公共交通アプリです。[Mobroute Go API](http://sr.ht/~mil/mobroute)を利用することで、Transitoアプリではスマートフォン上で直接経路計算を実行できます。現在AndroidおよびLinuxをサポートするクロスプラットフォームアプリです。
- [Tiramisu Transit](https://github.com/CMU-RERC-APT/tiramisu3-pr#mobile-app-client) - Carnegie Mellon Universityが開発・導入した、リアルタイムのバス到着情報を表示する適応型モバイル交通アプリです。Ionic frameworkを使用して記述されています。現在は保守されていません。
- [Transportr](https://github.com/grote/Transportr) - 世界中の多様な交通ネットワークに接続するために、[public-transport-enabler](https://github.com/schildbach/public-transport-enabler)を使用するAndroidアプリです。
- [Trufi App](https://github.com/trufi-association/trufi-app) - [OpenTripPlanner](http://www.opentripplanner.org/)を使用するクロスプラットフォームFlutterアプリです。

### ネイティブアプリ（クローズドソース） {: #native-apps-closed-source}


- [Transit](http://transitapp.com/)
- [CityMapper](https://citymapper.com/)
- [Moovit](http://moovitapp.com/)
- [Transit Display](http://transitdisplay.com/) - 複数の交通モードに対応したリアルタイム交通表示ソフトウェアです。
- [Ualabee](https://ualabee.com/company/) - ユーザーの相互作用に重点を置いたコミュニティ主導の旅程プランナーであり、ユーザーは異常を報告し、写真をアップロードし、交通データを編集し、他の乗客とチャットできます。
- [ÖPNV Navigator](https://navigatorapp.net/)
- [TripGo](https://tripgo.com/)

## ハードウェア {: #hardware}


実験的および本番運用の公共交通ハードウェア。

- [Bus Tracking GPS](https://github.com/herrdragon/busTrackingGps) - 公共交通バスを追跡するための安価なオープンソースソリューションの、マイアミ向けプロトタイプのコードです。
- [Train departure Display](https://github.com/chrisys/train-departure-display) - Raspberry Pi Zer0をベースとした、ほぼリアルタイムの英国鉄道駅の列車出発案内表示を再現したミニチュアです。

## SDK {: #sdks}

- [TripKit](https://github.com/alexander-albers/tripkit) - TripKit は、公共交通事業者からデータを取得するための Swift ライブラリです。
- [KPublicTransport](https://invent.kde.org/libraries/kpublictransport) - リアルタイムの公共交通データへのアクセスおよび公共交通の旅程(journey)クエリの実行のための C++ ライブラリです。
- [SkedGo's TripKit SDKs](https://developer.tripgo.com) - 旅程計画 UI コンポーネントを含む、[SkedGo](https://skedgo.com) の TripGo API にアクセスするための Android、iOS、React 向けオープンソース SDK です。

## 可視化 {: #visualizations}

### GTFS ベースの可視化 {: #gtfs-based-visualizations}


- [All Transit](https://all-transit.com) - Mapbox GL JS、Deck.gl、Transitland を使用した、インタラクティブな GTFS のルート・路線系統(route)およびスケジュールのアニメーション（米国の都市向け）。Github リポジトリは[こちら](https://github.com/kylebarron/all-transit)です。
- [BusGraphs Access Analyzer](https://gitlab.com/publictransitanalytics-pub/readme) - 実在および仮想の固定ルート公共交通ネットワークによって提供されるアクセスを測定し、このアクセスをさまざまな方法で可視化および分解するための Web アプリケーションです。
- [fastest-bus-analysis-in-the-west](https://github.com/vta/fastest-bus-analysis-in-the-west) - Ridership/APC、Swiftly の速度および停車時間データ、バス停留所等(stop)インベントリ、GTFS、地理空間 shape を組み合わせ、クロス分析用の、停留所等(stop)ごと、ルート・路線系統(route)ごと、時間グループごとにフィルタリング可能なデータセットを作成する Python Pandas スクリプトです。このデータセットは、[Tableau](https://public.tableau.com/profile/vivek7797#!/vizhome/stopsandspeedanalyses/Story1) で可視化され、VTA のプランナーが、停留所等(stop)の統合や専用レーンなどの高速化手法を通じて、バスおよび鉄道ネットワークをより高速かつ信頼性の高いものにできる場所を見つけるのに役立ちます。
- [gtfspy-webviz](https://github.com/CxAalto/gtfspy-webviz) - [gtfspy](https://github.com/CxAalto/gtfspy) を使用した GTFS データのアニメーションおよび可視化のための Web アプリケーションです。
- [gtfs-to-geojson](https://www.transit.chat/gtfs-to-geojson) - フィード一覧を備えた、gtfs から geojson へのシンプルなオンラインコンバーターです。
- [gtfs-visualizations](https://github.com/cmichi/gtfs-visualizations) - GTFS データセットのルート・路線系統(route)を可視化するためのオープンソース NodeJS アプリケーションです。
- [Mapnificent](https://www.mapnificent.net/) - 指定した時間内に公共交通機関で到達できるエリアを表示します。オープンソース版は [GitHub](https://github.com/mapnificent/mapnificent) にあり、https://www.mapnificent.net/ で公開されています。
- [MIT COAXS](http://mittransportanalyst.github.io/) - アクセシビリティに基づくステークホルダーエンゲージメントを使用した交通回廊の共同創造的計画（[OpenTripPlanner Analyst](http://www.opentripplanner.org/analyst/) を使用してルート・路線系統(route)シナリオを表示します）。
- [MOTIS](https://motis-project.de/) - [可視化](https://europe.motis-project.de/)を含む複合交通モビリティ情報システムです。
- [MTA Frequency](http://www.tyleragreen.com/maps/new_york/) - [Transitland](https://transit.land/) を使用して構築された、ニューヨーク市の地下鉄およびバスの運行頻度可視化です。
- [SEPTA Rail OTP Report](https://apps.phor.net/septa/) - GTFS を使用したオンラインの定時運行実績レポートおよび詳細分析ツールです。
- [Simple Transit Map](https://github.com/ioTransit/simple-transit-map) - Web マップをホストおよび更新する方法のオンライン例です。
- [Simple Transit Site](https://transit.chat/simple-transit-site) - gtfs のみから交通機関 Web サイトを作成する方法のオンライン例です。[Github](https://github.com/ioTransit/simple-transit-site) で公開されています。
- [TNExT](https://github.com/ODOT-PTS/TNExT) - Transit Network Explorer Tool（TNExT）は、オレゴン州における地域および州全体の交通ネットワークの可視化、分析、レポート作成のために開発された Web ベースのソフトウェアツールです。
- [Toronto Transit Explorer](https://github.com/sidewalklabs/totx) - トロント市全域における公共交通、自転車、徒歩でのアクセシビリティを可視化する Java アプリケーションです。経路探索には、修正されたバージョンの [R5](https://github.com/conveyal/r5) を使用します。
- [Transit Vis](https://github.com/zackAemmer/transit_vis) - King County Metro GTFS-RT フィード（OneBusAway API）から導出されたパフォーマンス指標を表示する可視化ツールです。[こちら](https://www.transitvis.com/)で閲覧できます。[この論文](https://link.springer.com/article/10.1007/s12469-022-00291-7)で使用されています。
- [TransitFlow](https://github.com/transitland/transitland-processing-animation) Processing および Transitland を使用して、世界中の GTFS データをアニメーション化します。
- [TRAVIC Transit Visualization Client](http://tracker.geops.ch/) - 静的 GTFS データ（および場合によってはリアルタイムデータ）に基づいて移動する車両を可視化します。260 を超える都市をサポートしています。geOps 組織の Github アカウントは[こちら](https://github.com/geops)です。
- [Traze](https://traze.app/) by [Veridict](https://www.veridict.com) - 世界中の公共交通車両の可視化です。事業者から利用できない場合でも、他のユーザーと協力してリアルタイム更新を取得できます。GTFS および GTFS-RT を含む複数のソースに基づいています。（以前は Livemap24 として知られていました。）
- [Visualizing MBTA Data](http://mbtaviz.github.io/) - 人々がボストンの地下鉄システムをどのように利用しているかを示すインタラクティブなグラフです。
- [GTFS Viz 🚉](https://github.com/gabrielAHN/gtfs-viz) - [duckdb-wasm 🦆](https://duckdb.org/docs/api/wasm/overview.html) を使用し、クライアント側でバックエンドなしに、大規模な GTFS Data をブラウザ上で可視化する Web アプリです。

### 交通路線図の作成 {: #transit-map-creation}


- [Brand New Subway](https://jpwright.github.io/subway/) - プレイヤーがNYC地下鉄システムを思いのままに変更できる、インタラクティブな交通計画ゲームです。
- [BENO Metro Mapm Creator](https://beno.uk/metromapcreator/#) - 非常に昔ながらですが、定番の交通路線図作成ツールです。
- [Tennessine Metro Designer](https://tennessine.co.uk/metro/) - モダンで美しい交通路線図デザイナーです。
- [loom](https://github.com/ad-freiburg/loom) - 地理的に正確または模式的な交通路線図を自動生成するためのソフトウェアスイートです。
- [Metro Map Maker](https://metromapmaker.com/)   - オープンソースでシンプルな地下鉄路線図作成ソフトウェアです。
- [MetroDreamin'](https://metrodreamin.com/explore) - ユーザーがエージェントとともにインタラクティブな交通路線図を作成、保存、いいね、共有できる、モダンなオープンソースソフトウェアです。
- [Rail Map Generators](https://wongchito.github.io/RailMapGenerator) - さまざまな都市の公共交通システムのスタイルで、鉄道路線図および案内パネルを生成するためのツールです。
- [MetroSets](https://metrosets.ac.tuwien.ac.at/) - 地下鉄路線図のメタファーを用いて集合システムを可視化する、柔軟なWebツールです。この[論文](https://www.computer.org/csdl/journal/tg/2021/02/09224192/1nV7Me0F3Lq)に基づいています。

#### 交通機関の可視化を作成するための一般的な描画アプリケーション {: #general-drawing-applications-for-making-transit-visualizations}

- [Adobe illustrator](https://www.adobe.com/ca/products/illustrator.html) - 業界をリードするベクターグラフィックスソフトウェアです（メンバーシッププランが必要です）。
- [Inkscape](https://inkscape.org/) - Adobe Illustratorに類似した無料のデザインツールです。

#### 交通機関の可視化を作成するための一般的な GIS アプリケーション {: #general-gis-applications-for-making-transit-visualizations}

 - [Felt](https://felt.com/) - 美的に優れたモダンな GIS ソフトウェアです。
 - [Google Mymaps](https://www.google.ca/maps/about/mymaps/) - Google My Maps を使用してカスタムマップを作成および共有します。
 - [Google Earth](https://www.google.com/earth/about/) - 世界で最も詳細な衛星アプリケーションの1つを使用して、カスタムマップを作成および共有します。

### 交通マップ集約 {: #transit-map-aggregation}

 - [UrbanRail.Net](http://www.urbanrail.net/) - 詳細かつ最新の情報を備えた、都市鉄道交通（地下鉄、路面電車、通勤鉄道）の世界的な参照マップです。
 - [OpenRailwayMap](https://www.openrailwaymap.org/) - OpenStreetMap データを使用した世界の鉄道路線図です。
 - [AllRailMap](https://www.allrailmap.com/) - OpenStreetMap データを使用した、もう1つの世界の鉄道路線図です。
 - [European Railway Atlas](https://europeanrailwayatlas.com/) - 購入可能なヨーロッパの鉄道路線図の参照書です。
 - [Rail Transit Maps](http://www-personal.umich.edu/~yopopov/rrt/railroadmaps/) - ヨーロッパ（特にロシア）を対象とする鉄道路線図のコレクションです。
 - [Tramscale](https://alexander.co.tz/tramscale/) - 世界各地の路面電車システムの規模を示す地図を紹介するウェブサイトです。
 - [Timelines](https://alexander.co.tz/timelines/) - 世界各地の高速鉄道プロジェクトのタイムラインを比較します。
 - [Metrolinemap](https://www.metrolinemap.com/) - 世界の地下鉄システムのインタラクティブマップです。
 - [Metrocyclopaedia](https://blog.csaladen.es/metro/ ) - 世界各地の地下鉄システムの3Dマップです（Metrolinemap のデータを使用しています）。
 - [RailFansCanada](https://map.railfans.ca/) - カナダのさまざまな都市鉄道システムの現在および将来を詳述するインタラクティブなシステムマップです。
 - [North American Transit](https://www.google.com/maps/d/u/0/viewer?mid=1GAXiiEp8a62LvZNDueYN76NPTCoUxvdx&ll=43.71257881237152%2C-79.385523993394&z=11) - 都市間鉄道、地下鉄、路面電車、観光路線を含む、北米のすべての旅客鉄道の地図です。
 - [Intercity Rail map](https://asm.transitdocs.com/) - Amtrak および Via の列車のリアルタイム位置と時刻表情報の地図です。
 - [Indian Railways Map](https://indiarailinfo.com/atlas) - インドの主要鉄道網のインタラクティブマップです。
 - [National Rail Network Map](https://www.arcgis.com/apps/mapviewer/index.html?webmap=96ec03e4fc8546bd8a864e39a2c3fc41) - この地図は、旅客線および貨物線を含む、米国の鉄道路線の範囲と所有状況を示します。
 - [Ferrocarta](https://ferrocarta.net/) - ブラジル、カナダ、フランスのすべての旅客鉄道ネットワークを対象とする一連の地図です。
 - [Train Lookout](https://trainlookout.com/) - 列車での旅程を簡単に記録、地図化、共有するためのツールです。
 - [Australian Rail Maps](http://www.railmaps.com.au/) - 国、州、都市レベルの詳細なオーストラリア鉄道路線図です。
 - [Steam Engine "IS"](https://parovoz.com/maps/supermap/) - ソ連の鉄道路線図です。
 - [Carto.Metro](https://cartometro.com/) - 世界の都市（特にフランス）の地下鉄および路面電車ネットワークの詳細な地図です。
 - [Railway Stations](https://map.railway-stations.org/) - 世界各地の鉄道駅の写真です。
 - [INAT](https://www.inat.fr/maps/) - 世界各地の地下鉄システムの美しい静的地図です。
 - [Transit Maps](https://transitmap.net/) - 世界各地の交通マップのデザインに関する批評とレビューです。
 - [Transit Explorer](https://www.thetransportpolitic.com/transitexplorer/) - 世界各地の固定軌道交通を含む地図です。
- [Britsh Railways](https://www.merrittcartographic.co.uk/british_railways.html) イギリスの鉄道ネットワークのインタラクティブマップです。
- [TransitLand Map](https://www.transit.land/map)  - 交通サービス（GTFS Feed を持つもの）の世界地図です。
 - [DB InfraGO](https://geovdbn.deutschebahn.com/pgv/public/map/isr.xhtml)  - ドイツの鉄道インフラのインタラクティブマップです。
 - [SNCF Carte interactive](https://www.sncf-reseau.com/fr/carte/carte-interactive-reseau-ferre-francais-0) - フランスの鉄道インフラのインタラクティブマップです。
 - [Project Mapping](https://www.projectmapping.co.uk/index.html) - 英国および世界の鉄道ネットワークの模式図です。
 - [China Railway Map](http://cnrail.geogv.org/enus/about) - 駅および鉄道情報を提示する、中国の旅客鉄道輸送システム向けオンラインインタラクティブマップです。
 - [Canadian Rail Atlas](https://rac.jmaponline.net/canadianrailatlas/) - カナダの約43,000キロメートルに及ぶ鉄道ネットワークの、使いやすいインタラクティブマップです。
 - [The Rail Map](https://www.therailmap.com/) - OpenStreetMap のデータを使用した、北米の列車路線を含むインタラクティブマップです。
 - [JR pass](https://www.jrpass.com/map#) - 日本の幹線鉄道のインタラクティブマップです。

## 事業者向けツール {: #agency-tools}


交通事業者向けのツールです。GTFS に特化したツールについては、[GTFS Data Collection and Maintenance Tools](../producing-data/#gtfs-data-collection-and-maintenance-tools) も参照してください。

- [Remix](http://getremix.com/) - 交通事業者がルートを容易に計画できる webapp です。
- [Next Train API](https://github.com/data-creative/next-train-api) - 任意の GTFS feed を JSON API として提供します。交通事業者と開発者は、オープンソースコードを自身の Heroku server にデプロイできます。
- [AC Transit RestroomFinder](https://github.com/actransitorg/ACTransit.RestroomFinder) - GPS と画面上の地図を使用して、バス運転士および現場スタッフ向けに最寄りの認可済みトイレを特定します。
- [AC Transit Training and Education Department (TED) application](https://github.com/actransitorg/ACTransit.Training) - この application は、主に Bus Operators および Heavy Duty Coach Mechanics（Apprentice および Journey）の職種における、District の輸送および保守従業員向け研修業務を支援しますが、この system は新しいコースおよび見習いプログラムにも対応しています。
- [AC Transit Customer Relations application (CusRel)](https://github.com/actransitorg/ACTransit.CusRel) - 部門間の通知付き振り分け、部門／担当者の割り当て、簡易 workflow、ticket 検索、定型レポート、日次リマインダーなどを備えた、顧客の問題およびフィードバック向け公共交通 ticketing system です。
- [PTV Lines](https://www.ptvgroup.com/en/products/ptv-lines) - 路線計画および公共交通サービス最適化のための cloud-based 公共交通 software です。
- [TransAM](https://github.com/camsys/transam_core) - 公共交通事業者向けのオープンソース資産管理 platform です。
- [RidePilot](https://github.com/camsys/ridepilot) - 小規模な人員輸送事業者のニーズを満たすための、オープンソースの Computer Aided Scheduling and Dispatch (CASD) software system です。
- [TNExT](https://github.com/ODOT-PTS/TNExT) - Transit Network Explorer Tool (TNExT) は、Oregon 州における地域および州全体の交通ネットワークの可視化、分析、報告のために開発された web-based software tool です。
- Route Trends ([webapp](https://metrotransitmn.shinyapps.io/route-trends/), [GitHub](https://github.com/metrotransit/route-trends)) - 乗客数の時系列を取り込み、[STL methodology](https://otexts.com/fpp2/stl.html) に従った季節成分、傾向成分、残差成分、およびそれらの成分に基づく不確実性を含む予測を返す R Shiny app です。[Metro Transit](https://www.metrotransit.org/)（Minneapolis-St. Paul）が支援しています。
- [TBEST](https://tbest.org/) - TBEST (Transit Boardings Estimation and Simulation Tool) は、社会経済、土地利用、および交通ネットワークのデータを、シナリオベースの交通乗客数推定および分析のための platform に統合する、多面的な GIS-based modeling、planning、および analysis tool を開発する取り組みです。Florida Department of Transportation が資金提供しています。無料で使用できますが、オープンソースではありません。
- [RideSheet](https://docs.ridesheet.org) – 小規模なデマンド型サービス向けの、シンプルな spreadsheet-based tool です。
