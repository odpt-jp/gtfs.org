# GTFS Schedule の変更 {: #gtfs-schedule-changes}


GTFS Schedule Reference は不変のものではありません。これは、GTFS を利用する交通事業者、開発者、その他の関係者のコミュニティによって開発・維持されているオープンな仕様です。GTFS データの生産者および利用者からなるこのコミュニティが、新たな機能を実現するために仕様を拡張する提案を行うことが想定されています。

GTFS に貢献するには、[GTFS Schedule Amendment Process](../../../../community/governance/gtfs_schedule_amendment_process) を読み、GTFS Github リポジトリ（<a href="https://github.com/google/transit" target="_blank">google/transit</a>）で公開されている <a href="https://github.com/google/transit/issues" target="_blank">issues</a> および <a href="https://github.com/google/transit/pulls" target="_blank">pull requests</a> の議論をフォローしてください。![](../../../assets/mark-github.svg)

<!-- <div class="row">
    <div class="active-container">
        <h3 class="title"><a class="no-icon" href="https://github.com/google/transit/pull/303" target="_blank">座席内オプションを伴う便間の乗換を追加する</a></h3>
        <p class="maintainer">#303 は、2022年1月26日に <a class="no-icon" href="https://github.com/gcamp" target="_blank">gcamp</a> によって作成されました</p>
    </div>
</div>
<div class="row"></div> -->

<!-- <div class="row no-active">
    <div class="no-active-container">
        <h3 class="title">現在、GTFS Schedule に対する有効な提案はありません。</h3>
        <p class="prompt">提案がありますか？ &ensp;➜&ensp; <a href="https://github.com/google/transit/pulls" target="_blank">pull request</a> を作成してください。</p>
    </div>
</div>
<div class="row"></div> -->

## 最近採用された提案 &ensp;<img src="../../../../assets/pr-merged.svg" style="height:1em;"/> {: #recently-adopted-proposals-ensp}


最近マージされ、現在は[公式 GTFS Schedule Reference](../../reference)の機能となっている提案です。詳細については、完全な[改訂履歴](../revision_history)を参照してください。

<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/433" class="no-icon" target="_blank">GTFS Flex を採用</a></h3>
        <p class="maintainer"><a href="https://github.com/tzujenchanmbd" class="no-icon" target="_blank">tzujenchanmbd</a> による #433 は、2024年3月19日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li><a href="../../../../community/extensions/flex" class="no-icon" target="_blank">GTFS-Flex proposal</a>により、乗客は経路検索ツールでデマンド型サービスを見つけることができます</li>
	    <li>GeoJson を GTFS に統合する locations.geojson を含む、複数のファイルが仕様に追加されました</li>
        </ul>
    </div>
</div>

<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/405" class="no-icon" target="_blank">networks.txt と route_networks.txt を追加</a></h3>
        <p class="maintainer"><a href="https://github.com/tzujenchanmbd" class="no-icon" target="_blank">tzujenchanmbd</a> による #406 は、2023年11月28日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>運賃に関連付けられたルート・路線系統(route)のネットワークを構築するため、<code>networks.txt</code> と <code>route_networks.txt</code> の2つの新しいファイルを追加します</li>
	    <li>ダイヤおよび運賃ファイルを分離できるよう、<code>routes.network_id</code> の代替手段を提供します</li>
        </ul>
    </div>
</div>

<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/406" class="no-icon" target="_blank">Best Practices: データセット公開ガイドライン<br>およびすべてのファイルに対する実践推奨事項を追加</a></h3>
        <p class="maintainer"><a href="https://github.com/Sergiodero" class="no-icon" target="_blank">Sergiodero</a> による #406 は、2023年11月16日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>GTFS Best Practices の2つのセクション、データセット公開ガイドラインおよびすべてのファイルに対する実践推奨事項を仕様に追加します</li>
            <li>Google の transitfeed ツールのマージ機能への参照を更新し、代わりにマージツールの一覧を参照するようにします</li>
        </ul>
    </div>
</div>

<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/386" class="no-icon" target="_blank">Best practices: 推奨される存在を追加</a></h3>
        <p class="maintainer"><a href="https://github.com/emmambd" class="no-icon" target="_blank">emmambd</a> による #386 は、2023年8月1日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>RFC の慣例に準拠する、新しい推奨される存在を仕様に追加します</li>
            <li>フィールドまたはファイルが必須ではないものの、それを追加することは検討するべきベストプラクティスであることを明確に示すことができます</li>
            <li>GTFS Best Practices に基づく推奨される存在を反映するため、複数のファイルおよびフィールドに関する情報を更新します</li>
        </ul>
    </div>
</div>

<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/357" class="no-icon" target="_blank">時刻または曜日による変動運賃を追加</a></h3>
        <p class="maintainer"><a href="https://github.com/isabelle-dr" class="no-icon" target="_blank">isabelle-dr</a> による #357 は、2023年7月27日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>時間変動運賃は、<a href="../../../../community/extensions/fares-v2">GTFS Fares-v2 extension proposal</a>の一部として開発された重要な機能です</li>
            <li>ピーク時運賃およびオフピーク時運賃など、時刻または曜日に基づいて区別される運賃を表現できます</li>
            <li>運賃が適用される時点を定義するため、新しいファイル <code>timeframes.txt</code> を追加します</li>
            <li>乗車区間(leg)の開始または終了が指定された時間枠内にある場合にのみ運賃区間ルールが適用されることを指定するため、<code>fare_leg_rules.txt</code> を <code>from_timeframe_id</code> および <code>to_timeframe_id</code> で拡張します</li>
        </ul>
    </div>
</div>

<div class="row">
    <div class="leftcontainer">
        <h3 class="title"><a href="https://github.com/google/transit/pull/355" class="no-icon" target="_blank">チケットメディアを追加</a></h3>
        <p class="maintainer"><a href="https://github.com/isabelle-dr" class="no-icon" target="_blank">isabelle-dr</a> による #355 は、2023年3月14日にマージされました</p>
    </div>
    <div class="featurelist">
        <ul>
            <li>チケットメディアは、<a href="../../../../community/extensions/fares-v2">GTFS Fares-v2 extension proposal</a>における主要な要素です</li>
            <li>これは、乗客が乗車を認証するために使用できるものを表します（例: 交通系カード、モバイルアプリ、または非接触型銀行カードを使用したタッチ決済）</li>
            <li>チケット商品は、特定のチケットメディアに関連付けることができます（例: 月間パスは交通系カードでのみ利用可能です）</li>
            <li>チケット商品の価格は、チケットメディアに基づいて定義できます（例: モバイルアプリ経由で購入した場合、チケットはより安価です）</li>
        </ul>
    </div>
</div>

<div class="row"></div>
