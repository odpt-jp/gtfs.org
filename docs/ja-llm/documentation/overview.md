# General Transit Feed Specification (GTFS) {: #general-transit-feed-specification-gtfs}


General Transit Feed Specification (GTFS) は、乗客に交通システムに関する関連情報を配信するために使用される [Open Standard](https://www.interoperablemobility.org/definitions/#open_standard) です。これにより、公共交通事業者は、多種多様なソフトウェアアプリケーションで利用できる形式で交通データを公開することができます。

GTFS は、[GTFS Schedule](../schedule/reference) と [GTFS Realtime](../realtime/reference) の2つの主要な部分で構成されています。

## [GTFS Schedule](../schedule/reference) {: #gtfs-schedule}


GTFS Schedule は、静的な公共交通情報の共通形式を定義するフィード仕様です。これは、単一の ZIP ファイルに格納された、主にテキストファイル（.txt）からなる単純なファイル群で構成されます。 

各ファイルは、停留所等(stop)、ルート・路線系統(route)、便(trip)など、交通情報の特定の側面を記述します。最も基本的な形式では、GTFS Schedule データセットは `agency.txt`、`routes.txt`、`trips.txt`、`stops.txt`、`stop_times.txt`、`calendar.txt`、`calendar_dates.txt` の 7 つのファイルで構成されます。

この基本的なファイルセットに加えて、運賃、翻訳、乗換、駅構内の構内通路(pathway)など、その他のサービス要素に関する情報を提供するために、追加の（任意の）ファイルを含めることもできます。現在、GTFS の基本要素を拡張する任意のファイルは 15 を超えており、その中には、地理的エリアを表現するために使用できる、テキストファイル（.txt）以外の新しい形式を導入した locations.geojson も含まれます。 

すべての GTFS Schedule ファイルの正本は、公式の [GTFS Schedule Reference](../schedule/reference) です。これは、GTFS Schedule データセットを構成する各ファイル内のすべての情報要素に関する要件の詳細情報を提供します。

## [GTFS Realtime](../realtime/reference) {: #gtfs-realtime}


GTFS Realtime は、公共交通事業者が現在の到着時刻および出発時刻、運行情報、車両位置情報に関する最新情報を提供できるようにするフィード仕様であり、ユーザーが円滑に旅程を計画できるようにします。

この仕様は現在、以下の種類の情報をサポートしています。

- 便の更新(trip update) - 遅延、運休、変更されたルート・路線系統(route)
- 運行情報(alert) - 移設された停留所等(stop)、駅、ルート・路線系統(route)、またはネットワーク全体に影響する予期しない事象
- 車両位置情報(vehicle position) - 位置および混雑度を含む車両に関する情報

これらの詳細については、[Feed Entities](../realtime/feed-entities/overview) セクションを参照してください。

GTFS Realtime は、実装の容易さ、GTFS との優れた相互運用性、および乗客向け情報への注力を中心に設計されました。これは、[initial Live Transit Updates](https://developers.google.com/transit/google-transit#LiveTransitUpdates) のパートナー事業者、複数の交通開発者、および Google との協力により実現しました。この仕様は、[Apache 2.0 License](http://www.apache.org/licenses/LICENSE-2.0.html) の下で公開されています。

GTFS Realtime のデータ交換形式は、構造化データをシリアル化するための言語およびプラットフォームに依存しない仕組みである [Protocol Buffers](https://developers.google.com/protocol-buffers/) に基づいています（XML をより小さく、高速かつ簡潔にしたものと考えてください）。

GTFS Schedule と同様に、[GTFS Realtime Reference](../realtime/reference) は、あらゆる GTFS Realtime フィードに対する規則および要件を定める信頼できる情報源であり、[gtfs-realtime.proto](../realtime/proto) ファイルは、使用される要素の階層およびその型定義を定義します。
