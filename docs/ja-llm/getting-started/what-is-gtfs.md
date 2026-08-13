# GTFS: 公共交通データを誰もが利用できるようにする {: #gtfs-making-public-transit-data-universally-accessible}

## 交通機関の乗客情報のためのオープンデータ標準 {: #an-open-data-standard-for-transit-passenger-information}


GTFS としても知られる General Transit Feed Specification は、公共交通事業者が時刻表、停留所等(stop)、運賃などのサービス詳細を記述するための構造を提供する、標準化されたデータ形式です。

これにより、公共交通事業者は、最も一般的には経路検索アプリケーションをはじめとする、多種多様なソフトウェアアプリケーションで利用できる形式で交通データを公開できます。これは、ユーザーがスマートフォンまたは類似のデバイスを使用して、公共交通サービスを利用するための移動情報を容易に取得できることを意味します。

<img class="center" width="560" height="100%" src="../../../assets/what-is-gtfs-001.png">

現在、GTFS は世界中の数千の公共交通事業者にとって主要な [Open Standard](https://www.interoperablemobility.org/definitions/#open_standard) です。一部の事業者はこのデータを自ら作成していますが、他の事業者はデータの作成および維持をベンダーに委託しています。

## 静的データと動的データのサポート {: #support-for-static-and-dynamic-data}


GTFS は、[GTFS Schedule](../../documentation/schedule/reference) と [GTFS Realtime](../../documentation/realtime/reference) の2つの主要な部分で構成されています。

<img class="center" width="560" height="100%" src="../../../assets/what-is-gtfs-002.png">

GTFS Schedule には、ルート・路線系統(route)、時刻表、運賃、地理的な交通機関の詳細など、多くの機能に関する情報が含まれており、単純なテキストファイル[^1]で提供されます。この簡潔な形式により、複雑なソフトウェアやプロプライエタリなソフトウェアに依存することなく、容易に作成および保守できます。

GTFS Realtime には、[Protocol Buffers](https://developers.google.com/protocol-buffers/) 形式を使用した、便の更新(trip update)、車両位置情報(vehicle position)、および運行情報(alert)が含まれています。GTFS のこの部分は GTFS Schedule と連携して機能し、運行の中断や更新された到着時刻を乗客に通知します。

GTFS Schedule および GTFS Realtime のリファレンスドキュメントは、[技術ドキュメントセクション](../../documentation/overview)で利用できます。

<iframe class="center" width="560" height="315" src="https://www.youtube-nocookie.com/embed/SDz2460AjNo?si=wFsaN4_Hr3ypxWdp" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<a href="https://www.flaticon.com/authors/freepik" title="Icons by Freepik">Freepik によるアイコン - Flaticon</a>

[^1]: テキストファイルに加えて、デマンド型サービスの特定の要素を表現するために、GTFS では [GeoJSON](https://geojson.org/) 形式もサポートされています。
