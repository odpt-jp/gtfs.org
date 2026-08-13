# GTFS Schedule の機能 {: #gtfs-schedule-features}


GTFS Reference format は、交通システムの現在のニーズを満たすために進化するにつれて、その機能はますます複雑になる可能性があります。**GTFS Features** は、GTFS Reference format によって実現される機能を明確かつ決定的に説明することを目的としています。これにより、交通事業者、ベンダー、利用者、研究者は GTFS の能力を理解し、次の問いに答えることができます。**GTFS で何ができるのか？** 

以下の機能グループでは、各機能の目的と、それらに関連するファイルおよびフィールドを説明し、ユーザーが特定の機能をサポートするために必要なデータを理解するのに役立ちます。

## 基本 {: #base}

これらの基本的な機能は、GTFS feedの中核を構成します。これらは、公共交通サービスを表現するために必要な最小限の要素です。

<div class="grid cards" markdown>

- :material-subway-variant:{ .lg .middle } __事業者__

    公共交通サービスを担う事業者に関する詳細を伝えます。
    
    [:octicons-arrow-right-24: 詳細を見る](../base/#agency)

- :material-subway-variant:{ .lg .middle } __停留所等(stop)__

    公共交通サービスが乗客を乗車・降車させる場所を定義します。

    [:octicons-arrow-right-24: 詳細を見る](../base/#stops)

- :material-subway-variant:{ .lg .middle }  __ルート・路線系統(route)__

    名称やサービスの種類など、公共交通ルート・路線系統(route)の要素を定義します。

    [:octicons-arrow-right-24: 詳細を見る](../base/#routes)

- :material-subway-variant:{ .lg .middle } __運行日(service day)__

    便(trip)および運行除外をスケジュールするための構造を作成します。

    [:octicons-arrow-right-24: 詳細を見る](../base/#service-dates)

- :material-subway-variant:{ .lg .middle } __便(trip)__

    定義されたルート・路線系統(route)に沿って、予定時刻に運行する公共交通車両を表現します。

    [:octicons-arrow-right-24: 詳細を見る](../base/#trips)

-   :material-subway-variant:{ .lg .middle } __停車時刻(stop_time)__

    各停留所等(stop)における各便(trip)の到着時刻および出発時刻を定義します。

    [:octicons-arrow-right-24: 詳細を見る](../base/#stop-times)

</div>

<div class="gtfs-feature-tracker" data-category="Base"></div>

## 基本アドオン {: #base-add-ons}

これらの機能は GTFS データセットを強化し、乗客体験を向上させ、事業者、ベンダー、データ再利用者間の連携を促進します。これには、既存ファイルへの新しいフィールドの追加や、新しいファイルの作成が含まれることがあります。

<div class="grid cards" markdown>

- :material-plus-box-multiple-outline:{ .lg .middle } __フィード情報__

    フィード自体に関する重要な情報を伝達します。

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#feed-information)

- :material-plus-box-multiple-outline:{ .lg .middle } __ルート形状(shape)__

    便(trip)に沿って車両が通行する地理的な経路を定義します。

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#shapes)

- :material-plus-box-multiple-outline:{ .lg .middle } __ルート・路線系統(route)の色__

    特定のルート・路線系統(route)に割り当てられた配色を正確に描写し、伝達します。

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#route-colors)

- :material-plus-box-multiple-outline:{ .lg .middle } __自転車の持ち込み可否__

    車両が自転車を収容できるかどうかを伝達します。

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#bike-allowed)

- :material-plus-box-multiple-outline:{ .lg .middle } __行先表示(headsign)__

    便(trip)の目的地を示すために車両で使用される標識を伝達します。

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#headsigns)

- :material-plus-box-multiple-outline:{ .lg .middle } __場所の種類__

    出入口など、交通駅構内の主要な区域を分類します。

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#location-types)

- :material-plus-box-multiple-outline:{ .lg .middle } __運行頻度__

    定期的な頻度または特定の運行間隔で運行するサービスを表現します。                           

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#frequencies)

- :material-plus-box-multiple-outline:{ .lg .middle } __乗り換え__

    異なる交通サービス間で許可される乗り換えを記述します。

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#transfers)

-   :material-plus-box-multiple-outline:{ .lg .middle } __翻訳__

    複数の言語でサービス情報を伝達します。

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#translations)

- :material-plus-box-multiple-outline:{ .lg .middle } __帰属情報__

    データセットの作成に関与した者を伝達します。

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#attributions)

- :material-plus-box-multiple-outline:{ .lg .middle } __自動車の持ち込み可否__

    車両が自動車を収容できるかどうかを伝達します。

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#cars-allowed)

- :material-plus-box-multiple-outline:{ .lg .middle } __停留所等(stop)へのアクセス__

    親駅に対する停留所等(stop)へのアクセス方法を伝達します。

    [:octicons-arrow-right-24: 詳細](../base_add-ons/#stop-access)
</div>

<div class="gtfs-feature-tracker" data-category="Base add-ons"></div>

## アクセシビリティ {: #accessibility}

アクセシビリティ機能は、障害のある人がサービスを利用するために不可欠な情報を提供します。

<div class="grid cards" markdown>

- :material-wheelchair:{ .lg .middle } __停留所等(stop)の車椅子アクセシビリティ__

    場所から車椅子で乗車可能かどうかを示します。     

    [:octicons-arrow-right-24: 詳細を見る](../accessibility/#stops-wheelchair-accessibility)

- :material-wheelchair:{ .lg .middle } __便(trip)の車椅子アクセシビリティ__

    車両が車椅子を使用する乗客に対応できるかどうかを示します。       

    [:octicons-arrow-right-24: 詳細を見る](../accessibility/#trips-wheelchair-accessibility)

- :material-wheelchair:{ .lg .middle } __読み上げ用フィールド(text-to-speech field)__

    停留所名のテキストを音声に変換するために必要な入力を提供します。

    [:octicons-arrow-right-24: 詳細を見る](../accessibility/#text-to-speech)

</div>

<div class="gtfs-feature-tracker" data-category="Accessibility"></div>

## 運賃 {: #fares}

GTFS では、ゾーン、距離、時間帯に基づく運賃など、さまざまな運賃体系をモデル化できます。便の料金および支払方法を乗客に知らせます。

<div class="grid cards" markdown>

-   :material-cash:{ .lg .middle } __チケット商品__

    ユーザーが利用可能なチケットまたは運賃タイプの一覧を定義します。

    [:octicons-arrow-right-24: 詳細を見る](../fares/#fare-products)

-   :material-cash:{ .lg .middle } __チケットメディア__

    チケット商品を保持および／または検証するために使用できるメディアを定義します。

    [:octicons-arrow-right-24: 詳細を見る](../fares/#fare-media)

-   :material-cash:{ .lg .middle } __乗客カテゴリ__

    特定の運賃の対象となる、異なる乗客カテゴリを表現します。

    [:octicons-arrow-right-24: 詳細を見る](../fares/#rider-categories)

-   :material-cash:{ .lg .middle } __ルート・路線系統に基づく運賃__

    特定のルート・路線系統(route)のグループに異なる運賃を適用するために使用されるルールを記述します。

    [:octicons-arrow-right-24: 詳細を見る](../fares/#route-based-fares)

-   :material-cash:{ .lg .middle } __時間に基づく運賃__

    時間帯または曜日によって区別される運賃を記述します。

    [:octicons-arrow-right-24: 詳細を見る](../fares/#time-based-fares)

-   :material-cash:{ .lg .middle } __ゾーンに基づく運賃__

    あるエリアから別のエリアへ移動する際に区別される運賃を記述します。

    [:octicons-arrow-right-24: 詳細を見る](../fares/#zone-based-fares)

-   :material-cash:{ .lg .middle } __運賃の乗り換え__

    便のある乗車区間(leg)から別の乗車区間(leg)へ乗り換える際に適用される料金を定義します。

    [:octicons-arrow-right-24: 詳細を見る](../fares/#fare-transfers)

-   :material-cash:{ .lg .middle } __非接触 EMV サポート__

    ユーザーが非接触 EMV カードまたはデバイスを使用して運賃を支払えるかどうかを伝えます。

    [:octicons-arrow-right-24: 詳細を見る](../fares/#contactless-emv-support)

-   :material-cash:{ .lg .middle } __運賃 V1__

    運賃情報をより簡易に表現できるようにするレガシー機能です。

    [:octicons-arrow-right-24: 詳細を見る](../fares/#fares-v1)

</div>

<div class="gtfs-feature-tracker" data-category="Fares"></div>

## 構内通路(pathway) {: #pathways}


構内通路(pathway)機能により、大規模な公共交通機関の駅をモデル化し、乗客を入口から乗車エリアへ案内することができます。これにより、経路の詳細、推定ナビゲーション時間、および案内システムを提供します。

<div class="grid cards" markdown>

-   :material-escalator:{ .lg .middle } __構内通路の接続__

    公共交通機関の駅内にある関連地点を接続する経路をモデル化します。

    [:octicons-arrow-right-24: 詳細](../pathways/#pathway-connections)

-   :material-escalator:{ .lg .middle } __構内通路の詳細__

    構内通路(pathway)の物理的特性に関する追加の詳細を提供します。

    [:octicons-arrow-right-24: 詳細](../pathways/#pathway-details)

-   :material-escalator:{ .lg .middle } __階層__

    公共交通機関の駅内にあるすべての異なる階層を記述し、一覧にします。

    [:octicons-arrow-right-24: 詳細](../pathways/#levels)

-   :material-escalator:{ .lg .middle } __駅構内の移動時間__

    公共交通機関の駅内の経路を移動するための推定時間を伝達します。

    [:octicons-arrow-right-24: 詳細](../pathways/#in-station-traversal-time)

-   :material-escalator:{ .lg .middle } __構内通路の標識__

    構内通路(pathway)に関連付けられた駅構内の標識を伝達します。

    [:octicons-arrow-right-24: 詳細](../pathways/#pathway-signs)

</div>

<div class="gtfs-feature-tracker" data-category="Pathways"></div>

## フレキシブルサービス {: #flexible-services}

通常の時刻表や固定ルートに従わないフレキシブルサービス、またはデマンド型サービス。

<div class="grid cards" markdown>

- :material-transit-detour:{ .lg .middle } __連続した停車地点__

    利用者を停留所等(stop)の間で乗車させたり、および／または降車させたりできるかを示します。
    
    [:octicons-arrow-right-24: 詳細](../flexible_services/#continuous-stops)

- :material-transit-detour:{ .lg .middle } __予約ルール__

    利用者がデマンド型サービスの便(trip)を予約できるかを示します。            

    [:octicons-arrow-right-24: 詳細](../flexible_services/#booking-rules)

- :material-transit-detour:{ .lg .middle } __迂回可能な事前定義ルート__

    乗車または降車のためにルートから短時間迂回できる車両。   

    [:octicons-arrow-right-24: 詳細](../flexible_services/#predefined-routes-with-deviation)

- :material-transit-detour:{ .lg .middle } __ゾーンベースのデマンド型サービス__

    特定のエリア内の任意の場所で乗車／降車できるサービス。

    [:octicons-arrow-right-24: 詳細](../flexible_services/#zone-based-demand-responsive-services)

- :material-transit-detour:{ .lg .middle } __固定停留所型デマンド型サービス__

    停留所等(stop)のグループ内の任意の場所で乗車／降車できるサービス。
   
    [:octicons-arrow-right-24: 詳細](../flexible_services/#fixed-stops-demand-responsive-services)

</div>

<div class="gtfs-feature-tracker" data-category="Flexible services"></div>
