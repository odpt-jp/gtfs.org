# GTFS Schedule の機能 {: #gtfs-schedule-features}

GTFS Reference 形式は、公共交通システムの現在のニーズに対応するために進化するにつれて、その機能がますます複雑になることがあります。**GTFS の機能(GTFS Features)** は、GTFS Reference 形式によって実現される機能を明確かつ決定的に説明することを目的としています。これにより、交通事業者、ベンダー、利用者、研究者が GTFS の機能を理解し、「GTFS で何ができるのか？」という問いに答えることができます。

以下の機能グループでは、それぞれの機能の目的と、それに関連するファイルおよびフィールドを説明しています。これにより、特定の機能をサポートするためにどのデータが必要かを理解することができます。

## 基本要素 {: #base}

これらの基本的な機能は、GTFS フィードの中核を構成します。これらは、公共交通サービスを表現するために必要な最小限の要素です。

<div class="grid cards" markdown>

- :material-subway-variant:{ .lg .middle } __Agency__

    公共交通サービスを担当する事業者に関する詳細を伝えます。
    
    [:octicons-arrow-right-24: 詳細を見る](../base/#agency)

- :material-subway-variant:{ .lg .middle } __Stops__

    公共交通サービスが乗客を乗降させる場所を定義します。

    [:octicons-arrow-right-24: 詳細を見る](../base/#stops)

- :material-subway-variant:{ .lg .middle }  __Routes__

    路線名やサービスの種類など、公共交通のルート・路線系統(route)の要素を定義します。

    [:octicons-arrow-right-24: 詳細を見る](../base/#routes)

- :material-subway-variant:{ .lg .middle } __Service Dates__

    便(trip)のスケジュールおよび運行除外日を設定するための構造を作成します。

    [:octicons-arrow-right-24: 詳細を見る](../base/#service-dates)

- :material-subway-variant:{ .lg .middle } __Trips__

    定義されたルート・路線系統(route)に沿って、予定された時刻に運行する公共交通車両を表します。

    [:octicons-arrow-right-24: 詳細を見る](../base/#trips)

-   :material-subway-variant:{ .lg .middle } __Stop Times__

    各便(trip)における各停留所等(stop)での到着時刻および出発時刻を定義します。

    [:octicons-arrow-right-24: 詳細を見る](../base/#stop-times)

</div>

## 基本アドオン {: #base-add-ons}

これらの機能は GTFS データセットを拡張し、乗客の利便性を向上させ、事業者、ベンダー、データ再利用者間の協力を促進します。既存ファイルへの新しいフィールドの追加や、新しいファイルの作成を伴う場合があります。

<div class="grid cards" markdown>

- :material-plus-box-multiple-outline:{ .lg .middle } __フィード情報__

    フィード自体に関する重要な情報を伝達します。

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#feed-information)

- :material-plus-box-multiple-outline:{ .lg .middle } __ルート形状(Shapes)__

    便(trip)に沿って車両がたどる地理的経路を定義します。

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#shapes)

- :material-plus-box-multiple-outline:{ .lg .middle } __ルートカラー__

    特定のルート・路線系統(route)に割り当てられた配色を正確に表現し、伝達します。

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#route-colors)

- :material-plus-box-multiple-outline:{ .lg .middle } __自転車搭載可否__

    車両が自転車を搭載できるかどうかを伝達します。

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#bike-allowed)

- :material-plus-box-multiple-outline:{ .lg .middle } __行先表示(Headsigns)__

    便(trip)の目的地を示す車両の表示内容を伝達します。

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#headsigns)

- :material-plus-box-multiple-outline:{ .lg .middle } __ロケーションタイプ__

    駅構内の入口や出口など、主要なエリアを分類します。

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#location-types)

- :material-plus-box-multiple-outline:{ .lg .middle } __運行頻度(Frequencies)__

    一定の運行間隔または特定のヘッドウェイで運行するサービスを表現します。                           

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#frequencies)

- :material-plus-box-multiple-outline:{ .lg .middle } __乗換(Transfers)__

    異なる交通サービス間で許可される乗換を記述します。

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#transfers)

- :material-plus-box-multiple-outline:{ .lg .middle } __翻訳(Translations)__

    サービス情報を複数の言語で伝達します。

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#translations)

- :material-plus-box-multiple-outline:{ .lg .middle } __帰属情報(Attributions)__

    データセットの作成に関与した人物や組織を伝達します。

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#attributions)

- :material-plus-box-multiple-outline:{ .lg .middle } __自動車搭載可否__

    車両が自動車を搭載できるかどうかを伝達します。

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#cars-allowed)

- :material-plus-box-multiple-outline:{ .lg .middle } __停留所アクセス(Stop Access)__

    停留所(stop)が親駅に対してどのようにアクセスされるかを伝達します。

    [:octicons-arrow-right-24: 詳細を見る](../base_add-ons/#stop-access)
</div>

## アクセシビリティ {: #accessibility}

アクセシビリティ機能は、障害のある方がサービスを利用するために必要な情報を提供します。

<div class="grid cards" markdown>

- :material-wheelchair:{ .lg .middle } __停留所等の車椅子対応__

    その場所から車椅子での乗降が可能かどうかを示します。     

    [:octicons-arrow-right-24: 詳細を見る](../accessibility/#stops-wheelchair-accessibility)

- :material-wheelchair:{ .lg .middle } __便の車椅子対応__

    車両が車椅子を利用する乗客に対応できるかどうかを示します。       

    [:octicons-arrow-right-24: 詳細を見る](../accessibility/#trips-wheelchair-accessibility)

- :material-wheelchair:{ .lg .middle } __読み上げ用フィールド(text-to-speech)__

    停留所名などのテキストを音声に変換するために必要な入力を提供します。

    [:octicons-arrow-right-24: 詳細を見る](../accessibility/#text-to-speech)

</div>

## 運賃 {: #fares}

GTFS は、ゾーン制、距離制、時間帯制など、さまざまな運賃体系をモデル化することができます。これにより、乗客に便の料金や支払い方法を知らせることができます。

<div class="grid cards" markdown>

-   :material-cash:{ .lg .middle } __チケット商品(Fare Products)__

    利用者が購入できるチケットや運賃タイプの一覧を定義します。

    [:octicons-arrow-right-24: 詳しく見る](../fares/#fare-products)

-   :material-cash:{ .lg .middle } __チケットメディア(Fare Media)__

    チケット商品を保持および／または認証するために使用できるメディアを定義します。

    [:octicons-arrow-right-24: 詳しく見る](../fares/#fare-media)

-   :material-cash:{ .lg .middle } __乗客カテゴリ(Rider Categories)__

    特定の運賃を利用できる異なる乗客のカテゴリを表します。

    [:octicons-arrow-right-24: 詳しく見る](../fares/#rider-categories)

-   :material-cash:{ .lg .middle } __ルートベース運賃(Route-Based Fares)__

    特定のルート群に対して異なる運賃を適用するためのルールを記述します。

    [:octicons-arrow-right-24: 詳しく見る](../fares/#route-based-fares)

-   :material-cash:{ .lg .middle } __時間帯ベース運賃(Time-Based Fares)__

    時間帯や曜日によって区別される運賃を記述します。

    [:octicons-arrow-right-24: 詳しく見る](../fares/#time-based-fares)

-   :material-cash:{ .lg .middle } __ゾーンベース運賃(Zone-Based Fares)__

    あるエリアから別のエリアへ移動する際に区別される運賃を記述します。

    [:octicons-arrow-right-24: 詳しく見る](../fares/#zone-based-fares)

-   :material-cash:{ .lg .middle } __運賃乗り継ぎ(Fare Transfers)__

    便の乗車区間(leg)を乗り継ぐ際に適用される料金を定義します。

    [:octicons-arrow-right-24: 詳しく見る](../fares/#fare-transfers)

-   :material-cash:{ .lg .middle } __コンタクトレスEMV対応(Contactless EMV Support)__

    利用者がコンタクトレスEMVカードまたはデバイスを使用して運賃を支払うことができるかどうかを示します。

    [:octicons-arrow-right-24: 詳しく見る](../fares/#contactless-emv-support)

-   :material-cash:{ .lg .middle } __運賃V1(Fares V1)__

    運賃情報をより簡易に表現するための旧仕様です。

    [:octicons-arrow-right-24: 詳しく見る](../fares/#fares-v1)

</div>

## 構内通路(Pathways) {: #pathways}


構内通路(pathways)の機能を使用すると、大規模な交通駅をモデル化し、乗客が入口から乗車エリアまで案内されるようにすることができます。これにより、経路の詳細、推定移動時間、および案内システムを提供することができます。

<div class="grid cards" markdown>

-   :material-escalator:{ .lg .middle } __構内通路の接続(Pathway Connections)__

    交通駅内の関連する地点を接続する経路をモデル化します。

    [:octicons-arrow-right-24: 詳細を見る](../pathways/#pathway-connections)

-   :material-escalator:{ .lg .middle } __構内通路の詳細(Pathway Details)__

    構内通路の物理的特性に関する追加の詳細を提供します。

    [:octicons-arrow-right-24: 詳細を見る](../pathways/#pathway-details)

-   :material-escalator:{ .lg .middle } __階層(Levels)__

    交通駅内のすべての異なる階層を記述し、一覧化します。

    [:octicons-arrow-right-24: 詳細を見る](../pathways/#levels)

-   :material-escalator:{ .lg .middle } __駅構内の移動時間(In-Station Traversal Time)__

    駅構内の経路を移動するための推定時間を伝達します。

    [:octicons-arrow-right-24: 詳細を見る](../pathways/#in-station-traversal-time)

-   :material-escalator:{ .lg .middle } __構内通路の案内表示(Pathway Signs)__

    構内通路に関連する駅構内の案内表示を伝達します。

    [:octicons-arrow-right-24: 詳細を見る](../pathways/#pathway-signs)

</div>

## フレキシブルサービス {: #flexible-services}

フレキシブルサービス、またはデマンド型サービスとは、定期的な時刻表や固定ルートに従わないサービスのことです。

<div class="grid cards" markdown>

- :material-transit-detour:{ .lg .middle } __連続停留所(Continuous Stops)__

    乗客が停留所間で乗車および／または降車できるかどうかを示します。
    
    [:octicons-arrow-right-24: 詳細を見る](../flexible_services/#continuous-stops)

- :material-transit-detour:{ .lg .middle } __予約ルール(Booking Rules)__

    乗客がデマンド型サービスで便を予約できるかどうかを示します。            

    [:octicons-arrow-right-24: 詳細を見る](../flexible_services/#booking-rules)

- :material-transit-detour:{ .lg .middle } __逸脱可能な事前定義ルート(Predefined Routes with Deviation)__

    車両が乗客の乗降のために一時的にルートから逸脱できる場合を示します。   

    [:octicons-arrow-right-24: 詳細を見る](../flexible_services/#predefined-routes-with-deviation)

- :material-transit-detour:{ .lg .middle } __ゾーンベースのデマンド型サービス(Zone-based Demand Responsive Services)__

    特定のエリア内の任意の場所で乗車および降車が可能なサービスです。

    [:octicons-arrow-right-24: 詳細を見る](../flexible_services/#zone-based-demand-responsive-services)

- :material-transit-detour:{ .lg .middle } __固定停留所型デマンド型サービス(Fixed-Stops Demand Responsive Services)__

    特定の停留所グループ内の任意の場所で乗車および降車が可能なサービスです。
   
    [:octicons-arrow-right-24: 詳細を見る](../flexible_services/#fixed-stops-demand-responsive-services)

</div>
