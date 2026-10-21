## General Transit Feed Specification リファレンス {: #general-transit-feed-specification-reference}


**2026年4月27日改訂。詳細については[改訂履歴](../change-history/revision-history/)を参照してください。**

この文書は、GTFS データセットを構成するファイルの形式および構造を定義します。

## 目次 {: #table-of-contents}


1.  [文書の規約](#document-conventions)
2.  [データセットファイル](#dataset-files)
3.  [ファイル要件](#file-requirements)
4.  [データセットの公開および一般的な慣行](#dataset-publishing-general-practices)
5.  [フィールド定義](#field-definitions)
    -   [agency.txt](#agencytxt)
    -   [stops.txt](#stopstxt)
    -   [routes.txt](#routestxt)
    -   [trips.txt](#tripstxt)
    -   [stop_times.txt](#stop_timestxt)
    -   [calendar.txt](#calendartxt)
    -   [calendar_dates.txt](#calendar_datestxt)
    -   [fare_attributes.txt](#fare_attributestxt)
    -   [fare_rules.txt](#fare_rulestxt)
    -   [timeframes.txt](#timeframestxt) 
    -   [rider\_categories.txt](#rider_categoriestxt)   
    -   [fare_media.txt](#fare_mediatxt)
    -   [fare_products.txt](#fare_productstxt) 
    -   [fare_leg_rules.txt](#fare_leg_rulestxt)
    -   [fare_leg_join_rules.txt](#fare_leg_join_rulestxt)
    -   [fare_transfer_rules.txt](#fare_transfer_rulestxt)
    -   [areas.txt](#areastxt)
    -   [stop_areas.txt](#stop_areastxt)
    -   [networks.txt](#networkstxt)
    -   [route_networks.txt](#route_networkstxt)
    -   [shapes.txt](#shapestxt)
    -   [frequencies.txt](#frequenciestxt)
    -   [transfers.txt](#transferstxt)
    -   [pathways.txt](#pathwaystxt)
    -   [levels.txt](#levelstxt)
    -   [location_groups.txt](#location_groupstxt)
    -   [location_group_stops.txt](#location_group_stopstxt)
    -   [locations.geojson](#locationsgeojson)
    -   [booking_rules.txt](#booking_rulestxt)
    -   [translations.txt](#translationstxt)
    -   [feed_info.txt](#feed_infotxt)
    -   [attributions.txt](#attributionstxt)

## 文書の規約 {: #document-conventions}

本書におけるキーワード「MUST」、「MUST NOT」、「REQUIRED」、「SHALL」、「SHALL NOT」、「SHOULD」、「SHOULD NOT」、「RECOMMENDED」、「MAY」、および「OPTIONAL」は、[RFC 2119](https://tools.ietf.org/html/rfc2119) に記載されているとおりに解釈されなければなりません。

### 用語の定義 {: #term-definitions}


このセクションでは、この文書全体で使用される用語を定義します。

* **データセット** - この仕様参照で定義される完全なファイル一式です。データセットを変更すると、データセットの新しいバージョンが作成されます。データセットは、zipファイル名を含む公開された恒久的なURLで公開するべきです。（例: https://www.agency.org/gtfs/gtfs.zip）。
* **レコード** - 単一のエンティティ（例: 交通事業者、停留所等(stop)、ルート・路線系統(route)など）を記述する複数の異なるフィールド値から構成される基本的なデータ構造です。表では、行として表されます。
* **フィールド** - オブジェクトまたはエンティティの属性です。表では、列として表されます。フィールドは、ファイル内でヘッダーとして追加されている場合に存在します。フィールド値が定義されている場合も、定義されていない場合もあります。
* **フィールド値** - フィールド内の個々のエントリです。表では、単一のセルとして表されます。
* **運行日(service day)** - 運行日は、ルート・路線系統(route)のスケジュールを示すために使用される期間です。運行日の正確な定義は事業者ごとに異なりますが、運行日は暦日と一致しないことがよくあります。ある日に運行が開始され、翌日に終了する場合、運行日は24:00:00を超えることがあります。例えば、金曜日の08:00:00から土曜日の02:00:00まで運行するサービスは、単一の運行日において08:00:00から26:00:00まで運行すると表記することができます。
* **読み上げ用フィールド(text-to-speech field)** - このフィールドには、その親フィールドと同じ情報を含めるべきです（空の場合は親フィールドにフォールバックします）。これはtext-to-speechとして読み上げられることを目的としているため、略語は削除するべきです（「St」は「Street」または「Saint」として読み上げるべきです。「Elizabeth I」は「Elizabeth the first」とするべきです）。または、そのまま読み上げられるように保持するべきです（「JFK Airport」は略称として発音されます）。
* **乗車区間(leg)** - 乗客が便(trip)に沿った連続する一対の地点間で乗車し、降車する移動です。
* **旅程(journey)** - 出発地から目的地までの全体的な移動であり、すべての乗車区間(leg)およびその間の乗換を含みます。
* **部分旅程(sub-journey)** - 旅程(journey)の部分集合を構成する2つ以上の乗車区間(leg)です。
* **チケット商品** - 移動の支払いまたは検証に使用できる、購入可能な運賃商品です。
* **有効運賃区間(effective fare leg)** - 運賃計算の目的で、[fare_leg_rules.txt](#fare_leg_rulestxt) の照合ルールにおいて単一の乗車区間(leg)として扱うべき、2つ以上の乗車区間(leg)からなる部分旅程(sub-journey)です。

### 存在 {: #presence}

フィールドおよびファイルに適用される存在条件:

* **必須** - フィールドまたはファイルはデータセットに含まれ、各レコードに対して有効な値を含まなければなりません。
* **任意** - フィールドまたはファイルはデータセットから省略することができます。
* **条件付き必須** - フィールドまたはファイルは、フィールドまたはファイルの説明に記載された条件下で含まれなければなりません。
* **条件付き禁止** - フィールドまたはファイルは、フィールドまたはファイルの説明に記載された条件下で含まれてはいけません。
* **推奨** - フィールドまたはファイルはデータセットから省略することができますが、含めることがベストプラクティスです。このフィールドまたはファイルを省略する前に、ベストプラクティスを慎重に評価し、省略によるすべての影響を理解するべきです。

### フィールド型 {: #field-types}


- **Color** - 6桁の16進数でエンコードされた色です。有効な値を生成するには、[https://htmlcolorcodes.com](https://htmlcolorcodes.com)を参照してください（先頭の「#」は含めてはいけません）。 <br> *例: 白の場合は`FFFFFF`、黒の場合は`000000`、またはNYMTAのA、C、E線の場合は`0039A6`。*
- **Currency code** - ISO 4217のアルファベット通貨コードです。現在の通貨の一覧については、[https://en.wikipedia.org/wiki/ISO_4217#Active\_codes](https://en.wikipedia.org/wiki/ISO_4217#Active_codes)を参照してください。 <br> *例: カナダドルの場合は`CAD`、ユーロの場合は`EUR`、日本円の場合は`JPY`。*
- **Currency amount** - 通貨額を示す10進数値です。小数点以下の桁数は、対応するCurrency codeについて[ISO 4217](https://en.wikipedia.org/wiki/ISO_4217#Active_codes)で指定されています。すべての金融計算は、データを利用するプログラミング言語に応じて、decimal、currency、または金融計算に適したその他の同等の型として処理するべきです。計算中に金額の増減が生じるため、通貨額をfloatとして処理することは推奨されません。
- **Date** - YYYYMMDD形式の運行日(service day)です。運行日(service day)内の時刻は24:00:00を超える場合があるため、運行日(service day)には翌日以降の情報が含まれる場合があります。 <br> *例: 2018年9月13日の場合は`20180913`。*
- **Email** - メールアドレスです。 <br> *例: `example@example.com`*
- **Enum** - 「Description」列で定義された事前定義済み定数の集合からの選択肢です。 <br> *例: `route_type`フィールドには、路面電車の場合は`0`、地下鉄の場合は`1`が含まれます...*
- **ID** - IDフィールド値は内部IDであり、乗客に表示することを意図したものではなく、任意のUTF-8文字の並びです。印字可能なASCII文字のみを使用することが推奨されます。ファイル内で一意でなければならない場合、IDには「unique ID」とラベル付けされます。1つの.txtファイルで定義されたIDは、別の.txtファイルで参照されることがよくあります。別のテーブル内のIDを参照するIDには、「foreign ID」とラベル付けされます。<br> *例: [stops.txt](#stopstxt)の`stop_id`フィールドは「unique ID」です。[stops.txt](#stopstxt)の`parent_station`フィールドは「`stops.stop_id`を参照するforeign ID」です。*
- **Language code** - IETF BCP 47言語コードです。IETF BCP 47の概要については、[https://www.rfc-editor.org/info/rfc5646/](https://www.rfc-editor.org/info/rfc5646/)および[http://www.w3.org/International/articles/language-tags/](http://www.w3.org/International/articles/language-tags/)を参照してください。 <br> *例: 英語の場合は`en`、アメリカ英語の場合は`en-US`、ドイツ語の場合は`de`。*
- **Latitude** - 10進度によるWGS84緯度です。値は-90.0以上90.0以下でなければなりません。*<br> 例: ローマのコロッセオの場合は`41.890169`。*
- **Longitude** - 10進度によるWGS84経度です。値は-180.0以上180.0以下でなければなりません。 <br> *例: ローマのコロッセオの場合は`12.492269`。*
- **Float** - 浮動小数点数です。
- **Integer** - 整数です。
- **Phone number** - 電話番号です。
- **Time** - HH:MM:SS形式の時刻です（H:MM:SSも受け入れられます）。時刻は、運行日(service day)の「正午から12時間前」を基準として計測されます（夏時間の変更が発生する日を除き、実質的には午前0時です）。運行日(service day)の午前0時以降に発生する時刻については、HH:MM:SS形式で24:00:00より大きい値として時刻を入力してください。 <br> *例: 午後2時30分の場合は`14:30:00`、翌日の午前1時35分の場合は`25:35:00`。*
- **Local time** - HH:MM:SS形式の時刻です（H:MM:SSも受け入れられます）。指定された場所の現地時刻で表示される時計時刻を表します。
- **Text** - 表示を目的としたUTF-8文字列であり、したがって人間が読めるものでなければなりません。
- **Timezone** - [https://www.iana.org/time-zones](https://www.iana.org/time-zones)のTZタイムゾーンです。タイムゾーン名にはスペース文字は含まれませんが、アンダースコアを含む場合があります。有効な値の一覧については、[http://en.wikipedia.org/wiki/List\_of\_tz\_zones](http://en.wikipedia.org/wiki/List\_of\_tz\_zones)を参照してください。 <br> *例: `Asia/Tokyo`、`America/Los_Angeles`、または`Africa/Cairo`。*
- **URL** - http://またはhttps://を含む完全修飾URLです。URL内の特殊文字は正しくエスケープされなければなりません。完全修飾URL値の作成方法については、次の[http://www.w3.org/Addressing/URL/4\_URI\_Recommentations.html](http://www.w3.org/Addressing/URL/4\_URI\_Recommentations.html)を参照してください。

### フィールド記号 {: #field-signs}

Float または Integer フィールド型に適用される記号:

* **非負** - 0 以上。
* **非ゼロ** - 0 と等しくありません。
* **正** - 0 より大きい。

_例: **非負の float** - 0 以上の浮動小数点数。_

### データセット属性 {: #dataset-attributes}

データセットの**主キー**は、行を一意に識別するフィールド、またはフィールドの組み合わせです。`Primary key (*)` は、ファイルに提供されるすべてのフィールドが行を一意に識別するために使用される場合に用いられます。`Primary key (none)` は、そのファイルで許可される行が1行のみであることを意味します。 

_例: `trip_id` および `stop_sequence` フィールドは、[stop_times.txt](#stop_timestxt) の主キーを構成します。_

## データセットファイル {: #dataset-files}


この仕様では、以下のファイルを定義します。

|  ファイル名 | 存在要件 | 説明 |
|  ------ | ------ | ------ |
|  [agency.txt](#agencytxt) | **必須** | このデータセットでサービスが表現される交通事業者。 |
| [stops.txt](#stopstxt) | **条件付き必須** | 車両が乗客を乗車または降車させる停留所等(stop)。駅および駅入口も定義します。 <br><br>条件付き必須:<br> - デマンド型ゾーンが [locations.geojson](#locationsgeojson) で定義されている場合は任意。 <br>- それ以外の場合は**必須**。 |
|  [routes.txt](#routestxt) | **必須** | 交通ルート・路線系統(route)。ルート・路線系統(route)は、乗客に単一のサービスとして表示される便(trip)のグループです。 |
|  [trips.txt](#tripstxt)  | **必須** | 各ルート・路線系統(route)の便(trip)。便(trip)は、特定の期間中に発生する2つ以上の停留所等(stop)の連続です。 |
|  [stop_times.txt](#stop_timestxt) | **必須** | 各便(trip)について、車両が停留所等(stop)に到着および出発する時刻。 |
|  [calendar.txt](#calendartxt)  | **条件付き必須** | 開始日および終了日を伴う週間スケジュールを使用して指定される運行日(service day)。 <br><br>条件付き必須:<br> - すべての運行日(service day)が [calendar_dates.txt](#calendar_datestxt) で定義されている場合を除き、**必須**。<br> - それ以外の場合は任意。 |
|  [calendar_dates.txt](#calendar_datestxt)  | **条件付き必須** | [calendar.txt](#calendartxt) で定義されたサービスの例外。 <br><br>条件付き必須:<br> - [calendar.txt](#calendartxt) が省略される場合は**必須**。この場合、[calendar_dates.txt](#calendar_datestxt) にはすべての運行日(service day)を含めなければなりません。 <br> - それ以外の場合は任意。 |
|  [fare_attributes.txt](#fare_attributestxt)  | 任意 | 交通事業者のルート・路線系統(route)に関する運賃情報。 |
|  [fare_rules.txt](#fare_rulestxt)  | 任意 | 旅程(journey)に運賃を適用するためのルール。 |
|  [timeframes.txt](#timeframestxt)  | 任意 | 日付および時刻の要因に依存する運賃について、運賃ルールで使用する日付および時刻の期間。 |
| [rider_categories.txt](#rider_categoriestxt) | 任意 | 乗客のカテゴリ（例: 高齢者、学生）を定義します。 |
|  [fare_media.txt](#fare_mediatxt)  | 任意 | チケット商品を利用するために使用できるチケットメディアを記述します。 <br><br>ファイル [fare_media.txt](#fare_mediatxt) は、[fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) では表現されない概念を記述します。そのため、[fare_media.txt](#fare_mediatxt) の使用は、ファイル [fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) とは完全に独立しています。 |
|  [fare_products.txt](#fare_productstxt)  | 任意 | 乗客が購入できるさまざまな種類のチケットまたは運賃を記述します。<br><br>ファイル [fare_products.txt](#fare_productstxt) は、[fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) では表現されないチケット商品を記述します。そのため、[fare_products.txt](#fare_productstxt) の使用は、ファイル [fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) とは完全に独立しています。 |
|  [fare_leg_rules.txt](#fare_leg_rulestxt)  | 任意 | 個々の乗車区間(leg)に対する運賃ルール。<br><br>ファイル [fare_leg_rules.txt](#fare_leg_rulestxt) は、運賃体系をモデル化するためのより詳細な方法を提供します。そのため、[fare_leg_rules.txt](#fare_leg_rulestxt) の使用は、ファイル [fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) とは完全に独立しています。 |
|  [fare_leg_join_rules.txt](#fare_leg_join_rulestxt)  | 任意 | [fare_leg_rules.txt](#fare_leg_rulestxt) のルールとの照合を目的として、2つ以上の乗車区間(leg)を単一の**有効運賃区間(effective fare leg)**と見なすべきかを定義するルール。|
|  [fare_transfer_rules.txt](#fare_transfer_rulestxt)  | 任意 | 乗車区間(leg)間の乗換に対する運賃ルール。<br><br>[fare_leg_rules.txt](#fare_leg_rulestxt) とともに、ファイル [fare_transfer_rules.txt](#fare_transfer_rulestxt) は、運賃体系をモデル化するためのより詳細な方法を提供します。そのため、[fare_transfer_rules.txt](#fare_transfer_rulestxt) の使用は、ファイル [fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) とは完全に独立しています。 |
|  [areas.txt](#areastxt) | 任意 | 場所のエリアグループ。 |
|  [stop_areas.txt](#stop_areastxt) | 任意 | 停留所等(stop)をエリアに割り当てるルール。 |
|  [networks.txt](#networkstxt) | **条件付き禁止** | ルート・路線系統(route)のネットワークグループ。<br><br>条件付き禁止:<br>- [routes.txt](#routestxt) に `network_id` が存在する場合は**禁止**。<br>- それ以外の場合は任意。 |
|  [route_networks.txt](#route_networkstxt) | **条件付き禁止** | ルート・路線系統(route)をネットワークに割り当てるルール。<br><br>条件付き禁止:<br>- [routes.txt](#routestxt) に `network_id` が存在する場合は**禁止**。<br>- それ以外の場合は任意。 |
|  [shapes.txt](#shapestxt)  | **条件付き必須** | 車両の走行経路をマッピングするためのルール。ルートアラインメントと呼ばれることもあります。<br><br>条件付き必須: <br>- [routes.txt](#routestxt) または [stop_times.txt](#stop_timestxt) のいずれかで、便(trip)に連続的な乗車または降車の動作が定義されている場合は**必須**。 <br>- それ以外の場合は推奨。 |
|  [frequencies.txt](#frequenciestxt)  | 任意 | 間隔ベースのサービスにおける運行間隔（便(trip)間の時間）、または固定スケジュールサービスの圧縮表現。 |
|  [transfers.txt](#transferstxt)  | 任意 | ルート・路線系統(route)間の乗換地点における接続を行うためのルール。 |
|  [pathways.txt](#pathwaystxt)  | 任意 | 駅構内の場所を相互に接続する構内通路(pathway)。 |
|  [levels.txt](#levelstxt)  | **条件付き必須** | 駅構内の階層。<br><br>条件付き必須:<br>- エレベーターを伴う構内通路(pathway)（`pathway_mode=5`）を記述する場合は**必須**。<br>- それ以外の場合は任意。 |
|  [location_groups.txt](#location_groupstxt)  | 任意 | 乗客が乗車または降車をリクエストできる場所をまとめて示す停留所等(stop)のグループ。 |
|  [location_group_stops.txt](#location_group_stopstxt)  | 任意 | 停留所等(stop)をロケーショングループに割り当てるルール。 |
|  [locations.geojson](#locationsgeojson)  | 任意 | オンデマンドサービスによる乗客の乗車または降車リクエストのためのゾーン。GeoJSONポリゴンとして表現されます。 |
|  [booking_rules.txt](#booking_rulestxt)  | 任意 | 乗客がリクエストするサービスの予約情報。 |
|  [translations.txt](#translationstxt)  | 任意 | 顧客向けデータセット値の翻訳。 |
| [feed_info.txt](#feed_infotxt) | **条件付き必須** | 発行者、バージョン、有効期限情報を含むデータセットメタデータ。<br><br>条件付き必須:<br>- [translations.txt](#translationstxt) が提供される場合は**必須**。<br>- それ以外の場合は推奨。|
|  [attributions.txt](#attributionstxt)  | 任意 | データセットの帰属情報。 |

## ファイル要件 {: #file-requirements}


以下の要件が、データセットファイルの形式および内容に適用されます。

* すべてのファイルは、カンマ区切りのテキストとして保存しなければなりません。
* 各ファイルの1行目には、フィールド名を含めなければなりません。[フィールド定義](#field-definitions)セクションの各サブセクションは、GTFSデータセット内のファイルのいずれか1つに対応しており、そのファイルで使用できるフィールド名を一覧表示しています。
* すべてのファイル名およびフィールド名では、大文字と小文字が区別されます。
* フィールド値には、タブ、キャリッジリターン、または改行を含めてはいけません。
* 引用符またはカンマを含むフィールド値は、引用符で囲まなければなりません。さらに、フィールド値内の各引用符の前には、引用符を付けなければなりません。これは、Microsoft Excelがカンマ区切り（CSV）ファイルを出力する方法と一致しています。CSVファイル形式の詳細については、[https://tools.ietf.org/html/rfc4180](https://tools.ietf.org/html/rfc4180)を参照してください。
以下の例は、フィールド値がカンマ区切りファイル内でどのように表示されるかを示しています。
  * **元のフィールド値:** `Contains "quotes", commas and text`
  * **CSVファイル内のフィールド値:** `"Contains ""quotes"", commas and text"`
* フィールド値には、HTMLタグ、コメント、またはエスケープシーケンスを含めてはいけません。
* フィールドまたはフィールド名の間にある余分なスペースは削除するべきです。多くのパーサーはスペースを値の一部と見なすため、エラーが発生する可能性があります。
* 各行は、CRLFまたはLFの改行文字で終わらなければなりません。
* すべてのUnicode文字をサポートするため、ファイルはUTF-8でエンコードするべきです。Unicode byte-order mark（BOM）文字を含むファイルも許容されます。BOM文字およびUTF-8の詳細については、[http://unicode.org/faq/utf_bom.html#BOM](http://unicode.org/faq/utf_bom.html#BOM)を参照してください。
* すべてのデータセットファイルは、まとめてzip化しなければなりません。ファイルはサブフォルダ内ではなく、ルートレベルに直接配置しなければなりません。
* すべての顧客向けテキスト文字列（停留所等(stop)名、ルート・路線系統(route)名、行先表示(headsign)を含む）は、小文字を表示できる表示装置における地名の大文字化に関する地域の慣例に従い、Mixed Case（ALL CAPSではない）を使用するべきです（例: 「Brighton Churchill Square」、「Villiers-sur-Marne」、「Market Street」）。
* 名前およびその他のテキストについては、略称で呼ばれる場所（例: 「JFK Airport」）を除き、フィード全体で略語の使用を避けるべきです（例: StreetをSt.とすること）。略語は、スクリーンリーダーソフトウェアおよび音声ユーザーインターフェースによるアクセシビリティに問題を生じさせる可能性があります。利用ソフトウェアは、表示のために完全な単語を略語へ確実に変換できるよう設計できますが、略語から完全な単語への変換は、より高いエラーリスクを伴います。

## データセットの公開と一般的な慣行 {: #dataset-publishing-general-practices}


* データセットは、zipファイル名を含む公開された恒久的なURLで公開するべきです（例: www.agency.org/gtfs/gtfs.zip）。利用するソフトウェアアプリケーションによるダウンロードを容易にするため、理想的には、ファイルへのアクセスにログインを要求せず、URLから直接ダウンロードできるようにするべきです。GTFSデータセットをオープンにダウンロード可能にすることが推奨され（かつ最も一般的な慣行です）、データ提供者がライセンスその他の理由によりGTFSへのアクセスを制御する必要がある場合は、自動ダウンロードを容易にするAPIキーを使用してGTFSデータセットへのアクセスを制御することが推奨されます。
* GTFSデータは、安定した場所にある単一のファイルが常に交通事業者（または複数の事業者）のサービスに関する最新の公式記述を含むよう、反復的に公開するべきです。
* データセットは、可能な限りデータの反復間で`stop_id`、`route_id`、および`agency_id`の永続的な識別子（idフィールド）を維持するべきです。
* 1つのGTFSデータセットには、現在および今後のサービス（「マージ済み」データセットと呼ばれることがあります）を含めるべきです。2つの異なるGTFSフィードからマージ済みデータセットを作成するために使用できる、複数の[マージツール](../../../resources/gtfs/#gtfs-merge-tools)があります。
    * 公開されるGTFSデータセットは、常に少なくとも今後7日間有効であるべきであり、理想的には、運行事業者がスケジュールが継続して運行されると確信している期間と同じ長さの期間、有効であるべきです。
    * 可能であれば、GTFSデータセットは少なくとも今後30日間のサービスを対象とするべきです。
 * 古いサービス（期限切れのcalendar）はフィードから削除するべきです。
 * サービスの変更が7日以内に発効する場合、このサービス変更は静的GTFSデータセットではなく、GTFS-realtimeフィード（サービスアドバイザリまたは便の更新(trip update)）を通じて表現するべきです。
 * GTFSデータをホストするweb-serverは、ファイルの更新日を正しく報告するように設定するべきです（[HTTP/1.1 - Request for Comments 2616、Section 14.29](https://tools.ietf.org/html/rfc2616#section-14.29)を参照してください）。

## フィールド定義 {: #field-definitions}

### agency.txt {: #agencytxt}


ファイル: **必須**

主キー (`agency_id`)

|  フィールド名 | 型 | 存在要件 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `agency_id` | 一意のID | **条件付き必須** | 多くの場合、交通事業者と同義である交通ブランドを識別します。単一の事業者が複数の独立したサービスを運営する場合など、一部のケースでは、事業者とブランドは異なります。この文書では、「brand」の代わりに「agency」という用語を使用します。データセットには複数の事業者のデータを含めることができます。<br><br>条件付き必須:<br>- データセットに複数の交通事業者のデータが含まれる場合は**必須**です。<br>- それ以外の場合は推奨です。 |
|  `agency_name` | テキスト | **必須** | 交通事業者の正式名称です。 |
|  `agency_url` | URL | **必須** | 交通事業者のURLです。 |
|  `agency_timezone` | タイムゾーン | **必須** | 交通事業者が所在するタイムゾーンです。データセット内で複数の事業者が指定されている場合、それぞれが同じ`agency_timezone`を持たなければなりません。 |
|  `agency_lang` | 言語コード | 任意 | この交通事業者が使用する主要言語です。GTFSコンシューマーがデータセットの大文字・小文字の規則やその他の言語固有の設定を選択するのに役立つため、提供するべきです。 |
|  `agency_phone` | 電話番号 | 任意 | 指定された事業者の音声電話番号です。このフィールドは、事業者のサービス提供地域で一般的な形式で電話番号を示す文字列値です。番号の数字をグループ化するための句読点を含めることができます。ダイヤル可能なテキスト（例: TriMetの「503-238-RIDE」）は許可されますが、このフィールドにはその他の説明的なテキストを含めてはいけません。 |
|  `agency_fare_url` | URL | 任意 | 乗客がその事業者のチケットまたはその他の運賃手段を購入できるWebページのURL、またはその事業者の運賃に関する情報を含むWebページのURLです。 |
|  `agency_email` | Email | 任意 | 事業者のカスタマーサービス部門が積極的に監視しているメールアドレスです。このメールアドレスは、交通機関の乗客が事業者のカスタマーサービス担当者に連絡できる直接の連絡先であるべきです。 |
|  `cemv_support` | Enum | 任意 | 乗客が、運賃検証機において、非接触EMV（Europay、Mastercard、Visa）カードまたはモバイルデバイスをチケットメディアとして使用することで、この事業者に関連付けられた交通サービス（すなわち、便(trip)）を利用できるかどうかを示します（pay-as-you-goまたはopen-loopシステムなど）。このフィールドは、cEMVを使用して他のチケット商品を購入したり、別のチケットメディアに価値を追加したりできることを示すものではありません。<br><br>cEMVのサポートは、この事業者のすべてのサービスが、cEMVカードまたはモバイルデバイスをチケットメディアとして使用して利用可能な場合にのみ示すべきです。<br><br>有効な選択肢は次のとおりです。<br><br>`0`または空 - この事業者に関連付けられた便(trip)に関するcEMV情報はありません。<br>`1` - 乗客は、この事業者に関連付けられた便(trip)のチケットメディアとしてcEMVを使用することができます。<br>`2` - この事業者に関連付けられた便(trip)のチケットメディアとしてcEMVはサポートされていません。<br><br>同じサービスに対して`agency.cemv_support`と`routes.cemv_support`の両方が提供される場合、`routes.cemv_support`の値が優先されなければなりません。<br><br>このフィールドは、他のすべての運賃関連ファイルから独立しており、個別に使用することができます。このフィールドと運賃関連ファイル（[fare_media.txt](#fare_mediatxt)、[fare_products.txt](#fare_productstxt)、[fare_leg_rules.txt](#fare_leg_rulestxt)など）の間に矛盾する情報がある場合、それらのファイル内の情報が`agency.cemv_support`より優先されなければなりません。 |

### stops.txt {: #stopstxt}


ファイル: **条件付き必須**

主キー (`stop_id`)

|  フィールド名 | 型 | 存在要件 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `stop_id` | 一意の ID | **必須** | 場所を識別します: 停留所等(stop)/プラットフォーム、駅、出入口、汎用ノード、または乗車エリア（`location_type` を参照）。<br><br>ID は、すべての `stops.stop_id`、locations.geojson の `id`、および `location_groups.location_group_id` の値全体で一意でなければなりません。<br><br>複数のルート・路線系統(route)が同じ `stop_id` を使用することができます。 |
|  `stop_code` | テキスト | 任意 | 乗客が場所を識別するための短いテキストまたは番号です。これらのコードは、乗客が特定の場所に関する情報を容易に取得できるよう、電話ベースの交通情報システムで使用されたり、案内表示に印刷されたりすることがよくあります。`stop_code` が一般公開される場合、`stop_id` と同じにすることができます。乗客に提示されるコードがない場所では、このフィールドを空にするべきです。 |
|  `stop_name` | テキスト | **条件付き必須** | 場所の名称です。`stop_name` は、時刻表に印刷される、オンラインで公開される、または案内表示で示される、事業者が乗客向けに使用する場所の名称と一致するべきです。他言語への翻訳には、[translations.txt](#translationstxt) を使用してください。<br><br>場所が乗車エリア（`location_type=4`）である場合、`stop_name` には事業者が表示する乗車エリアの名称を含めるべきです。これは、欧州の一部の都市間鉄道駅のように1文字だけの場合もあれば、「Wheelchair boarding area」（NYC の Subway）や「Head of short trains」（Paris の RER）のようなテキストの場合もあります。<br><br>条件付き必須:<br>- 停留所等(stop)（`location_type=0`）、駅（`location_type=1`）、または出入口（`location_type=2`）である場所では**必須**です。<br>- 汎用ノード（`location_type=3`）または乗車エリア（`location_type=4`）である場所では任意です。|
|  `tts_stop_name` | テキスト | 任意 | `stop_name` の読み上げ可能なバージョンです。詳細については、[用語定義](#term-definitions)の「読み上げ用フィールド(text-to-speech field)」を参照してください。 |
|  `stop_desc` | テキスト | 任意 | 有用で質の高い情報を提供する場所の説明です。`stop_name` と重複するべきではありません。|
|  `stop_lat` | 緯度 | **条件付き必須** | 場所の緯度です。<br><br>停留所等(stop)/プラットフォーム（`location_type=0`）および乗車エリア（`location_type=4`）について、座標は、バス停ポールが存在する場合はその座標、存在しない場合は乗客が車両に乗車する場所（歩道またはプラットフォーム上であり、車両が停車する車道または線路上ではありません）の座標でなければなりません。<br><br>条件付き必須:<br>- 停留所等(stop)（`location_type=0`）、駅（`location_type=1`）、または出入口（`location_type=2`）である場所では**必須**です。<br>- 汎用ノード（`location_type=3`）または乗車エリア（`location_type=4`）である場所では任意です。|
|  `stop_lon` | 経度 | **条件付き必須** | 場所の経度です。<br><br>停留所等(stop)/プラットフォーム（`location_type=0`）および乗車エリア（`location_type=4`）について、座標は、バス停ポールが存在する場合はその座標、存在しない場合は乗客が車両に乗車する場所（歩道またはプラットフォーム上であり、車両が停車する車道または線路上ではありません）の座標でなければなりません。<br><br>条件付き必須:<br>- 停留所等(stop)（`location_type=0`）、駅（`location_type=1`）、または出入口（`location_type=2`）である場所では**必須**です。<br>- 汎用ノード（`location_type=3`）または乗車エリア（`location_type=4`）である場所では任意です。 |
|  `zone_id` | ID | 任意 | 停留所等(stop)の運賃ゾーンを識別します。このレコードが駅または駅の出入口を表す場合、`zone_id` は無視されます。|
|  `stop_url` | URL | 任意 | 場所に関する Web ページの URL です。これは `agency.agency_url` および `routes.route_url` のフィールド値とは異なるべきです。 |
|  `location_type` | 列挙型 | 任意 | 場所の種類です。有効な選択肢は次のとおりです。<br><br>`0`（または空） - **停留所等(stop)**（または**プラットフォーム**）。乗客が交通車両に乗車または降車する場所です。`parent_station` 内で定義される場合はプラットフォームと呼ばれます。<br>`1` - **駅**。1つ以上のプラットフォームを含む物理的な構造物または区域です。<br>`2` - **出入口**。乗客が道路から駅に入場または駅から退出できる場所です。出入口が複数の駅に属する場合、構内通路(pathway)によって両方にリンクすることができますが、データ提供者はそのうち1つを親として選択しなければなりません。<br>`3` - **汎用ノード**。駅構内にある場所で、他のいずれの `location_type` にも該当せず、[pathways.txt](#pathwaystxt) で定義される構内通路(pathway)を相互に接続するために使用することができます。<br>`4` - **乗車エリア**。プラットフォーム上の特定の場所で、乗客が車両に乗車および/または降車できる場所です。|
|  `parent_station` | `stops.stop_id` を参照する外部 ID | **条件付き必須** | [stops.txt](#stopstxt) で定義される異なる場所間の階層を定義します。親となる場所の ID を、次のように含みます。<br><br>- **停留所等(stop)/プラットフォーム**（`location_type=0`）: `parent_station` フィールドには駅の ID を含めます。<br>- **駅**（`location_type=1`）: このフィールドは空でなければなりません。<br>- **出入口**（`location_type=2`）または**汎用ノード**（`location_type=3`）: `parent_station` フィールドには駅（`location_type=1`）の ID を含めます。<br>- **乗車エリア**（`location_type=4`）: `parent_station` フィールドにはプラットフォームの ID を含めます。<br><br>条件付き必須:<br>- 出入口（`location_type=2`）、汎用ノード（`location_type=3`）、または乗車エリア（`location_type=4`）である場所では**必須**です。<br>- 停留所等(stop)/プラットフォーム（`location_type=0`）では任意です。<br>- 駅（`location_type=1`）では禁止です。|
|  `stop_timezone` | タイムゾーン | 任意 | 場所のタイムゾーンです。場所に親駅がある場合、独自のタイムゾーンを適用する代わりに親駅のタイムゾーンを継承します。`stop_timezone` が空の駅および親を持たない停留所等(stop)は、`agency.agency_timezone` で指定されたタイムゾーンを継承します。[stop_times.txt](#stop_timestxt) で提供される時刻は、`stop_timezone` ではなく `agency.agency_timezone` で指定されたタイムゾーンです。これにより、便(trip)がどのタイムゾーンを通過するかにかかわらず、便(trip)内の時刻値が常に増加することが保証されます。 |
|  `wheelchair_boarding` | 列挙型 | 任意 | その場所から車椅子で乗車できるかどうかを示します。有効な選択肢は次のとおりです。<br><br>親を持たない停留所等(stop)の場合:<br>`0` または空 - 停留所等(stop)に関するアクセシビリティ情報はありません。<br>`1` - この停留所等(stop)では、一部の車両に車椅子の乗客が乗車できます。<br>`2` - この停留所等(stop)では車椅子での乗車はできません。<br><br>子停留所等(stop)の場合:<br>`0` または空 - 親駅で指定されている場合、停留所等(stop)は親駅から `wheelchair_boarding` の動作を継承します。<br>`1` - 駅の外部から特定の停留所等(stop)/プラットフォームまで、アクセシブルな経路が存在します。<br>`2` - 駅の外部から特定の停留所等(stop)/プラットフォームまで、アクセシブルな経路は存在しません。<br><br>駅の出入口の場合:<br>`0` または空 - 親駅に指定されている場合、駅の出入口は親駅から `wheelchair_boarding` の動作を継承します。<br>`1` - 駅の出入口は車椅子で利用可能です。<br>`2` - 駅の出入口から停留所等(stop)/プラットフォームまでのアクセシブルな経路はありません。 |
|  `level_id` | `levels.level_id` を参照する外部 ID | 任意 | 場所の階層です。同じ階層を、リンクされていない複数の駅で使用することができます。|
|  `platform_code` | テキスト | 任意 | プラットフォーム停留所等(stop)（駅に属する停留所等(stop)）のプラットフォーム識別子です。これはプラットフォーム識別子のみ（例: "G" または "3"）とするべきです。「platform」や「track」（またはフィードの言語に固有の同等語）などの語を含めるべきではありません。これにより、フィード利用者はプラットフォーム識別子を他言語へより容易に国際化およびローカライズできます。 |
|  `stop_access` | 列挙型 | **条件付き禁止** | 特定の駅において停留所等(stop)へどのようにアクセスするかを示します。有効な選択肢は次のとおりです。<br><br>`0` - 停留所等(stop)/プラットフォームには道路ネットワークから直接アクセスできません。駅に出入口が定義されている場合は駅の出入口から、そうでない場合は駅自体からアクセスしなければなりません。駅に構内通路(pathway)が定義されている場合、それらを使用して停留所等(stop)/プラットフォームにアクセスしなければなりません。<br>`1` - 利用アプリケーションは、親駅の出入口または構内通路(pathway)に依存せず、停留所等(stop)へ直接アクセスするための経路案内を生成するべきです。<br><br>`stop_access` が空の場合、指定された停留所等(stop)またはプラットフォームへのアクセスは未定義と見なされます。<br><br>**条件付き禁止**:<br>- 駅（`location_type=1`）、出入口（`location_type=2`）、汎用ノード（`location_type=3`）、または乗車エリア（`location_type=4`）である場所では**禁止**です。<br>- `parent_station` が空の場合は**禁止**です。<br> - それ以外では任意です。 |

### routes.txt {: #routestxt}


ファイル: **必須**

主キー (`route_id`)

|  フィールド名 | 型 | 存在要件 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `route_id` | 一意の ID | **必須** | ルート・路線系統(route)を識別します。 |
|  `agency_id` | `agency.agency_id` を参照する外部 ID | **条件付き必須** | 指定されたルート・路線系統(route)の事業者です。<br><br>条件付き必須:<br>- [agency.txt](#agencytxt) で複数の事業者が定義されている場合は**必須**です。<br>- それ以外の場合は推奨です。 |
|  `route_short_name` | テキスト | **条件付き必須** | ルート・路線系統(route)の短縮名です。多くの場合、乗客がルート・路線系統(route)を識別するために使用する短く抽象的な識別子（例: 「32」、「100X」、「Green」）です。`route_short_name` と `route_long_name` の両方を定義することができます。<br><br>条件付き必須:<br>- `routes.route_long_name` が空の場合は**必須**です。<br>- 簡潔なサービス指定がある場合は推奨です。これは一般に知られているサービスの乗客向け名称とするべきであり、12文字を超えるべきではありません。 |
|  `route_long_name` | テキスト | **条件付き必須** | ルート・路線系統(route)の正式名称です。この名称は一般に `route_short_name` よりも説明的であり、多くの場合、ルート・路線系統(route)の目的地または停留所等(stop)を含みます。`route_short_name` と `route_long_name` の両方を定義することができます。<br><br>条件付き必須:<br>- `routes.route_short_name` が空の場合は**必須**です。<br>- それ以外の場合は任意です。 |
|  `route_desc` | テキスト | 任意 | 有用で質の高い情報を提供するルート・路線系統(route)の説明です。`route_short_name` または `route_long_name` と重複するべきではありません。 <hr> _例: 「『A』列車は、常時、マンハッタンのInwood-207 StとクイーンズのFar Rockaway-Mott Avenueの間を運行します。また、おおむね午前6時から深夜頃までは、Inwood-207 StとLefferts Boulevardの間でも追加の『A』列車が運行します（列車は通常、Lefferts BlvdとFar Rockawayを交互に運行します）。」_ |
|  `route_type` | 列挙型 | **必須** | ルート・路線系統(route)で使用される交通手段の種類を示します。有効な選択肢は次のとおりです。<br><br>`0` - 路面電車、ストリートカー、ライトレール。都市圏内のあらゆるライトレールまたは地上レベルのシステム。<br>`1` - 地下鉄、メトロ。都市圏内のあらゆる地下鉄道システム。<br>`2` - 鉄道。都市間または長距離の移動に使用されます。<br>`3` - バス。短距離および長距離のバス路線に使用されます。<br>`4` - フェリー。短距離および長距離の船舶サービスに使用されます。<br>`5` - ケーブル路面電車。ケーブルが車両の下を通る地上レベルの鉄道車両に使用されます（例: サンフランシスコのケーブルカー）。<br>`6` - 索道、懸垂式ケーブルカー（例: ゴンドラリフト、ロープウェイ）。キャビン、車両、ゴンドラまたは開放型チェアが1本以上のケーブルによって吊り下げられるケーブル輸送です。<br>`7` - ケーブルカー。急勾配向けに設計されたあらゆる鉄道システム。<br>`11` - トロリーバス。ポールを使用して架線から電力を得る電気バス。<br>`12` - モノレール。軌道が単一のレールまたは梁で構成される鉄道。 |
|  `route_url` | URL | 任意 | 特定のルート・路線系統(route)に関する Web ページの URL です。`agency.agency_url` の値とは異なるべきです。 |
|  `route_color` | 色 | 任意 | 公開資料と一致するルート・路線系統(route)の色指定です。省略または空の場合、デフォルトは白（`FFFFFF`）です。白黒画面で表示した場合に、`route_color` と `route_text_color` の色の差は十分なコントラストを提供するべきです。 |
|  `route_text_color` | 色 | 任意 | `route_color` の背景に対して描画されるテキストに使用する、判読可能な色です。省略または空の場合、デフォルトは黒（`000000`）です。白黒画面で表示した場合に、`route_color` と `route_text_color` の色の差は十分なコントラストを提供するべきです。 |
|  `route_sort_order` | 非負整数 | 任意 | 乗客への表示に最適な方法でルート・路線系統(route)を並べます。`route_sort_order` の値が小さいルート・路線系統(route)を先に表示するべきです。 |
|  `continuous_pickup` | 列挙型 | **条件付き禁止** | [shapes.txt](#shapestxt) で記述される車両の走行経路に沿った任意の地点で、乗客がこのルート・路線系統(route)のすべての便(trip)に乗車できることを示します。有効な選択肢は次のとおりです。<br><br>`0` - 連続停車による乗車。<br>`1` または空 - 連続停車による乗車なし。<br>`2` - 連続停車による乗車を手配するために事業者へ電話しなければなりません。<br>`3` - 連続停車による乗車を手配するために運転手と調整しなければなりません。<br><br>`routes.continuous_pickup` の値は、ルート・路線系統(route)に沿った特定の停車時刻(stop_time)について `stop_times.continuous_pickup` に値を定義することで上書きすることができます。<br><br>**条件付き禁止**:<br>- このルート・路線系統(route)のいずれかの便(trip)について `stop_times.start_pickup_drop_off_window` または `stop_times.end_pickup_drop_off_window` が定義されている場合、`1` または空以外の値は**禁止**です。<br> - それ以外の場合は任意です。 |
|  `continuous_drop_off` | 列挙型 | **条件付き禁止** | [shapes.txt](#shapestxt) で記述される車両の走行経路に沿った任意の地点で、乗客がこのルート・路線系統(route)のすべての便(trip)から降車できることを示します。有効な選択肢は次のとおりです。<br><br>`0` - 連続停車による降車。<br>`1` または空 - 連続停車による降車なし。<br>`2` - 連続停車による降車を手配するために事業者へ電話しなければなりません。<br>`3` - 連続停車による降車を手配するために運転手と調整しなければなりません。<br><br>`routes.continuous_drop_off` の値は、ルート・路線系統(route)に沿った特定の停車時刻(stop_time)について `stop_times.continuous_drop_off` に値を定義することで上書きすることができます。<br><br>**条件付き禁止**:<br>- このルート・路線系統(route)のいずれかの便(trip)について `stop_times.start_pickup_drop_off_window` または `stop_times.end_pickup_drop_off_window` が定義されている場合、`1` または空以外の値は**禁止**です。<br> - それ以外の場合は任意です。 |
| `network_id` | ID | **条件付き禁止** | ルート・路線系統(route)のグループを識別します。[routes.txt](#routestxt) の複数の行が同じ `network_id` を持つことができます。<br><br>条件付き禁止:<br>- [route_networks.txt](#route_networkstxt) または [networks.txt](#networkstxt) ファイルが存在する場合は**禁止**です。<br>- それ以外の場合は任意です。 |
|  `cemv_support` | 列挙型 | 任意 | 乗客が、運賃検証機（pay-as-you-go または open-loop システムなど）において、非接触 EMV（Europay、Mastercard、Visa）カードまたはモバイルデバイスをチケットメディアとして使用することで、このルート・路線系統(route)に関連付けられた交通サービス（すなわち便(trip)）を利用できるかどうかを示します。このフィールドは、cEMV を使用して他のチケット商品を購入したり、別のチケットメディアに価値を追加したりできることを示すものではありません。<br><br>cEMV のサポートは、このルート・路線系統(route)に属するすべてのサービスが、cEMV カードまたはモバイルデバイスをチケットメディアとして使用して利用可能な場合にのみ示すべきです。<br><br>有効な選択肢は次のとおりです。<br><br>`0` または空 - このルート・路線系統(route)に関連付けられた便(trip)について cEMV 情報はありません。<br>`1` - 乗客は、このルート・路線系統(route)に関連付けられた便(trip)について cEMV をチケットメディアとして使用することができます。<br>`2` - このルート・路線系統(route)に関連付けられた便(trip)について、cEMV はチケットメディアとしてサポートされていません。<br><br>同じサービスについて `agency.cemv_support` と `routes.cemv_support` の両方が提供される場合、`routes.cemv_support` の値が優先されなければなりません。<br><br>このフィールドは他のすべての運賃関連ファイルから独立しており、個別に使用することができます。このフィールドと、運賃関連ファイル（[fare_media.txt](#fare_mediatxt)、[fare_products.txt](#fare_productstxt)、または [fare_leg_rules.txt](#fare_leg_rulestxt) など）との間に情報の競合がある場合、それらのファイル内の情報が `agency.cemv_support` より優先されなければなりません。 |

### trips.txt {: #tripstxt}


ファイル: **必須**

主キー (`trip_id`)

|  フィールド名 | 型 | 存在要件 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `route_id` | `routes.route_id` を参照する外部 ID | **必須** | ルート・路線系統(route)を識別します。 |
|  `service_id` | `calendar.service_id` または `calendar_dates.service_id` を参照する外部 ID | **必須** | 1つ以上のルート・路線系統(route)で運行が利用可能な日付の集合を識別します。 |
|  `trip_id` | 一意の ID | **必須** | 便(trip)を識別します。 |
| `trip_headsign` | テキスト | 任意 | 乗客に対して便(trip)の目的地を識別するために標識に表示されるテキストです。このフィールドは、車両に表示され、ルート・路線系統(route)内の便(trip)を区別するために使用される可能性がある行先表示(headsign)テキストを持つすべてのサービスに推奨されます。<br><br> 便(trip)の途中で行先表示(headsign)が変わる場合、便(trip)に沿った特定の停車時刻(stop_time)について `stop_times.stop_headsign` に値を定義することで、`trip_headsign` の値を上書きすることができます。 |
|  `trip_short_name` | テキスト | 任意 | たとえば通勤鉄道の便(trip)の列車番号を識別するために、乗客に対して便(trip)を識別するために使用される公開向けテキストです。乗客が通常便(trip)名に依存しない場合、`trip_short_name` は空にするべきです。`trip_short_name` の値を指定する場合、運行日(service day)内で便(trip)を一意に識別するべきです。目的地名または各駅停車／急行の指定に使用するべきではありません。 |
|  `direction_id` | 列挙型 | 任意 | 便(trip)の進行方向を示します。このフィールドは経路探索で使用するべきではありません。時刻表を公開する際に便(trip)を方向別に分ける方法を提供します。有効な選択肢は次のとおりです。<br><br>`0` - 一方向への移動（例: 往路）。<br>`1` - 反対方向への移動（例: 復路）。<hr>*例: `trip_headsign` および `direction_id` フィールドを併用して、便(trip)の集合における各方向の移動に名前を割り当てることができます。[trips.txt](#tripstxt) ファイルには、時刻表で使用するために次のレコードを含めることができます。* <br> `trip_id,...,trip_headsign,direction_id` <br> `1234,...,Airport,0` <br> `1505,...,Downtown,1` |
|  `block_id` | ID | 任意 | 便(trip)が属するブロックを識別します。ブロックは、共有される運行日(service day)および `block_id` によって定義される、同じ車両を使用して運行される単一の便(trip)または連続する複数の便(trip)で構成されます。`block_id` は異なる運行日(service day)を持つ便(trip)を含むことができ、これにより別個のブロックとなります。[以下の例](#example-blocks-and-service-day)を参照してください。着席したままの乗換情報を提供するには、代わりに `transfer_type` が `4` の [transfers](#transferstxt) を提供するべきです。 |
|  `shape_id` | `shapes.shape_id` を参照する外部 ID | **条件付き必須** | 便(trip)の車両走行経路を記述する地理空間的なルート形状(shape)を識別します。 <br><br>条件付き必須: <br>- 便(trip)に、[routes.txt](#routestxt) または [stop_times.txt](#stop_timestxt) のいずれかで定義された連続的な乗車または降車の挙動がある場合は**必須**です。 <br>- それ以外の場合は任意です。 |
|  `wheelchair_accessible` | 列挙型 | 任意 | 車椅子での利用可能性を示します。有効な選択肢は次のとおりです。<br><br>`0` または空 - 便(trip)の利用可能性に関する情報はありません。<br>`1` - この特定の便(trip)で使用される車両は、車椅子の乗客を少なくとも1人収容できます。<br>`2` - この便(trip)では車椅子の乗客を収容できません。 |
|  `bikes_allowed` | 列挙型 | 任意 | 自転車の持込みが許可されているかどうかを示します。有効な選択肢は次のとおりです。<br><br>`0` または空 - 便(trip)の自転車に関する情報はありません。<br>`1` - この特定の便(trip)で使用される車両は、自転車を少なくとも1台収容できます。<br>`2` - この便(trip)では自転車は許可されていません。 |
|  `cars_allowed` | 列挙型 | 任意 | 自動車の持込みが許可されているかどうかを示します。有効な選択肢は次のとおりです。<br><br>`0` または空 - 便(trip)の自動車に関する情報はありません。<br>`1` - この特定の便(trip)で使用される車両は、自動車を少なくとも1台収容できます。<br>`2` - この便(trip)では自動車は許可されていません。 |
| `safe_duration_factor` | Float | 任意 | オンデマンド便(trip)について計算される移動時間の推定値に適用される乗数です。<br><br>このフィールドおよび `safe_duration_offset` フィールドの使用方法に関するガイダンスについては、以下の [safe duration フィールドを使用したオンデマンド便(trip)の移動時間推定値の計算](#calculating-on-demand-trip-time-estimates-with-safe-duration-fields)の節を参照してください。 |
| `safe_duration_offset` | Float | 任意 | オンデマンド便(trip)について計算される移動時間の推定値に適用される、秒単位の固定オフセット値です。<br><br>このフィールドおよび `safe_duration_factor` フィールドの使用方法に関するガイダンスについては、以下の [safe duration フィールドを使用したオンデマンド便(trip)の移動時間推定値の計算](#calculating-on-demand-trip-time-estimates-with-safe-duration-fields)の節を参照してください。 |

#### safe duration fields を用いたオンデマンド便の所要時間見積もりの計算 {: #calculating-on-demand-trip-time-estimates-with-safe-duration-fields}

`safe_duration_factor` と `safe_duration_offset` を組み合わせることで、95%のケースにおいて、乗客がオンデマンド便に要すると予想できる最長時間を見積もることができます。データ利用者は、`safe_duration_factor` と `safe_duration_offset` を使用して、次の計算を行うことが期待されます。<br>`SafeTravelDuration (seconds) = safe_duration_factor × DrivingDuration (seconds) + safe_duration_offset (seconds)`<br>ここで、`DrivingDuration` は、オンデマンドサービスについて計算対象となる距離を自家用車で移動する場合に要する時間であり、`SafeTravelDuration` は、乗客がオンデマンド便に要すると予想できる最長時間です。<br><br>

この計算は、便のうちオンデマンドである部分にのみ適用するべきです。サービスが迂回固定型サービスである場合、または乗客の便にオンデマンドサービスから固定ルートサービスへの乗り換えが含まれる場合、便の固定ルート部分の所要時間は、`departure_time` および `arrival_time` フィールドに従って計算するべきです。

#### 例: ブロックと運行日 {: #example-blocks-and-service-day}


以下の例は有効であり、曜日ごとに異なるブロックがあります。

| route_id | trip_id | service_id                     | block_id | <span style="font-weight:normal">*(最初の停車時刻(stop_time))*</span> | <span style="font-weight:normal">*(最後の停車時刻(stop_time))*</span> |
|----------|---------|--------------------------------|----------|-----------------------------------------|-------------------------|
| red      | trip_1  | mon-tues-wed-thurs-fri-sat-sun | red_loop | 22:00:00                                | 22:55:00                |
| red      | trip_2  | fri-sat-sun                    | red_loop | 23:00:00                                | 23:55:00                |
| red      | trip_3  | fri-sat                        | red_loop | 24:00:00                                | 24:55:00                |
| red      | trip_4  | mon-tues-wed-thurs             | red_loop | 20:00:00                                | 20:50:00                |
| red      | trip_5  | mon-tues-wed-thurs             | red_loop | 21:00:00                                | 21:50:00                |

上記の表に関する注記:

* 例えば、金曜日から土曜日の早朝にかけて、1台の車両が `trip_1`、`trip_2`、および `trip_3` を運行します（午後10時から午前12時55分まで）。最後の便(trip)は土曜日の午前12時から午前12時55分までですが、時刻が24:00:00から24:55:00であるため、金曜日の「運行日(service day)」の一部であることに注意してください。
* 月曜日、火曜日、水曜日、および木曜日には、1台の車両が午後8時から午後10時55分まで、1つのブロック内で `trip_1`、`trip_4`、および `trip_5` を運行します。

### stop_times.txt {: #stop_timestxt}


ファイル: **必須**

主キー (`trip_id`, `stop_sequence`)

|  フィールド名 | 型 | 存在要件 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `trip_id` | `trips.trip_id` を参照する外部 ID | **必須** | 便(trip)を識別します。  |
|  `arrival_time` | 時刻 | **条件付き必須** | `stops.stop_timezone` ではなく `agency.agency_timezone` で指定されたタイムゾーンにおける、特定の便(trip)（`stop_times.trip_id` で定義）の停留所等(stop)（`stop_times.stop_id` で定義）への到着時刻です。<br><br>停留所等(stop)での到着時刻と出発時刻が別々に存在しない場合、`arrival_time` と `departure_time` は同じであるべきです。<br><br>運行日(service day)の深夜0時以降に発生する時刻については、HH:MM:SS 形式で 24:00:00 より大きい値として時刻を入力してください。<br><br>正確な到着時刻および出発時刻（`timepoint=1`）が利用できない場合、推定または補間された到着時刻および出発時刻（`timepoint=0`）を提供するべきです。<br><br>条件付き必須:<br>- 便(trip)内の最初および最後の停留所等(stop)（`stop_times.stop_sequence` で定義）では**必須**です。<br>- `timepoint=1` の場合は**必須**です。<br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合は**禁止**です。<br>- それ以外の場合は任意です。|
|  `departure_time` | 時刻 | **条件付き必須** | `stops.stop_timezone` ではなく `agency.agency_timezone` で指定されたタイムゾーンにおける、特定の便(trip)（`stop_times.trip_id` で定義）の停留所等(stop)（`stop_times.stop_id` で定義）からの出発時刻です。<br><br>停留所等(stop)での到着時刻と出発時刻が別々に存在しない場合、`arrival_time` と `departure_time` は同じであるべきです。<br><br>運行日(service day)の深夜0時以降に発生する時刻については、HH:MM:SS 形式で 24:00:00 より大きい値として時刻を入力してください。<br><br>正確な到着時刻および出発時刻（`timepoint=1`）が利用できない場合、推定または補間された到着時刻および出発時刻（`timepoint=0`）を提供するべきです。<br><br>条件付き必須:<br>- `timepoint=1` の場合は**必須**です。<br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合は**禁止**です。<br>- それ以外の場合は任意です。 |
|  `stop_id` | `stops.stop_id` を参照する外部 ID | **条件付き必須** | 運行対象の停留所等(stop)を識別します。便(trip)中に運行されるすべての停留所等(stop)は、[stop_times.txt](#stop_timestxt) にレコードを持たなければなりません。参照される場所は停留所等(stop)/プラットフォームでなければならず、すなわち `stops.location_type` の値は `0` または空でなければなりません。同じ便(trip)内で停留所等(stop)が複数回運行されることがあり、複数の便(trip)およびルート・路線系統(route)が同じ停留所等(stop)を運行することがあります。<br><br>停留所等(stop)を使用するデマンド型サービスは、それらの停留所等(stop)でサービスが利用可能となる順序で参照するべきです。各 stop_time の `pickup/drop_off_type` および各 `start/end_pickup_drop_off_window` の時間制約によって禁止されない限り、データ利用者は、便(trip)内のある停留所等(stop)または場所から、その後にある任意の停留所等(stop)または場所への移動が可能であると想定するべきです。<br><br>条件付き必須:<br>- `stop_times.location_group_id` と `stop_times.location_id` の両方が定義されていない場合は**必須**です。<br>- `stop_times.location_group_id` または `stop_times.location_id` が定義されている場合は**禁止**です。 |
|  `location_group_id` | `location_groups.location_group_id` を参照する外部 ID | **条件付き禁止** | 乗客が乗車または降車をリクエストできる停留所等(stop)のグループを示す、運行対象のロケーショングループを識別します。便(trip)中に運行されるすべてのロケーショングループは、[stop_times.txt](#stop_timestxt) にレコードを持たなければなりません。複数の便(trip)およびルート・路線系統(route)が同じロケーショングループを運行することがあります。<br><br>ロケーショングループを使用するデマンド型サービスは、それらのロケーショングループでサービスが利用可能となる順序で参照するべきです。各 stop_time の `pickup/drop_off_type` および各 `start/end_pickup_drop_off_window` の時間制約によって禁止されない限り、データ利用者は、便(trip)内のある停留所等(stop)または場所から、その後にある任意の停留所等(stop)または場所への移動が可能であると想定するべきです。<br><br>**条件付き禁止**:<br>- `stop_times.stop_id` または `stop_times.location_id` が定義されている場合は**禁止**です。 |
|  `location_id` | `locations.geojson` の `id` を参照する外部 ID | **条件付き禁止** | 乗客が乗車または降車をリクエストできる運行対象ゾーンに対応する GeoJSON の場所を識別します。便(trip)中に運行されるすべての GeoJSON の場所は、[stop_times.txt](#stop_timestxt) にレコードを持たなければなりません。複数の便(trip)およびルート・路線系統(route)が同じ GeoJSON の場所を運行することがあります。<br><br>場所内のデマンド型サービスは、それらの場所でサービスが利用可能となる順序で参照するべきです。各 stop_time の `pickup/drop_off_type` および各 `start/end_pickup_drop_off_window` の時間制約によって禁止されない限り、データ利用者は、便(trip)内のある停留所等(stop)または場所から、その後にある任意の停留所等(stop)または場所への移動が可能であると想定するべきです。<br><br>**条件付き禁止**:<br>- `stop_times.stop_id` または `stop_times.location_group_id` が定義されている場合は**禁止**です。 |
|  `stop_sequence` | 非負整数 | **必須** | 特定の便(trip)における停留所等(stop)、ロケーショングループ、または GeoJSON の場所の順序です。値は便(trip)に沿って増加しなければなりませんが、連続している必要はありません。<hr>*例: 便(trip)上の最初の場所は `stop_sequence`=`1`、便(trip)上の2番目の場所は `stop_sequence`=`23`、3番目の場所は `stop_sequence`=`40` とすることができます。* <br><br> 同じロケーショングループまたは GeoJSON の場所内での移動には、同じ `location_group_id` または `location_id` を持つ [stop_times.txt](#stop_timestxt) 内の2つのレコードが必要です。 |
|  `stop_headsign` | テキスト | 任意 | 便(trip)の目的地を乗客に示す案内表示に表示されるテキストです。このフィールドは、停留所等(stop)間で行先表示(headsign)が変わる場合に、デフォルトの `trips.trip_headsign` を上書きします。行先表示(headsign)が便(trip)全体に表示される場合は、代わりに `trips.trip_headsign` を使用するべきです。<br><br> 1つの stop_time に指定された `stop_headsign` 値は、同じ便(trip)内の後続の stop_time には適用されません。同じ便(trip)内の複数の stop_time に対して `trip_headsign` を上書きする場合、`stop_headsign` 値を各 stop_time 行で繰り返さなければなりません。 |
| `start_pickup_drop_off_window` | 時刻 | **条件付き必須** | GeoJSON の場所、ロケーショングループ、または停留所等(stop)でデマンド型サービスが利用可能になる時刻です。<br><br>**条件付き必須**:<br>- `stop_times.location_group_id` または `stop_times.location_id` が定義されている場合は**必須**です。<br>- `end_pickup_drop_off_window` が定義されている場合は**必須**です。<br>- `arrival_time` または `departure_time` が定義されている場合は**禁止**です。<br>- それ以外の場合は任意です。  |
| `end_pickup_drop_off_window` | 時刻 | **条件付き必須** | GeoJSON の場所、ロケーショングループ、または停留所等(stop)でデマンド型サービスが終了する時刻です。<br><br>**条件付き必須**:<br>- `stop_times.location_group_id` または `stop_times.location_id` が定義されている場合は**必須**です。<br>- `start_pickup_drop_off_window` が定義されている場合は**必須**です。<br>- `arrival_time` または `departure_time` が定義されている場合は**禁止**です。<br>- それ以外の場合は任意です。 |
|  `pickup_type` | 列挙型 | **条件付き禁止** | 乗車方法を示します。有効な選択肢は次のとおりです:<br><br>`0` または空 - 定期的に予定された乗車。<br>`1` - 乗車不可。<br>`2` - 乗車を手配するために事業者へ電話しなければなりません。<br>`3` - 乗車を手配するために運転手と調整しなければなりません。<br><br> **条件付き禁止**: <br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合、`pickup_type=0` は**禁止**です。<br> - `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合、`pickup_type=3` は**禁止**です。<br> - それ以外の場合は任意です。 |
|  `drop_off_type` | 列挙型 | **条件付き禁止** | 降車方法を示します。有効な選択肢は次のとおりです:<br><br>`0` または空 - 定期的に予定された降車。<br>`1` - 降車不可。<br>`2` - 降車を手配するために事業者へ電話しなければなりません。<br>`3` - 降車を手配するために運転手と調整しなければなりません。<br><br> **条件付き禁止**:<br> - `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合、`drop_off_type=0` は**禁止**です。<br> - それ以外の場合は任意です。 |
|  `continuous_pickup` | 列挙型 | **条件付き禁止** | この stop_time から便(trip)の `stop_sequence` における次の stop_time までの間に、[shapes.txt](#shapestxt) で記述される車両の走行経路に沿った任意の地点で、乗客が公共交通車両に乗車できることを示します。有効な選択肢は次のとおりです: <br><br>`0` - 連続停車乗車。<br>`1` または空 - 連続停車乗車なし。<br>`2` - 連続停車乗車を手配するために事業者へ電話しなければなりません。<br>`3` - 連続停車乗車を手配するために運転手と調整しなければなりません。<br><br>このフィールドに値が設定されている場合、[routes.txt](#routestxt) で定義された連続乗車の動作を上書きします。このフィールドが空の場合、stop_time は [routes.txt](#routestxt) で定義された連続乗車の動作を継承します。<br><br>**条件付き禁止**:<br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合、`1` または空以外の値は**禁止**です。<br> - それ以外の場合は任意です。 |
|  `continuous_drop_off` | 列挙型 | **条件付き禁止** | この stop_time から便(trip)の `stop_sequence` における次の stop_time までの間に、[shapes.txt](#shapestxt) で記述される車両の走行経路に沿った任意の地点で、乗客が公共交通車両から降車できることを示します。有効な選択肢は次のとおりです: <br><br>`0` - 連続停車降車。<br>`1` または空 - 連続停車降車なし。<br>`2` - 連続停車降車を手配するために事業者へ電話しなければなりません。<br>`3` - 連続停車降車を手配するために運転手と調整しなければなりません。<br><br>このフィールドに値が設定されている場合、[routes.txt](#routestxt) で定義された連続降車の動作を上書きします。このフィールドが空の場合、stop_time は [routes.txt](#routestxt) で定義された連続降車の動作を継承します。<br><br>**条件付き禁止**:<br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合、`1` または空以外の値は**禁止**です。<br> - それ以外の場合は任意です。 |
|  `shape_dist_traveled` | 非負浮動小数点数 | 任意 | 最初の停留所等(stop)からこのレコードで指定された停留所等(stop)まで、関連するルート形状(shape)に沿って実際に移動した距離です。このフィールドは、便(trip)中の任意の2つの停留所等(stop)間で描画するルート形状(shape)の範囲を指定します。[shapes.txt](#shapestxt) で使用されるものと同じ単位でなければなりません。`shape_dist_traveled` に使用される値は `stop_sequence` とともに増加しなければならず、ルート・路線系統(route)に沿った逆方向の移動を示すために使用してはいけません。<br><br>ループまたは重複走行（車両が1つの便(trip)内で同じ線形の一部を横断または走行すること）があるルート・路線系統(route)に推奨されます。[`shapes.shape_dist_traveled`](#shapestxt) を参照してください。<hr>*例: バスがルート形状(shape)の開始地点から停留所等(stop)まで 5.25 キロメートル移動する場合、`shape_dist_traveled`=`5.25` です。*|
| `timepoint` | 列挙型 | 任意 | 停留所等(stop)の到着時刻および出発時刻が車両によって厳密に遵守されるか、または概算および/または補間された時刻であるかを示します。このフィールドにより、GTFS 作成者は、時刻が概算であることを示しながら補間された停車時刻(stop_time)を提供できます。有効な選択肢は次のとおりです:<br><br>`0` - 時刻は概算とみなされます。<br>`1` - 時刻は正確とみなされます。<br><br> 到着時刻または出発時刻が定義されている [stop_times.txt](#stop_timestxt) のすべてのレコードには、timepoint 値を設定するべきです。timepoint 値が提供されない場合、すべての時刻は正確とみなされます。 |
| `pickup_booking_rule_id` | `booking_rules.booking_rule_id` を参照する外部 ID | 任意 | この停車時刻(stop_time)における乗車予約ルールを識別します。<br><br>`pickup_type=2` の場合に推奨されます。 |
| `drop_off_booking_rule_id` | `booking_rules.booking_rule_id` を参照する外部 ID | 任意 | この停車時刻(stop_time)における降車予約ルールを識別します。<br><br>`drop_off_type=2` の場合に推奨されます。 |

#### オンデマンドサービスの経路探索動作 {: #on-demand-service-routing-behavior}

- 出発地と目的地の間の経路探索または所要時間を提供する場合、データ利用者は、`start_pickup_drop_off_window` および `end_pickup_drop_off_window` が定義されている、同じ `trip_id` を持つ中間の stop_times.txt レコードを無視するべきです。無視するべき内容を示す例については、[データ例ページ](../examples/flex/#ignoring-intermediate-stop-times-records-with-pickupdrop-off-windows)を参照してください。
- 同じ `trip_id` を持つ2つ以上の stop_times.txt レコード間で、locations.geojson の `id` ジオメトリ、`start/end_pickup_drop_off_window` 時刻、および `pickup_type` または `drop_off_type` が同時に重複することは禁止されています。禁止されている内容を示す例については、[データ例ページ](../examples/flex/#zone-overlap-constraint)を参照してください。

### calendar.txt {: #calendartxt}


ファイル: **条件付き必須**

主キー (`service_id`)

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ |------ |
|  `service_id` | 一意の ID | **必須** | 1つ以上のルート・路線系統(route)で運行が利用可能な日付の集合を識別します。 |
|  `monday` | 列挙型 | **必須** | `start_date` および `end_date` フィールドで指定された日付範囲内のすべての月曜日に運行するかどうかを示します。特定の日付に対する例外は [calendar_dates.txt](#calendar_datestxt) に記載される場合があることに注意してください。有効な選択肢は次のとおりです。<br><br>`1` - 日付範囲内のすべての月曜日に運行が利用可能です。<br>`0` - 日付範囲内の月曜日には運行が利用できません。 |
|  `tuesday` | 列挙型 | **必須** | `monday` と同じように機能しますが、火曜日に適用されます。 |
|  `wednesday` | 列挙型 | **必須** | `monday` と同じように機能しますが、水曜日に適用されます。  |
|  `thursday` | 列挙型 | **必須** | `monday` と同じように機能しますが、木曜日に適用されます。  |
|  `friday` | 列挙型 | **必須** | `monday` と同じように機能しますが、金曜日に適用されます。  |
|  `saturday` | 列挙型 | **必須** | `monday` と同じように機能しますが、土曜日に適用されます。 |
|  `sunday` | 列挙型 | **必須** | `monday` と同じように機能しますが、日曜日に適用されます。 |
|  `start_date` | 日付 | **必須** | 運行期間の開始運行日(service day)です。 |
|  `end_date` | 日付 | **必須** | 運行期間の終了運行日(service day)です。この運行日(service day)は期間に含まれます。 |

### calendar_dates.txt {: #calendar_datestxt}


ファイル: **条件付き必須**

主キー (`service_id`, `date`)

[calendar_dates.txt](#calendar_datestxt) テーブルは、日付ごとに運行を明示的に有効化または無効化します。これは2つの方法で使用することができます。

* 推奨: [calendar.txt](#calendartxt) で定義されたデフォルトの運行パターンに対する例外を定義するために、[calendar.txt](#calendartxt) と併せて [calendar_dates.txt](#calendar_datestxt) を使用します。運行が概ね規則的であり、明示的な日付において少数の変更がある場合（たとえば、特別イベント運行や学校のスケジュールに対応するため）は、この方法が適しています。この場合、`calendar_dates.service_id` は `calendar.service_id` を参照する外部IDです。
* 代替: [calendar.txt](#calendartxt) を省略し、各運行日を [calendar_dates.txt](#calendar_datestxt) で指定します。これにより、運行を大幅に変動させることができ、通常の週間スケジュールがない運行にも対応できます。この場合、`service_id` はIDです。

|  フィールド名 | 型 | 存在要件 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `service_id` | `calendar.service_id` を参照する外部IDまたはID | **必須** | 1つ以上のルート・路線系統(route)において運行例外が発生する日付の集合を識別します。[calendar.txt](#calendartxt) と [calendar_dates.txt](#calendar_datestxt) を併せて使用する場合、各 (`service_id`, `date`) の組は [calendar_dates.txt](#calendar_datestxt) に1回のみ出現することができます。`service_id` の値が [calendar.txt](#calendartxt) と [calendar_dates.txt](#calendar_datestxt) の両方に出現する場合、[calendar_dates.txt](#calendar_datestxt) の情報は [calendar.txt](#calendartxt) で指定された運行情報を変更します。 |
|  `date` | 日付 | **必須** | 運行例外が発生する日付です。 |
|  `exception_type` | 列挙型 | **必須** | date フィールドで指定された日付に運行が利用可能かどうかを示します。有効な選択肢は次のとおりです。<br><br> `1` - 指定された日付に運行が追加されています。<br>`2` - 指定された日付の運行が削除されています。<hr>*例: あるルート・路線系統(route)に、休日に利用可能な便(trip)のセットと、それ以外のすべての日に利用可能な別の便(trip)のセットがあるとします。1つの `service_id` は通常の運行スケジュールに対応し、別の `service_id` は休日のスケジュールに対応することができます。特定の休日について、[calendar_dates.txt](#calendar_datestxt) ファイルを使用して、その休日を休日用の `service_id` に追加し、通常の `service_id` スケジュールからその休日を削除することができます。* |

### fare_attributes.txt {: #fare_attributestxt}


ファイル: **任意**

主キー (`fare_id`)

**バージョン**<br>
運賃を記述するためのモデリングオプションは2つあります。GTFS-Fares V1は、最小限の運賃情報を記述するための従来のオプションです。GTFS-Fares V2は、事業者の運賃体系をより詳細に記述できる更新された方法です。両方をデータセット内に含めることができますが、データコンシューマーは、特定のデータセットに対して1つの方法のみを使用するべきです。GTFS-Fares V2をGTFS-Fares V1より優先することが推奨されます。<br><br>GTFS-Fares V1に関連するファイルは次のとおりです。<br>- [fare_attributes.txt](#fare_attributestxt)<br>- [fare_rules.txt](#fare_rulestxt)<br><br>GTFS-Fares V2に関連するファイルは次のとおりです。<br>- [fare_media.txt](#fare_mediatxt)<br>- [fare_products.txt](#fare_productstxt)<br>- [rider_categories.txt](#rider_categoriestxt)<br>- [fare_leg_rules.txt](#fare_leg_rulestxt)<br>- [fare_leg_join_rules.txt](#fare_leg_join_rulestxt)<br>- [fare_transfer_rules.txt](#fare_transfer_rulestxt)<br>- [timeframes.txt](#timeframestxt)<br>- [networks.txt](#networkstxt)<br>- [route_networks.txt](#route_networkstxt)<br>- [areas.txt](#areastxt)<br>- [stop_areas.txt](#stop_areastxt)

<br>

|  フィールド名 | 型 | 存在要件 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `fare_id` | 一意のID | **必須** | 運賃クラスを識別します。 |
|  `price` | 非負の浮動小数点数 | **必須** | `currency_type`で指定された単位による運賃価格です。 |
|  `currency_type` | 通貨コード | **必須** | 運賃の支払いに使用する通貨です。 |
|  `payment_method` | 列挙型 | **必須** | 運賃をいつ支払わなければならないかを示します。有効なオプションは次のとおりです。<br><br>`0` - 運賃は車内で支払われます。<br>`1` - 運賃は乗車前に支払わなければなりません。 |
|  `transfers` | 列挙型 | **必須** | この運賃で許可される乗換回数を示します。有効なオプションは次のとおりです。<br><br>`0` - この運賃では乗換は許可されません。<br>`1` - 乗客は1回乗り換えることができます。<br>`2` - 乗客は2回乗り換えることができます。<br>空 - 無制限の乗換が許可されます。 |
|  `agency_id` | `agency.agency_id`を参照する外部ID | **条件付き必須** | 運賃に関連する事業者を識別します。<br><br>条件付き必須:<br>- [agency.txt](#agencytxt)で複数の事業者が定義されている場合は**必須**です。<br>- それ以外の場合は推奨です。 |
|  `transfer_duration` | 非負の整数 | 任意 | 乗換が期限切れになるまでの秒単位の時間です。`transfers`=`0`の場合、このフィールドはチケットの有効期間を示すために使用することができ、または空のままにすることができます。 |

### fare_rules.txt {: #fare_rulestxt}


ファイル: **任意**

主キー (`*`)

[fare_rules.txt](#fare_rulestxt) テーブルは、[fare_attributes.txt](#fare_attributestxt) の運賃が旅程(journey)にどのように適用されるかを指定します。ほとんどの運賃体系では、以下のルールをいくつか組み合わせて使用します。

* 運賃は出発地または目的地の駅に依存します。
* 運賃は旅程(journey)が通過するゾーンに依存します。
* 運賃は旅程(journey)が使用するルート・路線系統(route)に依存します。

[fare_rules.txt](#fare_rulestxt) および [fare_attributes.txt](#fare_attributestxt) を使用して運賃体系を指定する方法を示す例については、GoogleTransitDataFeed オープンソースプロジェクト wiki の [FareExamples](https://web.archive.org/web/20111207224351/https://code.google.com/p/googletransitdatafeed/wiki/FareExamples) を参照してください。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `fare_id` | `fare_attributes.fare_id` を参照する外部 ID  | **必須** | 運賃クラスを識別します。 |
|  `route_id` | `routes.route_id` を参照する外部 ID | 任意 | 運賃クラスに関連付けられたルート・路線系統(route)を識別します。同じ運賃属性を持つ複数のルート・路線系統(route)が存在する場合、各ルート・路線系統(route)について [fare_rules.txt](#fare_rulestxt) にレコードを作成します。<hr>*例: 運賃クラス「b」がルート・路線系統(route)「TSW」および「TSE」で有効である場合、[fare_rules.txt](#fare_rulestxt) ファイルには、この運賃クラスについて以下のレコードが含まれます。* <br> ` fare_id,route_id`<br>`b,TSW` <br> `b,TSE`|
|  `origin_id` | `stops.zone_id` を参照する外部 ID | 任意 | 出発地ゾーンを識別します。運賃クラスに複数の出発地ゾーンがある場合、各 `origin_id` について [fare_rules.txt](#fare_rulestxt) にレコードを作成します。<hr>*例: 運賃クラス「b」がゾーン「2」またはゾーン「8」のいずれかを出発地とするすべての移動で有効である場合、[fare_rules.txt](#fare_rulestxt) ファイルには、この運賃クラスについて以下のレコードが含まれます。* <br> `fare_id,...,origin_id` <br> `b,...,2`  <br> `b,...,8` |
|  `destination_id` | `stops.zone_id` を参照する外部 ID | 任意 | 目的地ゾーンを識別します。運賃クラスに複数の目的地ゾーンがある場合、各 `destination_id` について [fare_rules.txt](#fare_rulestxt) にレコードを作成します。<hr>*例: `origin_id` および `destination_id` フィールドを併用して、運賃クラス「b」がゾーン 3 と 4 の間の移動、およびゾーン 3 と 5 の間の移動で有効であることを指定できます。[fare_rules.txt](#fare_rulestxt) ファイルには、この運賃クラスについて以下のレコードが含まれます。* <br>`fare_id,...,origin_id,destination_id` <br>`b,...,3,4`<br> `b,...,3,5` |
|  `contains_id` | `stops.zone_id` を参照する外部 ID | 任意 | 乗客が特定の運賃クラスを使用している間に入るゾーンを識別します。一部のシステムでは、正しい運賃クラスを計算するために使用されます。 <hr>*例: 運賃クラス「c」が、ゾーン 5、6、および 7 を通過する GRT ルート・路線系統(route)上のすべての移動に関連付けられている場合、[fare_rules.txt](#fare_rulestxt) には以下のレコードが含まれます。* <br> `fare_id,route_id,...,contains_id` <br>  `c,GRT,...,5` <br>`c,GRT,...,6` <br>`c,GRT,...,7` <br> *運賃を適用するにはすべての `contains_id` ゾーンが一致しなければならないため、ゾーン 5 および 6 を通過するがゾーン 7 を通過しない旅程(journey)には、運賃クラス「c」は適用されません。詳細については、GoogleTransitDataFeed プロジェクト wiki の [https://code.google.com/p/googletransitdatafeed/wiki/FareExamples](https://code.google.com/p/googletransitdatafeed/wiki/FareExamples) を参照してください。* |

### timeframes.txt {: #timeframestxt}


ファイル: **任意**

主キー (`*`)

時刻、曜日、または年内の特定の日に基づいて変動する運賃を記述するために使用します。時間枠は、[fare_leg_rules.txt](#fare_leg_rulestxt) のチケット商品に関連付けることができます。<br>
同じ `timeframe_group_id` および `service_id` の値に対して、時間間隔が重複してはいけません。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `timeframe_group_id` | ID | **必須** | 時間枠または時間枠のセットを識別します。 |
|  `start_time` | ローカル時刻 | **条件付き必須** | 時間枠の開始を定義します。間隔には開始時刻が含まれます。<br> `24:00:00` を超える値は禁止されています。`start_time` の空の値は `00:00:00` と見なされます。<br><br>条件付き必須:<br> - `timeframes.end_time` が定義されている場合は**必須**です。<br> - それ以外の場合は禁止されています。 |
|  `end_time` | ローカル時刻 | **条件付き必須** | 時間枠の終了を定義します。間隔には終了時刻は含まれません。<br> `24:00:00` を超える値は禁止されています。`end_time` の空の値は `24:00:00` と見なされます。<br><br>条件付き必須:<br> - `timeframes.start_time` が定義されている場合は**必須**です。<br> - それ以外の場合は禁止されています。 |
| `service_id` | `calendar.service_id` または `calendar_dates.service_id` を参照する外部ID | **必須** | 時間枠が有効となる日付のセットを識別します。 |

#### 時間枠のローカル時刻セマンティクス {: #timeframe-local-time-semantics}

- 運賃イベントの時刻を [timeframes.txt](#timeframestxt) と照合して評価する場合、イベント時刻は、運賃イベントの停留所等(stop)または親駅について指定されている場合は `stop_timezone` によって決定されるローカルタイムゾーンを使用して、ローカル時刻で計算されます。指定されていない場合は、代わりにフィードの事業者のタイムゾーンを使用するべきです。
- 「current day」は、ローカルタイムゾーンを基準として計算された、運賃イベントの時刻の現在の日付です。「current day」は、特に深夜をまたぐ便(trip)の場合、運賃区間の便(trip)の運行日(service day)とは異なる場合があります。
- 運賃イベントの「time-of-day」は、GTFS Local time field-type のセマンティクスを使用して、「current day」を基準に計算されます。

### rider_categories.txt {: #rider_categoriestxt}


ファイル: **任意** 

主キー (`rider_category_id`)

乗客のカテゴリ（例: 高齢者、学生）を定義します。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `rider_category_id` | 一意の ID | **必須** | 乗客カテゴリを識別します。 |
|  `rider_category_name` | テキスト | **必須** | 乗客に表示される乗客カテゴリ名です。 |
|  `is_default_fare_category` | 列挙型 | **必須** | [rider_categories.txt](#rider_categoriestxt) 内のエントリをデフォルトカテゴリ（すなわち、乗客に表示するべき主要カテゴリ）と見なすかどうかを指定します。例: 大人運賃、通常運賃など。有効な選択肢は次のとおりです。<br><br>`0` または空 - カテゴリはデフォルトと見なされません。<br>`1` - カテゴリはデフォルトと見なされます。<br><br>`fare_product_id` で指定された1つのチケット商品に対して複数の乗客カテゴリが適格である場合、これらの適格な乗客カテゴリのうち、デフォルトの乗客カテゴリとして示されるものが正確に1つ存在しなければなりません（`is_default_fare_category = 1`）。 |
|  `eligibility_url` | URL | 任意 | 特定の乗客カテゴリに関する詳細情報を提供し、および／またはその適格性基準を説明する、通常は運行事業者による Web ページの URL です。 |

### fare_media.txt {: #fare_mediatxt}


ファイル: **任意** 

主キー (`fare_media_id`)

チケット商品を利用するために使用できるさまざまなチケットメディアを記述します。チケットメディアは、チケット商品の表現および／または検証に使用される物理的または仮想的な保持媒体です。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `fare_media_id` | 一意の ID | **必須** | チケットメディアを識別します。 |
|  `fare_media_name` | テキスト | 任意 | チケットメディアの名称。<br><br>交通カード（`fare_media_type =2`）またはモバイルアプリ（`fare_media_type =4`）であるチケットメディアについては、`fare_media_name` を含めるべきであり、それらを提供する組織が乗客向けに使用する名称と一致するべきです。 |
|  `fare_media_type` | 列挙型 | **必須** | チケットメディアの種類。有効な選択肢は次のとおりです。<br><br>`0` - なし。運転士または車掌に現金を支払い、物理的な乗車券が提供されない場合など、チケット商品の購入または検証にチケットメディアが関与しない場合に使用します。<br>`1` - 乗客が事前購入した一定回数の乗車、または固定期間内の無制限の乗車を利用できる物理的な紙の乗車券。<br>`2` - 保存された乗車券、定期券、または金銭的価値を持つ物理的な交通カード。<br>`3` - アカウントベースのチケッティングのためのオープンループトークンコンテナとしての cEMV（非接触 Europay、Mastercard および Visa）。<br>`4` - 仮想交通カード、乗車券、定期券、または金銭的価値を保存したモバイルアプリ。|

### fare_products.txt {: #fare_productstxt}


ファイル: **任意**

主キー (`fare_product_id`, `rider_category_id`, `fare_media_id`)

乗客が購入可能な運賃の範囲、または乗換費用など、複数の乗車区間(leg)を含む旅程(journey)の合計運賃を計算する際に考慮される運賃を記述するために使用されます。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
| `fare_product_id` | ID | **必須** | チケット商品、またはチケット商品のセットを識別します。<br><br>同じ `fare_product_id` を共有する複数のレコードは、異なる `fare_media_id` または `rider_category_id` を含む限り許可されます。異なる `fare_media_id` は、チケット商品を利用するためのさまざまな方法が利用可能であり、異なる価格となる可能性があることを示します。異なる `rider_category_id` は、複数の乗客カテゴリがチケット商品の対象であり、異なる価格となる可能性があることを示します。 |
| `fare_product_name` | Text | 任意 | 乗客に表示されるチケット商品の名称です。 |
| `rider_category_id` | `rider_categories.rider_category_id` を参照する外部ID | 任意 | チケット商品の対象となる乗客カテゴリを識別します。<br><br>`fare_products.rider_category_id` が空の場合、チケット商品は任意の `rider_category_id` の対象となります。<br><br>`fare_product_id` によって指定される単一のチケット商品に対して複数の乗客カテゴリが対象となる場合、これらの乗客カテゴリのうち、デフォルトの乗客カテゴリ（`is_default_fare_category = 1`）として示されるものは1つだけでなければなりません。 |
|  `fare_media_id` | `fare_media.fare_media_id` を参照する外部ID | 任意 |  乗車中にチケット商品を利用するために使用できるチケットメディアを識別します。`fare_media_id` が空の場合、チケットメディアは不明であると見なされます。|
| `amount` | 通貨金額 | **必須** | チケット商品の費用です。乗換割引を表すために負の値とすることができます。無料のチケット商品を表すためにゼロとすることができます。通貨金額には、対応する通貨コードに対して ISO 4217 規格で指定された小数点以下の桁数を含めなければなりません。<hr>*例: 運賃が2米ドルの場合、金額は2ではなく2.00です。* |
| `currency` | 通貨コード | **必須** | チケット商品の費用の通貨です。 |

### fare_leg_rules.txt {: #fare_leg_rulestxt}


ファイル: **任意**

主キー (`network_id, from_area_id, to_area_id, from_timeframe_group_id, to_timeframe_group_id, fare_product_id`)

個々の乗車区間(leg)に対する運賃ルールです。

[fare_leg_rules.txt](#fare_leg_rulestxt) の運賃は、ファイル内のすべてのレコードをフィルタリングして、乗客が移動する乗車区間(leg)に一致するルールを見つけることで照会しなければなりません。

乗車区間(leg)の費用を処理するには:

1. ファイル [fare_leg_rules.txt](#fare_leg_rulestxt) は、移動の特性を定義するフィールドによってフィルタリングしなければなりません。これらのフィールドは次のとおりです:
    - `fare_leg_rules.network_id`
    - `fare_leg_rules.from_area_id`
    - `fare_leg_rules.to_area_id`
    - `fare_leg_rules.from_timeframe_group_id`
    - `fare_leg_rules.to_timeframe_group_id`
<br/>

2. 乗車区間(leg)が移動の特性に基づいて [fare_leg_rules.txt](#fare_leg_rulestxt) 内のレコードに完全に一致する場合、そのレコードを処理して乗車区間(leg)の費用を決定しなければなりません。このファイルでは、空のエントリを2つの方法で処理します: 空のセマンティクスまたは `rule_priority`。
<br/>

3. 完全一致が見つからず、かつ `rule_priority` フィールドが存在しない場合、乗車区間(leg)の費用を処理するために、`fare_leg_rules.network_id`、`fare_leg_rules.from_area_id`、および `fare_leg_rules.to_area_id` の空のエントリを確認しなければなりません:
    - `fare_leg_rules.network_id` の空のエントリは、`fare_leg_rules.network_id` に記載されているものを除く、[routes.txt](#routestxt) または [networks.txt](#networkstxt) で定義されているすべてのネットワークに対応します

    - `fare_leg_rules.from_area_id` の空のエントリは、`fare_leg_rules.from_area_id` に記載されているものを除く、`areas.area_id` で定義されているすべてのエリアに対応します
    - `fare_leg_rules.to_area_id` の空のエントリは、`fare_leg_rules.to_area_id` に記載されているものを除く、`areas.area_id` で定義されているすべてのエリアに対応します
<br/>

4. `rule_priority` フィールドが存在する場合:
    - `fare_leg_rules.network_id` の空のエントリは、乗車区間(leg)のネットワークがこのルールの一致に影響しないことを示します。
    - `fare_leg_rules.from_area_id` の空のエントリは、乗車区間(leg)の出発エリアがこのルールの一致に影響しないことを示します。
    - `fare_leg_rules.to_area_id` の空のエントリは、乗車区間(leg)の到着エリアがこのルールの一致に影響しないことを示します。
<br/>
      
5. 乗車区間(leg)が上記のいずれのルールにも一致しない場合、運賃は不明です。

<br/>

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
| `leg_group_id` | ID | 任意 | [fare_leg_rules.txt](#fare_leg_rulestxt) 内のエントリのグループを識別します。<br><br> `fare_transfer_rules.from_leg_group_id` と `fare_transfer_rules.to_leg_group_id` 間の運賃乗換ルールを記述するために使用されます。<br><br>[fare_leg_rules.txt](#fare_leg_rulestxt) 内の複数のエントリが同じ `fare_leg_rules.leg_group_id` に属することができます。<br><br>[fare_leg_rules.txt](#fare_leg_rulestxt) 内の同じエントリ（`fare_leg_rules.leg_group_id` を除く）は、複数の `fare_leg_rules.leg_group_id` に属してはいけません。|
| `network_id` | `routes.network_id` または `networks.network_id` を参照する外部ID| 任意 | 運賃乗車区間(leg)ルールに適用されるルートネットワークを識別します。<br><br>`rule_priority` フィールドが存在せず、かつフィルタリング対象の `network_id` に一致する `fare_leg_rules.network_id` 値がない場合、空の `fare_leg_rules.network_id` がデフォルトで一致します。<br><br> `fare_leg_rules.network_id` の空のエントリは、`fare_leg_rules.network_id` に記載されているものを除く、[routes.txt](#routestxt) または [networks.txt](#networkstxt) で定義されているすべてのネットワークに対応します。<br><br> ファイル内に `rule_priority` フィールドが存在する場合、空の `fare_leg_rules.network_id` は、乗車区間(leg)のルートネットワークがこのルールの一致に影響しないことを示します。<br><br>[複数の乗車区間(leg)からなる有効運賃区間(effective fare leg)](#fare_leg_join_rulestxt)に対して照合する場合、各乗車区間(leg)は一致に使用される同じ `network_id` を持たなければなりません。 |
| `from_area_id` | `areas.area_id` を参照する外部ID | 任意 | 出発エリアを識別します。<br><br>`rule_priority` フィールドが存在せず、かつフィルタリング対象の `area_id` に一致する `fare_leg_rules.from_area_id` 値がない場合、空の `fare_leg_rules.from_area_id` がデフォルトで一致します。 <br><br>`fare_leg_rules.from_area_id` の空のエントリは、`fare_leg_rules.from_area_id` に記載されているものを除く、`areas.area_id` で定義されているすべてのエリアに対応します。<br><br> ファイル内に `rule_priority` フィールドが存在する場合、空の `fare_leg_rules.from_area_id` は、乗車区間(leg)の出発エリアがこのルールの一致に影響しないことを示します。<br><br>[複数の乗車区間(leg)からなる有効運賃区間(effective fare leg)](#fare_leg_join_rulestxt)に対して照合する場合、有効運賃区間(effective fare leg)の最初の乗車区間(leg)を使用して出発エリアを決定します。 |
| `to_area_id` | `areas.area_id` を参照する外部ID | 任意 | 到着エリアを識別します。<br><br>`rule_priority` フィールドが存在せず、かつフィルタリング対象の `area_id` に一致する `fare_leg_rules.to_area_id` 値がない場合、空の `fare_leg_rules.to_area_id` がデフォルトで一致します。<br><br> `fare_leg_rules.to_area_id` の空のエントリは、`fare_leg_rules.to_area_id` に記載されているものを除く、`areas.area_id` で定義されているすべてのエリアに対応します。<br><br>ファイル内に `rule_priority` フィールドが存在する場合、空の `fare_leg_rules.to_area_id` は、乗車区間(leg)の到着エリアがこのルールの一致に影響しないことを示します。<br><br>[複数の乗車区間(leg)からなる有効運賃区間(effective fare leg)](#fare_leg_join_rulestxt)に対して照合する場合、有効運賃区間(effective fare leg)の最後の乗車区間(leg)を使用して到着エリアを決定します。 |
|  `from_timeframe_group_id` | `timeframes.timeframe_group_id` を参照する外部ID | 任意 |  運賃乗車区間(leg)の開始時における運賃検証イベントの時間枠を定義します。<br><br>運賃乗車区間(leg)の「開始時刻」は、イベントの発生が予定されている時刻です。たとえば、この時刻は、乗客が乗車して運賃を検証する運賃乗車区間(leg)の開始時におけるバスの予定出発時刻とすることができます。以下のルール照合セマンティクスでは、開始時刻は [timeframes.txt](#timeframestxt) の [Local Time Semantics](#timeframe-local-time-semantics) によって決定される現地時刻で計算されます。適切な場合、タイムゾーンの解決には運賃乗車区間(leg)の出発イベントの停留所等(stop)または駅を使用するべきです。<br><br>`from_timeframe_group_id` を指定する運賃乗車区間(leg)ルールについて、以下のすべての条件が真である [timeframes.txt](#timeframestxt) 内のレコードが少なくとも1つ存在する場合、そのルールは特定の乗車区間(leg)に一致します。<br>- `timeframe_group_id` の値が `from_timeframe_group_id` の値と等しいこと。<br>- レコードの `service_id` によって識別される運行日の集合に、運賃乗車区間(leg)の開始時刻の「当日」が含まれること。<br>- 運賃乗車区間(leg)の開始時刻の「時刻」が、レコードの `timeframes.start_time` 値以上かつ `timeframes.end_time` 値未満であること。<br><br>空の `fare_leg_rules.from_timeframe_group_id` は、乗車区間(leg)の開始時刻がこのルールの一致に影響しないことを示します。<br><br>[複数の乗車区間(leg)からなる有効運賃区間(effective fare leg)](#fare_leg_join_rulestxt)に対して照合する場合、有効運賃区間(effective fare leg)の最初の乗車区間(leg)を使用して開始運賃検証イベントを決定します。 |
|  `to_timeframe_group_id` |  `timeframes.timeframe_group_id` を参照する外部ID | 任意 |  運賃乗車区間(leg)の終了時における運賃検証イベントの時間枠を定義します。<br><br>運賃乗車区間(leg)の「終了時刻」は、イベントの発生が予定されている時刻です。たとえば、この時刻は、乗客が降車して運賃を検証する運賃乗車区間(leg)の終了時におけるバスの予定到着時刻とすることができます。以下のルール照合セマンティクスでは、終了時刻は [timeframes.txt](#timeframestxt) の [Local Time Semantics](#timeframe-local-time-semantics) によって決定される現地時刻で計算されます。適切な場合、タイムゾーンの解決には運賃乗車区間(leg)の到着イベントの停留所等(stop)または駅を使用するべきです。<br><br>`to_timeframe_group_id` を指定する運賃乗車区間(leg)ルールについて、以下のすべての条件が真である [timeframes.txt](#timeframestxt) 内のレコードが少なくとも1つ存在する場合、そのルールは特定の乗車区間(leg)に一致します。<br>- `timeframe_group_id` の値が `to_timeframe_group_id` の値と等しいこと。<br>- レコードの `service_id` によって識別される運行日の集合に、運賃乗車区間(leg)の終了時刻の「当日」が含まれること。<br>- 運賃乗車区間(leg)の終了時刻の「時刻」が、レコードの `timeframes.start_time` 値以上かつ `timeframes.end_time` 値未満であること。<br><br>空の `fare_leg_rules.to_timeframe_group_id` は、乗車区間(leg)の終了時刻がこのルールの一致に影響しないことを示します。<br><br>[複数の乗車区間(leg)からなる有効運賃区間(effective fare leg)](#fare_leg_join_rulestxt)に対して照合する場合、有効運賃区間(effective fare leg)の最後の乗車区間(leg)を使用して終了運賃検証イベントを決定します。 |
| `fare_product_id` | `fare_products.fare_product_id` を参照する外部ID | **必須** | 乗車区間(leg)を移動するために必要なチケット商品。 |
| `rule_priority` | 非負整数 | 任意 | 一致するルールが乗車区間(leg)に適用される優先順位を定義し、特定のルールが他のルールより優先されるようにします。[fare_leg_rules.txt](#fare_leg_rulestxt) 内の複数のエントリが一致する場合、`rule_priority` の値が最も高いルールまたはルールの集合が選択されます。<br><br>`rule_priority` の空の値はゼロとして扱われます。 |

### fare_leg_join_rules.txt {: #fare_leg_join_rulestxt}


ファイル: **任意**

主キー (`from_network_id, to_network_id, from_stop_id, to_stop_id`)

乗換を伴う連続する2つの乗車区間(leg)からなる部分旅程(sub-journey)について、乗換がファイル内の特定のレコードで指定されたすべての一致条件に一致する場合、それら2つの乗車区間(leg)は、[fare_leg_rules.txt](#fare_leg_rulestxt) のルールとの照合を目的として、単一の**有効運賃区間(effective fare leg)**と見なすべきです。

- `from_stop_id` および `to_stop_id` によって明示的に上書きされない限り、乗換前の乗車区間(leg)の最後の駅と乗換後の乗車区間(leg)の最初の駅は、レコードにおいて同一でなければなりません。
- ファイル内の特定のレコードについて、一致条件フィールド値が空白または未指定である場合、そのフィールドは照合の目的で無視するべきです。
- 部分旅程(sub-journey)に、それぞれが結合ルールに一致する連続した乗換が含まれる場合、部分旅程(sub-journey)全体を単一の**有効運賃区間(effective fare leg)**と見なすべきです。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
| `from_network_id` | `routes.network_id` または `networks.network_id` を参照する外部ID| **必須** | 指定されたルートネットワークを使用する乗換前の乗車区間(leg)に一致します。指定する場合、同じ `to_network_id` も指定しなければなりません。 |
| `to_network_id` | `routes.network_id` または `networks.network_id` を参照する外部ID| **必須** | 指定されたルートネットワークを使用する乗換後の乗車区間(leg)に一致します。指定する場合、同じ `from_network_id` も指定しなければなりません。 |
| `from_stop_id` | `stops.stop_id` を参照する外部ID| **条件付き必須** | 指定された停留所等(stop)（`location_type=0` または空）または駅（`location_type=1`）で終了する乗換前の乗車区間(leg)に一致します。<br><br>条件付き必須:<br> - `to_stop_id` が定義されている場合は**必須**です。<br> - それ以外の場合は任意です。 |
| `to_stop_id` | `stops.stop_id` を参照する外部ID| **条件付き必須** | 指定された停留所等(stop)（`location_type=0` または空）または駅（`location_type=1`）で開始する乗換後の乗車区間(leg)に一致します。<br><br>条件付き必須:<br> - `from_stop_id` が定義されている場合は**必須**です。<br> - それ以外の場合は任意です。 |

### fare_transfer_rules.txt {: #fare_transfer_rulestxt}


ファイル: **任意**

主キー (`from_leg_group_id, to_leg_group_id, fare_product_id, transfer_count, duration_limit`)

[fare_leg_rules.txt](#fare_leg_rulestxt) で定義される乗車区間(leg)間の乗り換えに関する運賃ルールです。`from_leg_group_id` から `to_leg_group_id` への運賃乗り換えルールは、逆方向には適用されません。

複数の乗車区間(leg)からなる旅程(journey)の費用を処理するには:

1. 乗客の旅程(journey)に基づき、移動のすべての個別の乗車区間(leg)または有効運賃区間(effective fare leg)について、[fare_leg_rules.txt](#fare_leg_rulestxt) で定義される適用可能な運賃乗車区間グループを決定するべきです。
2. ファイル [fare_transfer_rules.txt](#fare_transfer_rulestxt) は、乗り換えの特性を定義するフィールドでフィルタリングしなければなりません。これらのフィールドは次のとおりです:
    - `fare_transfer_rules.from_leg_group_id`
    - `fare_transfer_rules.to_leg_group_id`<br/>
    <br/>

3. 乗り換えの特性に基づいて、乗り換えが [fare_transfer_rules.txt](#fare_transfer_rulestxt) 内のレコードと完全に一致する場合、そのレコードを処理して乗り換え費用を決定しなければなりません。
4. 完全一致が見つからない場合、乗り換え費用を処理するために、`from_leg_group_id` または `to_leg_group_id` の空のエントリを確認しなければなりません:
    - `fare_transfer_rules.from_leg_group_id` の空のエントリは、`fare_transfer_rules.from_leg_group_id` に列挙されているものを除く、`fare_leg_rules.leg_group_id` で定義されるすべての乗車区間グループに対応します
    - `fare_transfer_rules.to_leg_group_id` の空のエントリは、`fare_transfer_rules.to_leg_group_id` に列挙されているものを除く、`fare_leg_rules.leg_group_id` で定義されるすべての乗車区間グループに対応します<br/>
    <br/>
5. 乗り換えが上記のいずれのルールにも一致しない場合、乗り換えの取り決めはなく、乗車区間(leg)は別個のものと見なされます。

<br/>

|  フィールド名 | 型 | 存在性 | 説明 |
|  ------ | ------ | ------ | ------ |
| `from_leg_group_id` | `fare_leg_rules.leg_group_id` を参照する外部 ID | 任意 | 乗り換え前の運賃乗車区間ルールのグループを識別します。<br><br>フィルタリング対象の `leg_group_id` に一致する `fare_transfer_rules.from_leg_group_id` 値がない場合、空の `fare_transfer_rules.from_leg_group_id` がデフォルトで一致します。<br><br>`fare_transfer_rules.from_leg_group_id` の空のエントリは、`fare_transfer_rules.from_leg_group_id` に列挙されているものを除く、`fare_leg_rules.leg_group_id` で定義されるすべての乗車区間グループに対応します。|
| `to_leg_group_id` | `fare_leg_rules.leg_group_id` を参照する外部 ID | 任意 | 乗り換え後の運賃乗車区間ルールのグループを識別します。<br><br>フィルタリング対象の `leg_group_id` に一致する `fare_transfer_rules.to_leg_group_id` 値がない場合、空の `fare_transfer_rules.to_leg_group_id` がデフォルトで一致します。<br><br>`fare_transfer_rules.to_leg_group_id` の空のエントリは、`fare_transfer_rules.to_leg_group_id` に列挙されているものを除く、`fare_leg_rules.leg_group_id` で定義されるすべての乗車区間グループに対応します。 |
| `transfer_count` | ゼロ以外の整数 | **条件付き禁止** | 乗り換えルールを連続して適用できる乗り換え回数を定義します。<br><br>有効な選択肢は次のとおりです:<br>`-1` - 制限なし。<br>`1` 以上 - 乗り換えルールが適用される乗り換え回数を定義します。<br><br>部分旅程(sub-journey)が異なる `transfer_count` を持つ複数のレコードに一致する場合、部分旅程(sub-journey)の現在の乗り換え回数以上である最小の `transfer_count` を持つルールを選択するべきです。<br><br>条件付き禁止:<br>- `fare_transfer_rules.from_leg_group_id` が `fare_transfer_rules.to_leg_group_id` と等しくない場合は、**禁止**です。<br>- `fare_transfer_rules.from_leg_group_id` が `fare_transfer_rules.to_leg_group_id` と等しい場合は、**必須**です。 |
| `duration_limit` | 正の整数 | 任意 | 乗り換えの時間制限を定義します。<br><br>秒単位の整数増分で表現しなければなりません。<br><br>時間制限がない場合、`fare_transfer_rules.duration_limit` は空でなければなりません。 |
| `duration_limit_type` | Enum | **条件付き必須** | `fare_transfer_rules.duration_limit` の相対的な開始および終了を定義します。<br><br>有効な選択肢は次のとおりです:<br>`0` - 乗り換え部分旅程(sub-journey)の最初の乗車区間(leg)における出発時の運賃認証と、乗り換え部分旅程(sub-journey)の最後の乗車区間(leg)における到着時の運賃認証の間。<br>`1` - 乗り換え部分旅程(sub-journey)の最初の乗車区間(leg)における出発時の運賃認証と、乗り換え部分旅程(sub-journey)の最後の乗車区間(leg)における出発時の運賃認証の間。<br>`2` - 乗り換え部分旅程(sub-journey)の最初の乗車区間(leg)における到着時の運賃認証と、乗り換え部分旅程(sub-journey)の最後の乗車区間(leg)における出発時の運賃認証の間。<br>`3` - 乗り換え部分旅程(sub-journey)の最初の乗車区間(leg)における到着時の運賃認証と、乗り換え部分旅程(sub-journey)の最後の乗車区間(leg)における到着時の運賃認証の間。<br><br>複数の乗車区間(leg)からなる旅程(journey)内で、同じ `from_leg_group_id` および `to_leg_group_id` を持つ乗り換えルールが連続して複数回一致する場合、ルールで指定された `duration_limit` は、最初に一致した乗車区間(leg)から測定するべきです。<br><br>条件付き必須:<br>- `fare_transfer_rules.duration_limit` が定義されている場合は、**必須**です。<br>- `fare_transfer_rules.duration_limit` が空の場合は、**禁止**です。 |
| `fare_transfer_type` | Enum | **必須** | 旅程(journey)内の乗車区間(leg)間を乗り換える際の費用処理方法を示します: <br>![](../../assets/2-leg.svg) <br>有効な選択肢は次のとおりです:<br>`0` - 乗り換え前の乗車区間(leg)の `fare_leg_rules.fare_product_id` に `fare_transfer_rules.fare_product_id` を加算します。A + AB。<br>`1` - 乗り換え前の乗車区間(leg)の `fare_leg_rules.fare_product_id` に `fare_transfer_rules.fare_product_id` および乗り換え後の乗車区間(leg)の `fare_leg_rules.fare_product_id` を加算します。A + AB + B。<br>`2` - `fare_transfer_rules.fare_product_id`。AB。<br><br>旅程(journey)内の複数の乗り換え間における費用処理の相互作用:<br>![](../../assets/3-leg.svg)<br><table><thead><tr><th>`fare_transfer_type`</th><th>A > B の処理</th><th>B > C の処理</th></tr></thead><tbody><tr><td>`0`</td><td>A + AB</td><td>S + BC</td></tr><tr><td>`1`</td><td>A + AB +B</td><td>S + BC + C</td></tr><tr><td>`2`</td><td>AB</td><td>S + BC</td></tr></tbody></table>ここで、S は先行する乗車区間(leg)および乗り換えの処理済み費用の合計を示します。 |
| `fare_product_id` | `fare_products.fare_product_id` を参照する外部 ID | 任意 | 2つの運賃乗車区間間を乗り換えるために必要なチケット商品です。空の場合、乗り換えルールの費用は 0 です。|

### areas.txt {: #areastxt}


ファイル: **任意**

主キー (`area_id`)

エリア識別子を定義します。

|  フィールド名 | 型 | 有無 | 説明 |
|  ------ | ------ | ------ | ------ |
| `area_id` | 一意のID | **必須** | エリアを識別します。[areas.txt](#areastxt) 内で一意でなければなりません。 |
| `area_name` | テキスト | **任意** | 乗客に表示されるエリアの名称です。 |

### stop_areas.txt {: #stop_areastxt}


ファイル: **任意**

主キー (`*`)

[stops.txt](#stopstxt) の停留所等(stop)をエリアに割り当てます。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
| `area_id` | `areas.area_id` を参照する外部 ID | **必須** | 1つまたは複数の `stop_id` が属するエリアを識別します。同じ `stop_id` を複数の `area_id` で定義することができます。 |
| `stop_id` | `stops.stop_id` を参照する外部 ID | **必須** | 停留所等(stop)を識別します。このフィールドで駅（すなわち、`stops.location_type=1` の停留所等(stop)）が定義されている場合、そのすべてのプラットフォーム（すなわち、この駅が `stops.parent_station` として定義されている、`stops.location_type=0` のすべての停留所等(stop)）は同じエリアの一部であると見なされます。この動作は、プラットフォームを他のエリアに割り当てることで上書きできます。 |

### networks.txt {: #networkstxt}


ファイル: **条件付きで禁止**

主キー (`network_id`)

運賃区間ルールに適用されるネットワーク識別子を定義します。 

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
| `network_id` | 一意の ID | **必須** | ネットワークを識別します。[networks.txt](#networkstxt) 内で一意でなければなりません。 |
| `network_name` | テキスト | **任意** | 地域の事業者およびその乗客が使用する、運賃区間ルールに適用されるネットワークの名称です。 |

### route_networks.txt {: #route_networkstxt}


ファイル: **条件付きで禁止**

主キー (`route_id`)

[routes.txt](#routestxt) のルート・路線系統(route)をネットワークに割り当てます。 

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
| `network_id` | `networks.network_id` を参照する外部 ID | **必須** | 1つまたは複数の `route_id` が属するネットワークを識別します。`route_id` は1つの `network_id` でのみ定義できます。 |
| `route_id` | `routes.route_id` を参照する外部 ID | **必須** | ルート・路線系統(route)を識別します。 |

### shapes.txt {: #shapestxt}


ファイル: **条件付き必須**

主キー (`shape_id`, `shape_pt_sequence`)

ルート形状(shape)は、車両がルート・路線系統(route)の経路に沿って走行する経路を記述するものであり、shapes.txt ファイルで定義されます。ルート形状(shape)は便(trip)に関連付けられ、車両が順番に通過する一連の地点で構成されます。ルート形状(shape)は停留所等(stop)の位置と正確に交差する必要はありませんが、便(trip)上のすべての停留所等(stop)は、その便(trip)のルート形状(shape)から、すなわちルート形状(shape)の地点を結ぶ直線区間の近くから、わずかな距離内にあるべきです。shapes.txt ファイルは、すべてのルートベースのサービスに含めるべきです（ゾーンベースのデマンド型サービスには含めません）。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `shape_id` | ID | **必須** | ルート形状(shape)を識別します。 |
|  `shape_pt_lat` | 緯度 | **必須** | ルート形状(shape)の地点の緯度です。[shapes.txt](#shapestxt) の各レコードは、ルート形状(shape)の定義に使用されるルート形状(shape)の地点を表します。 |
|  `shape_pt_lon` | 経度 | **必須** | ルート形状(shape)の地点の経度です。 |
|  `shape_pt_sequence` | 非負整数 | **必須** | ルート形状(shape)の地点が接続されてルート形状(shape)を形成する順序です。値は便(trip)に沿って増加しなければなりませんが、連続している必要はありません。<hr>*例: ルート形状(shape)「A_shp」の定義に3つの地点がある場合、[shapes.txt](#shapestxt) ファイルにはルート形状(shape)を定義するために次のレコードが含まれる可能性があります。* <br> `shape_id,shape_pt_lat,shape_pt_lon,shape_pt_sequence` <br> `A_shp,37.61956,-122.48161,0` <br> `A_shp,37.64430,-122.41070,6` <br> `A_shp,37.65863,-122.30839,11` |
|  `shape_dist_traveled` | 非負浮動小数点数 | 任意 | 最初のルート形状(shape)の地点から、このレコードで指定された地点まで、ルート形状(shape)に沿って実際に移動した距離です。旅程(journey)プランナーが地図上でルート形状(shape)の正しい部分を表示するために使用します。値は `shape_pt_sequence` とともに増加しなければなりません。ルート・路線系統(route)に沿った逆方向の移動を示すために使用してはいけません。距離の単位は、[stop_times.txt](#stop_timestxt) で使用されるものと一貫していなければなりません。<br><br>ループまたはインライン化（車両が1つの便(trip)内で同じ経路の一部を横断または走行すること）があるルート・路線系統(route)に推奨されます。<br><img src="../../../assets/inlining.svg" width=200px style="display: block; margin-left: auto; margin-right: auto;"> <br>車両が便(trip)の途中で経路を引き返す、または横断する場合、`shape_dist_traveled` は、[shapes.txt](#shapestxt) 内の地点の部分が [stop_times.txt](#stop_timestxt) 内のレコードとどのように対応するかを明確にするために重要です。<hr>*例: バスが上記で定義された A_shp の3つの地点に沿って走行する場合、追加の `shape_dist_traveled` 値（ここではキロメートルで示します）は次のようになります。* <br> `shape_id,shape_pt_lat,shape_pt_lon,shape_pt_sequence,shape_dist_traveled`<br>`A_shp,37.61956,-122.48161,0,0`<br>`A_shp,37.64430,-122.41070,6,6.8310` <br> `A_shp,37.65863,-122.30839,11,15.8765` |

### frequencies.txt {: #frequenciestxt}


ファイル: **任意**

主キー (`trip_id`, `start_time`)

[Frequencies.txt](#frequenciestxt) は、規則的な運行間隔（便間の時間）で運行する便を表します。このファイルは、2つの異なる種類のサービスを表すために使用することができます。

* 1日を通じて固定された時刻表に従わない、運行間隔ベースのサービス（`exact_times`=`0`）。代わりに、運行事業者は便についてあらかじめ定められた運行間隔を厳密に維持しようとします。
* 指定された期間にわたって便の運行間隔が完全に同一である、時刻表ベースのサービス（`exact_times`=`1`）の圧縮表現。時刻表ベースのサービスでは、運行事業者は時刻表を厳密に遵守しようとします。


|  フィールド名 | 型 | 存在要件 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `trip_id` | `trips.trip_id` を参照する外部 ID | **必須** | 指定された運行間隔のサービスが適用される便を識別します。 |
|  `start_time` | 時刻 | **必須** | 指定された運行間隔で、最初の車両が便の最初の停留所等(stop)から出発する時刻です。 |
|  `end_time` | 時刻 | **必須** | 便の最初の停留所等(stop)において、サービスが異なる運行間隔に変更される（または終了する）時刻です。 |
|  `headway_secs` | 正の整数 | **必須** | `start_time` と `end_time` で指定された時間間隔中における、便について同じ停留所等(stop)からの出発間の秒単位の時間（運行間隔）です。同じ便に対して複数の運行間隔を定義することができますが、重複してはいけません。新しい運行間隔は、前の運行間隔が終了する正確な時刻に開始することができます。  |
|  `exact_times` | 列挙型 | 任意 | 便のサービスの種類を示します。詳細についてはファイルの説明を参照してください。有効な選択肢は次のとおりです。<br><br>`0` または空 - 運行間隔ベースの便。<br>`1` - 1日を通じて運行間隔が完全に同一である時刻表ベースの便。この場合、`end_time` の値は、最後に希望する便の `start_time` より大きく、かつ最後に希望する便の start_time + `headway_secs` より小さくなければなりません。 |

### transfers.txt {: #transferstxt}


ファイル: **任意**

主キー (`from_stop_id`, `to_stop_id`, `from_trip_id`, `to_trip_id`, `from_route_id`, `to_route_id`)

旅程を計算する際、GTFSを利用するアプリケーションは、許容される時間および停留所等(stop)の近接性に基づいて乗換を補間します。[Transfers.txt](#transferstxt) は、選択された乗換に対する追加ルールおよび上書きを指定します。

フィールド `from_trip_id`、`to_trip_id`、`from_route_id`、および `to_route_id` により、乗換ルールのより高い特異性の順序を指定できます。`from_stop_id` および `to_stop_id` とともに、特異性の順位は以下のとおりです。

1. 両方の `trip_id` が定義されている: `from_trip_id` および `to_trip_id`。
2. 1つの `trip_id` と `route_id` の組が定義されている: (`from_trip_id` および `to_route_id`) または (`from_route_id` および `to_trip_id`)。
3. 1つの `trip_id` が定義されている: `from_trip_id` または `to_trip_id`。
4. 両方の `route_id` が定義されている: `from_route_id` および `to_route_id`。
5. 1つの `route_id` が定義されている: `from_route_id` または `to_route_id`。
6. `from_stop_id` および `to_stop_id` のみが定義されている: route または trip 関連フィールドは設定されていません。

到着する便(trip)と出発する便(trip)の所与の順序付きペアについて、これら2つの便(trip)間に適用される、最も特異性の高い乗換が選択されます。任意の便(trip)のペアについて、適用可能な同等に最大の特異性を持つ乗換が2つ存在するべきではありません。

|  フィールド名 | 型 | 存在性 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `from_stop_id` | `stops.stop_id` を参照する外部ID | **条件付き必須** | ルート・路線系統(route)間の接続が開始する停留所等(stop)（`location_type=0`）または駅（`location_type=1`）を識別します。このフィールドが駅を参照する場合、乗換ルールはそのすべての子停留所等(stop)に適用されます。`transfer_type` が `4` または `5` の場合、停留所等(stop)（`location_type=0`）を参照しなければなりません。<br>条件付き必須:<br>- `transfer_type` が空、`0`、`1`、`2`、または `3` の場合は**必須**です。<br>- `transfer_type` が `4` または `5` の場合は任意です。 |
|  `to_stop_id` | `stops.stop_id` を参照する外部ID | **条件付き必須** | ルート・路線系統(route)間の接続が終了する停留所等(stop)（`location_type=0`）または駅（`location_type=1`）を識別します。このフィールドが駅を参照する場合、乗換ルールはすべての子停留所等(stop)に適用されます。`transfer_type` が 4 または 5 の場合、停留所等(stop)（`location_type=0`）を参照しなければなりません。<br><br>条件付き必須:<br>- `transfer_type` が空、`0`、`1`、`2`、または `3` の場合は**必須**です。<br>- `transfer_type` が `4` または `5` の場合は任意です。 |
| `from_route_id`  | `routes.route_id` を参照する外部ID | 任意 | 接続が開始するルート・路線系統(route)を識別します。<br><br>`from_route_id` が定義されている場合、乗換は、指定された `from_stop_id` における当該ルート・路線系統(route)の到着便(trip)に適用されます。<br><br>`from_trip_id` と `from_route_id` の両方が定義されている場合、`trip_id` は `route_id` に属していなければならず、`from_trip_id` が優先されます。 |
| `to_route_id`  | `routes.route_id` を参照する外部ID | 任意 | 接続が終了するルート・路線系統(route)を識別します。<br><br>`to_route_id` が定義されている場合、乗換は、指定された `to_stop_id` における当該ルート・路線系統(route)の出発便(trip)に適用されます。<br><br>`to_trip_id` と `to_route_id` の両方が定義されている場合、`trip_id` は `route_id` に属していなければならず、`to_trip_id` が優先されます。 |
| `from_trip_id`  | `trips.trip_id` を参照する外部ID | **条件付き必須** | ルート・路線系統(route)間の接続が開始する便(trip)を識別します。<br><br>`from_trip_id` が定義されている場合、乗換は、指定された `from_stop_id` における到着便(trip)に適用されます。<br><br>`from_trip_id` と `from_route_id` の両方が定義されている場合、`trip_id` は `route_id` に属していなければならず、`from_trip_id` が優先されます。<br><br>条件付き必須:<br>- `transfer_type` が `4` または `5` の場合は**必須**です。 <br>- それ以外の場合は任意です。 |
| `to_trip_id`  | `trips.trip_id` を参照する外部ID | **条件付き必須** | ルート・路線系統(route)間の接続が終了する便(trip)を識別します。<br><br>`to_trip_id` が定義されている場合、乗換は、指定された `to_stop_id` における出発便(trip)に適用されます。<br><br>`to_trip_id` と `to_route_id` の両方が定義されている場合、`trip_id` は `route_id` に属していなければならず、`to_trip_id` が優先されます。 <br><br>条件付き必須:<br>- `transfer_type` が `4` または `5` の場合は**必須**です。 <br>- それ以外の場合は任意です。 |
|  `transfer_type` | 列挙型 | **必須** | 指定された (`from_stop_id`, `to_stop_id`) ペアの接続タイプを示します。有効な選択肢は以下のとおりです。<br><br> `0` または空 - ルート・路線系統(route)間の推奨乗換地点。<br>`1` - 2つのルート・路線系統(route)間の時刻指定乗換地点。出発車両は到着車両を待機し、乗客がルート・路線系統(route)間を乗り換えるための十分な時間を確保して出発することが期待されます。<br>`2` - 接続を確保するため、乗換には到着と出発の間に最小限の時間が必要です。乗換に必要な時間は `min_transfer_time` で指定されます。<br>`3` - 当該地点ではルート・路線系統(route)間の乗換はできません。<br>`4` - 乗客は同じ車両に乗車したまま、ある便(trip)から別の便(trip)へ乗り換えることができます（「座席内乗換」）。このタイプの乗換の詳細は[以下](#linked-trips)を参照してください。  <br>`5` - 連続する便(trip)間では座席内乗換は許可されません。乗客は車両から降車し、再乗車しなければなりません。このタイプの乗換の詳細は[以下](#linked-trips)を参照してください。 |
|  `min_transfer_time` | 非負整数 | **条件付き必須** | 指定された停留所等(stop)におけるルート・路線系統(route)間の乗換を許可するために利用可能でなければならない時間（秒）。`min_transfer_time` は、各ルート・路線系統(route)の時刻表の変動を考慮するためのバッファ時間を含め、一般的な乗客が2つの停留所等(stop)間を移動できるのに十分であるべきです。 <br><br>条件付き必須:<br>- `transfer_type` が `2` の場合は**必須**です。 <br>- それ以外の場合は任意です。 |

#### 連結された便(trip) {: #linked-trips}


以下は、座席を移動せずに乗り換える場合とそうでない場合の両方で、便(trip)同士を連結するために使用される `transfer_type=4` および `=5` に適用されます。

連結される便(trip)は、同一の車両によって運行されなければなりません。車両は、他の車両と連結または切り離されることができます。

連結された便(trip)の乗換と block_id の両方が提供され、それらが矛盾する結果を生成する場合、連結された便(trip)の乗換を使用しなければなりません。

`from_trip_id` の最後の停留所等(stop)は `to_trip_id` の最初の停留所等(stop)と地理的に近接しているべきであり、`from_trip_id` の最後の到着時刻は `to_trip_id` の最初の出発時刻より前で、かつ近接しているべきです。`to_trip_id` の便(trip)が翌運行日(service day)に発生する場合、`from_trip_id` の最後の到着時刻は `to_trip_id` の最初の出発時刻より後であることができます。 

通常の場合、便(trip)は1対1で連結することができますが、より複雑な便(trip)の継続を表すために、1対n、n対1、またはn対nで連結することもできます。例えば、2つの列車便(trip)（以下の図の便(trip) Aおよび便(trip) B）は、共通の駅での車両連結操作の後、1つの列車便(trip)（便(trip) C）に統合できます。

- 1対nの継続では、各 `to_trip_id` の `trips.service_id` は同一でなければなりません。
- n対1の継続では、各 `from_trip_id` の `trips.service_id` は同一でなければなりません。
- n対nの継続は、両方の制約を満たさなければなりません。
- `trip.service_id` がいずれの運行日にも重複してはいけないことを条件として、便(trip)は複数の異なる継続の一部として連結することができます。 

<pre>
便(trip) A
───────────────────\
                    \    便(trip) C
                     ─────────────
便(trip) B              /
───────────────────/
</pre>

### pathways.txt {: #pathwaystxt}


ファイル: **任意**

主キー (`pathway_id`)

ファイル [pathways.txt](#pathwaystxt) および [levels.txt](#levelstxt) は、地下鉄駅または鉄道駅を記述するためにグラフ表現を使用します。ノードは場所を表し、エッジは構内通路(pathway)を表します。

駅の入口/出口（`location_type=2` の場所として表されるノード）からプラットフォーム（`location_type=0` または空の場所として表されるノード）へ移動するために、乗客は、構内通路(pathway)として表される通路、改札口、階段、その他のエッジを通過します。汎用ノード（`location_type=3` で表されるノード）は、駅全体の構内通路(pathway)を接続するために使用できます。

構内通路(pathway)は、駅の内部アクセスグラフを網羅的に定義することを意図しています。駅内に構内通路(pathway)が定義されている場合、データ利用者は、その駅内のすべての関連する接続が記述されていると仮定するべきです。ただし、`stops.txt` の任意の `stop_access` フィールドは、停留所等(stop)が道路ネットワークから直接アクセス可能か、または駅で定義された構内通路(pathway)を通じてアクセス可能かを明示的に定義するために使用できます。したがって、以下のガイドラインが適用されます。

- 孤立した場所を作らない: 駅内のいずれかの場所に構内通路(pathway)がある場合、以下を除き、その駅内のすべての場所に構内通路(pathway)があるべきです。
    - 乗車エリア（`location_type=4`、以下のガイドラインを参照）があるプラットフォーム
    - `stops.stop_access=1` を持つ停留所等(stop)（`location_type=0` または空）
- 乗車エリアがあるプラットフォームには構内通路(pathway)を設定しない: 乗車エリア（`location_type=4`）があるプラットフォーム（`location_type=0` または空）は、地点ではなく親オブジェクトとして扱われます。この場合、プラットフォームに構内通路(pathway)を割り当ててはいけません。すべての構内通路(pathway)は、プラットフォームの各乗車エリアに割り当てるべきです。
- 閉鎖されたプラットフォームを作らない: 駅内のいずれかの場所に構内通路(pathway)がある場合、各プラットフォーム（`location_type=0` または空）または乗車エリア（`location_type=4`）は、構内通路(pathway)の連鎖を通じて少なくとも1つの入口/出口（`location_type=2`）に接続されなければなりません。ただし、以下の場合を除きます。
    - 停留所等(stop)（`location_type=0` または空）が `stops.stop_access=1` で明示的にマークされている場合。この場合、道路ネットワークから直接アクセス可能であると見なされます。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `pathway_id` | 一意の ID | **必須** | 構内通路(pathway)を識別します。システムによりレコードの内部識別子として使用されます。データセット内で一意でなければなりません。<br><br>異なる構内通路(pathway)は、`from_stop_id` と `to_stop_id` に同じ値を持つことができます。<hr>_例: 2つのエスカレーターが反対方向に並んでいる場合、または階段とエレベーターが同じ場所から同じ場所へ移動する場合、異なる `pathway_id` が同じ `from_stop_id` および `to_stop_id` の値を持つことがあります。_|
|  `from_stop_id` | `stops.stop_id` を参照する外部 ID | **必須** | 構内通路(pathway)が始まる場所です。<br><br>プラットフォーム（`location_type=0` または空）、入口/出口（`location_type=2`）、汎用ノード（`location_type=3`）、または乗車エリア（`location_type=4`）を識別する `stop_id` を含まなければなりません。<br><br>駅（`location_type=1`）、または `stop_access=1` を持つ停留所等(stop)（`location_type=0` または空）を識別する `stop_id` の値は禁止されています。|
|  `to_stop_id` | `stops.stop_id` を参照する外部 ID | **必須** | 構内通路(pathway)が終わる場所です。<br><br>プラットフォーム（`location_type=0` または空）、入口/出口（`location_type=2`）、汎用ノード（`location_type=3`）、または乗車エリア（`location_type=4`）を識別する `stop_id` を含まなければなりません。<br><br>駅（`location_type=1`）、または `stop_access=1` を持つ停留所等(stop)（`location_type=0` または空）を識別する `stop_id` の値は禁止されています。|
|  `pathway_mode` | 列挙型 | **必須** | 指定された (`from_stop_id`, `to_stop_id`) の組の間にある構内通路(pathway)の種類です。有効な選択肢は以下のとおりです。<br><br>`1` - 通路。<br>`2` - 階段。<br>`3` - 動く歩道/トラベレーター。<br>`4` - エスカレーター。<br>`5` - エレベーター。<br>`6` - 改札口（または支払いゲート）: 通過するために支払い証明が必要な駅のエリアへ入る構内通路(pathway)です。改札口は、駅の有料エリアと無料エリアを分離する場合や、同じ駅内の異なる支払いエリア同士を分離する場合があります。この情報は、乗客が不要な支払いを行う必要がある近道を利用して駅内を経路探索することを避けるために使用できます。たとえば、バス専用道路へ到達するために地下鉄のプラットフォームを歩くよう乗客に案内する場合です。<br>`7`- 出場ゲート: 支払い証明が不要な無料エリアへ、有料エリアから出る構内通路(pathway)です。|
|  `is_bidirectional` | 列挙型 | **必須** | 構内通路(pathway)を通行できる方向を示します。<br><br>`0` - `from_stop_id` から `to_stop_id` へのみ使用できる一方向の構内通路(pathway)。<br>`1` - 両方向に使用できる双方向の構内通路(pathway)。<br><br>出場ゲート（`pathway_mode=7`）は双方向であってはいけません。|
| `length` | 非負の浮動小数点数 | 任意 | 出発場所（`from_stop_id` で定義）から到着場所（`to_stop_id` で定義）までの構内通路(pathway)の水平方向の長さ（メートル単位）です。<br><br>このフィールドは、通路（`pathway_mode=1`）、改札口（`pathway_mode=6`）、および出場ゲート（`pathway_mode=7`）に対して推奨です。|
| `traversal_time` | 正の整数 | 任意 | 出発場所（`from_stop_id` で定義）から到着場所（`to_stop_id` で定義）まで構内通路(pathway)を歩いて通過するために必要な平均時間（秒単位）です。<br><br>このフィールドは、動く歩道（`pathway_mode=3`）、エスカレーター（`pathway_mode=4`）、およびエレベーター（`pathway_mode=5`）に対して推奨です。|
| `stair_count` | null ではない整数 | 任意 | 構内通路(pathway)の階段数です。<br><br>正の `stair_count` は、乗客が `from_stop_id` から `to_stop_id` へ上ることを意味します。負の `stair_count` は、乗客が `from_stop_id` から `to_stop_id` へ下ることを意味します。<br><br>このフィールドは、階段（`pathway_mode=2`）に対して推奨です。<br><br>推定された階段数しか提供できない場合、1階あたり15段として近似することが推奨されます。|
| `max_slope` | 浮動小数点数 | 任意 | 構内通路(pathway)の最大勾配比です。有効な選択肢は以下のとおりです。<br><br>`0` または空 - 勾配なし。<br>`Float` - 構内通路(pathway)の勾配比。上りは正、下りは負です。<br><br>このフィールドは、通路（`pathway_mode=1`）および動く歩道（`pathway_mode=3`）でのみ使用するべきです。<hr>_例: 米国では、0.083（8.3% とも表記）は手動車椅子の最大勾配比であり、1m ごとに 0.083m（つまり 8.3cm）上昇することを意味します。_|
| `min_width` | 正の浮動小数点数 | 任意 | 構内通路(pathway)の最小幅（メートル単位）です。<br><br>最小幅が1メートル未満の場合、このフィールドは推奨です。|
| `signposted_as` | テキスト | 任意 | 乗客に見える物理的な標識上の一般向けテキストです。<br><br>「～への標識に従ってください」など、乗客へのテキストによる案内を提供するために使用できます。`singposted_as` のテキストは、標識に印刷されているとおりに正確に表示されるべきです。<br><br>物理的な標識が多言語の場合、このフィールドは `feed_info.feed_lang` のフィールド定義における `stops.stop_name` の例に従って設定および翻訳することができます。|
| `reversed_signposted_as` | テキスト | 任意 | `signposted_as` と同じですが、構内通路(pathway)を `to_stop_id` から `from_stop_id` へ使用する場合のものです。|

### levels.txt {: #levelstxt}


ファイル: **条件付き必須**

主キー (`level_id`)

駅構内の階層を記述します。[pathways.txt](#pathwaystxt) と組み合わせて使用すると有用です。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `level_id` | 一意の ID | **必須** | 駅構内の階層を識別します。|
|  `level_index` | Float | **必須** | 階層の相対的な位置を示す数値インデックスです。<br><br>地上階のインデックスは `0` とするべきであり、地上より上の階層は正のインデックス、地上より下の階層は負のインデックスで示します。|
|  `level_name` | Text | 任意 | 建物または駅構内で乗客に表示される階層の名称です。<hr>_例: エレベーターで「Mezzanine」、「Platform」、または「-1」へ移動します。_|

### location_groups.txt {: #location_groupstxt}


ファイル: **任意**

主キー (`location_group_id`)

location groupを定義します。location groupは、乗客が乗車または降車をリクエストできる停留所等(stop)のグループです。

| フィールド名 | 型 | 存在 | 説明 |
| ---------- | ---- | ------------ | ----------- |
| `location_group_id` | 一意のID | **必須** | location groupを識別します。IDは、すべての`stops.stop_id`、locations.geojsonの`id`、および`location_groups.location_group_id`の値全体で一意でなければなりません。<br><br>location groupは、乗客が乗車または降車をリクエストできる場所をまとめて示す停留所等(stop)のグループです。 | 
| `location_group_name` | テキスト | 任意 | 乗客に表示されるlocation groupの名称です。 |

### location_group_stops.txt {: #location_group_stopstxt}


ファイル: **任意**

主キー (`*`)

stops.txt の停留所等(stop)をロケーショングループに割り当てます。

| フィールド名 | 型 | 存在 | 説明 |
| ---------- | ---- | ------------ | ----------- |
| `location_group_id` | `location_groups.location_group_id` を参照する外部 ID | **必須** | 1つまたは複数の `stop_id` が属するロケーショングループを識別します。同じ `stop_id` は複数の `location_group_id` で定義することができます。 | 
| `stop_id` | `stops.stop_id` を参照する外部 ID | **必須** | ロケーショングループに属する停留所等(stop)を識別します。 |

### locations.geojson {: #locationsgeojson}


ファイル: **任意**

乗客がデマンド型サービスによる乗車または降車をリクエストできるゾーンを定義します。これらのゾーンは GeoJSON ポリゴンとして表現されます。

- このファイルは、[RFC 7946](https://tools.ietf.org/html/rfc7946) で説明されている GeoJSON 形式のサブセットを使用します。
- 各ポリゴンは、[OpenGIS Simple Features Specification, section 6.1.11](http://www.opengis.net/doc/is/sfa/1.2.1) の定義により有効でなければなりません。
- `locations.geojson` ファイルには `FeatureCollection` を含めなければなりません。
- `FeatureCollection` は、乗客が乗車または降車をリクエストできるさまざまな停留所等(stop)の位置を定義します。
- すべての GeoJSON `Feature` には `id` がなければなりません。`id` は、すべての `stops.stop_id`、locations.geojson の `id`、および `location_group_id` の値全体で一意でなければなりません。
- すべての GeoJSON `Feature` には、以下の表に従ったオブジェクトおよび関連するキーを含めるべきです。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
| -&nbsp;`type` | String | **必須** | 位置の `"FeatureCollection"`。 |
| -&nbsp;`features` | Array | **必須** | 位置を説明する `"Feature"` オブジェクトのコレクション。 |
| &nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`type` | String | **必須** | `"Feature"` |
| &nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`id` | String | **必須** | 位置を識別します。ID は、すべての `stops.stop_id`、locations.geojson の `id`、および `location_groups.location_group_id` の値全体で一意でなければなりません。 |
| &nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`properties` | Object | **必須** | 位置のプロパティキー。 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`stop_name` | String | 任意 | 乗客に表示される位置の名称を示します。 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`stop_desc` | String | 任意 | 乗客が位置を把握するのに役立つ、意味のある位置の説明。 |
| &nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`geometry` | Object | **必須** | 位置のジオメトリ。 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`type` | String | **必須** | 次の型でなければなりません:<br>-&nbsp;`"Polygon"`<br>-&nbsp;`"MultiPolygon"` |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`coordinates` | Array | **必須** | 位置のジオメトリを定義する地理座標（緯度および経度）。 |

### booking_rules.txt {: #booking_rulestxt}


ファイル: **任意**

主キー (`booking_rule_id`)

乗客がリクエストするサービスの予約ルールを定義します

|  フィールド名 | 型 | 存在要件 | 説明 |
|  ------ | ------ | ------ | ------ |
| `booking_rule_id` | 一意のID | **必須** | ルールを識別します。 |
| `booking_type` | Enum | **必須** | 予約をどの程度前から行うことができるかを示します。有効な選択肢は次のとおりです。<br><br>`0` - リアルタイム予約。<br>`1` - 事前通知を伴う当日までの予約。<br>`2` - 前日以前までの予約。 |
| `prior_notice_duration_min` | Integer | **条件付き必須** | リクエストを行うために必要な、移動前の最小分数です。<br><br>**条件付き必須**:<br>- `booking_type=1` の場合は**必須**です。<br>- それ以外の場合は禁止です。 |
| `prior_notice_duration_max` | Integer | **条件付き禁止** | 予約リクエストを行うことができる、移動前の最大分数です。<br><br>**条件付き禁止**:<br>- `booking_type=0` および `booking_type=2` の場合は禁止です。<br>- `booking_type=1` の場合は任意です。|
| `prior_notice_last_day` | Integer | **条件付き必須** | 予約リクエストを行うことができる、移動前の最終日です。<br><br>例: 「乗車は1日前の午後5時までに予約しなければなりません」は、`prior_notice_last_day=1` としてエンコードされます。<br><br>**条件付き必須**:<br>- `booking_type=2` の場合は**必須**です。<br>- それ以外の場合は禁止です。 |
| `prior_notice_last_time` | Time | **条件付き必須** | 予約リクエストを行うことができる、移動前の最終日の最終時刻です。<br><br>例: 「乗車は1日前の午後5時までに予約しなければなりません」は、`prior_notice_last_time=17:00:00` としてエンコードされます。<br><br>**条件付き必須**:<br>- `prior_notice_last_day` が定義されている場合は**必須**です。<br>- それ以外の場合は禁止です。 |
| `prior_notice_start_day` | Integer | **条件付き禁止** | 予約リクエストを行うことができる、移動前の最も早い日です。<br><br>例: 「乗車は最も早くて1週間前の午前0時から予約できます」は、`prior_notice_start_day=7` としてエンコードされます。<br><br>**条件付き禁止**:<br>- `booking_type=0` の場合は禁止です。<br> - `prior_notice_duration_max` が定義されている場合、`booking_type=1` では禁止です。<br> - それ以外の場合は任意です。 |
| `prior_notice_start_time` | Time | **条件付き必須** | 予約リクエストを行うことができる、移動前の最も早い日の最も早い時刻です。<br><br>例: 「乗車は最も早くて1週間前の午前0時から予約できます」は、`prior_notice_start_time=00:00:00` としてエンコードされます。<br><br>**条件付き必須**:<br>- `prior_notice_start_day` が定義されている場合は**必須**です。<br>- それ以外の場合は禁止です。 |
| `prior_notice_service_id` | `calendar.service_id` を参照するForeign ID | **条件付き禁止** | `prior_notice_last_day` または `prior_notice_start_day` を数える運行日(service day)を示します。<br><br>例: 空の場合、`prior_notice_start_day=2` は2暦日前となります。祝日を除く平日のみを含む `service_id` として定義されている場合、`prior_notice_start_day=2` は2営業日前となります。<br><br>**条件付き禁止**:<br> - `booking_type=2` の場合は任意です。<br> - それ以外の場合は禁止です。 |
| `message` | Text | 任意 | オンデマンドの乗車・降車を予約する際に、`stop_time` でサービスを利用する乗客へのメッセージです。乗客がサービスを利用するために取る必要がある行動について、ユーザーインターフェース内で伝達される最小限の情報を提供することを目的としています。 |
| `pickup_message` | Text | 任意 | `message` と同じように機能しますが、乗客がオンデマンド乗車のみを利用する場合に使用されます。 |
| `drop_off_message` | Text | 任意 | `message` と同じように機能しますが、乗客がオンデマンド降車のみを利用する場合に使用されます。 |
| `phone_number` | Phone number | 任意 | 予約リクエストを行うために電話する電話番号です。 |
| `info_url` | URL | 任意 | 予約ルールに関する情報を提供するURLです。 |
| `booking_url` | URL | 任意 | 予約リクエストを行うことができるオンラインインターフェースまたはアプリのURLです。 |

### translations.txt {: #translationstxt}


ファイル: **任意**

主キー (`table_name`, `field_name`, `language`, `record_id`, `record_sub_id`, `field_value`)

複数の公用語がある地域では、交通事業者・運行事業者は通常、言語固有の名称およびWebページを持っています。これらの地域の乗客に最適なサービスを提供するため、データセットにこれらの言語依存の値を含めることが有用です。

同じ値を翻訳するために、参照方法（`record_id`、`record_sub_id`）と`field_value`の両方が異なる2つの行で使用される場合、（`record_id`、`record_sub_id`）で提供される翻訳が優先されます。

|  フィールド名 | 型 | 必須性 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `table_name` | Enum | **必須** | 翻訳対象のフィールドを含むテーブルを定義します。許可される値は以下のとおりです。<br><br>- `agency`<br>- `stops`<br>- `routes`<br>- `trips`<br>- `stop_times`<br>- `pathways`<br>- `levels`<br>- `feed_info`<br>- `attributions`<br><br> GTFSに追加されるすべてのファイルは、上記のとおりファイル名と同等の`table_name`値を持ちます（つまり、`.txt`ファイル拡張子は含みません）。 |
|  `field_name` | Text | **必須** | 翻訳対象のフィールド名です。型が`Text`のフィールドは翻訳することができます。型が`URL`、`Email`、`Phone number`のフィールドも、適切な言語のリソースを提供するために「翻訳」することができます。その他の型のフィールドは翻訳するべきではありません。 |
|  `language` | Language code | **必須** | 翻訳の言語です。<br><br>言語が`feed_info.feed_lang`と同じ場合、フィールドの元の値は、特定の翻訳がない言語で使用するデフォルト値であるとみなされます（`default_lang`で別途指定されている場合を除きます）。<hr>_例: スイスでは、公用語が2つある州の都市は公式には「Biel/Bienne」と呼ばれますが、フランス語では単に「Bienne」、ドイツ語では「Biel」と呼ばれます。_ |
|  `translation` | Text または URL または Email または Phone number | **必須** | 翻訳された値です。 |
|  `record_id` | Foreign ID | **条件付き必須** | 翻訳対象のフィールドに対応するレコードを定義します。`record_id`の値は、各テーブルの主キー属性および以下で定義される、テーブルの主キーの最初のフィールドまたは唯一のフィールドでなければなりません。<br><br>- [agency.txt](#agencytxt)の`agency_id`<br>- [stops.txt](#stopstxt)の`stop_id`<br>- [routes.txt](#routestxt)の`route_id`<br>- [trips.txt](#tripstxt)の`trip_id`<br>- [stop_times.txt](#stop_timestxt)の`trip_id`<br>- [pathways.txt](#pathwaystxt)の`pathway_id`<br>- [levels.txt](#levelstxt)の`level_id`<br>- [attributions.txt](#attributionstxt)の`attribution_id`。<br><br>上記で定義されていないテーブル内のフィールドは翻訳するべきではありません。ただし、データ作成者は公式仕様の範囲外である追加フィールドを追加することがあり、これらの非公式フィールドは翻訳することができます。以下は、それらのテーブルに対して`record_id`を使用する推奨方法です。<br><br>- [calendar.txt](#calendartxt)の`service_id`<br>- [calendar_dates.txt](#calendar_datestxt)の`service_id`<br>- [fare_attributes.txt](#fare_attributestxt)の`fare_id`<br>- [fare_rules.txt](#fare_rulestxt)の`fare_id`<br>- [shapes.txt](#shapestxt)の`shape_id`<br>- [frequencies.txt](#frequenciestxt)の`trip_id`<br>- [transfers.txt](#transferstxt)の`from_stop_id`。<br><br>条件付き必須:<br>- `table_name`が`feed_info`の場合は**禁止**です。<br>- `field_value`が定義されている場合は**禁止**です。<br>- `field_value`が空の場合は**必須**です。 |
|  `record_sub_id` | Foreign ID | **条件付き必須** | テーブルに一意のIDがない場合に、翻訳対象のフィールドを含むレコードを特定するのに役立ちます。したがって、`record_sub_id`の値は、以下のテーブルで定義されるテーブルの副IDです。<br><br>- [agency.txt](#agencytxt)ではなし<br>- [stops.txt](#stopstxt)ではなし<br>- [routes.txt](#routestxt)ではなし<br>- [trips.txt](#tripstxt)ではなし<br>- [stop_times.txt](#stop_timestxt)の`stop_sequence`<br>- [pathways.txt](#pathwaystxt)ではなし<br>- [levels.txt](#levelstxt)ではなし<br>- [attributions.txt](#attributionstxt)ではなし。<br><br>上記で定義されていないテーブル内のフィールドは翻訳するべきではありません。ただし、データ作成者は公式仕様の範囲外である追加フィールドを追加することがあり、これらの非公式フィールドは翻訳することができます。以下は、それらのテーブルに対して`record_sub_id`を使用する推奨方法です。<br><br>- [calendar.txt](#calendartxt)ではなし<br>- [calendar_dates.txt](#calendar_datestxt)の`date`<br>- [fare_attributes.txt](#fare_attributestxt)ではなし<br>- [fare_rules.txt](#fare_rulestxt)の`route_id`<br>- [shapes.txt](#shapestxt)ではなし<br>- [frequencies.txt](#frequenciestxt)の`start_time`<br>- [transfers.txt](#transferstxt)の`to_stop_id`。<br><br>条件付き必須:<br>- `table_name`が`feed_info`の場合は**禁止**です。<br>- `field_value`が定義されている場合は**禁止**です。<br>- `table_name=stop_times`かつ`record_id`が定義されている場合は**必須**です。 |
|  `field_value` | Text または URL または Email または Phone number | **条件付き必須** | `record_id`および`record_sub_id`を使用して翻訳対象のレコードを定義する代わりに、このフィールドを使用して翻訳対象の値を定義することができます。使用する場合、`table_name`および`field_name`で識別されるフィールドがfield_valueで定義されたものと完全に同じ値を含むときに、翻訳が適用されます。<br><br>フィールドは、`field_value`で定義された値と**完全に**一致しなければなりません。値の一部のみが`field_value`と一致する場合、翻訳は適用されません。<br><br>2つの翻訳ルールが同じレコードに一致する場合（1つは`field_value`を使用し、もう1つは`record_id`を使用する場合）、`record_id`を使用するルールが優先されます。<br><br>条件付き必須:<br>- `table_name`が`feed_info`の場合は**禁止**です。<br>- `record_id`が定義されている場合は**禁止**です。<br>- `record_id`が空の場合は**必須**です。 |

### feed_info.txt {: #feed_infotxt}


ファイル: **条件付き必須**

主キー（なし）

このファイルには、データセットが記述するサービスではなく、データセット自体に関する情報が含まれます。場合によっては、データセットの発行者は、いずれの事業者とも異なる組織です。

|  フィールド名 | 型 | 存在要件 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `feed_publisher_name` | テキスト | **必須** | データセットを発行する組織の正式名称です。これは `agency.agency_name` の値のいずれかと同じである場合があります。 |
|  `feed_publisher_url` | URL | **必須** | データセットを発行する組織のウェブサイトのURLです。これは `agency.agency_url` の値のいずれかと同じである場合があります。 |
|  `feed_lang` | 言語コード | **必須** | このデータセット内のテキストに使用されるデフォルト言語です。この設定は、GTFSコンシューマーがデータセットの大文字・小文字の規則やその他の言語固有の設定を選択するのに役立ちます。テキストをデフォルト言語以外の言語に翻訳する必要がある場合は、ファイル `translations.txt` を使用できます。<br><br>元のテキストが複数の言語であるデータセットでは、デフォルト言語は多言語とすることができます。そのような場合、`feed_lang` フィールドには規格 ISO 639-2 で定義される言語コード `mul` を含めるべきであり、データセットで使用される各言語の翻訳を `translations.txt` で提供するべきです。データセット内のすべての元のテキストが同じ言語である場合、`mul` を使用するべきではありません。<hr>_例: スイスのような多言語国家のデータセットを考えます。このデータセットでは、元の `stops.stop_name` フィールドに異なる言語の停留所等(stop)名が設定されています。各停留所等(stop)名は、その停留所等(stop)の地理的位置における主要言語に従って記述されます。例えば、フランス語圏の都市ジュネーブでは `Genève`、ドイツ語圏の都市チューリッヒでは `Zürich`、バイリンガルの都市ビール/ビエンヌでは `Biel/Bienne` です。データセットの `feed_lang` は `mul` とし、`translations.txt` には、ドイツ語では `Genf`、`Zürich`、`Biel`、フランス語では `Genève`、`Zurich`、`Bienne`、イタリア語では `Ginevra`、`Zurigo`、`Bienna`、英語では `Geneva`、`Zurich`、`Biel/Bienne` の翻訳を提供します。_ |
|  `default_lang` | 言語コード | 任意 | データコンシューマーが乗客の言語を知らない場合に使用するべき言語を定義します。多くの場合、`en`（英語）です。 |
|  `feed_start_date` | 日付 | 推奨 | データセットは、`feed_start_date` の日の開始から `feed_end_date` の日の終了までの期間におけるサービスについて、完全かつ信頼できるスケジュール情報を提供します。利用できない場合は、両方の日を空欄のままにすることができます。両方が指定されている場合、`feed_end_date` の日付は `feed_start_date` の日付より前であってはいけません。データセット提供者は、将来のサービスの見込みを示すためにこの期間外のスケジュールデータを提供することが推奨されますが、データセットコンシューマーは、その非公式な性質を考慮して扱うべきです。`feed_start_date` または `feed_end_date` が [calendar.txt](#calendartxt) および [calendar_dates.txt](#calendar_datestxt) で定義される有効なカレンダー日付を超える場合、データセットは、`feed_start_date` または `feed_end_date` の範囲内であるものの有効なカレンダー日付に含まれない日付にはサービスがないことを明示的に表明しています。 |
|  `feed_end_date` | 日付 | 推奨 | （上記を参照） |
|  `feed_version` | テキスト | 推奨 | 現在のGTFSデータセットのバージョンを示す文字列です。GTFSを利用するアプリケーションは、データセット発行者が最新のデータセットが取り込まれたかどうかを判断できるよう、この値を表示できます。 |
|  `feed_contact_email` | メールアドレス | 任意 | GTFSデータセットおよびデータ公開の慣行に関する連絡用メールアドレスです。`feed_contact_email` は、GTFSを利用するアプリケーション向けの技術的な連絡先です。カスタマーサービスの連絡先情報は [agency.txt](#agencytxt) を通じて提供してください。`feed_contact_email` または `feed_contact_url` の少なくとも一方を提供することが推奨されます。 |
|  `feed_contact_url` | URL | 任意 | GTFSデータセットおよびデータ公開の慣行に関する連絡先情報、ウェブフォーム、サポートデスク、またはその他の連絡手段のURLです。`feed_contact_url` は、GTFSを利用するアプリケーション向けの技術的な連絡先です。カスタマーサービスの連絡先情報は [agency.txt](#agencytxt) を通じて提供してください。`feed_contact_url` または `feed_contact_email` の少なくとも一方を提供することが推奨されます。 |

### attributions.txt {: #attributionstxt}


ファイル: **任意**

主キー (`attribution_id`)

このファイルは、データセットに適用される帰属情報を定義します。

|  フィールド名 | 型 | 存在 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `attribution_id` | 一意の ID | 任意 | データセットまたはそのサブセットの帰属情報を識別します。これは主に翻訳に役立ちます。 |
|  `agency_id` | `agency.agency_id` を参照する外部 ID | 任意 | 帰属情報が適用される事業者。<br><br>1つの `agency_id`、`route_id`、または `trip_id` の帰属情報が定義されている場合、他のものは空でなければなりません。いずれも指定されていない場合、帰属情報はデータセット全体に適用されます。 |
|  `route_id` | `routes.route_id` を参照する外部 ID  | 任意 | 帰属情報がルート・路線系統(route)に適用されることを除き、`agency_id` と同じように機能します。同じルート・路線系統(route)に複数の帰属情報を適用することができます。 |
|  `trip_id` | `trips.trip_id` を参照する外部 ID  | 任意 | 帰属情報が便(trip)に適用されることを除き、`agency_id` と同じように機能します。同じ便(trip)に複数の帰属情報を適用することができます。 |
|  `organization_name` | テキスト | **必須** | データセットの帰属先となる組織の名称。 |
|  `is_producer` | 列挙型 | 任意 | 組織の役割がプロデューサーであることを示します。有効な選択肢は以下のとおりです。<br><br>`0` または空 - 組織はこの役割を持ちません。<br>`1` - 組織はこの役割を持ちます。<br><br>`is_producer`、`is_operator`、または `is_authority` のフィールドの少なくとも1つを `1` に設定するべきです。 |
|  `is_operator` | 列挙型 | 任意 | 組織の役割がオペレーターであることを除き、`is_producer` と同じように機能します。 |
|  `is_authority` | 列挙型 | 任意 | 組織の役割が当局であることを除き、`is_producer` と同じように機能します。 |
|  `attribution_url` | URL | 任意 | 組織の URL。 |
|  `attribution_email` | メールアドレス | 任意 | 組織のメールアドレス。 |
|  `attribution_phone` | 電話番号 | 任意 | 組織の電話番号。 |
