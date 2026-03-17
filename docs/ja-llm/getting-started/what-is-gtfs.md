# GTFS: 公共交通データを普遍的にアクセス可能にする {: #gtfs-making-public-transit-data-universally-accessible}

## 交通利用者情報のためのオープンデータ標準 {: #an-open-data-standard-for-transit-passenger-information}

GTFS（General Transit Feed Specification）は、公共交通事業者が運行スケジュール、停留所、運賃などのサービスの詳細を記述するための構造を提供する標準化されたデータ形式です。

これにより、公共交通事業者は自らの交通データを、さまざまなソフトウェアアプリケーション（最も一般的には経路検索アプリ）で利用可能な形式で公開することができます。つまり、利用者はスマートフォンなどのデバイスを使って、公共交通サービスを利用するための経路情報を簡単に取得できるということです。

<img class="center" width="560" height="100%" src="../../../assets/what-is-gtfs-001.png">

現在、GTFSは世界中の数千の公共交通事業者にとって、標準的な[オープンスタンダード](https://www.interoperablemobility.org/definitions/#open_standard)となっています。一部の事業者は自らこのデータを作成していますが、他の事業者はベンダーに依頼してデータの作成と維持管理を行っています。

## 静的データと動的データのサポート {: #support-for-static-and-dynamic-data}


GTFS は、主に 2 つの部分から構成されています：[GTFS Schedule](../../documentation/schedule/reference) と [GTFS Realtime](../../documentation/realtime/reference) です。

<img class="center" width="560" height="100%" src="../../../assets/what-is-gtfs-002.png">

GTFS Schedule には、ルート・路線系統、時刻表、運賃、地理的な交通情報など、さまざまな情報が含まれており、シンプルなテキストファイル[^1]で提供されます。このシンプルな形式により、複雑または独自のソフトウェアに依存することなく、容易に作成および保守を行うことができます。

GTFS Realtime には、便の更新(trip updates)、車両位置情報(vehicle positions)、運行情報(alerts) が含まれており、[Protocol Buffers](https://developers.google.com/protocol-buffers/) 形式を使用しています。GTFS のこの部分は、GTFS Schedule と連携して動作し、乗客に運行の中断や到着時刻の更新を知らせる役割を果たします。

GTFS Schedule および GTFS Realtime のリファレンスドキュメントは、[技術ドキュメントセクション](../../documentation/overview)で参照することができます。

<iframe class="center" width="560" height="315" src="https://www.youtube-nocookie.com/embed/SDz2460AjNo?si=wFsaN4_Hr3ypxWdp" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<a href="https://www.flaticon.com/authors/freepik" title="Icons by Freepik">Freepik によるアイコン - Flaticon</a>

[^1]: テキストファイルに加えて、デマンド型サービスの特定の要素を表現するために、[GeoJSON](https://geojson.org/) 形式も GTFS でサポートされるようになりました。
