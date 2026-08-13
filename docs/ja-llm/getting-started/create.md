# GTFS データセットの作成 {: #creating-a-gtfs-dataset}

## GTFSフィードの概要 {: #overview-of-a-gtfs-feed}

すべてのGTFSフィードは、.txtファイル拡張子で保存された一連のCSVファイルであるGTFS Reference形式のデータセットから始まります[^1]。最も基本的な実装では、GTFSデータセットは通常、7つの基本ファイルから始まり、これらを1つの.zipファイルにまとめて、安定した公開URLでホストします。これがGTFSフィードです。

<img class="center" width="560" height="100%" src="../../../assets/create_001.png">

各ファイルは、複数の情報フィールドを持つ複数のレコード（データ行）のリストで構成されます。たとえば、[routes.txt](../../documentation/schedule/reference/#routestxt)に記載される各行は公共交通のルート・路線系統(route)を表し、そのフィールドは名称、説明、運行事業者など、そのルート・路線系統(route)の複数の要素を記述します。

<img class="center" width="560" height="100%" src="../../../assets/create_002.png">

GTFSデータセットの基本ファイルは、次のように説明できます。GTFS Scheduleデータセットには1つ以上のルート・路線系統(route)（[routes.txt](../../documentation/schedule/reference/#routestxt)）があり、各ルート・路線系統(route)には1つ以上の便(trip)（[trips.txt](../../documentation/schedule/reference/#tripstxt)）があります。各便(trip)は、指定された時刻（[stop_times.txt](../../documentation/schedule/reference/#stop_timestxt)）に一連の停留所等(stop)（[stops.txt](../../documentation/schedule/reference/#stopstxt)）を訪れます。便(trip)および停車時刻(stop_time)には時刻情報のみが含まれます。カレンダーは、特定の便(trip)がどの日に運行されるかを決定するために使用されます（[calendar.txt](../../documentation/schedule/reference/#calendartxt)および[calendar_dates.txt](../../documentation/schedule/reference/#calendar_datestxt)）。さらに、複数の事業者（[agency.txt](../../documentation/schedule/reference/#agencytxt)）が複数のルート・路線系統(route)を運行することができます。これらのファイルは、それらの間で相互参照されるフィールドによってリンクされています。

<img class="center" width="560" height="100%" src="../../../assets/create_003.png">

基本的なGTFSデータセットを作成するためにこれらのファイルを設定した後、交通事業者とベンダー間のその他の機能または特定のニーズを実現するために、追加の（任意の）ファイルを追加できます。これらのファイルの例には、次のものがあります。 

- 便(trip)の経路をグラフィカルに表現できる[shapes.txt](../../documentation/schedule/reference/#shapestxt)、
- ユーザーが駅構内を移動する際に役立つ案内を生成できる情報を提供する[pathways.txt](../../documentation/schedule/reference/#pathwaystxt)、
- 停車時刻(stop_time)を指定する代替方法を提供する[frequencies.txt](../../documentation/schedule/reference/#frequenciestxt)。

有効にできるすべてのGTFS機能の詳細については、[「GTFSでできること」](../features/overview/)セクションを参照してください。 

GTFS Scheduleデータセットは、車両位置情報(vehicle position)や運行情報のようなリアルタイム情報で補完できます。これを行うには、既存のGTFS Scheduleデータセットとは別にGTFS Realtimeフィードを作成する必要があります。 

GTFS Realtimeフィードは、HTTP経由で提供され、頻繁に更新される通常のバイナリファイルで構成されます。あらゆる種類のwebserverがこのファイルをホストおよび提供できます。GTFS Realtimeデータ交換形式は、構造化データをシリアライズするための言語およびプラットフォームに依存しない仕組みである[Protocol Buffers](https://developers.google.com/protocol-buffers/)に基づいています。GTFS Realtimeは、便の更新(trip update)、運行情報(alert)、車両位置情報(vehicle position)の3種類の情報を提供できます。これらは、伝達する必要があるサービス情報に応じて組み合わせることができます。

GTFS Realtimeでは車両群の実際の状態を提示できるため、フィードは定期的に更新する必要があります。できれば、サービスのAutomatic Vehicle Location systemから新しいデータが届くたびに更新するべきです。GTFS ScheduleデータセットとGTFS Realtimeフィードを組み合わせることで、利用アプリケーションは乗客に正確かつ最新の情報を提供できます。詳細については、Technical Documentationを参照してください。

## 初めてのGTFSフィードを作成しますか？ {: #producing-your-first-gtfs-feed}


初めてのGTFSフィードを作成しようとしている事業者は、まず既存のドキュメントを読む必要があります。

まず、["GTFSで何ができますか？"](../features/overview) セクションでGTFSの機能を確認し、GTFS形式を使用して表現したい交通サービスのさまざまな機能を特定してください。より詳細に確認するには、[GTFS Schedule](../../documentation/schedule/reference) および [GTFS Realtime](../../documentation/realtime/reference) の公式リファレンスドキュメントで、これらの機能のモデリングおよび準拠の確保に関する詳細なガイダンスを提供しています。

次に、システムから必要なすべてのデータを収集してください。これには、すべての停留所等(stop)、ルート・路線系統(route)、時刻表、運賃などに関する情報が含まれます。これらの詳細の多くは、GTFSデータセットに入力されるデータとなります。

システムの規模と複雑さに応じて、データを社内で作成するか、外部のGTFSベンダーに依頼してデータをGTFS形式に変換することができます。 

一部のケースでは、少数のルート・路線系統(route)を持つ小規模な事業者が、スプレッドシートやテキストエディタなどの一般的に利用可能なソフトウェアを使用して、自らデータを作成しています。 

より大規模なシステムを扱う場合、ほとんどの事業者は専門ベンダーから専用のGTFS管理ソフトウェアを導入しますが、独自の社内ツールを開発することを選択する場合もあります。最後に、システムの特性により事業者が自らデータセットを作成することが困難である場合、GTFSデータの作成は、GTFSデータの作成を専門とする企業に完全に委託することができます。

<a href="https://www.flaticon.com/authors/freepik" title="Icons by Freepik">Freepikによって作成されたアイコン - Flaticon</a>

[^1]: テキストファイルに加えて、現在ではデマンド型サービスの特定の要素を表現するために、[GeoJSON](https://geojson.org/)形式もGTFSでサポートされています。
