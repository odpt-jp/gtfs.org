## 移行ガイド - ADDED から NEW または DUPLICATED の便への移行 {: #migration-guide-transition-from-added-to-new-or-duplicated-trips}


GTFS-realtime の `trip.schedule_relationship` における `NEW` は、既存のスケジュール済みの便(trip)とは無関係なスケジュールで運行される新しい便(trip)を表します。

GTFS-realtime の `trip.schedule_relationship` における `DUPLICATED` は、運行開始日および時刻を除き、既存のスケジュール済みの便(trip)と同一である新しい便(trip)を表します。 

この移行ガイドでは、`ADDED` 列挙型を使用していた既存のプロデューサーおよびコンシューマーが、`NEW` および `DUPLICATED` 列挙型のいずれかへ移行する方法を定義します。目標は、移行中にプロデューサーおよびコンシューマーへの影響を最小限に抑えることです。 

*`ADDED` 列挙型を使用したことが**ない**プロデューサーまたはコンシューマーの場合、対応は不要です。`ADDED` エンティティを生成または消費することなく、`NEW` および/または `DUPLICATED` の便(trip)を生成または消費できます。* 

`NEW` 列挙型の完全な履歴については、[GitHub 上の `NEW` および `REPLACEMENT` の提案](https://github.com/google/transit/pull/504)を参照してください。

`DUPLICATED` 列挙型の完全な履歴については、[GitHub 上の `DUPLICATED` の提案](https://github.com/google/transit/pull/221)を参照してください。

### どちらに移行するか {: #which-one-to-migrate-to}


`NEW` と `DUPLICATED` の列挙型はいずれも、GTFS static で当初運行予定ではなかった便(trip)を指定するために使用されます。

便(trip)をテンプレートとして、どの運行予定便も使用して記述できない場合は、`NEW` を使用してください。たとえば、その便(trip)がルート・路線系統(route)の通常便とは異なる停留所等(stop)に停車する場合、または通常便ではすべての停留所等(stop)で乗車と降車の両方が可能であるにもかかわらず、追加便がルート・路線系統(route)の始点でのみ乗車可能である場合です。

便(trip)が運行予定便の複製であり、元の運行予定便と同じ時刻または異なる時刻に運行される場合は、`DUPLICATED` を使用してください。

### 同一フィードでの `ADDED` および `NEW` エンティティの使用 {: #using-added-and-new-entities-in-the-same-feed}


スケジュールに関連しない便(trip)を指定するために `ADDED` 列挙型を使用しているプロデューサーは、既存のコンシューマーへの影響を避けるため、これらの便について引き続き `ADDED` エンティティを生成するとともに、同じ便に対して `NEW` エンティティも追加することが推奨されます。

ただし、コンシューマーが誤って同じ便を2回追加することを防ぐため、同じ便を参照するエンティティは、同じ `trip_id`、`route_id`、および `start_date` を使用してリンクしなければなりません。
さらに、`stop_time_update` の内容も同一でなければなりません。

#### プロデューサー {: #producers}


~~~
entity {
  id: "ei0"
  trip_update {
    trip: {
      trip_id: "1" // <-- 静的GTFS内に見つからないtrip_id
      route_id: "A"
      schedule_relationship: ADDED
      start_date: "20200821" // <-- 新しい便の日付
      start_time: "11:30:00" // <-- 新しい便の時刻
    }
    stop_time_update {
	... // 便の全停車地点リスト
    }
  }
}

entity {
  id: "ei10"
  trip_update {
    trip: {
      trip_id: "1" // <-- 上記と同じtrip_id
      route_id: "A" // <-- 上記と同じroute_id
      schedule_relationship: NEW
      start_date: "20200821" // <-- 上記と同じ日付
      start_time: "11:30:00" // <-- 上記と同じ時刻
    }
    stop_time_update {
	... // <-- 上記と同じ内容
    }
  }
}
~~~

`ADDED` の使用が定められた期限までに非推奨となること、およびコンシューマーが代わりに `NEW` の便(trip)を取り込むべきであることを、既存のコンシューマー（例: 開発者メーリングリスト経由）に通知することが提案されます。`ADDED` と `NEW` の便エンティティを対応付けるために使用される上記の戦略についても言及し、この移行ガイドへのリンクを含めるべきです。期限経過後は、フィードから `ADDED` エンティティを削除し、新たに追加された便については `NEW` エンティティのみを公開することができます。

#### コンシューマ {: #consumers}


前述のとおり、プロデューサは、同じ `trip_id` を使用して新しい各便(trip)に対して最初に2つのエンティティを公開することで、`ADDED` 列挙型から `NEW` 列挙型へ移行します。

したがって、コンシューマが `NEW` 便(trip)のサポートを実装する際には、`NEW` 便(trip)の `trip_id` と同じ `trip_id` を持つ `ADDED` 便(trip)をコンシューマが無視することが重要です。

### 同一フィードでの ADDED および DUPLICATED エンティティの使用 {: #using-added-and-duplicated-entities-in-same-feed}

#### プロデューサー {: #producers}


重複した便に対して `ADDED` 列挙値を使用してきたプロデューサーである場合、既存のコンシューマーへの影響を避けるため、これらの便について引き続き `ADDED` エンティティを生成するとともに、同じ便に対する `DUPLICATED` エンティティも追加することが推奨されます。  

ただし、コンシューマーが誤って同じ便を2回追加することを防ぐため、同じ便を参照するエンティティは、同じ `trip_id` を使用してリンク**しなければなりません**。2つのエンティティは、次の2つの方法のうち**いずれか1つ**でリンクできます。  

 1. 両方のエンティティの `trip.trip_id` は同じで**なければなりません**。または
 2. `ADDED` 便の `trip.trip_id` は、`DUPLICATED` 便の `trip_properties.trip_id` と同じで**なければなりません**。
 
以下は、GTFS の `trip_id 1` を複製するための最初の方法 (1) の例です。`ADDED` および `DUPLICATED` エンティティで `trip.trip_id` が一致しています。

~~~
entity {
  id: "ei0"
  trip_update {
    trip: {
      trip_id: "1" // <-- コピーする静的 GTFS の trip_id
      schedule_relationship: ADDED
      start_date: "20200821" // <-- 新しい便の日付
      start_time: "11:30:00" // <-- 新しい便の時刻
    }
    stop_time_update {
	...
    }
  }
}

entity {
  id: "ei10"
  trip_update {
    trip: {
      trip_id: "1" // <-- コピーする静的 GTFS の trip_id
      schedule_relationship: DUPLICATED
    }
    trip_properties {
      trip_id: "NewTripId987" // <-- この便に固有の新しい trip_id
      start_date: "20200821"  // <-- 新しい便の日付
      start_time: "11:30:00"  // <-- 新しい便の時刻
    }
    stop_time_update {
	...
    }
  }
}
~~~

以下は、GTFS の `trip_id 1` を複製するための2番目の方法 (2) の例です。`ADDED` 便の `trip.trip_id` が、`DUPLICATED` 便の `trip_properties.trip_id` と一致しています。

~~~
entity {
  id: "ei0"
  trip_update {
    trip: {
      trip_id: "NewTripId987" // <-- この便に固有の新しい trip_id
      schedule_relationship: ADDED
      start_date: "20200821" // <-- 新しい便の日付
      start_time: "11:30:00" // <-- 新しい便の時刻
    }
    stop_time_update {
	...
    }
  }
}

entity {
  id: "ei10"
  trip_update {
    trip: {
      trip_id: "1" // <-- コピーする静的 GTFS の trip_id
      schedule_relationship: DUPLICATED
    }
    trip_properties {
      trip_id: "NewTripId987" // <-- ADDED の trip.trip_id と一致
      start_date: "20200821"  // <-- 新しい便の日付
      start_time: "11:30:00"  // <-- 新しい便の時刻
    }
    stop_time_update {
	...
    }
  }
}
~~~

既存のコンシューマーに対して（例: 開発者向けメーリングリストを通じて）、`ADDED` の使用が設定された期限までに非推奨となること、およびコンシューマーは代わりに `DUPLICATED` 便の利用を開始するべきであることを通知することが推奨されます。`ADDED` と `DUPLICATED` の便エンティティを対応付けるために使用される上記の戦略についても言及し、この移行ガイドへのリンクを含めるべきです。期限を過ぎた後は、フィードから `ADDED` エンティティを削除し、重複した便については `DUPLICATED` エンティティのみを公開できます。

#### コンシューマ {: #consumers}


前述のとおり、プロデューサは、重複する各便(trip)について最初に2つのエンティティを公開し、エンティティ間のIDを対応付けるために上記2つのオプションのいずれかを使用することで、`ADDED` 列挙型から `DUPLICATED` 列挙型へ移行します。
 
したがって、コンシューマが `DUPLICATED` 便(trip)のサポートを実装する際には、コンシューマが以下を行うことが重要です。

 1. `DUPLICATED` 便(trip)の `trip.trip_id` と同じ `trip.trip_id` を持つ `ADDED` 便(trip)を無視します
 1. `DUPLICATED` 便(trip)の `trip_properties.trip_id` と同じ `trip.trip_id` を持つ `ADDED` 便(trip)を無視します
