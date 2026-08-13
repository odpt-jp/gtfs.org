# GTFS-Flex {: #gtfs-flex}


GTFS Flex は、デマンド型サービスの発見可能性を促進することを目的とした GTFS Schedule の拡張プロジェクトです。 

大部分は 2024年3月に GTFS に採用されました。GTFS Flex の正式に採用された部分を使用してモデル化できる内容の例は、[このページ](../../../documentation/schedule/examples/flex)で確認できます。

🤔 ダイヤル・ア・ライドのようなサービスは、乗客に見過ごされがちであり、乗客はその存在すら知らないことがあります。このアクセシビリティの欠如は、交通事業者、経路検索サービス、および乗客にとっての課題です。地域の空港に到着し、オンデマンドバスサービスしか提供されていない農村地域へ行きたいと考えている観光客の一団を想像してください。観光客は好みの経路検索アプリを確認しますが、利用可能な公共交通機関の選択肢を見つけられません。結局、レンタカーを借りることになります。観光客であるため、オンデマンドサービスを案内するために廊下に掲示された紙のチラシをすべて見逃してしまいます。サービスが十分に利用されないだけでなく、現在および将来の乗客需要を満たすための発見可能性も欠いています。ここで GTFS-Flex が役立ちます。GTFS-Flex は乗客がサービスを発見するのを支援し、皆様が熱心に促進してきたサービスを乗客が利用できるようにします。

<img src="../../../assets/flex-userjourney-resize.jpg" alt="GTFS-Flex のユーザー旅程">

🔮 MobilityData は、GTFS-Flex が GTFS-OnDemand を使用したトランザクションおよびリアルタイムのコンポーネントへの拡張を含め、デマンド型サービスのより深い標準化への扉を開くことを期待しています。この分野における増加する交通手段の数と概念の複雑さを最適に扱うための推奨戦略を準備しています。

[完全な提案を見る](https://github.com/MobilityData/gtfs-flex){ .md-button .md-button--primary }

## 最新のPull Request {: #latest-pull-request}

この拡張は、スケジュールに従って運行される一方で、以下のような1つ以上の柔軟な機能も含むサービスについて説明します。

- **Dial-a-ride service**: 車両は、特定の運行時間中に乗車および降車が許可されるゾーンを運行します。
- **Route deviation services**: 車両は固定ルートおよび順序付けられた停留所等(stop)の集合を運行し、停留所等(stop)間で乗客を乗車または降車させるために迂回することができます。
- **Point-to-zone service**: 乗客は鉄道駅などの固定された停留所等(stop)で乗車し、その後、エリア内の任意の場所で降車することができます。またはその逆も可能です。一部の場所からの出発は、スケジュール化されるか、他のサービスに合わせて時刻設定されます。
- **Point deviation or checkpoint service**: 乗客は固定された停留所等(stop)で乗車し、その後、順序付けられていない停留所等(stop)のリスト内の任意の場所で降車することができます。またはその逆も可能です。運転手は、リクエストが行われた停留所等(stop)のみを運行します。

詳細については、[original proposal](https://github.com/MobilityData/gtfs-flex/blob/master/spec/reference.md)および[issue#382](https://github.com/google/transit/issues/382)（スコープを変更したためクローズ済み）を参照してください。

6月28日のワーキングミーティングでは、現在生成および利用されているすべてのフィールドを対象とする反復作業を進めることで、グループコミュニティの合意が得られました。したがって、[adoption tracker](#adoption-tracker)で「**in discussion**」として表示されるすべてのフィールドが、このPRに含まれています。

このPRにおける変更は以下のとおりです。

- ファイルの変更:
    - `stop_areas.txt`を変更し、GeoJSON locationおよび/または停留所等(stop)のグループ化を可能にします。これにより、これらの機能の事前定義されたグループを`stop_times.txt`の個別の行で指定できます。
    - `stop_times.txt`を変更し、追加および拡張されたファイルとフィールドの解釈方法をデータ利用者に伝えるために必要な、現行仕様の要素を明確化します。
- ファイルの拡張:
    - `stop_times.txt`を`start_pickup_drop_off_window`および`end_pickup_drop_off_window`で拡張し、GeoJSON location、stop areaまたは停留所等(stop)においてデマンド型サービスが利用可能になる時刻および終了する時刻を定義します。
    - `stop_times.txt`を`pickup_booking_rule_id`および`drop_off_booking_rule_id`で拡張し、予約ルールへのリンクを定義します。
- 新規ファイルの追加:
    - `locations.geojson`: 乗客が乗車または降車のいずれかをリクエストできるゾーン（`Polygon`または`Multipolygon`）を定義します。
    - `booking_rules.txt`: サービスのリクエスト方法に関する情報を乗客に提供する予約ルールを定義します。

ドイツのAngermündeおよびGartzerにおける[RufBus](https://uvg-online.com/rufbus-angermuende/)の[data example](https://docs.google.com/spreadsheets/d/1w5EHuHfxvejqApJFHA1Z0K2KytD9zahwbf8zyRlP_Ls/edit#gid=1451132209)はこちらです。以下の画像は、データが旅程プランナーでどのように表示されるかを示す例です。

<img src="https://github.com/google/transit/assets/126435471/c986f79a-0164-4e38-a552-7e37405fe133" width="180" height="400">

Pull Requestページにアクセスして、投稿全文を読み、議論に参加してください。 

[Pull Requestを見る](https://github.com/google/transit/pull/388){ .md-button .md-button--primary }

[Slackで#gtfs-flexに参加する](https://share.mobilitydata.org/slack){ .md-button .md-button--primary }

## 初期実装 {: #early-implementations}


以下は、GTFS-Flexの初期実装の例です。現在の実装を確認するには、[Mobility Database](https://mobilitydatabase.org/)をご覧ください。

- [MNDoT Flex Pilot Project: Trillium、IBI、Transit、MNDoT、Cambridge Systematics、およびToken Transit](https://blog.transitapp.com/case-study/mndot-gtfs-flex-bringing-rural-riders-into-the-fold/) 
- [Open Trip Planner](https://www.opentripplanner.org/)
- [バーモント州のMobility on Demand Sandbox](https://www.connectingcommuters.org/)
- [Tulare County Area Transit](https://gotcrta.org/)
- [Northwest Oregon Transit Alliance（NW Connector）](https://nwconnector.org/other-services/)
- [Vamos Mobility App](https://vamosmobileapp.com/)
- [RTD Denver Flexride](https://www.rtd-denver.com/services/flexride)
- [Nebraska Public Transit DRT OTP Project: Trillium、Olsson、Cambridge Systematics、およびTransitPlus](https://trips.nebraskatransit.com/#/)
- [One-Call/One-Click project: Find a RideのTrip planner](https://www.findaride.org/tripplanner)

GTFS-Flexの実装をこのページに追加するには、お問い合わせください

<a class="md-button md-button--primary" href=mailto:specification@mobilitydata.org >お問い合わせ</a>

## 導入状況トラッカー {: #adoption-tracker}

### 現在 {: #current}


<iframe class="airtable-embed" src="https://airtable.com/embed/appopXWyO2ne6THIw/shrUPyCZWOWrvO2mX?backgroundColor=purple" frameborder="0" onmousewheel="" width="100%" height="533" style="background: transparent; border: 1px solid #ccc;"></iframe>

[変更をリクエストする](https://airtable.com/shrcac1fXUrMxfoDV){ .md-button .md-button--primary }
[組織を追加する（コンシューマー）](https://airtable.com/shrgnVR5Su9tkHvUv){ .md-button .md-button--primary }
[組織を追加する（プロデューサー）](https://airtable.com/shrsU4idBtcLuRuwZ){ .md-button .md-button--primary }

## 履歴 {: #history}


- **2013年**: Brian Ferris（Google）により原案が作成されました
- **2016年**: <a href="https://github.com/MobilityData/gtfs-flex/tree/master" target="_blank">GTFS-Flex GitHubでの議論が開始されました</a>
- **2017年**: <a href="https://www.oregon.gov/odot/RPTD/RPTD%20Document%20Library/GTFS-Flex-N-CATT.pdf" target="_blank">Mobility on Demand（MOD）サンドボックスプログラム（FTA、Vermont DOT、OTP）</a>
- **2018年**: MobilityDataがGTFS-Flexの管理者となり、GTFS-Flex v2を提案しました
- **2020年11月**: リポジトリがGTFS-Flexの最新バージョンとなり、OTP2がGTFS-Flex v2データを取り込みました
- **2022年5月**: MnDoTパイロットが開始されました（Cambridge Systematics、MNDoT、Token Transit、Transit、Trillium（OptiBus））。  
- **2023年5月**: <a href="https://github.com/google/transit/issues/382" target="_blank">GTFS-Flexに関する作業: サービス発見が開始されました</a>
- **2023年6月**:  <a href="https://mobilitydata.org/recap-mobilitydata-working-meeting-gtfs-flex-service-discovery/" target="_blank">GTFS-Flexに関する概念的なワーキングミーティングが開催されました</a>
- **2023年7月**: <a href="https://github.com/google/transit/pull/388" target="_blank">Pull Request #388が公開されました</a>
- **2023年8月および9月**: <a href="https://github.com/google/transit/pull/388" target="_blank">「GTFSにおけるGeoJSON？」に関する議論が開催されました</a>
- **2024年3月**: <a href="https://github.com/google/transit/pull/433" target="_blank">GTFS Flexが正式に採用されました</a>
