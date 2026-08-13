# GTFS Realtime の変更 {: #gtfs-realtime-changes}


GTFS Realtime Reference は不変のものではありません。むしろ、GTFS Realtime を利用する交通事業者、開発者、その他の関係者のコミュニティによって開発・維持されているオープンな仕様です。GTFS Realtime データの producer および consumer からなるこのコミュニティは、新しい機能を実現するために仕様を拡張する提案を行うことが期待されています。

GTFS Realtime に貢献するには、[GTFS Realtime Amendment Process](../../../../community/governance/gtfs_realtime_amendment_process) を読み、GTFS Github リポジトリ（<a href="https://github.com/google/transit" target="_blank">google/transit</a>）で公開されている <a href="https://github.com/google/transit/issues" target="_blank">issues</a> および <a href="https://github.com/google/transit/pulls" target="_blank">pull requests</a> の議論をフォローしてください。 ![](../../../assets/mark-github.svg)

<!-- <div class="row">
    <div class="active-container">
        <h3 class="title"><a class="no-icon" href="https://github.com/google/transit/pull/332" target="_blank">運行情報(alert) に cause_detail および effect_detail を追加</a></h3>
        <p class="maintainer">#332 は、<a class="no-icon" href="https://github.com/mckenzie-maidl-ibigroup" target="_blank">mckenzie-maidl-ibigroup</a> により 2022年5月31日に作成されました</p>
    </div>
</div>
<div class="row"></div> -->

<!-- <div class="row no-active">
    <div class="no-active-container">
        <h3 class="title">現在、GTFS Realtime に関する有効な提案はありません。</h3>
        <p class="prompt">提案がありますか？ &ensp;➜&ensp; <a href="https://github.com/google/transit/pulls" target="_blank">pull request</a> を作成してください。</p>
    </div>
</div>
<div class="row"></div> -->

## 最近採用された提案 &ensp;<img src="../../../../assets/pr-merged.svg" style="height:1em;"/> {: #recently-adopted-proposals-ensp}


最近マージされ、現在は[公式 GTFS Realtime リファレンス](../../reference)の機能となっている提案です。詳細については、完全な[改訂履歴](../revision_history)を参照してください。


<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/403" class="no-icon" target="_blank">Trip-Modifications を追加</a></h3>
        <p class="maintainer"><a href="https://github.com/gcamp" class="no-icon" target="_blank">gcamp</a> による #332 は、2024年3月11日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>複数の便(trip)に影響する迂回を記述するために使用される、実験的機能として便の変更(trip modification)を追加します。</li>
            <li>便の変更(trip modification)では、特定の停留所等(stop)を取り消し、便(trip)の時刻を調整し、便(trip)が通行する新しいルート形状(shape)を提供し、途中の臨時停留所等(stop)の位置を提供することができます。 </li>
        </ul>
    </div>
</div>

<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/352" class="no-icon" target="_blank">schedule_relationship に DELETED enum を追加</a></h3>
        <p class="maintainer"><a href="https://github.com/mads14" class="no-icon" target="_blank">mads14</a> による #332 は、2022年11月30日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>新しい実験的な <code>DELETED</code> 便(trip) schedule_relationship enum を追加します</li>
            <li>これは、交通事業者が便(trip)を一般向けアプリケーションから完全に消去する意図があることを伝えるために使用できます</li>
        </ul>
    </div>
</div>

<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/332" class="no-icon" target="_blank">運行情報(alert)に cause_detail および effect_detail を追加</a></h3>
        <p class="maintainer"><a href="https://github.com/mckenzie-maidl-ibigroup" class="no-icon" target="_blank">mckenzie-maidl-ibigroup</a> による #332 は、2022年6月26日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>運行情報(alert)の原因および影響に説明を追加します</li>
        </ul>
    </div>
</div>

<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/340" class="no-icon" target="_blank">GTFS-rt: 車椅子アクセスの更新</a></h3>
        <p class="maintainer"><a href="https://github.com/flaktack" class="no-icon" target="_blank">flaktack</a> による #340 は、2022年7月25日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>便(trip)のアクセシビリティに関するリアルタイム情報を追加します</li>
            <li>提供される場合、GTFS Schedule データセットの <code>trips.wheelchair_accessible</code> を上書きします</li>
        </ul>
    </div>
</div>

<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/283" class="no-icon" target="_blank">運行情報(alert)内の機能/画像</a></h3>
        <p class="maintainer"><a href="https://github.com/gcamp" class="no-icon" target="_blank">gcamp</a> による #283 は、2021年11月26日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>運行情報(alert)の理解を促進するため、アプリの運行情報(alert)に表示する画像（例: 写真または地図）への URL リンクのフィールドを追加します</li>
            <li>変更には、画像のサイズ制限の適用、運行情報(alert)ごとに1つの画像、画像のコンテンツまたは言語が変更された場合に URL も変更されることの保証が含まれます</li>
        </ul>
    </div>
</div>

<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/272" class="no-icon" target="_blank">実験的機能として GTFS-NewShapes を追加</a></h3>
        <p class="maintainer"><a href="https://github.com/ericouyang" class="no-icon" target="_blank">ericouyang</a> による #272 は、2021年8月30日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>迂回を反映するために、ルート形状(shape)をリアルタイムで更新する機能</li>
            <li>ルートの更新は、既存の <code>shape_id</code> を参照するか、エンコードされたポリラインとして新しいルート形状(shape)をリアルタイムで定義することで反映されます</li>
        </ul>
    </div>
</div>

<div class="row"></div>
