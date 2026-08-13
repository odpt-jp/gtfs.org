# 構内通路(pathway)と物理的アクセシビリティ {: #pathways-and-physical-accessibility}

## アクセシビリティ情報を表示する理由 {: #why-display-accessibility-information}


**人口の大きな割合に影響します：** 世界保健機関は、[世界の人々の16%が障害を有している](https://www.who.int/news-room/fact-sheets/detail/disability-and-health)と推定しており、障害のある人々は「障害のない人々と比べて、利用しにくく手頃な価格ではない交通機関を利用することが15倍困難である」としています。障害のある人々は、ケアやサービスへのアクセスが制限されることも一因として、[新たな健康状態を獲得する割合も高くなっています](https://www.who.int/publications/i/item/9789240063600)。

**乗客にとって重要です：** 乗客には、利用可能な交通手段に関する最新かつ正確な情報が必要です。多くの事業者はすでに、乗客が旅程を計画し、利用可能な選択肢を理解するうえで重要なルート・路線系統(route)、時刻表、停留所等(stop)の位置情報を表現するために、General Transit Feed Specification (GTFS) を使用しています。アクセシビリティ上のニーズを持つ乗客にとって、停留所等(stop)や車両のアクセシビリティを知ることは、位置を知ることと同じくらい重要です。これらの乗客は、どこかで立ち往生したり、最終停留所等(stop)まで到達できないことに手遅れになってから気付いたりしないよう、旅程のあらゆる部分について知る必要があります。

**法律で定められている場合があります：** 所在地によっては、地域または国の法律により、障害のある人々に平等なアクセスと機会を提供することが求められる場合があります。検討するべき情報源には、以下のようなものがあります。

* **米国：** [Americans and Disabilities Act (ADA)](https://www.ada.gov/topics/intro-to-ada/#public-transit) および 1973年 Rehabilitation Act の [Section 504](https://www.dol.gov/agencies/oasam/centers-offices/civil-rights-center/statutes/section-504-rehabilitation-act-of-1973)
* **日本：** 国土交通省の高齢者、障害者等の移動等の円滑化の促進に関する法律（「[バリアフリー法](https://www.mlit.go.jp/sogoseisaku/barrierfree/index.html)」）
* **欧州連合：** [Employment, Social Affairs & Inclusion](https://ec.europa.eu/social/main.jsp?catId=1485&langId=en)

## アクセシビリティチェックリスト {: #accessibility-checklist}


以下は、データにアクセシビリティ情報を追加するために必要な手順です。次のセクションでは、各手順についてより詳細な情報を提供します。
 
* 手順1: `stops.txt` に車椅子アクセシビリティ情報を追加します
* 手順2: `trips.txt` に車椅子アクセシビリティ情報を追加します
* 手順3: `stops.txt` に音声ナビゲーション情報を追加します
* 手順4: GTFS Pathways を使用して、交通駅に関する物理的アクセシビリティ情報を追加します

## GTFS における車椅子アクセシビリティの追加 {: #adding-wheelchair-accessibility-in-gtfs}


GTFS が一連の .txt ファイルで構成されていることは、すでにご存じかもしれません。車椅子アクセシビリティは、2つのフィールドを更新することで表示できます。`stops.txt` の `wheelchair_boarding` と、`trips.txt` の `wheelchair_accessible` です。

**stops.txt における車椅子アクセシビリティ**   
`stops.txt` のフィールド `wheelchair_boarding` では、指定された場所から車椅子で乗車できるかどうかを示すことができます。

[参照: stops.txt](../../reference/#stopstxt)

このフィールドを空欄のままにすると、アクセシビリティ情報は表示されません。そのため、乗客はアクセシビリティについて確信を持てず、実際に車椅子で乗車できないのか、単に情報が欠けているだけなのかを判断できません。車椅子での乗車が利用できない場合でも、その情報を入力して乗客に明確に伝え、正確な情報に基づいて旅程を計画できるようにすることが最善です。

**trips.txt における車椅子アクセシビリティ**   
`trips.txt` のフィールド `wheelchair_accessible` では、特定の便(trip)で使用される車両が車椅子に対応できるかどうかを示すことができます。

[参照: trips.txt](../../reference/#tripstxt)

`wheelchair_boarding` と同様に、このフィールドを空欄のままにすると、アクセシビリティ情報は表示されません。車両が車椅子に対応していない場合でも、その情報を入力して乗客に明確に伝え、正確な情報に基づいて旅程を計画できるようにすることが最善です。

## 音声ナビゲーション支援の追加 {: #adding-audio-navigation-aids}


読み上げ機能は、GTFS のアクセシビリティを向上させるもう1つの方法です。正確な読み上げ情報により、支援技術を使用してテキストを音声で読み上げる乗客が、正しい情報を取得できるようになります。この情報は、`stops.txt` の `tts_stop_name` を各 `stop_name` に対応するよう更新することで、GTFS に含めることができます。GTFS 内の各停留所等(stop)には、正しく発音できるよう、停留所等(stop)の名称を発音どおりに表記した読み上げ用の曖昧性解消情報を設定するべきです。 

[例: 読み上げ機能](../../examples/text-to-speech)

`tts_stop_name` は現在、GTFS 仕様内で正式に採用されている唯一の読み上げ用フィールド(text-to-speech field)ですが、他のフィールドについても議論されており、追加される可能性があります。これには、`tts_agency_name`、`tts_route_short_name`、`tts_route_long_name`、`tts_trip_headsign`、`tts_trip_short_name`、および `tts_stop_headsign` が含まれます。

乗客がこの情報の恩恵を受けるには、読み上げ機能をサポートするアプリを使用する必要があります。[NaviLensGo](https://www.navilens.com/en/) などの一部のアプリは、視覚障害のある乗客が駅構内を移動し、適切な車両を見つけられるよう支援するために特別に設計されています。

## 駅に関する物理的アクセシビリティ情報の追加 {: #adding-physical-accessibility-information-about-a-station}


GTFS-Pathways は、交通駅の詳細を表現する GTFS のコンポーネントです。これにより乗客は、交通駅で必要な乗り換えを行えるかどうかを理解することができます。 

GTFS-Pathways は、`pathways.txt` で説明される情報を関連付けるために、`pathways.txt` および `levels.txt` ファイルを追加するとともに、`stops.txt` に `location_type` フィールドを追加します。 

<img class="center" src="../../../../assets/pathways-visual.jpg">

### 駅の出入口の位置を記述する {: #describe-the-location-of-station-entrances-and-exits}


GTFS では、出入口および駅構内に関する情報を使用して、駅を正確に記述することができます。この例では、バンクーバー中心部にある Waterfront Station の一部を記述します。この駅は市の Skytrain ネットワークの一部であり、Canada Line、Expo Line、SeaBus、および West Coast Express が運行しています。地上階にある3つの入口により、乗客は駅に出入りできます。駅の残りの部分は地下にあり、運賃確認のためのコンコース階と、プラットフォームのある下層階があります。 

まず、駅およびその入口の位置を [stops.txt](../../reference/#stopstxt) で定義します。

[**stops.txt**](../../reference/#stopstxt)

```
stop_id,stop_name,stop_lat,stop_lon,location_type,parent_station,wheelchair_boarding
12034,Waterfront Station,49.285687,-123.111773,1,,
90,Waterfront Station Stairs Entrance on Granville,49.285054,-123.114375,2,12034,2
91,Waterfront Station Escalator Entrance on Granville,49.285061,-123.114395,2,12034,2
92,Waterfront Station Elevator Entrance on Granville,49.285257,-123.114163,2,12034,1
93,Waterfront Station Entrance on Cordova,49.285607,-123.111993,2,12034,1
94,Waterfront Station Entrance on Howe,49.286898,-123.113367,2,12034,2
```

上記のファイルでは、最初のレコードは駅の位置に関するものであるため、`location_type` は `1` に設定されています。残りの5つは、3つの駅入口に関するものです（Granville の入口には、階段、エスカレーター、エレベーターという実際には3つの別個の入口があるため、5つのレコードが必要です）。これら5つのレコードは、`location_type` が `2` に設定されているため、入口として定義されています。

さらに、Waterfront Station の `stop_id` は、入口を駅に関連付けるために、入口の `parent_station` に記載されています。アクセシブルな入口では `wheelchair_boarding` が `1` に設定され、アクセシブルでない入口では `2` に設定されています。

### 階段とエスカレーターを記述する {: #describe-stairs-and-escalators}


Granville street にある Waterfront Station の入口には、エレベーター、エスカレーター、階段があり、これらの入口は上記の [stops.txt](../../reference/#stopstxt) でノードとして定義されています。入口を駅構内の区画に接続するには、Waterfront Station の `parent_station` の下に、追加のノードを [stops.txt](../../reference/#stopstxt) で作成しなければなりません。以下の [stops.txt](../../reference/#stopstxt) ファイルでは、階段およびエスカレーターの下部に対応する汎用ノード（`location_type 3`）が定義されています。

[**stops.txt**](../../reference/#stopstxt)

```
stop_id,stop_name,stop_lat,stop_lon,location_type,parent_station,wheelchair_boarding
...
95,Waterfront Station Granville Stair Landing, 49.285169,-123.114198,3,12034,
96,Waterfront Station Granville Escalator Landing,49.285183,-123.114222,3,12034,
```

<img class="center" src="../../../../assets/pathways.png" width=700px>

次に、ファイル [pathways.txt](../../reference/#pathwaystxt) を使用してノードをリンクし、構内通路(pathway)を作成します。ここで、最初のレコードは階段の上部および下部に関するノードをリンクします。`pathway_mode` は階段を示すために `2` に設定され、最後のフィールドは乗客が階段を両方向（上りおよび下り）に移動できることを記述しています。 

同様に、2番目のレコードはエスカレーター（`pathway_mode` は `4` に設定）を記述しています。エスカレーターは一方向にしか移動できないため、フィールド `is_bidirectional` は `0` に設定されます。したがって、エスカレーターはノード `96` から `91` へ（一方向、上向きに）移動します。

[**pathways.txt**](../../reference/#pathwaystxt)

```
pathway_id,from_stop_id,to_stop_id_pathway_mode,is_bidirectional
stairsA,90,95,2,1
escalatorA,96,91,4,0
```

### エレベーターと構内通路を記述する {: #describe-elevators-and-pathways}


Granville street のエレベーターは、エスカレーターと階段が終わるコンコース階の構内通路に乗客を運びます。地上階のエレベーターは、すでに上記で駅出入口として定義されています（`stop_id` `92`）。したがって、コンコース階のエレベーター扉も定義する必要があります。 

さらに、下図に示すように、Granville street の階段、エスカレーター、エレベーターの下部と主要駅舎を接続する地下通路があります。そのため、通路区間を定義するために2つの追加ノードを作成します。

<img class="center" src="../../../../assets/pathways-2.png" width=500px>

[**stops.txt**](../../reference/#stopstxt)

```
stop_id,stop_name,stop_lat,stop_lon,location_type,parent_station,wheelchair_boarding
…
97,地下通路の曲がり角,49.286253,-123.112660,3,12034,
98,地下通路の終端,49.286106,-123.112428,3,12034,
99,エレベーター_コンコース,49.285257,-123.114163,3,12034,
```

<img class="center" src="../../../../assets/pathways-3.png" width=500px>

最後に、以下の [pathways.txt](../../reference/#pathwaystxt) ファイルに示すように、ノードを相互に接続して地下構内通路を定義します。

[**pathways.txt**](../../reference/#pathwaystxt)

```
pathway_id,from_stop_id,to_stop_id_pathway_mode,is_bidirectional
underground_walkway1,99,96,1,1
underground_walkway2,96,95,1,1
underground_walkway3,95,97,1,1
underground_walkway4,97,98,1,1
```

## GTFS-Pathways への今後の追加 {: #future-additions-to-gtfs-pathways}


GTFS-Pathways の中核仕様は GTFS に完全に統合されていますが、追加のアクセシビリティ情報をモデル化でき、それが乗客にとって有用であることが認識されています。これには、読み上げ用フィールド(text-to-speech field)の案内、車椅子支援情報、設備故障の報告、計画済みまたは予定された入口または出口の閉鎖、ならびにエレベーターおよびエスカレーターの運休に関する情報が含まれます。残りの部分についての詳細は、[この文書](http://bit.ly/gtfs-pathways)で確認できます。
