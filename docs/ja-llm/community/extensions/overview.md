# 拡張機能 {: #extensions}


<!-- GTFSはコミュニティ主導のデータ形式です。ユーザーは変更を提案し、投票することができます。詳細については、[GTFS Schedule](../schedule/process)および[GTFS Realtime](../realtime/process)の改定プロセスを参照してください。

現在開発中の拡張機能は、MobilityDataの[roadmap](https://mobilitydata.org/roadmaps/#transit)で確認できます。

拡張機能の提案は、以下の場所で確認できます。

- MobilityDataは、[GTFS拡張機能の提案一覧を掲載したポータル](https://mobilitydata.org/roadmaps/#transit)を維持しています。roadmapは、同団体のメンバーによる優先順位付けに基づいています。ポータルを通じてアイデアや拡張機能を提案できます。
- TransitWiki.orgには、[GTFS拡張機能プロジェクトの一覧](https://www.transitwiki.org/TransitWiki/index.php/General_Transit_Feed_Specification#GTFS_Extensions)があります。

詳細については、[specifications@mobilitydata.org](mailto:specifications@mobilitydata.org)までお問い合わせください。 -->

=== "GTFS Schedule"

    これらのフィールドが[公式仕様](../../../documentation/schedule/reference)に含まれていない場合でも、交通事業者とソフトウェアベンダーの間で伝達される、さまざまなアプリケーション固有のニーズに対応するため、追加のファイルおよびフィールドをGTFS Scheduleデータセットに拡張できます。
    
    以下は、実装できるGTFS Schedule拡張機能の一覧です。
    
    !!! info "仕様における拡張機能の公式化" 

        拡張機能は有効な提案となり、その後[Specification Amendment Process](../../governance/gtfs_schedule_amendment_process/)を通じて公式仕様に[統合](../../../documentation/schedule/change-history/recent_additions/)されることがあります。

    !!! note "この一覧への貢献"

        GTFSコミュニティと共有したい拡張機能がありますか？GTFS.orgへの拡張機能の追加を<a href="https://forms.gle/fUVHy5EEw2uXMdmT6" target="_blank">こちら</a>からリクエストしてください。

    <div class="row">
    <div class="leftcontainer">
            <h3 class="title"><a href="http://bit.ly/gtfs-pathways" class="no-icon" target="_blank">GTFS-Pathways</a></h3>
            <p class="maintainer"><a href="https://mobilitydata.org/" class="no-icon" target="_blank">MobilityData</a>が管理</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>ルート計画および経路案内のために、交通駅構内の場所同士を結ぶ構内通路(pathway)に焦点を当てたアクセシビリティ対応です。</li>
            <li>GTFS-Pathwaysの中核仕様は完全にGTFSへ統合されていますが、読み上げ用の案内、車椅子支援情報、設備故障の報告、予定またはスケジュールされた出入口の閉鎖、エレベーターおよびエスカレーターの停止などの追加情報を追加する必要があります。</li>
        </ul>
    </div>
    </div>

    <div class="row">
    <div class="leftcontainer">
            <a href="../fares-v2" class="no-icon" target="_blank"><h3 class="title">GTFS-Fares v2</h3></a>
            <p class="maintainer"><a href="https://mobilitydata.org/" class="no-icon" target="_blank">MobilityData</a>が管理</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>旅程計画アプリが乗客に料金情報を表示できるようにします。</li>
            <li>GTFS-Fares v2の基本実装は最近GTFSへの採用が投票で可決されましたが、この拡張機能に残されている機能には、ゾーン制・距離制運賃、乗客カテゴリ、運賃上限および範囲、運賃バンドル、パスおよびコンテナ、ラッシュアワー・休日料金、乗換順序、および単一ルートの挙動が含まれます。</li>
        </ul>
    </div>
    </div>

    <div class="row">
    <div class="leftcontainer">
            <a href="../flex" class="no-icon" target="_blank"><h3 class="title">GTFS-Flex</h3></a>
            <p class="maintainer"><a href="https://mobilitydata.org/" class="no-icon" target="_blank">MobilityData</a>が管理</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>常に固定停留所等(stop)と同じ経路に従うわけではない、迂回型固定ルートおよびオンデマンド交通サービスの機能をデータモデリングに含めることで、GTFSを拡張します。</li>
            <li>この提案は2つの拡張機能で構成されています。サービス自体を記述するGTFS-FlexibleTripsと、GTFS-FlexibleTripsの予約情報を提供するGTFS-BookingRulesです。</li>
        </ul>
    </div>
    </div>

    <div class="row">
    <div class="leftcontainer">
            <h3 class="title"><a href="http://bit.ly/gtfs-occupancies" class="no-icon" target="_blank">GTFS-Occupancies</a></h3>
            <p class="maintainer"><a href="https://mobilitydata.org/" class="no-icon" target="_blank">MobilityData</a>が管理</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>通常時または予測に基づく車両の乗客混雑度を記述します。</li>
            <li>過去の傾向に基づく将来の便(trip)の静的予測を提供することで、GTFS Realtimeで記述される混雑度情報の利用可能性を補完します。これにより、乗客は混雑度の好みや快適性に基づいて旅程を計画できます。</li>
        </ul>
    </div>
    </div>

    <div class="row">
    <div class="leftcontainer">
            <h3 class="title"><a href="https://developers.google.com/transit/gtfs/reference/gtfs-extensions" class="no-icon" target="_blank">Google Transit Extensions to GTFS</a></h3>
            <p class="maintainer"><a href="https://developers.google.com/transit" class="no-icon" target="_blank">Google</a>が管理</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>翻訳サポートを提供し、ルートタイプを拡張し、より詳細な乗換ルールを可能にし、その他多数の機能を追加します。</li>
        </ul>
    </div>
    </div>

    <div class="row">
    <div class="leftcontainer">
            <h3 class="title"><a href="https://www.transitwiki.org/TransitWiki/index.php/File:GTFS%2B_Additional_Files_Format_Ver_1.7.pdf" class="no-icon" target="_blank">MTC GTFS+</a></h3>
            <p class="maintainer"><a href="https://mtc.ca.gov/" class="no-icon" target="_blank">MTC</a>が管理</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>サンフランシスコ・ベイエリア都市圏交通委員会によって作成されました。追加のリアルタイム情報、案内、乗客カテゴリなどを追加します。</li>
        </ul>
    </div>
    </div>

    <div class="row">
    <div class="leftcontainer">
            <h3 class="title"><a href="https://github.com/mbta/gtfs-documentation/" class="no-icon" target="_blank">MBTA</a></h3>
            <p class="maintainer"><a href="https://www.mbta.com/" class="no-icon" target="_blank">MBTA</a>が管理</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>定時運行実績の追跡チェックポイント、駅および施設情報などを追加します。</li>
        </ul>
    </div>
    </div>
    <div class="row">
        <div class="leftcontainer">
                <h3 class="title"><a href="https://github.com/ODOT-PTS/gtfs-eligibilities" class="no-icon" target="_blank">GTFS-Eligibilities</a></h3>
                <p class="maintainer"><a href="https://github.com/ODOT-PTS/gtfs-eligibilities" class="no-icon" target="_blank">オレゴン州運輸局</a>が管理</p>
        </div>
        <div class="featurelist">
            <p>GTFS-eligibilitiesは、ユーザーアカウントに基づいて運用されるシステムが、ユーザーアカウント情報に基づいて便(trip)が利用資格を満たすかどうかを理解する手段を提供するべきであるという概念を中心に構成されています。これは、提案されるフィールドが以下を提供することを意味します。</p>
            <ul>
                <li>年齢、性別、所属企業、移動目的、および提供される支援レベルなど、ユーザーアカウントに関連する共通属性。</li>
                <li>地域で定義された属性およびステータスのカスタマイズ可能な認証。カスタム利用資格に加え、カスタム利用資格をどのように認証できるかを理解する方法も提供されます。</li>
            </ul>
        </div>
    </div>
     <div class="row">
        <div class="leftcontainer">
                <h3 class="title"><a href="https://github.com/ODOT-PTS/gtfs-capabilities" class="no-icon" target="_blank">GTFS-Capabilities</a></h3>
                <p class="maintainer"><a href="https://github.com/ODOT-PTS/gtfs-capabilities" class="no-icon" target="_blank">オレゴン州運輸局</a>が管理</p>
        </div>
        <div class="featurelist">
            <p>障害のある人々および移動補助具を使用する人々にサービスを提供するために、サービスが提供できる可能性のある追加機能を記述します。</p>
            <ul>
                <li>運転手または事業者が提供するその他の人的リソースなど、人から乗客が利用できるサービスに関する情報。</li>
                <li>車両情報は、列車編成および推奨乗車エリアを表現するため、（さらに拡張された）<a href="https://docs.google.com/document/d/156RiBjI6FnWJvO8_XWX11Q9nLdOiBdS_rilA-oamlv8/edit#heading=h.tosuo6e9e0z7">GTFS-VehicleCategories</a>仕様で記述され、GTFS-VehicleBoardingsにも含まれます。<a href="https://docs.google.com/document/d/1mcQ-oEaP5WiGh46DmUQqmeS-rQ5W96L-c3TRKinGS0g/edit#heading=h.oxdoxruczgni">GTFS-seats</a>ドラフト拡張機能も参照してください。</li>
                <li>移動補助具に関連する車両設備、およびそれらの補助具を使用した乗車が他の乗客および補助具の収容力にどのように影響するかを記述することに焦点を当てています。</li>
            </ul>
        </div>
    </div>
    <div class="row"></div>

=== "GTFS Realtime"

    これらのフィールドが[公式仕様](../../../documentation/realtime/reference)に含まれていない場合でも、交通事業者とソフトウェアベンダーの間で伝達される、さまざまなアプリケーション固有のニーズに対応するため、追加のファイルおよびフィールドをGTFS Realtimeフィードに拡張できます。
    
    以下は、実装できるGTFS Realtime拡張機能の一覧です。
    
    !!! info "仕様における拡張機能の公式化" 
        
        拡張機能は有効な提案となり、その後[Specification Amendment Process](../../governance/gtfs_realtime_amendment_process/)を通じて公式仕様に[統合](../../../documentation/realtime/change-history/recent_additions/)されることがあります。
    
    !!! note "この一覧への貢献"

        GTFSコミュニティと共有したい拡張機能がありますか？GTFS.orgへの拡張機能の追加を<a href="https://forms.gle/fUVHy5EEw2uXMdmT6" target="_blank">こちら</a>からリクエストしてください。

    <div class="row">
    <div class="leftcontainer">
            <h3 class="title"><a href="https://docs.google.com/document/d/1qJOTe4m_a4dcJnvXYt4smYj4QQ1ejZ8CvLBYzDM5IyM/edit#bookmark=id.av58okxmwekh" class="no-icon" target="_blank">GTFS-PathwayUpdates</a></h3>
            <p class="maintainer"><a href="https://mobilitydata.org/" class="no-icon" target="_blank">MobilityData</a>が管理</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>エレベーターの停止や構内通路(pathway)の閉鎖など、駅構内通路の変更をリアルタイムで記述します。</li>
        </ul>
    </div>
    </div>

    <div class="row">
    <div class="leftcontainer">
            <h3 class="title"><a href="https://github.com/google/transit/pull/212" class="no-icon" target="_blank">GTFS-OccupancyStatus</a></h3>
            <p class="maintainer"><a href="https://mobilitydata.org/" class="no-icon" target="_blank">MobilityData</a>が管理</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>交通車両の混雑に関する情報をリアルタイムで提供します。</li>
        </ul>
    </div>
    </div>

    <div class="row"></div>

詳細については、[specifications@mobilitydata.org](mailto:specifications@mobilitydata.org)までお問い合わせください。
