# GTFS Realtime Protobuf {: #gtfs-realtime-protobuf}

[gtfs-realtime.proto](gtfs-realtime.proto) ファイルをダウンロードし、それを使用して GTFS-realtime フィードをコンパイルしてください。ファイルの内容を以下にインラインで示します。
protobufs の使用に関する詳細は、[Protocol Buffers Developer Guide](https://developers.google.com/protocol-buffers/docs/overview) を参照してください。
```protobuf
// Copyright 2015 The GTFS Specifications Authors.
//
// Apache License, Version 2.0（以下「License」）に基づいてライセンスされています。
// このファイルは、License に準拠する場合を除き、使用してはいけません。
// License のコピーは以下から取得できます。
//
//     http://www.apache.org/licenses/LICENSE-2.0
//
// 適用法で要求される場合または書面で合意された場合を除き、
// License に基づいて配布されるソフトウェアは「現状有姿」で配布され、
// 明示的または黙示的を問わず、いかなる保証または条件もありません。
// License に基づく特定の言語を規定する許可および
// 制限については、License を参照してください。

// GTFS Realtime のプロトコル定義ファイル。
//
// GTFS Realtime により、交通事業者は利用者に対して、サービスの障害（駅の閉鎖、路線の
// 運休、重大な遅延など）、車両の位置、および予想到着時刻に関する
// リアルタイム情報を提供できます。
//
// このプロトコルは以下で公開されています。
// https://github.com/google/transit/tree/master/gtfs-realtime

syntax = "proto2";
option java_package = "com.google.transit.realtime";
package transit_realtime;

// フィードメッセージの内容。
// フィードは、フィードメッセージの連続ストリームです。ストリーム内の各メッセージは、
// 適切な HTTP GET リクエストへの応答として取得されます。
// リアルタイムフィードは、常に既存の GTFS フィードとの関係で定義されます。
// すべてのエンティティ ID は、GTFS フィードに対して解決されます。
// このファイルに記載されている「required」および「optional」は、意味論上の多重度ではなく、
// Protocol Buffer の多重度を指すことに注意してください。フィールドの
// 意味論上の多重度については、以下の reference.md を参照してください。
// https://github.com/google/transit/tree/master/gtfs-realtime
message FeedMessage {
  // このフィードおよびフィードメッセージに関するメタデータ。
  required FeedHeader header = 1;

  // フィードの内容。
  repeated FeedEntity entity = 2;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime 仕様を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// フィードに関するメタデータ。フィードメッセージに含まれます。
message FeedHeader {
  // フィード仕様のバージョン。
  // 現在のバージョンは 2.0 です。有効なバージョンは「2.0」、「1.0」です。
  required string gtfs_realtime_version = 1;

  // 現在の取得が増分であるかどうかを決定します。現在、
  // DIFFERENTIAL モードはサポートされておらず、このモードを使用するフィードの
  // 動作は未定義です。DIFFERENTIAL モードの動作を完全に規定することについて、
  // GTFS Realtime メーリングリストで議論が行われています。これらの議論が
  // 最終決定された時点で、ドキュメントが更新されます。
  enum Incrementality {
    FULL_DATASET = 0;
    DIFFERENTIAL = 1;
  }
  optional Incrementality incrementality = 2 [default = FULL_DATASET];

  // このタイムスタンプは、このフィードの内容が作成された時点（サーバー時刻）を
  // 識別します。POSIX 時刻（すなわち、1970 年 1 月 1 日 00:00:00 UTC からの
  // 秒数）です。
  optional uint64 timestamp = 3;

  // リアルタイムデータの基となる GTFS フィードの feed_info.feed_version と一致する
  // 文字列です。利用者はこれを使用して、現在アクティブな GTFS フィード、または
  // ダウンロード可能な新しいフィードがいつ利用可能かを識別できます。
  optional string feed_version = 4;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime 仕様を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 交通フィード内のエンティティの定義（または更新）。
message FeedEntity {
  // ID は増分サポートを提供するためにのみ使用されます。ID は FeedMessage 内で
  // 一意であるべきです。後続の FeedMessage には、同じ ID を持つ
  // FeedEntity が含まれることがあります。DIFFERENTIAL 更新の場合、ある ID を持つ
  // 新しい FeedEntity は、同じ ID を持つ古い FeedEntity を置き換えます
  // （または削除します。以下の is_deleted を参照してください）。
  // フィードが参照する実際の GTFS エンティティ（例: 駅、ルート・路線系統(route)、便(trip)）は、
  // 明示的なセレクタで指定しなければなりません（詳細は以下の EntitySelector を
  // 参照してください）。
  required string id = 1;

  // このエンティティを削除するかどうか。増分取得にのみ関連します。
  optional bool is_deleted = 2 [default = false];

  // エンティティ自体に関するデータ。以下のフィールドのうち、ちょうど 1つが
  // 存在しなければなりません（エンティティが削除される場合を除きます）。
  optional TripUpdate trip_update = 3;
  optional VehiclePosition vehicle = 4;
  optional Alert alert = 5;

  // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  optional Shape shape = 6;
  optional Stop stop = 7;
  optional TripModifications trip_modifications = 8;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

//
// フィードで使用されるエンティティ。
//

// 便(trip) に沿った車両の進行状況のリアルタイム更新。
// ScheduleRelationship の値に応じて、TripUpdate は以下を指定できます。
// - 時刻表に従って運行する便(trip)。
// - ルート・路線系統(route)に沿って運行するが、固定時刻表を持たない便(trip)。
// - 時刻表に対して追加または削除された便(trip)。
//
// 更新は、将来の予測到着・出発イベント、またはすでに発生した過去のイベントに
// 対するものにできます。
// 通常、イベントが現在時刻に近づくにつれて、更新はより正確かつ確実になるべきです
// （以下の uncertainty を参照してください）。
// それが不可能な場合でも、過去のイベントに関する情報は正確かつ確実であるべきです。
// 特に、更新が過去の時刻を指しているにもかかわらず、その更新の uncertainty が 0 でない場合、
// クライアントは、その更新が（誤った）予測であり、便(trip) がまだ完了していないと
// 結論付けるべきです。
//
// 更新は、すでに完了した便(trip)を記述できることに注意してください。
// このためには、便(trip)の最後の停留所等(stop)に対する更新を提供するだけで十分です。
// その時刻が過去である場合、クライアントは便(trip)全体が過去のものであると結論付けます
// （先行する停留所等(stop)に対する更新も提供できますが、影響はありません）。
// このオプションは、予定より早く完了した便(trip)に最も関連しますが、時刻表によれば、
// その便(trip)は現在時刻においてまだ運行中です。この便(trip)の更新を削除すると、
// クライアントが便(trip)はまだ運行中であると想定する可能性があります。
// フィード提供者は過去の更新を削除することが許可されていますが、必須ではないことに
// 注意してください。これは、その操作が実用的に有用となるケースの 1つです。
message TripUpdate {
  // このメッセージが適用される便(trip)。実際の便(trip)インスタンスごとに、
  // TripUpdate エンティティは最大 1つまで存在できます。
  // 存在しない場合、予測情報が利用できないことを意味します。
  // これは、便(trip)が時刻表どおりに進行していることを意味するものではありません。
  required TripDescriptor trip = 1;

  // この便(trip)を運行する車両に関する追加情報。
  optional VehicleDescriptor vehicle = 3;

  // 単一の予測イベント（到着または出発）の時刻情報。
  // 時刻情報は、遅延および/または推定時刻と、不確実性で構成されます。
  // - 予測が GTFS 内の既存の時刻表に対して相対的に与えられる場合は、delay を使用するべきです。
  // - 予測時刻表があるかどうかにかかわらず、time を指定するべきです。
  //   time と delay の両方が指定されている場合、time が優先されます
  //   （ただし通常、時刻表に基づく便(trip)に対して time が指定される場合は、
  //   GTFS の予定時刻 + delay と等しくなるべきです）。
  //
  // uncertainty は time と delay の両方に等しく適用されます。
  // uncertainty は、実際の遅延における予想誤差をおおよそ指定します（ただし、
  // その正確な統計的意味はまだ定義されていないことに注意してください）。例えば、
  // コンピューターによる時刻制御下で運行される列車では、uncertainty を 0 にできます。
  message StopTimeEvent {
    // 遅延（秒単位）は正（車両が遅れていることを意味します）または
    // 負（車両が予定より早いことを意味します）にできます。遅延が 0 の場合、
    // 車両は正確に定時です。
    optional int32 delay = 1;

    // 絶対時刻としてのイベント。
    // Unix 時刻（すなわち、1970 年 1 月 1 日 00:00:00 UTC からの秒数）です。
    optional int64 time = 2;

    // uncertainty が省略された場合、不明として解釈されます。
    // 予測が不明または不確実性が高すぎる場合、delay（または time）フィールドは
    // 空にするべきです。その場合、uncertainty フィールドは無視されます。
    // 完全に確実な予測を指定するには、その uncertainty を 0 に設定してください。
    optional int32 uncertainty = 3;

    // NEW、REPLACEMENT、または DUPLICATED の便(trip)の予定時刻。
    // Unix 時刻（すなわち、1970 年 1 月 1 日 00:00:00 UTC からの秒数）です。
    // TripUpdate.schedule_relationship が NEW、REPLACEMENT、または DUPLICATED の場合は任意、それ以外では禁止です。
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    optional int64 scheduled_time = 4;

    // extensions 名前空間により、サードパーティ開発者は新機能および
    // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
    extensions 1000 to 1999;

    // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
    extensions 9000 to 9999;
  }

  // 便(trip)上の特定の停留所等(stop)における到着および/または出発イベントのリアルタイム更新。
  // 更新は過去および将来のイベントの両方に対して提供できます。
  // 作成者は過去のイベントを削除することが許可されていますが、必須ではありません。
  message StopTimeUpdate {
    // 更新は stop_sequence または stop_id を通じて特定の停留所等(stop)にリンクされるため、
    // 以下のフィールドのいずれか 1つを必ず設定しなければなりません。
    // 詳細については、TripDescriptor のドキュメントを参照してください。

    // 対応する GTFS フィードの stop_times.txt と同じでなければなりません。
    optional uint32 stop_sequence = 1;
    // 対応する GTFS フィードの stops.txt と同じでなければなりません。
    optional string stop_id = 4;

    optional StopTimeEvent arrival = 2;
    optional StopTimeEvent departure = 3;

    // 指定された停留所等(stop)から出発した後の予想混雑状況。
    // 将来の停留所等(stop)に対してのみ提供するべきです。
    // arrival または departure StopTimeEvent のいずれもなしに departure_occupancy_status を
    // 提供するには、ScheduleRelationship を NO_DATA に設定するべきです。 
    optional VehiclePosition.OccupancyStatus departure_occupancy_status = 7;

    // StopTimeEvent と静的時刻表との関係。
    enum ScheduleRelationship {
      // 車両は、時刻表の時刻どおりであるとは限りませんが、静的時刻表における
      // 停留所等(stop)の予定に従って運行しています。
      // arrival と departure の少なくとも一方を提供しなければなりません。この停留所等(stop)の
      // 時刻表に到着時刻と出発時刻の両方が含まれる場合、この更新にも両方を含めなければなりません。
      // 頻度ベースの便(trip)（exact_times = 0 の GTFS frequencies.txt）には SCHEDULED 値を
      // 使用するべきではなく、代わりに UNSCHEDULED を使用するべきです。
      SCHEDULED = 0;

      // 停留所等(stop)は通過されます。すなわち、車両はこの停留所等(stop)に停車しません。
      // arrival および departure は任意です。
      SKIPPED = 1;

      // この停留所等(stop)には StopTimeEvent が指定されません。
      // この値の主な目的は、便(trip)の一部に対してのみ時刻予測を提供することです。すなわち、
      // 便(trip)の最後の更新に NO_DATA 指定子がある場合、便(trip)内の残りの停留所等(stop)に対する
      // StopTimeEvent も未指定と見なされます。
      // arrival も departure も提供するべきではありません。
      NO_DATA = 2;

      // 車両は、exact_times = 0 の GTFS frequencies.txt で定義された便(trip)を運行しています。
      // この値は、GTFS frequencies.txt で定義されていない便(trip)、または exact_times = 1 の
      // GTFS frequencies.txt 内の便(trip)には使用するべきではありません。ScheduleRelationship=UNSCHEDULED の
      // StopTimeUpdate を含む便(trip)では、TripDescriptor.ScheduleRelationship=UNSCHEDULED も設定しなければなりません。
      // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、
      // 正式に採用される場合があります。
      UNSCHEDULED = 3;
    }
    optional ScheduleRelationship schedule_relationship = 5
    [default = SCHEDULED];

    // 停車時刻(stop_time)の更新値を提供します。
    // 注: このメッセージはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    message StopTimeProperties {
      // リアルタイムの停留所等(stop)割り当てをサポートします。GTFS stops.txt で定義された stop_id を参照します。
      // 新しい assigned_stop_id によって、GTFS stop_times.txt で定義された stop_id と比較して、エンドユーザーの
      // 便(trip)体験が大幅に異なる結果となるべきではありません。言い換えると、追加のコンテキストなしに
      // アプリ内で新しい停留所等(stop)が提示された場合、エンドユーザーはこの新しい stop_id を
      // 「異常な変更」と見なすべきではありません。
      // 例えば、このフィールドは、GTFS stop_times.txt で元々定義された停留所等(stop)と同じ駅に属する stop_id を使用して、
      // プラットフォーム割り当てに使用することを意図しています。
      // リアルタイムの到着または出発予測を提供せずに停留所等(stop)を割り当てるには、このフィールドを設定し、
      // StopTimeUpdate.schedule_relationship = NO_DATA を設定してください。
      // このフィールドが設定される場合、`StopTimeUpdate.stop_id` を省略し、`StopTimeUpdate.stop_sequence` のみを使用することが推奨されます。`StopTimeProperties.assigned_stop_id` と `StopTimeUpdate.stop_id` が設定される場合、`StopTimeUpdate.stop_id` は `assigned_stop_id` と一致しなければなりません。
      // プラットフォーム割り当ては、他の GTFS-realtime フィールドにも反映するべきです
      // （例: `VehiclePosition.stop_id`）。
      // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
      optional string assigned_stop_id = 1;

      // 停留所等(stop)における車両の更新された行先表示(headsign)。
      // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
      optional string stop_headsign = 2;
      
      enum DropOffPickupType {
        // 通常どおり予定された乗車/降車。
        REGULAR = 0;

        // 乗車/降車は利用できません。
        NONE = 1;

        // 乗車/降車を手配するには事業者に電話しなければなりません。
        PHONE_AGENCY = 2;

        // 乗車/降車を手配するには運転手と調整しなければなりません。
        COORDINATE_WITH_DRIVER = 3;
      }
      
      // 停留所等(stop)における車両の更新された乗車。
      // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
      optional DropOffPickupType pickup_type = 3;

      // 停留所等(stop)における車両の更新された降車。
      // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
      optional DropOffPickupType drop_off_type = 4;

      // extensions 名前空間により、サードパーティ開発者は新機能および
      // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
      extensions 1000 to 1999;

      // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
      extensions 9000 to 9999;
    }

    // GTFS stop_times.txt 内で定義された特定のプロパティに対するリアルタイム更新
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    optional StopTimeProperties stop_time_properties = 6;

    // extensions 名前空間により、サードパーティ開発者は新機能および
    // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
    extensions 1000 to 1999;

    // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
    extensions 9000 to 9999;
  }

  // 便(trip)の StopTimes に対する更新（将来のもの、すなわち予測、および場合によっては
  // 過去のもの、すなわちすでに発生したもの）。
  // 更新は stop_sequence でソートしなければならず、次に指定された停留所等(stop)までの
  // 便(trip)の後続するすべての停留所等(stop)に適用されます。
  //
  // 例 1:
  // 20 の停留所等(stop)を持つ便(trip)において、現在の停留所等(stop)の stop_sequence に対する
  // arrival delay および departure delay が 0 の StopTimeUpdate は、便(trip)が
  // 正確に定時であることを意味します。
  //
  // 例 2:
  // 同じ便(trip)インスタンスに対して、3つの StopTimeUpdate が提供されます。
  // - stop_sequence 3 に対する 5 分の遅延
  // - stop_sequence 8 に対する 1 分の遅延
  // - stop_sequence 10 に対する未指定の期間の遅延
  // これは以下のように解釈されます。
  // - stop_sequence 3、4、5、6、7 は 5 分の遅延があります。
  // - stop_sequence 8、9 は 1 分の遅延があります。
  // - stop_sequence 10、... は遅延が不明です。
  repeated StopTimeUpdate stop_time_update = 2;

  // 将来の StopTimes を推定するために、車両のリアルタイム進行状況が最後に測定された
  // 最新の時点。過去の StopTimes が提供される場合、到着/出発時刻はこの値より
  // 前であることがあります。POSIX 時刻（すなわち、1970 年 1 月 1 日 00:00:00 UTC からの
  // 秒数）です。
  optional uint64 timestamp = 4;

  // 便(trip)の現在の時刻表からの偏差。予測が GTFS 内の既存の時刻表に対して
  // 相対的に与えられる場合にのみ、delay を指定するべきです。
  //
  // 遅延（秒単位）は正（車両が遅れていることを意味します）または
  // 負（車両が予定より早いことを意味します）にできます。遅延が 0 の場合、
  // 車両は正確に定時です。
  //
  // StopTimeUpdate の遅延情報は便(trip)レベルの遅延情報より優先されます。そのため、
  // 便(trip)レベルの遅延は、遅延値が指定された StopTimeUpdate を持つ便(trip)上の次の
  // 停留所等(stop)までのみ伝播されます。
  //
  // フィード提供者は、データの鮮度を評価するために、遅延値が最後に更新された時点を示す
  // TripUpdate.timestamp 値を提供することが強く推奨されます。
  //
  // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、
  // 正式に採用される場合があります。
  optional int32 delay = 5;

  // 迂回時の新しい shape_id など、便(trip)の更新されたプロパティを定義します。または、
  // DUPLICATED の便(trip)の trip_id、start_date、および start_time を定義します。 
  // 注: このメッセージはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  message TripProperties {
    // （CSV）GTFS trips.txt で定義された既存の便(trip)の複製である新しい便(trip)の識別子を定義します。
    // ただし、異なる運行日(service day)および/または時刻に開始します（TripProperties.start_date および
    // TripProperties.start_time フィールドを使用して定義されます）。（CSV）GTFS における trips.trip_id の定義を参照してください。その値は
    // （CSV）GTFS で使用されるものとは異ならなければなりません。schedule_relationship=DUPLICATED の場合は必須です。それ以外の場合、このフィールドは
    // 設定してはいけず、利用者によって無視されます。
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    optional string trip_id = 1;
    // DUPLICATED の便(trip)が運行される運行日(service day)。YYYYMMDD 形式です。
    // schedule_relationship=DUPLICATED の場合は必須です。それ以外の場合、このフィールドは設定してはいけず、利用者によって無視されます。
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    optional string start_date = 2;
    // 便(trip)が複製される場合の出発開始時刻を定義します。（CSV）GTFS における stop_times.departure_time の定義を参照してください。
    // 複製された便(trip)の予定到着時刻および出発時刻は、元の便(trip)の departure_time とこのフィールドとの
    // オフセットに基づいて計算されます。例えば、GTFS の便(trip)に departure_time が 10:00:00 の停留所等(stop) A と、
    // departure_time が 10:01:00 の停留所等(stop) B があり、このフィールドに 10:30:00 の値が設定されている場合、
    // 複製された便(trip)の停留所等(stop) B の予定 departure_time は 10:31:00 になります。リアルタイム予測の
    // delay 値は、この計算された予定時刻に適用され、予測時刻を決定します。例えば、
    // 停留所等(stop) B に対して 30 の出発遅延が提供される場合、予測出発時刻は 10:31:30 です。リアルタイム
    // 予測の time 値にはオフセットは適用されず、提供された予測時刻を示します。
    // 例えば、停留所等(stop) B に対して 10:31:30 を表す出発時刻が提供される場合、予測出発時刻は
    // 10:31:30 です。schedule_relationship が DUPLICATED の場合、このフィールドは必須です。それ以外の場合、このフィールドは
    // 設定してはいけず、利用者によって無視されます。
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    optional string start_time = 3;
    // 便(trip)のルート形状(shape)が（CSV）GTFS で指定されたルート形状(shape)と異なる場合、または乗客の需要に応じて異なる経路を取る車両など、
    // （CSV）GTFS で提供されない場合にリアルタイムで指定するため、車両走行経路のルート形状(shape)の識別子を指定します。（CSV）GTFS における trips.shape_id の定義を参照してください。
    // （CSV）GTFS にもリアルタイムにもルート形状(shape)が定義されていない場合、ルート形状(shape)は不明と見なされます。このフィールドは、shapes.txt の（CSV）GTFS で定義されたルート形状(shape)、または同じ（protobuf）リアルタイムフィード内の `Shape` を参照できます。 
    // この便(trip)の停留所等(stop)の順序（stop sequences）は、（CSV）GTFS と同じままでなければなりません。 
    // 同じリアルタイムフィード内の `Shape` エンティティを参照する場合、このフィールドの値はエンティティ内の `shape_id` の値であるべきであり、`FeedEntity` の `id` ではありません。
    // 迂回が発生した場合など、元の便(trip)の一部であるが今後停車しない停留所等(stop)は、schedule_relationship=SKIPPED としてマークするべきです。または、`TripModifications` メッセージを通じて詳細を提供できます。
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。 
    optional string shape_id = 4;

    // 元のものと異なる場合、この便(trip)の行先表示(headsign)を指定します。
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    optional string trip_headsign = 5;

    // 元のものと異なる場合、この便(trip)の名称を指定します。
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    optional string trip_short_name = 6;

    // extensions 名前空間により、サードパーティ開発者は新機能および
    // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
    extensions 1000 to 1999;

    // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
    extensions 9000 to 9999;
  }
  optional TripProperties trip_properties = 6;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 指定された車両のリアルタイム位置情報。
message VehiclePosition {
  // この車両が運行する便(trip)。
  // 車両を指定された便(trip)インスタンスと識別できない場合、空または部分的にできます。
  optional TripDescriptor trip = 1;

  // この便(trip)を運行する車両に関する追加情報。
  optional VehicleDescriptor vehicle = 8;

  // この車両の現在位置。
  optional Position position = 2;

  // 現在の停留所等(stop)の停留所順序インデックス。current_stop_sequence
  // （すなわち、それが参照する停留所等(stop)）の意味は、current_status によって決定されます。
  // current_status がない場合、IN_TRANSIT_TO が想定されます。
  optional uint32 current_stop_sequence = 3;
  // 現在の停留所等(stop)を識別します。値は対応する GTFS フィードの stops.txt 内の値と
  // 同じでなければなりません。
  optional string stop_id = 7;

  enum VehicleStopStatus {
    // 車両は停留所等(stop)にまもなく到着します（停留所等(stop)表示では、
    // 車両記号が通常点滅します）。
    INCOMING_AT = 0;

    // 車両は停留所等(stop)に停車しています。
    STOPPED_AT = 1;

    // 車両は出発し、次の停留所等(stop)へ移動中です。
    IN_TRANSIT_TO = 2;
  }
  // 現在の停留所等(stop)に対する車両の正確な状態。
  // current_stop_sequence がない場合は無視されます。
  optional VehicleStopStatus current_status = 4 [default = IN_TRANSIT_TO];

  // 車両位置が測定された時点。POSIX 時刻
  // （すなわち、1970 年 1 月 1 日 00:00:00 UTC からの秒数）です。
  optional uint64 timestamp = 5;

  // この車両に影響している混雑レベル。
  enum CongestionLevel {
    UNKNOWN_CONGESTION_LEVEL = 0;
    RUNNING_SMOOTHLY = 1;
    STOP_AND_GO = 2;
    CONGESTION = 3;
    SEVERE_CONGESTION = 4;  // 人々が車を降りています。
  }
  optional CongestionLevel congestion_level = 6;

  // 車両または車両編成の乗客混雑状態。
  // 個々の作成者はすべての OccupancyStatus 値を公開しない場合があります。したがって、利用者は
  // OccupancyStatus 値が線形スケールに従うと想定してはいけません。
  // 利用者は、作成者が示し意図した状態として OccupancyStatus 値を表現するべきです。
  // 同様に、作成者は実際の車両混雑状態に対応する OccupancyStatus 値を使用しなければなりません。
  // 線形スケールで乗客混雑レベルを記述するには、`occupancy_percentage` を参照してください。
  // このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  enum OccupancyStatus {
    // 車両または車両編成は、ほとんどの尺度で空いていると見なされ、乗車中の
    // 乗客はほとんどまたはいませんが、乗客を受け入れています。
    EMPTY = 0;

    // 車両または車両編成には多数の利用可能な座席があります。
    // このカテゴリに該当するほど十分に多いと見なされる、利用可能な総座席数に対する
    // 空席数は、作成者の裁量で決定されます。
    MANY_SEATS_AVAILABLE = 1;

    // 車両または車両編成には比較的少数の利用可能な座席があります。
    // このカテゴリに該当するほど十分に少ないと見なされる、利用可能な総座席数に対する
    // 空席数は、フィード作成者の裁量で決定されます。
    FEW_SEATS_AVAILABLE = 2;

    // 車両または車両編成は現在、立席の乗客のみを収容できます。
    STANDING_ROOM_ONLY = 3;

    // 車両または車両編成は現在、立席の乗客のみを収容でき、
    // そのためのスペースも限られています。
    CRUSHED_STANDING_ROOM_ONLY = 4;

    // 車両または車両編成は、ほとんどの尺度で満員と見なされますが、乗客の
    // 乗車を許可している場合があります。
    FULL = 5;

    // 車両または車両編成は乗客を受け入れていませんが、通常は乗客の乗車を受け入れます。
    NOT_ACCEPTING_PASSENGERS = 6;

    // 車両または車両編成には、その時点で利用可能な混雑データがありません。
    NO_DATA_AVAILABLE = 7;

    // 車両または車両編成は乗車できず、乗客を受け入れることはありません。
    // 特殊車両または車両編成（機関車、保守車両など）に有用です。
    NOT_BOARDABLE = 8;

  }
  // multi_carriage_status に車両編成ごとの OccupancyStatus が設定される場合、
  // このフィールドは、乗客を受け入れるすべての車両編成を考慮した車両全体を記述するべきです。
  optional OccupancyStatus occupancy_status = 9;

  // 車両内の乗客混雑度を示すパーセンテージ値。
  // 値は小数なしの整数として表されます。0 は 0% を意味し、100 は 100% を意味します。
  // 値 100 は、座席および立席の両方の収容力と、現在の運行規制で許可されるものを含む、
  // 車両が設計された総最大収容人数を表すべきです。
  // 乗客数が設計上の最大収容人数を超える場合、値は 100 を超えることがあります。
  // occupancy_percentage の精度は、個々の乗客が車両に乗車または降車することを追跡できない程度に低くするべきです。
  // multi_carriage_status に車両編成ごとの occupancy_percentage が設定される場合、 
  // このフィールドは、乗客を受け入れるすべての車両編成を考慮した車両全体を記述するべきです。
  // このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  optional uint32 occupancy_percentage = 10;

  // 複数の車両編成で構成される車両に使用される、車両編成固有の詳細
  // このメッセージ/フィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  message CarriageDetails {

    // 車両編成の識別子。車両ごとに一意であるべきです。
    optional string id = 1;

    // 乗客が車両編成を識別するのに役立つよう表示できる、ユーザーに見えるラベル。
    // 例: 「7712」、「Car ABC-32」など。
    // このメッセージ/フィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    optional string label = 2;

    // この車両内の、この指定された車両編成の混雑状態
    // このメッセージ/フィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    optional OccupancyStatus occupancy_status = 3 [default = NO_DATA_AVAILABLE];

    // この車両内の、この指定された車両編成の混雑率。
    // 「VehiclePosition.occupancy_percentage」と同じ規則に従います。
    // この指定された車両編成のデータが利用できない場合は -1（protobuf のデフォルトは 0 であるため）
    // このメッセージ/フィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    optional int32 occupancy_percentage = 4 [default = -1];

    // 車両の CarriageDetails リスト内の他の車両編成に対する、この車両編成の順序を
    // 識別します。
    // 進行方向の最初の車両編成は、値 1 を持たなければなりません。
    // 2 番目の値は進行方向の 2 番目の車両編成に対応し、
    // 値 2 を持たなければならず、以下同様です。
    // 例えば、進行方向の最初の車両編成は値 1 を持ちます。
    // 進行方向の 2 番目の車両編成が値 3 を持つ場合、
    // 利用者はすべての車両編成のデータ（すなわち、multi_carriage_details フィールド）を破棄します。
    // データのない車両編成は、有効な carriage_sequence 番号で表現しなければならず、データのないフィールドは
    // 省略するべきです（代替として、それらのフィールドを含めて「データなし」の値に設定することもできます）。
    // このメッセージ/フィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    optional uint32 carriage_sequence = 5;

    // extensions 名前空間により、サードパーティ開発者は新機能および
    // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
    extensions 1000 to 1999;

    // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
    extensions 9000 to 9999;
  }

  // この指定された車両の複数の車両編成の詳細。
  // 最初の出現は、現在の進行方向における車両の最初の車両編成を表します。 
  // multi_carriage_details フィールドの出現数は、
  // 車両の車両編成数を表します。
  // また、機関車、保守車両などの乗車できない車両編成も含まれます。
  // これらは、プラットフォーム上のどこに立つべきかについて乗客に貴重な
  // 情報を提供するためです。
  // このメッセージ/フィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  repeated CarriageDetails multi_carriage_details = 11;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 公共交通ネットワークにおける何らかの事象を示す運行情報(alert)。
message Alert {
  // 運行情報(alert)をユーザーに表示する時刻。存在しない場合、
  // 運行情報(alert)はフィードに存在する限り表示されます。
  // 複数の範囲が指定される場合、運行情報(alert)はそのすべての期間に表示されます。
  repeated TimeRange active_period = 1;

  // この運行情報(alert)を通知するべきユーザーを持つエンティティ。
  repeated EntitySelector informed_entity = 5;

  // この運行情報(alert)の原因。cause_detail が含まれる場合、Cause も含めなければなりません。
  enum Cause {
    UNKNOWN_CAUSE = 1;
    OTHER_CAUSE = 2;        // 機械可読ではありません。
    TECHNICAL_PROBLEM = 3;
    STRIKE = 4;             // 公共交通事業者の従業員が業務を停止しました。
    DEMONSTRATION = 5;      // 人々が道路を封鎖しています。
    ACCIDENT = 6;
    HOLIDAY = 7;
    WEATHER = 8;
    MAINTENANCE = 9;
    CONSTRUCTION = 10;
    POLICE_ACTIVITY = 11;
    MEDICAL_EMERGENCY = 12;
  }
  optional Cause cause = 6 [default = UNKNOWN_CAUSE];

  // この問題が影響を受けるエンティティに及ぼす影響。effect_detail が含まれる場合、Effect も含めなければなりません。
  enum Effect {
    NO_SERVICE = 1;
    REDUCED_SERVICE = 2;

    // 軽微な遅延は対象としません。検出が困難であり、ユーザーへの
    // 影響が小さく、頻繁すぎて結果が煩雑になるためです。
    SIGNIFICANT_DELAYS = 3;

    DETOUR = 4;
    ADDITIONAL_SERVICE = 5;
    MODIFIED_SERVICE = 6;
    OTHER_EFFECT = 7;
    UNKNOWN_EFFECT = 8;
    STOP_MOVED = 9;
    NO_EFFECT = 10;
    ACCESSIBILITY_ISSUE = 11;
  }
  optional Effect effect = 7 [default = UNKNOWN_EFFECT];

  // 運行情報(alert)に関する追加情報を提供する URL。
  optional TranslatedString url = 8;

  // 運行情報(alert)のヘッダー。プレーンテキストとして運行情報(alert)テキストの短い要約を含みます。
  optional TranslatedString header_text = 10;

  // プレーンテキストとしての運行情報(alert)の完全な説明。説明内の情報は、
  // ヘッダーの情報を補足するべきです。
  optional TranslatedString description_text = 11;

  // 読み上げ用実装で使用する運行情報(alert)ヘッダーのテキスト。このフィールドは header_text の読み上げ用バージョンです。
  optional TranslatedString tts_header_text = 12;

  // 読み上げ用実装で使用する運行情報(alert)の完全な説明のテキスト。このフィールドは description_text の読み上げ用バージョンです。
  optional TranslatedString tts_description_text = 13;

  // この運行情報(alert)の重大度。
  enum SeverityLevel {
    UNKNOWN_SEVERITY = 1;
    INFO = 2;
    WARNING = 3;
    SEVERE = 4;
  }

  optional SeverityLevel severity_level = 14 [default = UNKNOWN_SEVERITY];

  // 運行情報(alert)テキストとともに表示する TranslatedImage。迂回、駅の閉鎖などの運行情報(alert)の影響を視覚的に説明するために使用します。画像は運行情報(alert)の理解を高めなければなりません。画像内で伝達される必須情報は、運行情報(alert)テキストにも含めなければなりません。
  // 以下の種類の画像は推奨されません: 主にテキストを含む画像、追加情報を提供しないマーケティング画像またはブランド画像。 
  // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  optional TranslatedImage image = 15;

  // `image` フィールド内のリンクされた画像の外観を説明するテキスト（例: 画像を表示できない場合、
  // またはアクセシビリティ上の理由でユーザーが画像を見られない場合）。代替画像テキストについては HTML 仕様を参照してください - https://html.spec.whatwg.org/#alt。
  // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  optional TranslatedString image_alternative_text = 16;


  // 事業者固有の言語を可能にする運行情報(alert)の原因の説明。Cause より具体的です。cause_detail が含まれる場合、Cause も含めなければなりません。
  // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  optional TranslatedString cause_detail = 17;

  // 事業者固有の言語を可能にする運行情報(alert)の影響の説明。Effect より具体的です。effect_detail が含まれる場合、Effect も含めなければなりません。
  // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  optional TranslatedString effect_detail = 18;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

//
// 上記で使用される低レベルのデータ構造。
//

// 時間間隔。時刻「t」が開始時刻以上かつ終了時刻未満の場合、
// この間隔は時刻「t」においてアクティブと見なされます。
message TimeRange {
  // 開始時刻。POSIX 時刻（すなわち、1970 年 1 月 1 日
  // 00:00:00 UTC からの秒数）です。
  // 存在しない場合、間隔は負の無限大から開始します。
  optional uint64 start = 1;

  // 終了時刻。POSIX 時刻（すなわち、1970 年 1 月 1 日
  // 00:00:00 UTC からの秒数）です。
  // 存在しない場合、間隔は正の無限大で終了します。
  optional uint64 end = 2;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 位置。
message Position {
  // WGS-84 座標系における北緯（度）。
  required float latitude = 1;

  // WGS-84 座標系における東経（度）。
  required float longitude = 2;

  // 方位。北から時計回りの度数です。すなわち、0 は北、90 は東です。
  // これはコンパス方位、または次の停留所等(stop)もしくは中間地点への方向にできます。
  // これは、以前のデータから計算できる、以前の位置の連続から推定された方向であってはなりません。
  optional float bearing = 3;

  // 走行距離計の値。メートル単位です。
  optional double odometer = 4;
  // 車両によって測定された瞬間速度。メートル毎秒です。
  optional float speed = 5;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// GTFS の便(trip)のインスタンス、またはルート・路線系統(route)に沿った便(trip)のすべてのインスタンスを
// 識別する記述子。
// - 単一の便(trip)インスタンスを指定するには、trip_id（および必要に応じて
//   start_time）を設定します。route_id も設定する場合、それは指定された便(trip)が対応する
//   ものと同じであるべきです。
// - 指定されたルート・路線系統(route)に沿ったすべての便(trip)を指定するには、route_id のみを
//   設定するべきです。trip_id が不明な場合、TripUpdate 内の停留所順序 ID では
//   不十分であり、stop_id も提供しなければならないことに注意してください。さらに、
//   絶対到着/出発時刻を提供しなければなりません。
message TripDescriptor {
  // このセレクタが参照する GTFS フィードの trip_id。
  // 頻度ベースでない便(trip)の場合、このフィールドだけで便(trip)を一意に識別するのに十分です。
  // 頻度ベースの便(trip)の場合、start_time および start_date も
  // 必要になることがあります。TripUpdate 内で schedule_relationship が DUPLICATED の場合、trip_id は
  // 複製される静的 GTFS の便(trip)を識別します。VehiclePosition 内で schedule_relationship が DUPLICATED の場合、trip_id
  // は新しい複製便(trip)を識別し、対応する TripUpdate.TripProperties.trip_id の値を含まなければなりません。
  optional string trip_id = 1;

  // このセレクタが参照する GTFS の route_id。
  optional string route_id = 5;

  // このセレクタが参照する便(trip)の進行方向を示す、
  // GTFS フィード trips.txt ファイルの direction_id。
  optional uint32 direction_id = 6;

  // この便(trip)インスタンスの当初予定された開始時刻。
  // trip_id が頻度ベースでない便(trip)に対応する場合、このフィールドは
  // 省略するか、GTFS フィード内の値と等しくするべきです。trip_id が
  // 頻度ベースの便(trip)に対応する場合、便の更新(trip update)および車両位置情報(vehicle position)に対して start_time を
  // 指定しなければなりません。便(trip)が exact_times=1 の GTFS レコードに対応する場合、start_time は
  // 対応する期間の frequencies.txt start_time より headway_secs の
  // （ゼロを含む）何倍か後でなければなりません。便(trip)が exact_times=0 に対応する場合、
  // その start_time は任意にでき、当初は便(trip)の最初の出発時刻であることが期待されます。
  // 一度確立されたこの頻度ベースの便(trip)の start_time は、最初の
  // 出発時刻が変更された場合でも不変と見なすべきです。その時刻変更は代わりに
  // StopTimeUpdate に反映できます。
  // フィールドの形式および意味は、GTFS/frequencies.txt/start_time のものと同じです。
  // 例: 11:15:35 または 25:15:35。
  optional string start_time = 2;
  // この便(trip)インスタンスの予定開始日。
  // 翌日に予定された便(trip)と衝突するほど遅延した便(trip)を区別するために提供しなければなりません。
  // 例えば、毎日 8:00 と 20:00 に出発する列車が 12 時間遅延した場合、同じ時刻に
  // 2つの異なる便(trip)が存在することになります。
  // このような衝突が不可能な時刻表では、このフィールドを提供できますが必須ではありません。
  // 例えば、1 時間遅れた車両がもはや時刻表に関連していると見なされない、
  // 毎時運行のサービスなどです。
  // YYYYMMDD 形式です。
  optional string start_date = 3;

  // この便(trip)と静的時刻表との関係。一時的な時刻表に従って便(trip)が運行され、
  // GTFS に反映されていない場合、SCHEDULED としてマークするべきではなく、
  // おそらく ADDED とするべきです。
  enum ScheduleRelationship {
    // GTFS 時刻表に従って運行している便(trip)、または予定された便(trip)と関連付けるのに
    // 十分近い便(trip)。
    SCHEDULED = 0;

    // 動作が未定義であったため、この値は非推奨となりました。 
    // 開始日または時刻を除いて予定された便(trip)と同じ追加便(trip)には DUPLICATED を使用し、
    // 既存の便(trip)と無関係な追加便(trip)には NEW を使用してください。
    ADDED = 1 [deprecated = true];

    // 関連する時刻表なしに運行する便(trip)（GTFS frequencies.txt exact_times=0）。
    // ScheduleRelationship=UNSCHEDULED の便(trip)では、すべての StopTimeUpdates.ScheduleRelationship=UNSCHEDULED も設定しなければなりません。
    UNSCHEDULED = 2;

    // 時刻表には存在したが削除された便(trip)。
    CANCELED = 3;

    // 時刻表内の既存の便(trip)を置き換える便(trip)。
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    REPLACEMENT = 5;

    // 運行中の時刻表に加えて追加された便(trip)。例えば、故障した車両を置き換えるため、または
    // 突発的な乗客負荷に対応するためのものです。TripUpdate.TripProperties.trip_id、TripUpdate.TripProperties.start_date、
    // および TripUpdate.TripProperties.start_time とともに使用し、静的 GTFS の既存の便(trip)をコピーしますが、異なる運行日(service day)
    // および/または時刻に開始します。（CSV）GTFS 内の元の便(trip)に関連するサービス
    // （calendar.txt または calendar_dates.txt 内）が今後 30 日以内に運行される場合、便(trip)の複製が許可されます。複製する便(trip)は
    // TripUpdate.TripDescriptor.trip_id によって識別されます。この列挙値は、
    // TripUpdate.TripDescriptor.trip_id が参照する既存の便(trip)を変更しません。作成者が元の便(trip)をキャンセルする場合、
    // CANCELED または DELETED の値を持つ別の TripUpdate を公開しなければなりません。作成者が元の便(trip)を置き換える場合は、 
    // 代わりに `REPLACEMENT` を使用するべきです。
    //
    // exact_times が空または 0 に等しい GTFS frequencies.txt で定義された便(trip)は
    // 複製できません。新しい便(trip)の VehiclePosition.TripDescriptor.trip_id には
    // TripUpdate.TripProperties.trip_id の一致する値を含めなければならず、VehiclePosition.TripDescriptor.ScheduleRelationship
    // も DUPLICATED に設定しなければなりません。
    // 複製便(trip)を表すために ADDED 列挙値を使用していた既存の作成者および利用者は、
    // DUPLICATED 列挙値へ移行するために、移行ガイド（https://github.com/google/transit/tree/master/gtfs-realtime/spec/en/examples/migration-duplicated.md）
    // に従わなければなりません。
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    DUPLICATED = 6;


    // 時刻表には存在したが削除され、ユーザーに表示してはいけない便(trip)。
    // DELETED は、交通提供者が利用アプリケーションから対応する便(trip)に関する情報を完全に削除したいことを示すために、
    // CANCELED の代わりに使用するべきです。これにより、便(trip)は乗客にキャンセルとして表示されません。
    // 例えば、別の便(trip)によって完全に置き換えられる便(trip)などです。
    // この指定は、複数の便(trip)がキャンセルされ、代替サービスに置き換えられる場合に特に重要になります。
    // 利用者がキャンセルに関する明示的な情報を表示すると、より重要な
    // リアルタイム予測から注意をそらすことになります。
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    DELETED = 7;

    // 既存の便(trip)とは無関係な追加便(trip)。例えば、突発的な乗客負荷に対応するためのものです。
    // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
    NEW = 8;
  }
  optional ScheduleRelationship schedule_relationship = 4;

  message ModifiedTripSelector {
    // 含まれる TripModifications オブジェクトがこの便(trip)に影響する FeedEntity の「id」。
    optional string modifications_id = 1;

    // modifications_id によって変更される GTFS フィードの trip_id
    optional string affected_trip_id = 2;

    // この便(trip)インスタンスの当初予定された開始時刻。頻度ベースの変更された便(trip)に適用されます。TripDescriptor の start_time と同じ定義です。
    optional string start_time = 3;

    // YYYYMMDD 形式のこの便(trip)インスタンスの開始日。変更された便(trip)に適用されます。TripDescriptor の start_date と同じ定義です。
    optional string start_date = 4;

    // extensions 名前空間により、サードパーティ開発者は新機能および
    // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
    extensions 1000 to 1999;

    // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
    extensions 9000 to 9999;
  }

  // この便(trip)に対して行われた変更（ルート形状(shape)の変更、停留所等(stop)の削除または追加）へのリンク。
  // このフィールドが提供される場合、`TripDescriptor` の `trip_id`、`route_id`、`direction_id`、`start_time`、`start_date` フィールドは、`ModifiedTripSelector` 値を探さない利用者の混乱を避けるため、空のままにしなければなりません。
  optional ModifiedTripSelector modified_trip = 7;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 便(trip)を運行する車両の識別情報。
message VehicleDescriptor {
  // 車両の内部システム識別子。車両ごとに一意であるべきであり、
  // システム内を進行する車両を追跡するために使用できます。
  optional string id = 1;

  // ユーザーに見えるラベル。すなわち、正しい車両を識別するのに役立つよう乗客に
  // 表示しなければならないものです。
  optional string label = 2;

  // 車両のナンバープレート。
  optional string license_plate = 3;

  enum WheelchairAccessible {
    // 便(trip)には車椅子アクセシビリティに関する情報がありません。
    // これは**デフォルト**の動作です。静的 GTFS に
    // _wheelchair_accessible_ 値が含まれる場合、それは上書きされません。
    NO_VALUE = 0;

    // 便(trip)にはアクセシビリティ値が存在しません。
    // この値は GTFS の値を上書きします。
    UNKNOWN = 1;

    // 便(trip)は車椅子で利用可能です。
    // この値は GTFS の値を上書きします。
    WHEELCHAIR_ACCESSIBLE = 2;

    // 便(trip)は車椅子で**利用できません**。
    // この値は GTFS の値を上書きします。
    WHEELCHAIR_INACCESSIBLE = 3;
  }
  optional WheelchairAccessible wheelchair_accessible = 4 [default = NO_VALUE];

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// GTFS フィード内のエンティティのセレクタ。
message EntitySelector {
  // フィールドの値は、GTFS フィード内の適切なフィールドに対応するべきです。
  // 少なくとも 1つの指定子を指定しなければなりません。複数指定される場合、
  // 一致は指定されたすべての指定子に適用されなければなりません。
  optional string agency_id = 1;
  optional string route_id = 2;
  // GTFS の route_type に対応します。
  optional int32 route_type = 3;
  optional TripDescriptor trip = 4;
  optional string stop_id = 5;
  // GTFS trips.txt の便(trip)の direction_id に対応します。提供される場合、
  // route_id も提供しなければなりません。
  optional uint32 direction_id = 6;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// テキストの断片または URL の言語ごとのバージョンを含む国際化メッセージ。
// メッセージ内の文字列の 1つが選択されます。解決は以下のように進みます。
// 1. UI 言語が翻訳の言語コードと一致する場合、最初に一致した翻訳が選択されます。
// 2. デフォルトの UI 言語（例: 英語）が翻訳の言語コードと一致する場合、
//    最初に一致した翻訳が選択されます。
// 3. いずれかの翻訳に未指定の言語コードがある場合、その翻訳が
//    選択されます。
message TranslatedString {
  message Translation {
    // メッセージを含む UTF-8 文字列。
    required string text = 1;
    // BCP-47 言語コード。言語が不明な場合、またはフィードに対して
    // i18n がまったく行われていない場合は省略できます。未指定の言語タグを持つ翻訳は
    // 最大 1つまで許可されます。
    optional string language = 2;

    // extensions 名前空間により、サードパーティ開発者は新機能および
    // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
    extensions 1000 to 1999;

    // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
    extensions 9000 to 9999;
  }
  // 少なくとも 1つの翻訳を提供しなければなりません。
  repeated Translation translation = 1;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 画像にリンクする URL の言語ごとのバージョンを、メタ情報とともに含む国際化画像
// 利用者はメッセージ内の画像の 1つのみを保持します。解決は
// 以下のように進みます。
// 1. UI 言語が翻訳の言語コードと一致する場合、
//    最初に一致した翻訳が選択されます。
// 2. デフォルトの UI 言語（例: 英語）が翻訳の言語コードと一致する場合、
//    最初に一致した翻訳が選択されます。
// 3. いずれかの翻訳に未指定の言語コードがある場合、その翻訳が
//    選択されます。
// 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
message TranslatedImage {
  message LocalizedImage {
    // 画像にリンクする URL を含む文字列
    // リンク先の画像は 2MB 未満でなければなりません。 
    // 画像が、利用者側で更新を必要とするほど大幅に変更された場合、作成者は URL を新しいものに更新しなければなりません。
    // URL は http:// または https:// を含む完全修飾 URL であるべきであり、URL 内の特殊文字は正しくエスケープしなければなりません。完全修飾 URL 値の作成方法については、以下の http://www.w3.org/Addressing/URL/4_URI_Recommentations.html を参照してください。
    required string url = 1;

    // 表示する画像の種類を指定する IANA メディアタイプ。 
    // 種類は「image/」で始まらなければなりません。
    required string media_type = 2;

    // BCP-47 言語コード。言語が不明な場合、またはフィードに対して
    // i18n がまったく行われていない場合は省略できます。未指定の言語タグを持つ翻訳は
    // 最大 1つまで許可されます。
    optional string language = 3;


    // extensions 名前空間により、サードパーティ開発者は新機能および
    // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
    extensions 1000 to 1999;

    // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
    extensions 9000 to 9999;
  }
  // 少なくとも 1つのローカライズされた画像を提供しなければなりません。
  repeated LocalizedImage localized_image = 1;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 迂回など、車両が（CSV）GTFS の一部ではない経路を走行する場合の物理的経路を記述します。
// ルート形状(shape)は便(trip)に属し、ルート形状(shape)ポイントの連続で構成されます。
// ポイントを順番にたどることで、車両の経路が得られます。ルート形状(shape)は
// 停留所等(stop)の位置と正確に交差する必要はありませんが、便(trip)上のすべての停留所等(stop)は、
// その便(trip)のルート形状(shape)から短い距離内、すなわちルート形状(shape)ポイントを結ぶ直線セグメントの
// 近くにあるべきです。
// 注: このメッセージはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
message Shape {
  // ルート形状(shape)の識別子。（CSV）GTFS で定義された任意の shape_id とは異ならなければなりません。
  // このフィールドは reference.md に従えば必須ですが、「Required is Forever」であるため、ここでは optional として指定する必要があります。
  // https://developers.google.com/protocol-buffers/docs/proto#specifying_field_rules を参照してください。
  // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  optional string shape_id = 1;

  // ルート形状(shape)のエンコードされたポリライン表現。このポリラインは少なくとも 2つのポイントを含み、それが使用される便(trip)の完全なルート形状(shape)を表さなければなりません。 
  // エンコードされたポリラインの詳細については、https://developers.google.com/maps/documentation/utilities/polylinealgorithm を参照してください。
  // このフィールドは reference.md に従えば必須ですが、「Required is Forever」であるため、ここでは optional として指定する必要があります。
  // https://developers.google.com/protocol-buffers/docs/proto#specifying_field_rules を参照してください。
  // 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
  optional string encoded_polyline = 2;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 便(trip)が運行する停留所等(stop)を記述します。すべてのフィールドは GTFS-Static 仕様で記述されているとおりです。
// 注: このメッセージはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
message Stop {
  enum WheelchairBoarding {
    UNKNOWN = 0;
    AVAILABLE = 1;
    NOT_AVAILABLE = 2;
  }

  optional string stop_id = 1;
  optional TranslatedString stop_code = 2;
  optional TranslatedString stop_name = 3;
  optional TranslatedString tts_stop_name = 4;
  optional TranslatedString stop_desc = 5;
  optional float stop_lat = 6;
  optional float stop_lon = 7;
  optional string zone_id = 8;
  optional TranslatedString stop_url = 9;
  optional string parent_station = 11;
  optional string stop_timezone = 12;
  optional WheelchairBoarding wheelchair_boarding = 13 [default = UNKNOWN];
  optional string level_id = 14;
  optional TranslatedString platform_code = 15;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
message TripModifications {
  // `Modification` メッセージは、`start_stop_selector` から開始する、影響を受ける各便(trip)の n 個の停車時刻(stop_time)の範囲を置き換えます。
  message Modification {
    // この変更の影響を受ける元の便(trip)の最初の停車時刻(stop_time)の停留所等(stop)セレクタ。
    // `end_stop_selector` と組み合わせて使用します。 
    // `start_stop_selector` は必須であり、`travel_time_to_stop` とともに使用する基準停留所等(stop)を定義するために使用されます。
    optional StopSelector start_stop_selector = 1;

    // この変更の影響を受ける元の便(trip)の最後の停留所等(stop)の停留所等(stop)セレクタ。 
    // 選択は包括的であるため、その変更によって 1つの停車時刻(stop_time)のみが置き換えられる場合、`start_stop_selector` と `end_stop_selector` は同等でなければなりません。
    // 停車時刻(stop_time)が置き換えられない場合、`end_stop_selector` を提供してはいけません。それ以外の場合は必須です。
    optional StopSelector end_stop_selector = 2;

    // この変更の終了後のすべての出発時刻および到着時刻に追加する遅延秒数。 
    // 複数の変更が同じ便(trip)に適用される場合、便(trip)の進行に伴って遅延は累積します。 
    optional int32 propagated_modification_delay = 3 [default = 0];

    // 元の便(trip)の停留所等(stop)を置き換える、置換停留所等(stop)のリスト。 
    // 新しい停車時刻(stop_time)の長さは、置き換えられる停車時刻(stop_time)の数より少なく、同じ、または多くできます。 
    repeated ReplacementStop replacement_stops = 4;

    // ユーザー向けの通信のために、この Modification を記述する `Alert` を含む `FeedEntity` メッセージの `id` 値。
    optional string service_alert_id = 5;

    // このタイムスタンプは、変更が最後に変更された時点を識別します。
    // POSIX 時刻（すなわち、1970 年 1 月 1 日 00:00:00 UTC からの秒数）です。
    optional uint64 last_modified_time = 6;

    // extensions 名前空間により、サードパーティ開発者は新機能および
    // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
    extensions 1000 to 1999;

    // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
    extensions 9000 to 9999;
  }

  message SelectedTrips {
    // この置換の影響を受け、すべて同じ新しい `shape_id` を持つ便(trip)のリスト。schedule_relationship=REPLACEMENT の `TripUpdate` が、その便(trip)に対してすでに存在していてはいけません。
    repeated string trip_ids = 1;
    // この SelectedTrips 内の変更された便(trip)の新しいルート形状(shape)の ID。 
    // 同じ GTFS-RT フィード内の `Shape` メッセージを使用して追加された新しいルート形状(shape)、または GTFS-Static フィードの shapes.txt で定義された既存のルート形状(shape)を参照できます。 
    // リアルタイムフィード内の `Shape` エンティティを参照する場合、このフィールドの値はエンティティ内の `shape_id` の値であるべきであり、`FeedEntity` の `id` ではありません。
    optional string shape_id = 2;

    // extensions 名前空間により、サードパーティ開発者は新機能および
    // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
    extensions 1000 to 1999;

    // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
    extensions 9000 to 9999;
  }

  // この TripModifications の影響を受ける選択された便(trip)のリスト。
  repeated SelectedTrips selected_trips = 1;

  // trip_ids で定義された trip_id の便(trip)に対する、リアルタイム便(trip)記述子内の開始時刻のリスト。 
  // 頻度ベースの便(trip)における trip_id の複数の出発を対象とするのに有用です。
  repeated string start_times = 2;

  // 変更が発生する日付。YYYYMMDD 形式です。作成者は今後 1 週間以内に発生する迂回のみを送信するべきです。
  // 提供された日付はユーザー向け情報として使用するべきではありません。ユーザー向けの開始日および終了日を提供する必要がある場合、`service_alert_id` を持つリンクされた運行情報(alert)で提供できます。
  repeated string service_dates = 3;

  // 影響を受ける便(trip)に適用する変更のリスト。 
  repeated Modification modifications = 4;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
// stop_sequence または stop_id によって停留所等(stop)を選択します。2つの値の少なくとも一方を提供しなければなりません。
message StopSelector {
  // 対応する GTFS フィードの stop_times.txt と同じでなければなりません。
  optional uint32 stop_sequence = 1;
  // 対応する GTFS フィードの stops.txt と同じでなければなりません。
  optional string stop_id = 2;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}

// 注: このフィールドはまだ実験的であり、変更される可能性があります。将来、正式に採用される場合があります。
message ReplacementStop {
  // この停留所等(stop)の到着時刻と基準停留所等(stop)の到着時刻との差（秒単位）。基準停留所等(stop)は start_stop_selector の前の停留所等(stop)です。変更が便(trip)の最初の停留所等(stop)で開始する場合、便(trip)の最初の停留所等(stop)が基準停留所等(stop)です。
  // この値は単調増加でなければならず、元の便(trip)の最初の停留所等(stop)が基準停留所等(stop)である場合にのみ負の数にできます。
  optional int32 travel_time_to_stop = 1;

  // 便(trip)が今後訪問する置換停留所等(stop) ID。同じ GTFS-RT フィード内の GTFS-RT `Stop` メッセージを使用して追加された新しい停留所等(stop)、または（CSV）GTFS フィードの `stops.txt` で定義された既存の停留所等(stop)を参照できます。
  // リアルタイムフィード内の `Shape` エンティティを参照する場合、このフィールドの値はエンティティ内の `stop_id` の値であるべきであり、`FeedEntity` の `id` ではありません。置換停留所等(stop)は `location_type=0`（経路設定可能な停留所等(stop)）でなければなりません。
  optional string stop_id = 2;

  // extensions 名前空間により、サードパーティ開発者は新機能および
  // 仕様への変更を追加・評価するために、GTFS Realtime Specification を拡張できます。
  extensions 1000 to 1999;

  // 以下の拡張 ID は、任意の組織による私的利用のために予約されています。
  extensions 9000 to 9999;
}
```
