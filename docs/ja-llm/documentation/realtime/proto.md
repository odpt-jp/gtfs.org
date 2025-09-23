# GTFS Realtime Protobuf {: #gtfs-realtime-protobuf}

[gtfs-realtime.proto](gtfs-realtime.proto) ファイルをダウンロードし、それを使用して GTFS-realtime フィードをコンパイルしてください。ファイルの内容は以下にインラインで示されています。  
protobuf の使用に関する詳細は、[Protocol Buffers Developer Guide](https://developers.google.com/protocol-buffers/docs/overview) を参照してください。

```protobuf
// Copyright 2015 The GTFS Specifications Authors.
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at
//
//     http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

// GTFS Realtime のプロトコル定義ファイル。
//
// GTFS Realtime により、交通事業者は利用者に対してリアルタイムの情報を提供することができます。
// 例えば、サービスの中断（駅の閉鎖、路線の運休、重大な遅延など）、車両の位置、予想到着時刻などです。
//
// このプロトコルは以下で公開されています:
// https://github.com/google/transit/tree/master/gtfs-realtime

syntax = "proto2";
option java_package = "com.google.transit.realtime";
package transit_realtime;

// フィードメッセージの内容。
// フィードはフィードメッセージの連続したストリームです。ストリーム内の各メッセージは、適切な HTTP GET リクエストに対するレスポンスとして取得されます。
// リアルタイムフィードは常に既存の GTFS フィードに関連付けられて定義されます。
// すべての entity id は GTFS フィードに基づいて解決されます。
// このファイルに記載されている "required" および "optional" は Protocol Buffer の多重度(cardinality) を指し、意味的な多重度を指すものではありません。
// フィールドの意味的な多重度については reference.md を参照してください。
// https://github.com/google/transit/tree/master/gtfs-realtime
message FeedMessage {
  // このフィードおよびフィードメッセージに関するメタデータ。
  required FeedHeader header = 1;

  // フィードの内容。
  repeated FeedEntity entity = 2;

  // extensions 名前空間は、サードパーティ開発者が GTFS Realtime 仕様を拡張し、新機能や仕様の変更を追加・評価することを可能にします。
  extensions 1000 to 1999;

  // 以下の extension ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// フィードに関するメタデータ。フィードメッセージに含まれます。
message FeedHeader {
  // フィード仕様のバージョン。
  // 現在のバージョンは 2.0 です。有効なバージョンは "2.0", "1.0" です。
  required string gtfs_realtime_version = 1;

  // 現在の取得が増分的かどうかを決定します。現在、DIFFERENTIAL モードはサポートされておらず、このモードを使用するフィードの動作は未定義です。
  // DIFFERENTIAL モードの動作を完全に定義するための議論が GTFS Realtime メーリングリストで行われており、それが確定した際にドキュメントが更新されます。
  enum Incrementality {
    FULL_DATASET = 0;
    DIFFERENTIAL = 1;
  }
  optional Incrementality incrementality = 2 [default = FULL_DATASET];

  // このフィードの内容が作成された時刻（サーバー時間）を識別するタイムスタンプ。
  // POSIX 時間（1970年1月1日 00:00:00 UTC からの秒数）。
  optional uint64 timestamp = 3;

  // リアルタイムデータの基となる GTFS フィードの feed_info.feed_version に一致する文字列。
  // 利用者はこれを使用して、どの GTFS フィードが現在有効であるか、または新しいものがダウンロード可能かを識別できます。
  optional string feed_version = 4;

  // extensions 名前空間は、サードパーティ開発者が GTFS Realtime 仕様を拡張し、新機能や仕様の変更を追加・評価することを可能にします。
  extensions 1000 to 1999;

  // 以下の extension ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 交通フィード内のエンティティの定義（または更新）。
message FeedEntity {
  // id は増分サポートを提供するためにのみ使用されます。id は FeedMessage 内で一意である必要があります。
  // 連続する FeedMessage には同じ id を持つ FeedEntity が含まれる場合があります。
  // DIFFERENTIAL 更新の場合、新しい FeedEntity は同じ id を持つ古い FeedEntity を置き換えます（または削除します - 下記 is_deleted を参照）。
  // フィードで参照される実際の GTFS エンティティ（駅、ルート、便など）は、明示的なセレクタで指定されなければなりません（詳細は下記 EntitySelector を参照）。
  required string id = 1;

  // このエンティティが削除されるかどうか。増分取得にのみ関連します。
  optional bool is_deleted = 2 [default = false];

  // エンティティ自体に関するデータ。以下のフィールドのうち正確に1つが存在しなければなりません（エンティティが削除される場合を除く）。
  optional TripUpdate trip_update = 3;
  optional VehiclePosition vehicle = 4;
  optional Alert alert = 5;

  // 注意: このフィールドはまだ実験的であり、変更される可能性があります。将来的に正式に採用される可能性があります。
  optional Shape shape = 6;
  optional Stop stop = 7;
  optional TripModifications trip_modifications = 8;

  // extensions 名前空間は、サードパーティ開発者が GTFS Realtime 仕様を拡張し、新機能や仕様の変更を追加・評価することを可能にします。
  extensions 1000 to 1999;

  // 以下の extension ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}
```

（以下、protobuf のコード部分は翻訳不要のため省略）
