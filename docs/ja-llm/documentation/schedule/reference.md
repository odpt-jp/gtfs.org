## 一般公共交通フィード仕様リファレンス {: #general-transit-feed-specification-reference}


**2025年10月10日改訂。詳細については [改訂履歴](../change-history/revision-history) を参照してください。**

この文書は、GTFS データセットを構成するファイルの形式および構造を定義します。

## 目次 {: #table-of-contents}


1.  [文書の規約](#document-conventions)
2.  [データセットファイル](#dataset-files)
3.  [ファイル要件](#file-requirements)
4.  [データセットの公開と一般的な運用](#dataset-publishing-general-practices)
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

この文書における "MUST"、"MUST NOT"、"REQUIRED"、"SHALL"、"SHALL NOT"、"SHOULD"、"SHOULD NOT"、"RECOMMENDED"、"MAY"、および "OPTIONAL" というキーワードは、[RFC 2119](https://tools.ietf.org/html/rfc2119) に記載されている通りに解釈しなければなりません。

### 用語の定義 {: #term-definitions}

このセクションでは、本ドキュメント全体で使用される用語を定義します。

* **Dataset** - 本仕様書で定義されるファイル群の完全なセットです。データセットを変更すると、新しいバージョンのデータセットが作成されます。データセットは、zipファイル名を含む公開かつ永続的なURLで公開するべきです（例: https://www.agency.org/gtfs/gtfs.zip）。
* **Record** - 単一のエンティティ（例: 交通事業者、停留所(stop)、ルート(route)など）を記述する複数の異なるフィールド値から構成される基本的なデータ構造です。テーブルでは1行として表されます。
* **Field** - オブジェクトまたはエンティティの属性です。テーブルでは列として表されます。ファイル内でヘッダーとして追加された場合にフィールドが存在します。フィールド値が定義されている場合とされていない場合があります。
* **Field value** - フィールド内の個々のエントリです。テーブルでは1つのセルとして表されます。
* **Service day** - 運行日(service day)とは、ルートの運行スケジュールを示すために使用される時間区間です。運行日の正確な定義は事業者によって異なりますが、運行日はしばしば暦日と一致しません。運行日は、運行が1日をまたぐ場合、24:00:00を超えることがあります。たとえば、金曜日の08:00:00から土曜日の02:00:00まで運行する場合、単一の運行日として08:00:00から26:00:00までと表すことができます。
* **Text-to-speech field** - このフィールドには、親フィールド（空の場合にフォールバックするフィールド）と同じ情報を含めるべきです。音声読み上げ用に設計されているため、省略語は削除するか（例: 「St」は「Street」または「Saint」として読むべきです。「Elizabeth I」は「Elizabeth the first」とするべきです）、またはそのまま略語として読まれるように保持するべきです（例: 「JFK Airport」は略語のまま読み上げられます）。
* **Leg** - 乗客が1つの便(trip)の中で、連続する2つの地点間で乗車および降車を行う移動区間です。
* **Journey** - 出発地から目的地までの全体の移動であり、すべての乗車区間(leg)およびその間の乗り換えを含みます。
* **Sub-journey** - 旅程(journey)の一部を構成する2つ以上の乗車区間(leg)です。
* **Fare product** - 乗車の支払いまたは認証に使用できる購入可能なチケット商品です。
* **Effective Fare Leg** - 運賃計算の目的で、[fare_leg_rules.txt](#fare_leg_rulestxt) におけるマッチングルールにおいて単一の乗車区間(leg)として扱うべき、2つ以上の乗車区間(leg)からなる部分旅程(sub-journey)です。

### 存在条件 {: #presence}

フィールドおよびファイルに適用される存在条件は以下の通りです。

* **必須 (Required)** - フィールドまたはファイルはデータセットに含めなければならず、各レコードに対して有効な値を含まなければなりません。
* **任意 (Optional)** - フィールドまたはファイルはデータセットから省略することができます。
* **条件付き必須 (Conditionally Required)** - フィールドまたはファイルは、そのフィールドまたはファイルの説明に記載された条件の下で含めなければなりません。
* **条件付き禁止 (Conditionally Forbidden)** - フィールドまたはファイルは、そのフィールドまたはファイルの説明に記載された条件の下で含めてはいけません。
* **推奨 (Recommended)** - フィールドまたはファイルはデータセットから省略することができますが、含めることが推奨されます。このフィールドまたはファイルを省略する前に、その推奨事項を慎重に検討し、省略による影響を十分に理解するべきです。

### フィールド型 {: #field-types}


- **Color** - 6桁の16進数でエンコードされた色です。有効な値を生成するには [https://htmlcolorcodes.com](https://htmlcolorcodes.com) を参照してください（先頭の「#」は含めてはいけません）。<br> *例: 白は `FFFFFF`、黒は `000000`、NYMTA の A,C,E 線は `0039A6`。*
- **Currency code** - ISO 4217 のアルファベット通貨コードです。現在の通貨コード一覧については [https://en.wikipedia.org/wiki/ISO_4217#Active\_codes](https://en.wikipedia.org/wiki/ISO_4217#Active_codes) を参照してください。<br> *例: カナダドルは `CAD`、ユーロは `EUR`、日本円は `JPY`。*
- **Currency amount** - 通貨金額を示す10進数値です。小数点以下の桁数は、対応する Currency code に対して [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217#Active_codes) によって指定されています。すべての金額計算は、使用するプログラミング言語に応じて、decimal、currency、または金額計算に適した同等の型として処理するべきです。float 型で通貨金額を処理することは、計算中に金額の損失や増加が発生する可能性があるため推奨されません。
- **Date** - YYYYMMDD 形式の運行日(service day)です。運行日内の時刻は 24:00:00 を超える場合があるため、運行日には翌日以降の情報が含まれることがあります。<br> *例: `20180913` は 2018年9月13日を表します。*
- **Email** - 電子メールアドレスです。<br> *例: `example@example.com`*
- **Enum** - 「Description」列で定義された定数の集合から選択されるオプションです。<br> *例: `route_type` フィールドでは、トラムは `0`、地下鉄は `1` など。*
- **ID** - ID フィールド値は内部 ID であり、乗客に表示することを意図していません。任意の UTF-8 文字の並びで構成されます。印字可能な ASCII 文字のみを使用することが推奨されます。ID が「unique ID」とラベル付けされている場合、そのファイル内で一意でなければなりません。1つの .txt ファイルで定義された ID は、別の .txt ファイルで参照されることがよくあります。他のテーブル内の ID を参照する ID は「foreign ID」とラベル付けされます。<br> *例: [stops.txt](#stopstxt) の `stop_id` フィールドは「unique ID」です。[stops.txt](#stopstxt) の `parent_station` フィールドは「foreign ID referencing `stops.stop_id`」です。*
- **Language code** - IETF BCP 47 言語コードです。IETF BCP 47 の概要については [http://www.rfc-editor.org/rfc/bcp/bcp47.txt](http://www.rfc-editor.org/rfc/bcp/bcp47.txt) および [http://www.w3.org/International/articles/language-tags/](http://www.w3.org/International/articles/language-tags/) を参照してください。<br> *例: 英語は `en`、アメリカ英語は `en-US`、ドイツ語は `de`。*
- **Latitude** - WGS84 の緯度を10進度で表します。値は -90.0 以上 90.0 以下でなければなりません。<br> *例: ローマのコロッセオは `41.890169`。*
- **Longitude** - WGS84 の経度を10進度で表します。値は -180.0 以上 180.0 以下でなければなりません。<br> *例: ローマのコロッセオは `12.492269`。*
- **Float** - 浮動小数点数です。
- **Integer** - 整数です。
- **Phone number** - 電話番号です。
- **Time** - HH:MM:SS 形式の時刻です（H:MM:SS も許可されます）。時刻は運行日(service day)の「正午の12時間前」（実質的には深夜。ただし夏時間の変更がある日は除く）からの経過時間として測定されます。運行日を過ぎた時刻を表す場合は、HH:MM:SS 形式で 24:00:00 より大きい値を入力します。<br> *例: `14:30:00` は午後2時30分、`25:35:00` は翌日の午前1時35分。*
- **Text** - UTF-8 文字列で、人間が読めるように表示されることを目的としています。
- **Timezone** - [https://www.iana.org/time-zones](https://www.iana.org/time-zones) に定義されている TZ タイムゾーンです。タイムゾーン名にはスペース文字を含めてはいけませんが、アンダースコアを含むことはできます。有効な値の一覧については [http://en.wikipedia.org/wiki/List\_of\_tz\_zones](http://en.wikipedia.org/wiki/List\_of\_tz\_zones) を参照してください。<br> *例: `Asia/Tokyo`、`America/Los_Angeles`、`Africa/Cairo`。*
- **URL** - http:// または https:// を含む完全修飾 URL です。URL 内の特殊文字は正しくエスケープされていなければなりません。完全修飾 URL 値の作成方法については [http://www.w3.org/Addressing/URL/4\_URI\_Recommentations.html](http://www.w3.org/Addressing/URL/4\_URI\_Recommentations.html) を参照してください。

### フィールドの符号 {: #field-signs}

Float または Integer 型のフィールドに適用される符号:

* **Non-negative（非負）** - 0 以上。
* **Non-zero（非ゼロ）** - 0 ではない。
* **Positive（正）** - 0 より大きい。

_例: **Non-negative float（非負の浮動小数点数）** - 0 以上の浮動小数点数。_

### データセット属性 {: #dataset-attributes}

**主キー (primary key)** とは、行を一意に識別するフィールド、またはフィールドの組み合わせのことです。ファイル内のすべての提供フィールドが行を一意に識別するために使用される場合は、`Primary key (*)` が使用されます。`Primary key (none)` は、そのファイルが1行のみを許可することを意味します。

_例: `trip_id` フィールドと `stop_sequence` フィールドは、[stop_times.txt](#stop_timestxt) の主キーを構成します。_

## データセットファイル {: #dataset-files}

この仕様では、以下のファイルを定義しています。

| ファイル名 | 存在条件 | 説明 |
| ------ | ------ | ------ |
| [agency.txt](#agencytxt) | **必須** | このデータセットで表現される運行サービスを提供する交通事業者。 |
| [stops.txt](#stopstxt) | **条件付き必須** | 車両が乗客を乗降させる停留所等(stop)。また、駅や駅の入口も定義します。<br><br>条件付き必須:<br> - [locations.geojson](#locationsgeojson) でデマンド型ゾーンが定義されている場合は任意。<br>- それ以外の場合は **必須**。 |
| [routes.txt](#routestxt) | **必須** | 交通ルート・路線系統(route)。ルートは、乗客に単一のサービスとして表示される複数の便(trip)のグループです。 |
| [trips.txt](#tripstxt) | **必須** | 各ルートの便(trip)。便は、特定の時間帯に発生する2つ以上の停留所(stop)の連続です。 |
| [stop_times.txt](#stop_timestxt) | **必須** | 各便(trip)における車両の停留所(stop)到着時刻および出発時刻。 |
| [calendar.txt](#calendartxt) | **条件付き必須** | 開始日と終了日を持つ週単位のスケジュールで指定された運行日(service day)。<br><br>条件付き必須:<br> - すべての運行日(service day)が [calendar_dates.txt](#calendar_datestxt) に定義されていない限り **必須**。<br> - それ以外の場合は任意。 |
| [calendar_dates.txt](#calendar_datestxt) | **条件付き必須** | [calendar.txt](#calendartxt) で定義されたサービスに対する例外。<br><br>条件付き必須:<br> - [calendar.txt](#calendartxt) が省略されている場合は **必須**。この場合、[calendar_dates.txt](#calendar_datestxt) にはすべての運行日(service day)を含めなければなりません。<br> - それ以外の場合は任意。 |
| [fare_attributes.txt](#fare_attributestxt) | 任意 | 交通事業者のルートに関する運賃情報。 |
| [fare_rules.txt](#fare_rulestxt) | 任意 | 旅程(journey)に運賃を適用するためのルール。 |
| [timeframes.txt](#timeframestxt) | 任意 | 日付および時間に依存する運賃ルールで使用する期間。 |
| [rider_categories.txt](#rider_categoriestxt) | 任意 | 乗客のカテゴリ（例: 高齢者、学生）を定義します。 |
| [fare_media.txt](#fare_mediatxt) | 任意 | チケット商品を利用するために使用できるチケットメディアを記述します。<br><br>[fare_media.txt](#fare_mediatxt) は、[fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) では表現されない概念を記述します。そのため、[fare_media.txt](#fare_mediatxt) の使用は [fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) とは完全に独立しています。 |
| [fare_products.txt](#fare_productstxt) | 任意 | 乗客が購入できるさまざまな種類のチケットや運賃を記述します。<br><br>[fare_products.txt](#fare_productstxt) は、[fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) では表現されないチケット商品を記述します。そのため、[fare_products.txt](#fare_productstxt) の使用は [fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) とは完全に独立しています。 |
| [fare_leg_rules.txt](#fare_leg_rulestxt) | 任意 | 各乗車区間(leg)に対する運賃ルール。<br><br>[fare_leg_rules.txt](#fare_leg_rulestxt) は、運賃体系をより詳細にモデル化する方法を提供します。そのため、[fare_leg_rules.txt](#fare_leg_rulestxt) の使用は [fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) とは完全に独立しています。 |
| [fare_leg_join_rules.txt](#fare_leg_join_rulestxt) | 任意 | 2つ以上の乗車区間(leg)を、[fare_leg_rules.txt](#fare_leg_rulestxt) のルールに照らして単一の**有効運賃区間(effective fare leg)**として扱うためのルール。 |
| [fare_transfer_rules.txt](#fare_transfer_rulestxt) | 任意 | 乗車区間(leg)間の乗り換えに関する運賃ルール。<br><br>[fare_leg_rules.txt](#fare_leg_rulestxt) とともに、[fare_transfer_rules.txt](#fare_transfer_rulestxt) は運賃体系をより詳細にモデル化する方法を提供します。そのため、[fare_transfer_rules.txt](#fare_transfer_rulestxt) の使用は [fare_attributes.txt](#fare_attributestxt) および [fare_rules.txt](#fare_rulestxt) とは完全に独立しています。 |
| [areas.txt](#areastxt) | 任意 | 位置情報のエリアグループ。 |
| [stop_areas.txt](#stop_areastxt) | 任意 | 停留所(stop)をエリアに割り当てるためのルール。 |
| [networks.txt](#networkstxt) | **条件付き禁止** | ルートのネットワークグループ。<br><br>条件付き禁止:<br>- [routes.txt](#routestxt) に `network_id` が存在する場合は **禁止**。<br>- それ以外の場合は任意。 |
| [route_networks.txt](#route_networkstxt) | **条件付き禁止** | ルートをネットワークに割り当てるためのルール。<br><br>条件付き禁止:<br>- [routes.txt](#routestxt) に `network_id` が存在する場合は **禁止**。<br>- それ以外の場合は任意。 |
| [shapes.txt](#shapestxt) | 任意 | 車両の走行経路（ルート形状(shape)）をマッピングするためのルール。 |
| [frequencies.txt](#frequenciestxt) | 任意 | 頻度運行サービスの便間隔（ヘッドウェイ）または固定スケジュールサービスの圧縮表現。 |
| [transfers.txt](#transferstxt) | 任意 | ルート間の乗り換え地点での接続ルール。 |
| [pathways.txt](#pathwaystxt) | 任意 | 駅構内の位置を相互に接続する構内通路(pathway)。 |
| [levels.txt](#levelstxt) | **条件付き必須** | 駅構内の階層。<br><br>条件付き必須:<br>- エレベーター（`pathway_mode=5`）を含む構内通路(pathway)を記述する場合は **必須**。<br>- それ以外の場合は任意。 |
| [location_groups.txt](#location_groupstxt) | 任意 | 乗客が乗降をリクエストできる場所を示す停留所(stop)のグループ。 |
| [location_group_stops.txt](#location_group_stopstxt) | 任意 | 停留所(stop)をロケーショングループに割り当てるためのルール。 |
| [locations.geojson](#locationsgeojson) | 任意 | オンデマンドサービスによる乗降リクエストゾーンを GeoJSON ポリゴンとして表現。 |
| [booking_rules.txt](#booking_rulestxt) | 任意 | 乗客リクエスト型サービスの予約情報。 |
| [translations.txt](#translationstxt) | 任意 | 利用者向けデータセット値の翻訳。 |
| [feed_info.txt](#feed_infotxt) | **条件付き必須** | データセットのメタデータ（発行者、バージョン、有効期限情報など）。<br><br>条件付き必須:<br>- [translations.txt](#translationstxt) が提供されている場合は **必須**。<br>- それ以外の場合は推奨。 |
| [attributions.txt](#attributionstxt) | 任意 | データセットの帰属情報。 |

## ファイル要件 {: #file-requirements}

以下の要件は、データセットファイルの形式および内容に適用されます。

* すべてのファイルはカンマ区切りのテキストとして保存しなければなりません。
* 各ファイルの最初の行にはフィールド名を含めなければなりません。[Field Definitions](#field-definitions) セクションの各小節は、GTFS データセット内のファイルの1つに対応しており、そのファイルで使用できるフィールド名を一覧しています。
* すべてのファイル名およびフィールド名は大文字と小文字を区別します。
* フィールド値にタブ、キャリッジリターン、または改行を含めてはいけません。
* フィールド値に引用符またはカンマが含まれる場合は、引用符で囲まなければなりません。さらに、フィールド値内の各引用符の前には引用符を1つ追加しなければなりません。これは、Microsoft Excel がカンマ区切り (CSV) ファイルを出力する方法と一致しています。CSV ファイル形式の詳細については、[http://tools.ietf.org/html/rfc4180](http://tools.ietf.org/html/rfc4180) を参照してください。  
次の例は、カンマ区切りファイル内でフィールド値がどのように表現されるかを示しています。
  * **元のフィールド値:** `Contains "quotes", commas and text`
  * **CSV ファイル内のフィールド値:** `"Contains ""quotes"", commas and text"`
* フィールド値に HTML タグ、コメント、またはエスケープシーケンスを含めてはいけません。
* フィールドやフィールド名の間に余分なスペースがある場合は削除するべきです。多くのパーサーはスペースを値の一部として扱うため、エラーの原因となる可能性があります。
* 各行は CRLF または LF の改行文字で終わらなければなりません。
* すべてのファイルは UTF-8 でエンコードするべきです。これにより、すべての Unicode 文字をサポートできます。Unicode バイトオーダーマーク (BOM) 文字を含むファイルも許容されます。BOM 文字および UTF-8 の詳細については、[http://unicode.org/faq/utf_bom.html#BOM](http://unicode.org/faq/utf_bom.html#BOM) を参照してください。
* すべてのデータセットファイルは1つの zip ファイルにまとめなければなりません。ファイルはサブフォルダ内ではなく、ルートレベルに直接配置しなければなりません。
* すべての利用者向けテキスト文字列（停留所名、路線名、行先表示など）は、すべて大文字ではなく混在大小文字 (Mixed Case) を使用し、表示装置が小文字を表示できる場合には、地名の大文字小文字の地域慣習に従うべきです（例: “Brighton Churchill Square”, “Villiers-sur-Marne”, “Market Street”）。
* フィード全体で、名前やその他のテキストにおいて略語の使用は避けるべきです（例: Street を St. と略さない）。ただし、場所が略称で呼ばれている場合（例: “JFK Airport”）はこの限りではありません。略語はスクリーンリーダーソフトウェアや音声ユーザーインターフェースによるアクセシビリティに問題を引き起こす可能性があります。利用側のソフトウェアは、完全な単語を略語に変換するように設計することができますが、略語から完全な単語に変換するのは誤りのリスクが高くなります。

## データセットの公開および一般的な運用慣行 {: #dataset-publishing-general-practices}

* データセットは、zip ファイル名を含む公開かつ永続的な URL で公開するべきです（例: www.agency.org/gtfs/gtfs.zip）。理想的には、ファイルにアクセスするためにログインを必要とせず、直接ダウンロード可能な URL とすることで、利用するソフトウェアアプリケーションによるダウンロードを容易にするべきです。GTFS データセットをオープンにダウンロード可能とすることが推奨され（また最も一般的な慣行です）、データ提供者がライセンスやその他の理由で GTFS へのアクセスを制御する必要がある場合は、自動ダウンロードを容易にするために API キーを使用して GTFS データセットへのアクセスを制御することが推奨されます。
* GTFS データは、安定した場所にある単一のファイルが常に事業者（または複数の事業者）の最新の公式運行情報を含むように、反復的に公開するべきです。
* データセットは、可能な限りデータの反復を通じて `stop_id`、`route_id`、および `agency_id` の永続的な識別子（id フィールド）を維持するべきです。
* 1 つの GTFS データセットには、現在および今後の運行情報（「マージ済みデータセット」と呼ばれることもあります）を含めるべきです。2 つの異なる GTFS フィードからマージ済みデータセットを作成するために使用できる [マージツール](../../../resources/gtfs/#gtfs-merge-tools) が複数存在します。
    * 公開されている GTFS データセットは、常に少なくとも今後 7 日間は有効であるべきであり、理想的には、運行事業者がスケジュールの継続運行に自信を持てる期間まで有効であるべきです。
    * 可能であれば、GTFS データセットは少なくとも今後 30 日間の運行をカバーするべきです。
 * 古い運行情報（有効期限切れのカレンダー）はフィードから削除するべきです。
 * 7 日以内に有効となる運行変更がある場合は、この運行変更は静的な GTFS データセットではなく、GTFS-realtime フィード（運行情報または便の更新）を通じて表現するべきです。
 * GTFS データをホスティングする Web サーバーは、ファイルの更新日時を正しく報告するように設定しなければなりません（[HTTP/1.1 - Request for Comments 2616, セクション 14.29](https://tools.ietf.org/html/rfc2616#section-14.29) を参照）。

## フィールド定義 {: #field-definitions}

### agency.txt {: #agencytxt}


ファイル: **必須**

主キー (`agency_id`)

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `agency_id` | 一意のID | **条件付き必須** | 交通事業ブランドを識別します。これは多くの場合、交通事業者と同義です。ただし、単一の事業者が複数の独立したサービスを運営している場合など、事業者とブランドが異なる場合があります。本ドキュメントでは「agency」という用語を「ブランド」の代わりに使用します。1つのデータセットには複数の事業者のデータを含めることができます。<br><br>条件付き必須:<br>- **必須**: データセットに複数の交通事業者のデータが含まれている場合。<br>- それ以外の場合は推奨。 |
| `agency_name` | テキスト | **必須** | 交通事業者の正式名称。 |
| `agency_url` | URL | **必須** | 交通事業者のURL。 |
| `agency_timezone` | タイムゾーン | **必須** | 交通事業者が所在するタイムゾーン。データセット内で複数の事業者が指定されている場合、すべての事業者は同じ `agency_timezone` を持たなければなりません。 |
| `agency_lang` | 言語コード | 任意 | この交通事業者で主に使用される言語。GTFS利用者がデータセットの大文字小文字の規則やその他の言語固有設定を選択する際に役立つため、提供するべきです。 |
| `agency_phone` | 電話番号 | 任意 | 指定された事業者の音声通話用電話番号。このフィールドは、事業者のサービス地域で一般的な形式で電話番号を表す文字列値です。番号の桁を区切るために句読点を含めることができます。ダイヤル可能なテキスト（例: TriMet の "503-238-RIDE"）は許可されますが、その他の説明的なテキストを含めてはいけません。 |
| `agency_fare_url` | URL | 任意 | 乗客がその事業者のチケットやその他の運賃関連商品を購入できるウェブページ、またはその事業者の運賃情報を掲載しているウェブページのURL。 |
| `agency_email` | メールアドレス | 任意 | 事業者のカスタマーサービス部門が常時監視しているメールアドレス。このメールアドレスは、乗客が事業者のカスタマーサービス担当者に直接連絡できる窓口であるべきです。 |
| `cemv_support` | 列挙型 | 任意 | 乗客が、この事業者に関連する交通サービス（すなわち便）において、非接触型EMV（Europay、Mastercard、Visa）カードまたはモバイルデバイスをチケットメディアとして運賃検証機（例: ペイ・アズ・ユー・ゴー方式やオープンループシステム）で使用できるかどうかを示します。このフィールドは、cEMVを使用して他のチケット商品を購入したり、他のチケットメディアにチャージしたりできることを示すものではありません。<br><br>cEMVのサポートは、この事業者のすべてのサービスがcEMVカードまたはモバイルデバイスをチケットメディアとして利用可能な場合にのみ示すべきです。<br><br>有効な値は以下の通りです:<br><br>`0` または空欄 - この事業者に関連する便に対してcEMV情報がありません。<br>`1` - この事業者に関連する便で、乗客はcEMVをチケットメディアとして使用することができます。<br>`2` - この事業者に関連する便で、cEMVはチケットメディアとしてサポートされていません。<br><br>`agency.cemv_support` と `routes.cemv_support` が同じサービスに対して両方提供されている場合、`routes.cemv_support` の値が優先されなければなりません。<br><br>このフィールドは他の運賃関連ファイルとは独立しており、単独で使用することができます。このフィールドと他の運賃関連ファイル（例: `fare_media.txt`、`fare_products.txt`、`fare_leg_rules.txt`）の間に矛盾がある場合は、それらのファイルの情報が `agency.cemv_support` よりも優先されなければなりません。 |

### stops.txt {: #stopstxt}


ファイル: **条件付き必須**

主キー (`stop_id`)

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `stop_id` | 一意のID | **必須** | 停留所等(stop)・プラットフォーム、駅、出入口、汎用ノード、または乗車エリアを識別します（`location_type`を参照）。<br><br>IDは、すべての `stops.stop_id`、`locations.geojson` の `id`、および `location_groups.location_group_id` の値の中で一意でなければなりません。<br><br>複数のルート・路線系統(route)が同じ `stop_id` を使用することができます。 |
| `stop_code` | テキスト | 任意 | 乗客向けに停留所を識別する短いテキストまたは番号です。これらのコードは、電話ベースの交通情報システムや標識に印刷され、特定の停留所に関する情報を乗客が取得しやすくするために使用されることが多いです。`stop_code` は、公開されている場合、`stop_id` と同じであってもかまいません。このフィールドは、乗客に提示されるコードがない場所では空欄にするべきです。 |
| `stop_name` | テキスト | **条件付き必須** | 停留所等(stop)の名称です。`stop_name` は、時刻表、オンライン公開情報、または標識に表示される、事業者の乗客向け名称と一致するべきです。他言語への翻訳には [translations.txt](#translationstxt) を使用してください。<br><br>場所が乗車エリア（`location_type=4`）の場合、`stop_name` には事業者が表示する乗車エリアの名称を含めるべきです。これは、1文字（ヨーロッパの都市間鉄道駅など）や「車椅子乗車エリア」（ニューヨーク地下鉄）、「短編成先頭」（パリRER）などのテキストである場合があります。<br><br>条件付き必須:<br>- **必須**: 停留所(`location_type=0`)、駅(`location_type=1`)、出入口(`location_type=2`)の場合。<br>- 任意: 汎用ノード(`location_type=3`)、乗車エリア(`location_type=4`)の場合。|
| `tts_stop_name` | テキスト | 任意 | `stop_name` の読み上げ用バージョンです。詳細は [用語定義](#term-definitions) の「読み上げ用フィールド(text-to-speech field)」を参照してください。 |
| `stop_desc` | テキスト | 任意 | 有用で質の高い情報を提供する停留所等(stop)の説明です。`stop_name` の重複であってはいけません。|
| `stop_lat` | 緯度 | **条件付き必須** | 停留所等(stop)の緯度です。<br><br>停留所・プラットフォーム（`location_type=0`）および乗車エリア（`location_type=4`）の場合、座標はバスポール（存在する場合）の位置、または乗客が車両に乗車する位置（歩道またはプラットフォーム上であり、車道や線路上ではない）でなければなりません。<br><br>条件付き必須:<br>- **必須**: 停留所(`location_type=0`)、駅(`location_type=1`)、出入口(`location_type=2`)の場合。<br>- 任意: 汎用ノード(`location_type=3`)、乗車エリア(`location_type=4`)の場合。|
| `stop_lon` | 経度 | **条件付き必須** | 停留所等(stop)の経度です。<br><br>停留所・プラットフォーム（`location_type=0`）および乗車エリア（`location_type=4`）の場合、座標はバスポール（存在する場合）の位置、または乗客が車両に乗車する位置（歩道またはプラットフォーム上であり、車道や線路上ではない）でなければなりません。<br><br>条件付き必須:<br>- **必須**: 停留所(`location_type=0`)、駅(`location_type=1`)、出入口(`location_type=2`)の場合。<br>- 任意: 汎用ノード(`location_type=3`)、乗車エリア(`location_type=4`)の場合。 |
| `zone_id` | ID | 任意 | 停留所の運賃ゾーンを識別します。このレコードが駅または駅の出入口を表す場合、`zone_id` は無視されます。|
| `stop_url` | URL | 任意 | 停留所等(stop)に関するウェブページのURLです。これは `agency.agency_url` や `routes.route_url` のフィールド値とは異なるべきです。 |
| `location_type` | Enum | 任意 | 場所の種類を示します。有効なオプションは以下の通りです:<br><br>`0`（または空欄） - **停留所(Stop)**（または**プラットフォーム(Platform)**）。乗客が交通機関に乗降する場所です。`parent_station` 内で定義されている場合はプラットフォームと呼ばれます。<br>`1` - **駅(Station)**。1つ以上のプラットフォームを含む物理的な構造またはエリアです。<br>`2` - **出入口(Entrance/Exit)**。乗客が駅に出入りできる場所です。出入口が複数の駅に属する場合、両方に通路(pathway)で接続することができますが、データ提供者はそのうち1つを親として選ばなければなりません。<br>`3` - **汎用ノード(Generic Node)**。駅内の他の `location_type` に該当しない場所で、[pathways.txt](#pathwaystxt) で定義された通路を接続するために使用されることがあります。<br>`4` - **乗車エリア(Boarding Area)**。プラットフォーム上の特定の場所で、乗客が車両に乗降することができます。|
| `parent_station` | 外部ID（`stops.stop_id` を参照） | **条件付き必須** | [stops.txt](#stopstxt) で定義された異なる場所間の階層を定義します。親の場所のIDを以下のように含みます:<br><br>- **停留所・プラットフォーム**（`location_type=0`）: `parent_station` フィールドには駅のIDを含みます。<br>- **駅**（`location_type=1`）: このフィールドは空でなければなりません。<br>- **出入口**（`location_type=2`）または **汎用ノード**（`location_type=3`）: `parent_station` フィールドには駅（`location_type=1`）のIDを含みます。<br>- **乗車エリア**（`location_type=4`）: `parent_station` フィールドにはプラットフォームのIDを含みます。<br><br>条件付き必須:<br>- **必須**: 出入口(`location_type=2`)、汎用ノード(`location_type=3`)、乗車エリア(`location_type=4`)の場合。<br>- 任意: 停留所・プラットフォーム(`location_type=0`)の場合。<br>- 禁止: 駅(`location_type=1`)の場合。|
| `stop_timezone` | タイムゾーン | 任意 | 停留所等(stop)のタイムゾーンです。場所に親駅がある場合、その駅のタイムゾーンを継承し、自身のタイムゾーンは適用されません。駅および親を持たない停留所で `stop_timezone` が空の場合、`agency.agency_timezone` で指定されたタイムゾーンを継承します。[stop_times.txt](#stop_timestxt) で提供される時刻は `agency.agency_timezone` で指定されたタイムゾーンに基づきます。これにより、便(trip)が複数のタイムゾーンをまたぐ場合でも、便内の時刻値が常に進行方向に沿って増加することが保証されます。 |
| `wheelchair_boarding` | Enum | 任意 | 車椅子での乗降が可能かどうかを示します。有効なオプションは以下の通りです:<br><br>親を持たない停留所の場合:<br>`0` または空欄 - アクセシビリティ情報なし。<br>`1` - 一部の車両はこの停留所で車椅子乗客の乗降が可能。<br>`2` - この停留所では車椅子乗降が不可能。<br><br>子停留所の場合:<br>`0` または空欄 - 親駅の `wheelchair_boarding` の設定を継承します（親に指定されている場合）。<br>`1` - 駅外から特定の停留所・プラットフォームまでアクセス可能な経路があります。<br>`2` - 駅外から特定の停留所・プラットフォームまでアクセス可能な経路がありません。<br><br>駅の出入口の場合:<br>`0` または空欄 - 親駅の `wheelchair_boarding` の設定を継承します（親に指定されている場合）。<br>`1` - 駅の出入口は車椅子でアクセス可能です。<br>`2` - 駅の出入口から停留所・プラットフォームまでアクセス可能な経路がありません。 |
| `level_id` | 外部ID（`levels.level_id` を参照） | 任意 | 停留所等(stop)の階層（レベル）を示します。同じレベルは、関連のない複数の駅で使用することができます。|
| `platform_code` | テキスト | 任意 | 駅に属するプラットフォーム停留所のプラットフォーム識別子です。これは単なる識別子（例: "G" や "3"）であるべきです。「platform」や「track」（またはフィードの言語における同等語）といった単語を含めてはいけません。これにより、フィード利用者がプラットフォーム識別子を他言語に容易に国際化・ローカライズできるようになります。 |
| `stop_access` | Enum | **条件付き禁止** | 特定の駅に対して、停留所等(stop)がどのようにアクセスされるかを示します。有効なオプションは以下の通りです:<br><br>`0` - 停留所・プラットフォームは道路網から直接アクセスできません。駅に定義された出入口がある場合はそこから、ない場合は駅自体からアクセスしなければなりません。駅に通路(pathway)が定義されている場合、それらを使用して停留所・プラットフォームにアクセスしなければなりません。<br>`1` - 利用アプリケーションは、親駅の出入口や通路とは無関係に、停留所への直接アクセス経路を生成するべきです。<br><br>`stop_access` が空の場合、指定された停留所またはプラットフォームのアクセスは未定義と見なされます。<br><br>**条件付き禁止**:<br>- **禁止**: 駅(`location_type=1`)、出入口(`location_type=2`)、汎用ノード(`location_type=3`)、乗車エリア(`location_type=4`)の場合。<br>- **禁止**: `parent_station` が空の場合。<br>- それ以外の場合は任意。 |

### routes.txt {: #routestxt}


ファイル: **必須**

主キー (`route_id`)

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `route_id` | 一意のID | **必須** | ルートを識別します。 |
| `agency_id` | `agency.agency_id` を参照する外部ID | **条件付き必須** | 指定されたルートの事業者です。<br><br>条件付き必須:<br>- [agency.txt](#agencytxt) に複数の事業者が定義されている場合は **必須**。<br>- それ以外の場合は推奨。 |
| `route_short_name` | テキスト | **条件付き必須** | ルートの短い名称です。多くの場合、乗客がルートを識別するために使用する短く抽象的な識別子（例: "32"、"100X"、"Green"）です。`route_short_name` と `route_long_name` の両方を定義することができます。<br><br>条件付き必須:<br>- `routes.route_long_name` が空の場合は **必須**。<br>- 短いサービス名称がある場合は推奨。これは一般的に知られている乗客向けの名称であり、12文字以内であるべきです。 |
| `route_long_name` | テキスト | **条件付き必須** | ルートの正式名称です。この名称は通常、`route_short_name` よりも説明的であり、ルートの目的地や停留所を含むことが多いです。`route_short_name` と `route_long_name` の両方を定義することができます。<br><br>条件付き必須:<br>- `routes.route_short_name` が空の場合は **必須**。<br>- それ以外の場合は任意。 |
| `route_desc` | テキスト | 任意 | 有用で質の高い情報を提供するルートの説明です。`route_short_name` または `route_long_name` の重複であってはいけません。<hr> _例: 「A」列車は常時、マンハッタンの Inwood-207 St とクイーンズの Far Rockaway-Mott Avenue の間を運行します。また、午前6時頃から深夜まで、追加の「A」列車が Inwood-207 St と Lefferts Boulevard の間を運行します（列車は通常、Lefferts Blvd と Far Rockaway の間で交互に運行します）。_ |
| `route_type` | 列挙型 | **必須** | ルートで使用される交通手段の種類を示します。有効な選択肢は以下の通りです:<br><br>`0` - トラム、ストリートカー、ライトレール。都市圏内のライトレールまたは地上鉄道システム。<br>`1` - 地下鉄、メトロ。都市圏内の地下鉄システム。<br>`2` - 鉄道。都市間または長距離移動に使用。<br>`3` - バス。短距離および長距離のバス路線に使用。<br>`4` - フェリー。短距離および長距離の船舶サービスに使用。<br>`5` - ケーブルトラム。車両の下にケーブルが通る地上鉄道車両（例: サンフランシスコのケーブルカー）。<br>`6` - ロープウェイ、吊り下げ式ケーブルカー（例: ゴンドラリフト、空中トラムウェイ）。1本以上のケーブルで車両やゴンドラ、椅子を吊り下げるケーブル輸送。<br>`7` - フニクラー。急勾配用に設計された鉄道システム。<br>`11` - トロリーバス。ポールを使用して架線から電力を供給する電動バス。<br>`12` - モノレール。単一のレールまたはビームで構成される鉄道。 |
| `route_url` | URL | 任意 | 特定のルートに関するウェブページのURLです。`agency.agency_url` の値とは異なるべきです。 |
| `route_color` | 色 | 任意 | 公開資料と一致するルートの色指定です。省略または空の場合は白 (`FFFFFF`) がデフォルトです。`route_color` と `route_text_color` の色の差は、白黒画面で見たときに十分なコントラストを提供するべきです。 |
| `route_text_color` | 色 | 任意 | `route_color` の背景に対して読みやすい文字色です。省略または空の場合は黒 (`000000`) がデフォルトです。`route_color` と `route_text_color` の色の差は、白黒画面で見たときに十分なコントラストを提供するべきです。 |
| `route_sort_order` | 非負整数 | 任意 | 乗客に提示する際に理想的な順序でルートを並べるために使用します。`route_sort_order` の値が小さいルートほど先に表示されるべきです。 |
| `continuous_pickup` | 列挙型 | **条件付き禁止** | 乗客が [shapes.txt](#shapestxt) で記述された車両の走行経路上の任意の地点で乗車できることを示します（ルートのすべての便に適用）。有効な選択肢は以下の通りです:<br><br>`0` - 連続乗車可能。<br>`1` または空 - 連続乗車不可。<br>`2` - 事業者に電話して連続乗車を手配する必要あり。<br>`3` - 運転手と調整して連続乗車を手配する必要あり。<br><br>`routes.continuous_pickup` の値は、ルート上の特定の `stop_time` に対して `stop_times.continuous_pickup` の値を定義することで上書きすることができます。<br><br>**条件付き禁止**:<br>- このルートのいずれかの便で `stop_times.start_pickup_drop_off_window` または `stop_times.end_pickup_drop_off_window` が定義されている場合、`1` または空以外の値は **禁止**。<br>- それ以外の場合は任意。 |
| `continuous_drop_off` | 列挙型 | **条件付き禁止** | 乗客が [shapes.txt](#shapestxt) で記述された車両の走行経路上の任意の地点で降車できることを示します（ルートのすべての便に適用）。有効な選択肢は以下の通りです:<br><br>`0` - 連続降車可能。<br>`1` または空 - 連続降車不可。<br>`2` - 事業者に電話して連続降車を手配する必要あり。<br>`3` - 運転手と調整して連続降車を手配する必要あり。<br><br>`routes.continuous_drop_off` の値は、ルート上の特定の `stop_time` に対して `stop_times.continuous_drop_off` の値を定義することで上書きすることができます。<br><br>**条件付き禁止**:<br>- このルートのいずれかの便で `stop_times.start_pickup_drop_off_window` または `stop_times.end_pickup_drop_off_window` が定義されている場合、`1` または空以外の値は **禁止**。<br>- それ以外の場合は任意。 |
| `network_id` | ID | **条件付き禁止** | ルートのグループを識別します。[routes.txt](#routestxt) の複数行が同じ `network_id` を持つことができます。<br><br>条件付き禁止:<br>- [route_networks.txt](#route_networkstxt) ファイルが存在する場合は **禁止**。<br>- それ以外の場合は任意。 |
| `cemv_support` | 列挙型 | 任意 | 乗客が、このルートに関連する交通サービス（すなわち便）において、非接触型EMV（Europay、Mastercard、Visa）カードまたはモバイルデバイスをチケットメディアとして運賃検証機で使用できるかどうかを示します（例: 即時決済型またはオープンループシステム）。このフィールドは、cEMVを使用して他のチケット商品を購入したり、他のチケットメディアにチャージしたりできることを示すものではありません。<br><br>cEMVのサポートは、このルートのすべてのサービスがcEMVカードまたはモバイルデバイスをチケットメディアとして利用可能な場合にのみ示すべきです。<br><br>有効な選択肢は以下の通りです:<br><br>`0` または空 - このルートに関連する便にcEMV情報なし。<br>`1` - このルートに関連する便で乗客がcEMVをチケットメディアとして使用可能。<br>`2` - このルートに関連する便でcEMVはチケットメディアとして非対応。<br><br>同一サービスに対して `agency.cemv_support` と `routes.cemv_support` の両方が提供されている場合、`routes.cemv_support` の値が優先されます。<br><br>このフィールドは他の運賃関連ファイルとは独立しており、単独で使用することができます。このフィールドと他の運賃関連ファイル（`fare_media.txt`、`fare_products.txt`、`fare_leg_rules.txt` など）の間に矛盾がある場合は、それらのファイルの情報が `agency.cemv_support` よりも優先されます。 |

### trips.txt {: #tripstxt}


ファイル: **必須**

主キー (`trip_id`)

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `route_id` | `routes.route_id` を参照する外部ID | **必須** | ルート・路線系統(route)を識別します。 |
| `service_id` | `calendar.service_id` または `calendar_dates.service_id` を参照する外部ID | **必須** | 1つまたは複数のルートで運行が行われる日付の集合を識別します。 |
| `trip_id` | 一意のID | **必須** | 便(trip)を識別します。 |
| `trip_headsign` | テキスト | 任意 | 乗客に便の行先を示す表示に使用されるテキストです。このフィールドは、車両に行先表示(headsign)が表示され、ルート内の便を区別するために使用される可能性があるすべてのサービスで推奨されます。<br><br>便の途中で行先表示が変わる場合、`trip_headsign` の値は、便の特定の停車時刻(stop_time)に対して `stop_times.stop_headsign` に値を定義することで上書きすることができます。 |
| `trip_short_name` | テキスト | 任意 | 乗客に便を識別させるために使用される公開用テキストです。例えば、通勤鉄道の便番号を示すために使用されます。乗客が通常便名に依存しない場合、`trip_short_name` は空にするべきです。`trip_short_name` の値が指定される場合、運行日(service day)内で便を一意に識別する必要があります。行先名や快速／各駅などの区別に使用してはいけません。 |
| `direction_id` | 列挙型 | 任意 | 便の運行方向を示します。このフィールドは経路探索には使用すべきではなく、時刻表を公開する際に方向ごとに便を区別するための手段を提供します。有効な値は以下の通りです:<br><br>`0` - 一方向の運行（例: 下り）。<br>`1` - 反対方向の運行（例: 上り）。<hr>*例: `trip_headsign` フィールドと `direction_id` フィールドを組み合わせて、一連の便における各方向の名称を割り当てることができます。時刻表で使用するために [trips.txt](#tripstxt) ファイルに以下のようなレコードを含めることができます:*<br>`trip_id,...,trip_headsign,direction_id`<br>`1234,...,Airport,0`<br>`1505,...,Downtown,1` |
| `block_id` | ID | 任意 | 便が属するブロックを識別します。ブロックは、同一の車両で連続して運行される1つまたは複数の便で構成され、共通の運行日(service day)および `block_id` によって定義されます。`block_id` は異なる運行日を持つ便を含むことができ、その場合は別のブロックとなります。[以下の例](#example-blocks-and-service-day)を参照してください。座席に座ったままの乗り継ぎ情報を提供する場合は、代わりに `transfer_type` が `4` の [transfers](#transferstxt) を提供するべきです。 |
| `shape_id` | `shapes.shape_id` を参照する外部ID | **条件付き必須** | 便の車両の走行経路を示す地理的形状を識別します。<br><br>条件付き必須:<br>- **必須**: 便が [routes.txt](#routestxt) または [stop_times.txt](#stop_timestxt) で定義された連続乗降動作を持つ場合。<br>- それ以外の場合は任意。 |
| `wheelchair_accessible` | 列挙型 | 任意 | 車椅子での利用可否を示します。有効な値は以下の通りです:<br><br>`0` または空欄 - この便に関するアクセシビリティ情報はありません。<br>`1` - この便で使用される車両は、少なくとも1名の車椅子利用者を収容できます。<br>`2` - この便では車椅子利用者を収容できません。 |
| `bikes_allowed` | 列挙型 | 任意 | 自転車の持ち込み可否を示します。有効な値は以下の通りです:<br><br>`0` または空欄 - この便に関する自転車情報はありません。<br>`1` - この便で使用される車両は、少なくとも1台の自転車を収容できます。<br>`2` - この便では自転車を持ち込むことはできません。 |
| `cars_allowed` | 列挙型 | 任意 | 自動車の持ち込み可否を示します。有効な値は以下の通りです:<br><br>`0` または空欄 - この便に関する自動車情報はありません。<br>`1` - この便で使用される車両は、少なくとも1台の自動車を収容できます。<br>`2` - この便では自動車を持ち込むことはできません。 |

#### 例: ブロックと運行日 {: #example-blocks-and-service-day}

以下の例は有効であり、曜日ごとに異なるブロックを持っています。

| route_id | trip_id | service_id                     | block_id | <span style="font-weight:normal">*(最初の停車時刻)*</span> | <span style="font-weight:normal">*(最後の停車時刻)*</span> |
|----------|---------|--------------------------------|----------|-----------------------------------------|-------------------------|
| red      | trip_1  | mon-tues-wed-thurs-fri-sat-sun | red_loop | 22:00:00                                | 22:55:00                |
| red      | trip_2  | fri-sat-sun                    | red_loop | 23:00:00                                | 23:55:00                |
| red      | trip_3  | fri-sat                        | red_loop | 24:00:00                                | 24:55:00                |
| red      | trip_4  | mon-tues-wed-thurs             | red_loop | 20:00:00                                | 20:50:00                |
| red      | trip_5  | mon-tues-wed-thurs             | red_loop | 21:00:00                                | 21:50:00                |

上記の表に関する注記:

* 例えば金曜日から土曜日の朝にかけて、1台の車両が `trip_1`、`trip_2`、および `trip_3`（午後10時から午前0時55分まで）を運行します。最後の便は土曜日の午前0時から午前0時55分に運行されますが、時刻が 24:00:00 から 24:55:00 であるため、金曜日の「運行日(service day)」に属します。
* 月曜日、火曜日、水曜日、および木曜日には、1台の車両が `trip_1`、`trip_4`、および `trip_5` を午後8時から午後10時55分までのブロックで運行します。

### stop_times.txt {: #stop_timestxt}


ファイル: **必須**

主キー (`trip_id`, `stop_sequence`)

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `trip_id` | `trips.trip_id` を参照する外部ID | **必須** | 便(trip)を識別します。 |
| `arrival_time` | 時刻 | **条件付き必須** | 特定の便（`stop_times.trip_id` で定義）における停留所等（`stop_times.stop_id` で定義）での到着時刻を、`agency.agency_timezone` で指定されたタイムゾーンで示します。`stops.stop_timezone` ではありません。<br><br>停留所での到着時刻と出発時刻が別々に存在しない場合、`arrival_time` と `departure_time` は同じ値にするべきです。<br><br>運行日(service day)の深夜0時以降の時刻は、HH:MM:SS 形式で 24:00:00 より大きい値として入力します。<br><br>正確な到着・出発時刻（`timepoint=1`）が利用できない場合は、推定または補間された到着・出発時刻（`timepoint=0`）を提供するべきです。<br><br>条件付き必須:<br>- 便の最初および最後の停留所（`stop_times.stop_sequence` で定義）では **必須**。<br>- `timepoint=1` の場合は **必須**。<br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合は **禁止**。<br>- それ以外は任意。|
| `departure_time` | 時刻 | **条件付き必須** | 特定の便（`stop_times.trip_id` で定義）における停留所等（`stop_times.stop_id` で定義）からの出発時刻を、`agency.agency_timezone` で指定されたタイムゾーンで示します。`stops.stop_timezone` ではありません。<br><br>停留所での到着時刻と出発時刻が別々に存在しない場合、`arrival_time` と `departure_time` は同じ値にするべきです。<br><br>運行日(service day)の深夜0時以降の時刻は、HH:MM:SS 形式で 24:00:00 より大きい値として入力します。<br><br>正確な到着・出発時刻（`timepoint=1`）が利用できない場合は、推定または補間された到着・出発時刻（`timepoint=0`）を提供するべきです。<br><br>条件付き必須:<br>- `timepoint=1` の場合は **必須**。<br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合は **禁止**。<br>- それ以外は任意。|
| `stop_id` | `stops.stop_id` を参照する外部ID | **条件付き必須** | サービス対象の停留所等を識別します。便の運行中にサービスされるすべての停留所は [stop_times.txt](#stop_timestxt) にレコードを持たなければなりません。参照される場所は停留所またはプラットフォームでなければならず、`stops.location_type` の値は `0` または空である必要があります。1つの便で同じ停留所を複数回サービスすることができ、複数の便やルートが同じ停留所をサービスすることもできます。<br><br>停留所を使用するオンデマンドサービスでは、サービスが利用可能な順序で停留所を参照するべきです。データ利用者は、各 `stop_time` の `pickup/drop_off_type` および各 `start/end_pickup_drop_off_window` の時間制約で禁止されていない限り、便の中で前の停留所または場所から後の停留所または場所への移動が可能であると想定するべきです。<br><br>条件付き必須:<br>- `stop_times.location_group_id` および `stop_times.location_id` が定義されていない場合は **必須**。<br>- `stop_times.location_group_id` または `stop_times.location_id` が定義されている場合は **禁止**。|
| `location_group_id` | `location_groups.location_group_id` を参照する外部ID | **条件付き禁止** | 乗客が乗降をリクエストできる停留所群を示すサービス対象のロケーショングループを識別します。便の運行中にサービスされるすべてのロケーショングループは [stop_times.txt](#stop_timestxt) にレコードを持たなければなりません。複数の便やルートが同じロケーショングループをサービスすることができます。<br><br>ロケーショングループを使用するオンデマンドサービスでは、サービスが利用可能な順序でロケーショングループを参照するべきです。データ利用者は、各 `stop_time` の `pickup/drop_off_type` および各 `start/end_pickup_drop_off_window` の時間制約で禁止されていない限り、便の中で前の停留所または場所から後の停留所または場所への移動が可能であると想定するべきです。<br><br>**条件付き禁止**:<br>- `stop_times.stop_id` または `stop_times.location_id` が定義されている場合は **禁止**。|
| `location_id` | `locations.geojson` の `id` を参照する外部ID | **条件付き禁止** | 乗客が乗降をリクエストできるサービス対象ゾーンに対応する GeoJSON ロケーションを識別します。便の運行中にサービスされるすべての GeoJSON ロケーションは [stop_times.txt](#stop_timestxt) にレコードを持たなければなりません。複数の便やルートが同じ GeoJSON ロケーションをサービスすることができます。<br><br>ロケーション内でのオンデマンドサービスでは、サービスが利用可能な順序でロケーションを参照するべきです。データ利用者は、各 `stop_time` の `pickup/drop_off_type` および各 `start/end_pickup_drop_off_window` の時間制約で禁止されていない限り、便の中で前の停留所または場所から後の停留所または場所への移動が可能であると想定するべきです。<br><br>**条件付き禁止**:<br>- `stop_times.stop_id` または `stop_times.location_group_id` が定義されている場合は **禁止**。|
| `stop_sequence` | 非負整数 | **必須** | 特定の便における停留所、ロケーショングループ、または GeoJSON ロケーションの順序を示します。値は便の進行に沿って増加しなければなりませんが、連続している必要はありません。<hr>*例: 便の最初の場所が `stop_sequence`=`1`、2番目が `stop_sequence`=`23`、3番目が `stop_sequence`=`40` など。*<br><br>同一のロケーショングループまたは GeoJSON ロケーション内での移動には、同じ `location_group_id` または `location_id` を持つ2つの [stop_times.txt](#stop_timestxt) レコードが必要です。|
| `stop_headsign` | テキスト | 任意 | 乗客に便の行先を示す標識に表示されるテキストです。このフィールドは、停留所間で行先表示(headsign)が変わる場合に、デフォルトの `trips.trip_headsign` を上書きします。便全体で同じ行先表示が使用される場合は、`trips.trip_headsign` を使用するべきです。<br><br>1つの `stop_time` に指定された `stop_headsign` の値は、同じ便内の後続の `stop_time` には適用されません。複数の `stop_time` で `trip_headsign` を上書きしたい場合は、各 `stop_time` 行で同じ `stop_headsign` 値を繰り返す必要があります。|
| `start_pickup_drop_off_window` | 時刻 | **条件付き必須** | GeoJSON ロケーション、ロケーショングループ、または停留所でオンデマンドサービスが利用可能になる時刻です。<br><br>**条件付き必須**:<br>- `stop_times.location_group_id` または `stop_times.location_id` が定義されている場合は **必須**。<br>- `end_pickup_drop_off_window` が定義されている場合は **必須**。<br>- `arrival_time` または `departure_time` が定義されている場合は **禁止**。<br>- それ以外は任意。|
| `end_pickup_drop_off_window` | 時刻 | **条件付き必須** | GeoJSON ロケーション、ロケーショングループ、または停留所でオンデマンドサービスが終了する時刻です。<br><br>**条件付き必須**:<br>- `stop_times.location_group_id` または `stop_times.location_id` が定義されている場合は **必須**。<br>- `start_pickup_drop_off_window` が定義されている場合は **必須**。<br>- `arrival_time` または `departure_time` が定義されている場合は **禁止**。<br>- それ以外は任意。|
| `pickup_type` | 列挙型 | **条件付き禁止** | 乗車方法を示します。有効な値は以下の通りです:<br><br>`0` または空 - 定期的な乗車。<br>`1` - 乗車不可。<br>`2` - 事業者に電話して乗車を手配する必要あり。<br>`3` - 運転手と調整して乗車を手配する必要あり。<br><br>**条件付き禁止**:<br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合、`pickup_type=0` は **禁止**。<br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合、`pickup_type=3` は **禁止**。<br>- それ以外は任意。|
| `drop_off_type` | 列挙型 | **条件付き禁止** | 降車方法を示します。有効な値は以下の通りです:<br><br>`0` または空 - 定期的な降車。<br>`1` - 降車不可。<br>`2` - 事業者に電話して降車を手配する必要あり。<br>`3` - 運転手と調整して降車を手配する必要あり。<br><br>**条件付き禁止**:<br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合、`drop_off_type=0` は **禁止**。<br>- それ以外は任意。|
| `continuous_pickup` | 列挙型 | **条件付き禁止** | 乗客が、便の `stop_sequence` におけるこの `stop_time` から次の `stop_time` までの間、[shapes.txt](#shapestxt) で示される経路上の任意の地点で乗車できることを示します。有効な値は以下の通りです:<br><br>`0` - 連続乗車可能。<br>`1` または空 - 連続乗車不可。<br>`2` - 事業者に電話して連続乗車を手配する必要あり。<br>`3` - 運転手と調整して連続乗車を手配する必要あり。<br><br>このフィールドが設定されている場合、[routes.txt](#routestxt) で定義された連続乗車の挙動を上書きします。このフィールドが空の場合、`stop_time` は [routes.txt](#routestxt) で定義された連続乗車の挙動を継承します。<br><br>**条件付き禁止**:<br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合、`1` または空以外の値は **禁止**。<br>- それ以外は任意。|
| `continuous_drop_off` | 列挙型 | **条件付き禁止** | 乗客が、便の `stop_sequence` におけるこの `stop_time` から次の `stop_time` までの間、[shapes.txt](#shapestxt) で示される経路上の任意の地点で降車できることを示します。有効な値は以下の通りです:<br><br>`0` - 連続降車可能。<br>`1` または空 - 連続降車不可。<br>`2` - 事業者に電話して連続降車を手配する必要あり。<br>`3` - 運転手と調整して連続降車を手配する必要あり。<br><br>このフィールドが設定されている場合、[routes.txt](#routestxt) で定義された連続降車の挙動を上書きします。このフィールドが空の場合、`stop_time` は [routes.txt](#routestxt) で定義された連続降車の挙動を継承します。<br><br>**条件付き禁止**:<br>- `start_pickup_drop_off_window` または `end_pickup_drop_off_window` が定義されている場合、`1` または空以外の値は **禁止**。<br>- それ以外は任意。|
| `shape_dist_traveled` | 非負浮動小数点数 | 任意 | 関連するルート形状(shape)に沿って、最初の停留所からこのレコードで指定された停留所まで実際に移動した距離を示します。このフィールドは、便の任意の2つの停留所間で描画するルート形状の範囲を指定します。[shapes.txt](#shapestxt) で使用されている単位と同じ単位を使用しなければなりません。`shape_dist_traveled` の値は `stop_sequence` に沿って増加しなければならず、ルート上の逆方向の移動を示すために使用してはいけません。<br><br>ループや重複経路（同一経路を1便内で再通過する場合）を持つルートでは推奨されます。[`shapes.shape_dist_traveled`](#shapestxt) を参照してください。<hr>*例: バスがルート形状の始点から停留所まで5.25km移動した場合、`shape_dist_traveled`=`5.25`。*|
| `timepoint` | 列挙型 | 任意 | 停留所での到着・出発時刻が車両によって厳密に守られるか、またはおおよそ・補間された時刻であるかを示します。このフィールドにより、GTFS作成者は補間された停車時刻(stop_time)を提供しつつ、それらの時刻が概算であることを示すことができます。有効な値は以下の通りです:<br><br>`0` - 時刻は概算。<br>`1` - 時刻は正確。<br><br>[stop_times.txt](#stop_timestxt) のすべてのレコードで、到着または出発時刻が定義されている場合は、`timepoint` の値を設定するべきです。`timepoint` の値が提供されていない場合、すべての時刻は正確と見なされます。|
| `pickup_booking_rule_id` | `booking_rules.booking_rule_id` を参照する外部ID | 任意 | この停車時刻(stop_time)での乗車予約ルールを識別します。<br><br>`pickup_type=2` の場合に推奨されます。|
| `drop_off_booking_rule_id` | `booking_rules.booking_rule_id` を参照する外部ID | 任意 | この停車時刻(stop_time)での降車予約ルールを識別します。<br><br>`drop_off_type=2` の場合に推奨されます。|

#### オンデマンドサービスの経路設定動作 {: #on-demand-service-routing-behavior}

- 出発地と目的地の間の経路または所要時間を提供する際、データ利用者は、同じ `trip_id` を持ち、かつ `start_pickup_drop_off_window` および `end_pickup_drop_off_window` が定義されている中間の stop_times.txt のレコードを無視するべきです。無視するべき内容を示す例については、[データ例のページ](../examples/flex/#ignoring-intermediate-stop-times-records-with-pickupdrop-off-windows)を参照してください。
- 同じ `trip_id` を持つ2つ以上の stop_times.txt レコード間で、locations.geojson の `id` のジオメトリ、`start/end_pickup_drop_off_window` の時間、`pickup_type` または `drop_off_type` が同時に重複することは禁止されています。禁止されている内容を示す例については、[データ例のページ](../examples/flex/#zone-overlap-constraint)を参照してください。

### calendar.txt {: #calendartxt}


ファイル: **条件付き必須**

主キー (`service_id`)

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `service_id` | 一意のID | **必須** | 1つまたは複数のルートで運行が提供される日付のセットを識別します。 |
| `monday` | 列挙型(Enum) | **必須** | `start_date` および `end_date` フィールドで指定された日付範囲内のすべての月曜日にサービスが運行されるかどうかを示します。特定の日付に対する例外は [calendar_dates.txt](#calendar_datestxt) に記載される場合があります。有効な値は以下の通りです:<br><br>`1` - 日付範囲内のすべての月曜日にサービスが提供されます。<br>`0` - 日付範囲内の月曜日にはサービスが提供されません。 |
| `tuesday` | 列挙型(Enum) | **必須** | `monday` と同様に機能しますが、火曜日に適用されます。 |
| `wednesday` | 列挙型(Enum) | **必須** | `monday` と同様に機能しますが、水曜日に適用されます。 |
| `thursday` | 列挙型(Enum) | **必須** | `monday` と同様に機能しますが、木曜日に適用されます。 |
| `friday` | 列挙型(Enum) | **必須** | `monday` と同様に機能しますが、金曜日に適用されます。 |
| `saturday` | 列挙型(Enum) | **必須** | `monday` と同様に機能しますが、土曜日に適用されます。 |
| `sunday` | 列挙型(Enum) | **必須** | `monday` と同様に機能しますが、日曜日に適用されます。 |
| `start_date` | 日付 | **必須** | サービス区間の開始運行日を示します。 |
| `end_date` | 日付 | **必須** | サービス区間の終了運行日を示します。この運行日は区間に含まれます。 |

### calendar_dates.txt {: #calendar_datestxt}


ファイル: **条件付き必須**

主キー (`service_id`, `date`)

[calendar_dates.txt](#calendar_datestxt) テーブルは、日付ごとに運行を明示的に有効化または無効化します。これは2つの方法で使用することができます。

* 推奨: [calendar.txt](#calendartxt) と併用して、[calendar.txt](#calendartxt) で定義されたデフォルトの運行パターンに対する例外を定義します。運行が通常は規則的で、特定の日付にのみ変更がある場合（たとえば、特別イベントの運行や学校のスケジュールに対応する場合）に、この方法が適しています。この場合、`calendar_dates.service_id` は `calendar.service_id` を参照する外部IDです。
* 代替: [calendar.txt](#calendartxt) を省略し、[calendar_dates.txt](#calendar_datestxt) に運行するすべての日付を指定します。これにより、運行の大きな変動を許容し、通常の週次スケジュールを持たない運行にも対応できます。この場合、`service_id` は単独のIDです。

| フィールド名 | 型 | 出現 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `service_id` | `calendar.service_id` を参照する外部ID または ID | **必須** | 1つ以上のルートに対して運行例外が発生する日付の集合を識別します。[calendar.txt](#calendartxt) と [calendar_dates.txt](#calendar_datestxt) を併用する場合、各 (`service_id`, `date`) の組み合わせは [calendar_dates.txt](#calendar_datestxt) 内で一度しか出現してはいけません。`service_id` の値が [calendar.txt](#calendartxt) と [calendar_dates.txt](#calendar_datestxt) の両方に存在する場合、[calendar_dates.txt](#calendar_datestxt) の情報が [calendar.txt](#calendartxt) で指定された運行情報を修正します。 |
|  `date` | 日付 | **必須** | 運行例外が発生する日付です。 |
|  `exception_type` | 列挙型 | **必須** | 指定された日付フィールドの日に運行があるかどうかを示します。有効な値は以下の通りです:<br><br> `1` - 指定された日に運行が追加されています。<br>`2` - 指定された日に運行が削除されています。<hr>*例: あるルートが祝日用の便のセットと、それ以外の日用の便のセットを持っているとします。1つの `service_id` は通常の運行スケジュールに対応し、もう1つの `service_id` は祝日スケジュールに対応します。特定の祝日に対して、[calendar_dates.txt](#calendar_datestxt) ファイルを使用して、その祝日を祝日用の `service_id` に追加し、通常の `service_id` スケジュールから削除することができます。* |

### fare_attributes.txt {: #fare_attributestxt}


ファイル: **任意**

主キー (`fare_id`)

**バージョン**<br>
運賃を記述するためには2つのモデリングオプションがあります。GTFS-Fares V1 は、最小限の運賃情報を記述するための従来のオプションです。GTFS-Fares V2 は、事業者の運賃体系をより詳細に表現できる更新版の方法です。両方を同一のデータセット内に含めることは可能ですが、データ利用者は1つのデータセットに対してどちらか一方の方法のみを使用するべきです。GTFS-Fares V2 を GTFS-Fares V1 より優先して使用することが推奨されます。<br><br>GTFS-Fares V1 に関連するファイルは以下の通りです。<br>- [fare_attributes.txt](#fare_attributestxt)<br>- [fare_rules.txt](#fare_rulestxt)<br><br>GTFS-Fares V2 に関連するファイルは以下の通りです。<br>- [fare_media.txt](#fare_mediatxt)<br>- [fare_products.txt](#fare_productstxt)<br>- [rider_categories.txt](#rider_categoriestxt)<br>- [fare_leg_rules.txt](#fare_leg_rulestxt)<br>- [fare_leg_join_rules.txt](#fare_leg_join_rulestxt)<br>- [fare_transfer_rules.txt](#fare_transfer_rulestxt)<br>- [timeframes.txt](#timeframestxt)<br>- [networks.txt](#networkstxt)<br>- [route_networks.txt](#route_networkstxt)<br>- [areas.txt](#areastxt)<br>- [stop_areas.txt](#stop_areastxt)

<br>

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `fare_id` | 一意のID | **必須** | 運賃クラスを識別します。 |
| `price` | 非負の浮動小数点数 | **必須** | 運賃の価格を `currency_type` で指定された単位で表します。 |
| `currency_type` | 通貨コード | **必須** | 運賃の支払いに使用される通貨を示します。 |
| `payment_method` | 列挙型 | **必須** | 運賃を支払うタイミングを示します。有効なオプションは以下の通りです。<br><br>`0` - 乗車時に運賃を支払います。<br>`1` - 乗車前に運賃を支払わなければなりません。 |
| `transfers` | 列挙型 | **必須** | この運賃で許可される乗り換え回数を示します。有効なオプションは以下の通りです。<br><br>`0` - この運賃では乗り換えは許可されません。<br>`1` - 乗客は1回乗り換えることができます。<br>`2` - 乗客は2回乗り換えることができます。<br>空欄 - 無制限の乗り換えが許可されます。 |
| `agency_id` | `agency.agency_id` を参照する外部ID | **条件付き必須** | 運賃に関連する事業者を識別します。<br><br>条件付き必須:<br>- [agency.txt](#agencytxt) に複数の事業者が定義されている場合は **必須** です。<br>- それ以外の場合は推奨されます。 |
| `transfer_duration` | 非負の整数 | 任意 | 乗り換えが有効である時間を秒単位で示します。`transfers`=`0` の場合、このフィールドはチケットの有効期間を示すために使用することも、空欄のままにすることもできます。 |

### fare_rules.txt {: #fare_rulestxt}

ファイル: **任意**

主キー（`*`）

[fare_rules.txt](#fare_rulestxt) テーブルは、[fare_attributes.txt](#fare_attributestxt) に定義された運賃が旅程にどのように適用されるかを指定します。ほとんどの運賃体系は、以下のルールのいくつかを組み合わせて使用します。

* 運賃は出発駅または到着駅に依存します。
* 運賃は旅程が通過するゾーンに依存します。
* 運賃は旅程が利用するルートに依存します。

[fare_rules.txt](#fare_rulestxt) および [fare_attributes.txt](#fare_attributestxt) を使用して運賃体系を指定する方法の例については、GoogleTransitDataFeed オープンソースプロジェクトの wiki にある [FareExamples](https://web.archive.org/web/20111207224351/https://code.google.com/p/googletransitdatafeed/wiki/FareExamples) を参照してください。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `fare_id` | `fare_attributes.fare_id` を参照する外部 ID | **必須** | 運賃クラスを識別します。 |
| `route_id` | `routes.route_id` を参照する外部 ID | 任意 | 運賃クラスに関連付けられたルートを識別します。同じ運賃属性を持つ複数のルートが存在する場合は、各ルートに対して [fare_rules.txt](#fare_rulestxt) にレコードを作成します。<hr>*例: 運賃クラス「b」がルート「TSW」と「TSE」で有効な場合、[fare_rules.txt](#fare_rulestxt) ファイルには次のようなレコードが含まれます:* <br> `fare_id,route_id`<br>`b,TSW` <br> `b,TSE` |
| `origin_id` | `stops.zone_id` を参照する外部 ID | 任意 | 出発ゾーンを識別します。運賃クラスに複数の出発ゾーンがある場合は、各 `origin_id` に対して [fare_rules.txt](#fare_rulestxt) にレコードを作成します。<hr>*例: 運賃クラス「b」がゾーン「2」または「8」から出発するすべての旅程で有効な場合、[fare_rules.txt](#fare_rulestxt) ファイルには次のようなレコードが含まれます:* <br> `fare_id,...,origin_id` <br> `b,...,2`  <br> `b,...,8` |
| `destination_id` | `stops.zone_id` を参照する外部 ID | 任意 | 到着ゾーンを識別します。運賃クラスに複数の到着ゾーンがある場合は、各 `destination_id` に対して [fare_rules.txt](#fare_rulestxt) にレコードを作成します。<hr>*例: `origin_id` および `destination_id` フィールドを組み合わせて、運賃クラス「b」がゾーン3と4の間、またはゾーン3と5の間の移動で有効であることを指定する場合、[fare_rules.txt](#fare_rulestxt) ファイルには次のようなレコードが含まれます:* <br>`fare_id,...,origin_id,destination_id` <br>`b,...,3,4`<br> `b,...,3,5` |
| `contains_id` | `stops.zone_id` を参照する外部 ID | 任意 | 特定の運賃クラスを使用する際に乗客が通過するゾーンを識別します。一部のシステムでは、正しい運賃クラスを計算するために使用されます。<hr>*例: 運賃クラス「c」がゾーン5、6、7を通過する GRT ルート上のすべての旅程に関連付けられている場合、[fare_rules.txt](#fare_rulestxt) には次のようなレコードが含まれます:* <br> `fare_id,route_id,...,contains_id` <br>  `c,GRT,...,5` <br>`c,GRT,...,6` <br>`c,GRT,...,7` <br> *すべての `contains_id` ゾーンが一致しなければ運賃は適用されないため、ゾーン5と6を通過するがゾーン7を通過しない旅程には運賃クラス「c」は適用されません。詳細については、GoogleTransitDataFeed プロジェクトの wiki にある [https://code.google.com/p/googletransitdatafeed/wiki/FareExamples](https://code.google.com/p/googletransitdatafeed/wiki/FareExamples) を参照してください。* |

### timeframes.txt {: #timeframestxt}


ファイル: **任意**

主キー（`*`）

1日の時間帯、曜日、または特定の日付によって変動する運賃を記述するために使用されます。timeframe は [fare_leg_rules.txt](#fare_leg_rulestxt) 内のチケット商品(fare product)に関連付けることができます。<br>
同じ `timeframe_group_id` および `service_id` の値に対して、重複する時間間隔があってはいけません。

| フィールド名 | 型 | 出現 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `timeframe_group_id` | ID | **必須** | timeframe または timeframe の集合を識別します。 |
|  `start_time` | Time | **条件付き必須** | timeframe の開始時刻を定義します。この区間には開始時刻が含まれます。<br> `24:00:00` を超える値は使用してはいけません。`start_time` が空の場合は `00:00:00` と見なされます。<br><br> 条件付き必須:<br> - `timeframes.end_time` が定義されている場合は **必須**。<br> - それ以外の場合は **禁止**。 |
|  `end_time` | Time | **条件付き必須** | timeframe の終了時刻を定義します。この区間には終了時刻は含まれません。<br> `24:00:00` を超える値は使用してはいけません。`end_time` が空の場合は `24:00:00` と見なされます。<br><br> 条件付き必須:<br> - `timeframes.start_time` が定義されている場合は **必須**。<br> - それ以外の場合は **禁止**。 |
| `service_id` | `calendar.service_id` または `calendar_dates.service_id` を参照する外部 ID | **必須** | timeframe が有効となる日付の集合を識別します。 |

#### Timeframe のローカル時間の意味論 {: #timeframe-local-time-semantics}

- 運賃イベントの時刻を [timeframes.txt](#timeframestxt) と照合して評価する際、イベント時刻はローカルタイムゾーンを使用してローカル時間で計算されます。ローカルタイムゾーンは、運賃イベントに関連する停留所(stop)または親駅の `stop_timezone` によって決定されます。`stop_timezone` が指定されていない場合は、フィードの事業者のタイムゾーンを代わりに使用するべきです。
- 「現在の日(current day)」とは、ローカルタイムゾーンに基づいて計算された運賃イベント時刻の日付を指します。「現在の日」は、特に深夜をまたぐ便(trip)の場合、運賃区間(fare leg)の便の運行日(service day)と異なる場合があります。
- 運賃イベントの「時刻(time-of-day)」は、「現在の日(current day)」を基準として、GTFS の Time フィールド型の意味論に従って計算されます。

### rider_categories.txt {: #rider_categoriestxt}


ファイル: **任意**

主キー (`rider_category_id`)

乗客のカテゴリ（例: 高齢者、学生など）を定義します。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `rider_category_id` | 一意のID | **必須** | 乗客カテゴリを識別します。 |
| `rider_category_name` | テキスト | **必須** | 乗客に表示される乗客カテゴリ名です。 |
| `is_default_fare_category` | 列挙型 | **必須** | [rider_categories.txt](#rider_categoriestxt) のエントリがデフォルトカテゴリ（すなわち、乗客に表示される主要なカテゴリ）と見なされるかどうかを指定します。例: 大人運賃、通常運賃など。 有効なオプションは以下の通りです:<br><br>`0` または空欄 - カテゴリはデフォルトとは見なされません。<br>`1` - カテゴリはデフォルトと見なされます。<br><br>`fare_product_id` で指定された単一のチケット商品に対して複数の乗客カテゴリが適用可能な場合、これらのうち正確に1つの乗客カテゴリがデフォルト乗客カテゴリ（`is_default_fare_category = 1`）として指定されなければなりません。 |
| `eligibility_url` | URL | 任意 | 通常は運行事業者によるウェブページのURLで、特定の乗客カテゴリに関する詳細情報や、その適格基準を説明します。 |

### fare_media.txt {: #fare_mediatxt}


ファイル: **任意**

主キー (`fare_media_id`)

チケット商品を利用する際に使用されるさまざまなチケットメディアを記述します。チケットメディアは、チケット商品の表現および／または認証に使用される物理的または仮想的な媒体です。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `fare_media_id` | 一意のID | **必須** | チケットメディアを識別します。 |
| `fare_media_name` | テキスト | 任意 | チケットメディアの名称。<br><br>`fare_media_type =2` の交通系ICカードや、`fare_media_type =4` のモバイルアプリの場合、`fare_media_name` を含めるべきであり、提供する組織が乗客向けに使用している名称と一致している必要があります。 |
| `fare_media_type` | 列挙型 | **必須** | チケットメディアの種類。使用可能な値は以下の通りです。<br><br>`0` - なし。運賃商品の購入または認証にチケットメディアが関与しない場合に使用します。例: 物理的なチケットを発行せず、運転手や車掌に現金で支払う場合。<br>`1` - 物理的な紙のチケット。あらかじめ購入した一定回数分の乗車、または一定期間内の無制限乗車を可能にします。<br>`2` - チケット、定期券、または金額が保存された物理的な交通系ICカード。<br>`3` - アカウントベースチケッティングのオープンループトークンコンテナとしての cEMV（非接触型 Europay, Mastercard, Visa）。<br>`4` - 仮想交通系ICカード、チケット、定期券、または金額が保存されたモバイルアプリ。 |

### fare_products.txt {: #fare_productstxt}

ファイル: **任意**

主キー (`fare_product_id`, `rider_category_id`, `fare_media_id`)

乗客が購入可能な運賃の範囲、または複数の乗車区間(leg)を含む旅程(journey)における総運賃を計算する際に考慮される運賃（例: 乗り継ぎ料金など）を記述するために使用されます。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `fare_product_id` | ID | **必須** | チケット商品またはチケット商品のセットを識別します。<br><br>`fare_product_id` が同じ複数のレコードを含めることができますが、それらは異なる `fare_media_id` または `rider_category_id` を持たなければなりません。異なる `fare_media_id` は、チケット商品を利用するためのさまざまな方法（異なる価格の可能性あり）が存在することを示します。異なる `rider_category_id` は、複数の乗客カテゴリがそのチケット商品を利用できること（異なる価格の可能性あり）を示します。 |
| `fare_product_name` | Text | 任意 | 乗客に表示されるチケット商品の名称です。 |
| `rider_category_id` | 外部ID（`rider_categories.rider_category_id` を参照） | 任意 | チケット商品を利用できる乗客カテゴリを識別します。<br><br>`fare_products.rider_category_id` が空の場合、そのチケット商品はすべての `rider_category_id` に対して有効とみなされます。<br><br>1つの `fare_product_id` で指定された単一のチケット商品に複数の乗客カテゴリが適用される場合、それらのうち1つだけがデフォルトの乗客カテゴリ（`is_default_fare_category = 1`）として指定されなければなりません。 |
| `fare_media_id` | 外部ID（`fare_media.fare_media_id` を参照） | 任意 | 便(trip)の利用中にチケット商品を使用するために利用できるチケットメディアを識別します。`fare_media_id` が空の場合、チケットメディアは不明とみなされます。 |
| `amount` | 通貨額 | **必須** | チケット商品の料金です。乗り継ぎ割引を表すために負の値を使用することができます。無料のチケット商品を表すために0を使用することができます。 |
| `currency` | 通貨コード | **必須** | チケット商品の料金の通貨を示します。 |

### fare_leg_rules.txt {: #fare_leg_rulestxt}

ファイル: **任意**

主キー（`network_id, from_area_id, to_area_id, from_timeframe_group_id, to_timeframe_group_id, fare_product_id`）

個々の乗車区間(leg)に対する運賃ルールです。

[`fare_leg_rules.txt`](#fare_leg_rulestxt) 内の運賃は、ファイル内のすべてのレコードをフィルタリングし、乗客が移動する乗車区間(leg)に一致するルールを見つけることで照会しなければなりません。

乗車区間(leg)の運賃を処理する手順は以下の通りです。

1. [fare_leg_rules.txt](#fare_leg_rulestxt) ファイルを、移動の特性を定義するフィールドでフィルタリングしなければなりません。これらのフィールドは以下の通りです。
    - `fare_leg_rules.network_id`
    - `fare_leg_rules.from_area_id`
    - `fare_leg_rules.to_area_id`
    - `fare_leg_rules.from_timeframe_group_id`
    - `fare_leg_rules.to_timeframe_group_id`
<br/>

2. 乗車区間(leg)が移動の特性に基づいて [fare_leg_rules.txt](#fare_leg_rulestxt) 内のレコードと完全に一致する場合、そのレコードを処理して乗車区間(leg)の運賃を決定しなければなりません。このファイルでは、空のエントリを「空の意味」または「rule_priority」によって処理します。
<br/>

3. 完全一致するレコードが見つからず、かつ `rule_priority` フィールドが存在しない場合、`fare_leg_rules.network_id`、`fare_leg_rules.from_area_id`、および `fare_leg_rules.to_area_id` の空のエントリを確認して、乗車区間(leg)の運賃を処理しなければなりません。
    - `fare_leg_rules.network_id` の空のエントリは、[routes.txt](#routestxt) または [networks.txt](#networkstxt) に定義されているすべてのネットワークのうち、`fare_leg_rules.network_id` に記載されているものを除いたものに対応します。

    - `fare_leg_rules.from_area_id` の空のエントリは、`areas.area_id` に定義されているすべてのエリアのうち、`fare_leg_rules.from_area_id` に記載されているものを除いたものに対応します。
    - `fare_leg_rules.to_area_id` の空のエントリは、`areas.area_id` に定義されているすべてのエリアのうち、`fare_leg_rules.to_area_id` に記載されているものを除いたものに対応します。
<br/>

4. `rule_priority` フィールドが存在する場合、
    - `fare_leg_rules.network_id` の空のエントリは、このルールの一致において乗車区間(leg)のネットワークが影響しないことを示します。
    - `fare_leg_rules.from_area_id` の空のエントリは、このルールの一致において乗車区間(leg)の出発エリアが影響しないことを示します。
    - `fare_leg_rules.to_area_id` の空のエントリは、このルールの一致において乗車区間(leg)の到着エリアが影響しないことを示します。
<br/>
      
5. 上記のいずれのルールにも乗車区間(leg)が一致しない場合、その運賃は不明です。

<br/>

| フィールド名 | 型 | 出現 | 説明 |
|  ------ | ------ | ------ | ------ |
| `leg_group_id` | ID | 任意 | [fare_leg_rules.txt](#fare_leg_rulestxt) 内のエントリのグループを識別します。<br><br>`fare_transfer_rules.from_leg_group_id` と `fare_transfer_rules.to_leg_group_id` の間の運賃乗り継ぎルールを記述するために使用されます。<br><br>[fare_leg_rules.txt](#fare_leg_rulestxt) 内の複数のエントリが同じ `fare_leg_rules.leg_group_id` に属することがあります。<br><br>[fare_leg_rules.txt](#fare_leg_rulestxt) 内の同一エントリ（`fare_leg_rules.leg_group_id` を除く）は、複数の `fare_leg_rules.leg_group_id` に属してはいけません。|
| `network_id` | `routes.network_id` または `networks.network_id` を参照する外部 ID | 任意 | 運賃ルールが適用されるルートネットワークを識別します。<br><br>`rule_priority` フィールドが存在せず、フィルタリング対象の `network_id` に一致する `fare_leg_rules.network_id` の値が存在しない場合、空の `fare_leg_rules.network_id` がデフォルトで一致します。<br><br>`fare_leg_rules.network_id` の空のエントリは、[routes.txt](#routestxt) または [networks.txt](#networkstxt) に定義されているすべてのネットワークのうち、`fare_leg_rules.network_id` に記載されているものを除いたものに対応します。<br><br>ファイル内に `rule_priority` フィールドが存在する場合、空の `fare_leg_rules.network_id` は、乗車区間(leg)のルートネットワークがこのルールの一致に影響しないことを示します。<br><br>[複数の乗車区間(leg)からなる有効運賃区間(effective fare leg)](#fare_leg_join_rulestxt) に対して一致を行う場合、各乗車区間(leg)は同じ `network_id` を持たなければならず、その値が一致判定に使用されます。 |
| `from_area_id` | `areas.area_id` を参照する外部 ID | 任意 | 出発エリアを識別します。<br><br>`rule_priority` フィールドが存在せず、フィルタリング対象の `area_id` に一致する `fare_leg_rules.from_area_id` の値が存在しない場合、空の `fare_leg_rules.from_area_id` がデフォルトで一致します。<br><br>`fare_leg_rules.from_area_id` の空のエントリは、`areas.area_id` に定義されているすべてのエリアのうち、`fare_leg_rules.from_area_id` に記載されているものを除いたものに対応します。<br><br>ファイル内に `rule_priority` フィールドが存在する場合、空の `fare_leg_rules.from_area_id` は、乗車区間(leg)の出発エリアがこのルールの一致に影響しないことを示します。<br><br>[複数の乗車区間(leg)からなる有効運賃区間(effective fare leg)](#fare_leg_join_rulestxt) に対して一致を行う場合、有効運賃区間の最初の乗車区間(leg)が出発エリアの判定に使用されます。 |
| `to_area_id` | `areas.area_id` を参照する外部 ID | 任意 | 到着エリアを識別します。<br><br>`rule_priority` フィールドが存在せず、フィルタリング対象の `area_id` に一致する `fare_leg_rules.to_area_id` の値が存在しない場合、空の `fare_leg_rules.to_area_id` がデフォルトで一致します。<br><br>`fare_leg_rules.to_area_id` の空のエントリは、`areas.area_id` に定義されているすべてのエリアのうち、`fare_leg_rules.to_area_id` に記載されているものを除いたものに対応します。<br><br>ファイル内に `rule_priority` フィールドが存在する場合、空の `fare_leg_rules.to_area_id` は、乗車区間(leg)の到着エリアがこのルールの一致に影響しないことを示します。<br><br>[複数の乗車区間(leg)からなる有効運賃区間(effective fare leg)](#fare_leg_join_rulestxt) に対して一致を行う場合、有効運賃区間の最後の乗車区間(leg)が到着エリアの判定に使用されます。 |
| `from_timeframe_group_id` | `timeframes.timeframe_group_id` を参照する外部 ID | 任意 | 乗車区間(leg)の開始時点での運賃認証イベントに対する時間枠を定義します。<br><br>乗車区間(leg)の「開始時刻」は、そのイベントが予定されている時刻です。たとえば、乗客が乗車して運賃を認証する乗車区間(leg)の開始地点におけるバスの予定出発時刻などです。以下のルール一致の意味論において、開始時刻は [timeframes.txt](#timeframestxt) の [Local Time Semantics](#timeframe-local-time-semantics) に従ってローカル時刻で計算されます。適切な場合、乗車区間(leg)の出発イベントの停留所(stop)または駅をタイムゾーンの解決に使用するべきです。<br><br>`from_timeframe_group_id` を指定する運賃ルールは、[timeframes.txt](#timeframestxt) 内に以下のすべての条件を満たすレコードが少なくとも1つ存在する場合に、特定の乗車区間(leg)と一致します。<br>- `timeframe_group_id` の値が `from_timeframe_group_id` の値と等しい。<br>- レコードの `service_id` によって識別される日付の集合が、乗車区間(leg)の開始時刻の「当日」を含む。<br>- 乗車区間(leg)の開始時刻の「時刻」が、レコードの `timeframes.start_time` 以上かつ `timeframes.end_time` 未満である。<br><br>空の `fare_leg_rules.from_timeframe_group_id` は、乗車区間(leg)の開始時刻がこのルールの一致に影響しないことを示します。<br><br>[複数の乗車区間(leg)からなる有効運賃区間(effective fare leg)](#fare_leg_join_rulestxt) に対して一致を行う場合、有効運賃区間の最初の乗車区間(leg)が開始時点の運賃認証イベントの判定に使用されます。 |
| `to_timeframe_group_id` | `timeframes.timeframe_group_id` を参照する外部 ID | 任意 | 乗車区間(leg)の終了時点での運賃認証イベントに対する時間枠を定義します。<br><br>乗車区間(leg)の「終了時刻」は、そのイベントが予定されている時刻です。たとえば、乗客が降車して運賃を認証する乗車区間(leg)の終点におけるバスの予定到着時刻などです。以下のルール一致の意味論において、終了時刻は [timeframes.txt](#timeframestxt) の [Local Time Semantics](#timeframe-local-time-semantics) に従ってローカル時刻で計算されます。適切な場合、乗車区間(leg)の到着イベントの停留所(stop)または駅をタイムゾーンの解決に使用するべきです。<br><br>`to_timeframe_group_id` を指定する運賃ルールは、[timeframes.txt](#timeframestxt) 内に以下のすべての条件を満たすレコードが少なくとも1つ存在する場合に、特定の乗車区間(leg)と一致します。<br>- `timeframe_group_id` の値が `to_timeframe_group_id` の値と等しい。<br>- レコードの `service_id` によって識別される日付の集合が、乗車区間(leg)の終了時刻の「当日」を含む。<br>- 乗車区間(leg)の終了時刻の「時刻」が、レコードの `timeframes.start_time` 以上かつ `timeframes.end_time` 未満である。<br><br>空の `fare_leg_rules.to_timeframe_group_id` は、乗車区間(leg)の終了時刻がこのルールの一致に影響しないことを示します。<br><br>[複数の乗車区間(leg)からなる有効運賃区間(effective fare leg)](#fare_leg_join_rulestxt) に対して一致を行う場合、有効運賃区間の最後の乗車区間(leg)が終了時点の運賃認証イベントの判定に使用されます。 |
| `fare_product_id` | `fare_products.fare_product_id` を参照する外部 ID | **必須** | 乗車区間(leg)を移動するために必要なチケット商品を示します。 |
| `rule_priority` | 非負整数 | 任意 | ルールの一致が乗車区間(leg)に適用される際の優先順位を定義し、特定のルールが他のルールより優先されることを可能にします。`fare_leg_rules.txt` 内で複数のエントリが一致する場合、`rule_priority` の値が最も高いルールまたはルール群が選択されます。<br><br>`rule_priority` の値が空の場合は、0 として扱われます。 |

### fare_leg_join_rules.txt {: #fare_leg_join_rulestxt}

ファイル: **任意**

主キー（`from_network_id, to_network_id, from_stop_id, to_stop_id`）

乗り換えを伴う2つの連続した乗車区間(leg)からなる部分旅程(sub-journey)において、その乗り換えがこのファイル内の特定のレコードで指定されたすべての一致条件に合致する場合、これら2つの乗車区間は [`fare_leg_rules.txt`](#fare_leg_rulestxt) 内のルールと照合する際に、1つの**有効運賃区間(effective fare leg)** として扱うべきです。

- `from_stop_id` および `to_stop_id` によって明示的に上書きされない限り、乗り換え前の乗車区間の最終駅と乗り換え後の乗車区間の最初の駅は、レコード内で同一でなければなりません。
- 一致条件フィールド値が空白または未指定の場合、そのフィールドは照合の際に無視されるべきです。
- 部分旅程(sub-journey)が、各乗り換えが結合ルールに一致する連続した乗り換えを含む場合、その部分旅程全体を1つの**有効運賃区間(effective fare leg)** として扱うべきです。

| フィールド名 | 型 | 出現 | 説明 |
|  ------ | ------ | ------ | ------ |
| `from_network_id` | `routes.network_id` または `networks.network_id` を参照する外部ID | **必須** | 指定されたルートネットワークを使用する乗り換え前の乗車区間に一致します。指定された場合、同じ `to_network_id` も指定しなければなりません。 |
| `to_network_id` | `routes.network_id` または `networks.network_id` を参照する外部ID | **必須** | 指定されたルートネットワークを使用する乗り換え後の乗車区間に一致します。指定された場合、同じ `from_network_id` も指定しなければなりません。 |
| `from_stop_id` | `stops.stop_id` を参照する外部ID | **条件付き必須** | 指定された停留所(`location_type=0` または空)または駅(`location_type=1`)で終了する乗り換え前の乗車区間に一致します。<br><br>条件付き必須:<br> - `to_stop_id` が定義されている場合は **必須**。<br> - それ以外の場合は任意。 |
| `to_stop_id` | `stops.stop_id` を参照する外部ID | **条件付き必須** | 指定された停留所(`location_type=0` または空)または駅(`location_type=1`)で開始する乗り換え後の乗車区間に一致します。<br><br>条件付き必須:<br> - `from_stop_id` が定義されている場合は **必須**。<br> - それ以外の場合は任意。 |

### fare_transfer_rules.txt {: #fare_transfer_rulestxt}


ファイル: **任意**

主キー（`from_leg_group_id, to_leg_group_id, fare_product_id, transfer_count, duration_limit`）

[`fare_leg_rules.txt`](#fare_leg_rulestxt) で定義された乗車区間(leg)間の乗り換えに関する運賃ルールです。

複数の乗車区間(leg)からなる旅程(journey)の運賃を処理するには、次の手順に従います。

1. 乗客の旅程に基づき、`fare_leg_rules.txt` で定義された該当する運賃区間グループを、すべての個別の乗車区間(leg)について特定するべきです。
2. [fare_transfer_rules.txt](#fare_transfer_rulestxt) ファイルを、乗り換えの特性を定義する以下のフィールドでフィルタリングしなければなりません。これらのフィールドは次の通りです。
    - `fare_transfer_rules.from_leg_group_id`
    - `fare_transfer_rules.to_leg_group_id`<br/>
    <br/>

3. 乗り換えがその特性に基づいて [fare_transfer_rules.txt](#fare_transfer_rulestxt) のレコードと完全に一致する場合、そのレコードを処理して乗り換えコストを決定しなければなりません。
4. 完全一致が見つからない場合、`from_leg_group_id` または `to_leg_group_id` が空のエントリを確認して、乗り換えコストを処理しなければなりません。
    - `fare_transfer_rules.from_leg_group_id` が空のエントリは、`fare_leg_rules.leg_group_id` で定義されたすべての運賃区間グループのうち、`fare_transfer_rules.from_leg_group_id` に記載されているものを除いたすべてに対応します。
    - `fare_transfer_rules.to_leg_group_id` が空のエントリは、`fare_leg_rules.leg_group_id` で定義されたすべての運賃区間グループのうち、`fare_transfer_rules.to_leg_group_id` に記載されているものを除いたすべてに対応します。<br/>
    <br/>
5. 上記のいずれのルールにも一致しない場合、乗り換えの取り決めは存在せず、各乗車区間(leg)は別々のものとして扱われます。

<br/>

| フィールド名 | 型 | 出現 | 説明 |
|  ------ | ------ | ------ | ------ |
| `from_leg_group_id` | `fare_leg_rules.leg_group_id` を参照する外部ID | 任意 | 乗り換え前の運賃区間ルールのグループを識別します。<br><br>`leg_group_id` をフィルタリングする際に一致する `fare_transfer_rules.from_leg_group_id` の値がない場合、空の `fare_transfer_rules.from_leg_group_id` がデフォルトで一致します。<br><br>`fare_transfer_rules.from_leg_group_id` が空のエントリは、`fare_leg_rules.leg_group_id` で定義されたすべての運賃区間グループのうち、`fare_transfer_rules.from_leg_group_id` に記載されているものを除いたすべてに対応します。|
| `to_leg_group_id` | `fare_leg_rules.leg_group_id` を参照する外部ID | 任意 | 乗り換え後の運賃区間ルールのグループを識別します。<br><br>`leg_group_id` をフィルタリングする際に一致する `fare_transfer_rules.to_leg_group_id` の値がない場合、空の `fare_transfer_rules.to_leg_group_id` がデフォルトで一致します。<br><br>`fare_transfer_rules.to_leg_group_id` が空のエントリは、`fare_leg_rules.leg_group_id` で定義されたすべての運賃区間グループのうち、`fare_transfer_rules.to_leg_group_id` に記載されているものを除いたすべてに対応します。|
| `transfer_count` | 0以外の整数 | **条件付き禁止** | 乗り換えルールが適用される連続した乗り換えの回数を定義します。<br><br>有効な値は次の通りです。<br>`-1` - 制限なし。<br>`1` 以上 - 乗り換えルールが適用される乗り換えの回数を定義します。<br><br>部分旅程(sub-journey)が異なる `transfer_count` を持つ複数のレコードに一致する場合、現在の部分旅程の乗り換え回数以上で最小の `transfer_count` を持つルールを選択します。<br><br>条件付き禁止:<br>- **禁止**: `fare_transfer_rules.from_leg_group_id` が `fare_transfer_rules.to_leg_group_id` と等しくない場合。<br>- **必須**: `fare_transfer_rules.from_leg_group_id` が `fare_transfer_rules.to_leg_group_id` と等しい場合。|
| `duration_limit` | 正の整数 | 任意 | 乗り換えの時間制限を定義します。<br><br>秒単位の整数で表さなければなりません。<br><br>時間制限がない場合、`fare_transfer_rules.duration_limit` は空でなければなりません。|
| `duration_limit_type` | Enum | **条件付き必須** | `fare_transfer_rules.duration_limit` の相対的な開始点と終了点を定義します。<br><br>有効な値は次の通りです。<br>`0` - 現在の乗車区間(leg)の出発時の運賃認証から次の乗車区間(leg)の到着時の運賃認証まで。<br>`1` - 現在の乗車区間(leg)の出発時の運賃認証から次の乗車区間(leg)の出発時の運賃認証まで。<br>`2` - 現在の乗車区間(leg)の到着時の運賃認証から次の乗車区間(leg)の出発時の運賃認証まで。<br>`3` - 現在の乗車区間(leg)の到着時の運賃認証から次の乗車区間(leg)の到着時の運賃認証まで。<br><br>条件付き必須:<br>- **必須**: `fare_transfer_rules.duration_limit` が定義されている場合。<br>- **禁止**: `fare_transfer_rules.duration_limit` が空の場合。|
| `fare_transfer_type` | Enum | **必須** | 旅程(journey)内の乗車区間(leg)間の乗り換えにおけるコスト処理方法を示します。<br>![](../../assets/2-leg.svg) <br>有効な値は次の通りです。<br>`0` - 乗り換え元の `fare_leg_rules.fare_product_id` に `fare_transfer_rules.fare_product_id` を加えたもの。A + AB。<br>`1` - 乗り換え元の `fare_leg_rules.fare_product_id` に `fare_transfer_rules.fare_product_id` および乗り換え先の `fare_leg_rules.fare_product_id` を加えたもの。A + AB + B。<br>`2` - `fare_transfer_rules.fare_product_id` のみ。AB。<br><br>旅程内の複数の乗り換えにおけるコスト処理の相互作用:<br>![](../../assets/3-leg.svg)<br><table><thead><tr><th>`fare_transfer_type`</th><th>A > B の処理</th><th>B > C の処理</th></tr></thead><tbody><tr><td>`0`</td><td>A + AB</td><td>S + BC</td></tr><tr><td>`1`</td><td>A + AB + B</td><td>S + BC + C</td></tr><tr><td>`2`</td><td>AB</td><td>S + BC</td></tr></tbody></table>ここで、S はそれまでの乗車区間および乗り換えの合計処理済みコストを示します。|
| `fare_product_id` | `fare_products.fare_product_id` を参照する外部ID | 任意 | 2つの運賃区間間の乗り換えに必要なチケット商品を示します。空の場合、乗り換えルールのコストは0です。|

### areas.txt {: #areastxt}


ファイル: **任意**

主キー (`area_id`)

エリア識別子を定義します。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `area_id` | 一意のID | **必須** | エリアを識別します。[areas.txt](#areastxt) 内で一意でなければなりません。 |
| `area_name` | テキスト | **任意** | 乗客に表示されるエリア名です。 |

### stop_areas.txt {: #stop_areastxt}


ファイル: **任意**

主キー（`*`）

[stops.txt](#stopstxt) に含まれる停留所(stop)をエリアに割り当てます。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `area_id` | `areas.area_id` を参照する外部ID | **必須** | 1つまたは複数の `stop_id` が属するエリアを識別します。同じ `stop_id` が複数の `area_id` に定義されることがあります。 |
| `stop_id` | `stops.stop_id` を参照する外部ID | **必須** | 停留所(stop)を識別します。このフィールドに駅（すなわち `stops.location_type=1` の停留所）が定義されている場合、その駅に属するすべてのプラットフォーム（すなわち `stops.location_type=0` であり、かつこの駅が `stops.parent_station` に定義されているすべての停留所）は同じエリアの一部であるとみなされます。この動作は、プラットフォームを他のエリアに割り当てることで上書きすることができます。

### networks.txt {: #networkstxt}


ファイル: **条件付きで禁止**

主キー (`network_id`)

運賃区間ルールに適用されるネットワーク識別子を定義します。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `network_id` | 一意のID | **必須** | ネットワークを識別します。[networks.txt](#networkstxt) 内で一意でなければなりません。 |
| `network_name` | テキスト | **任意** | 事業者および乗客が使用する、運賃区間ルールに適用されるネットワークの名称です。

### route_networks.txt {: #route_networkstxt}


ファイル: **条件付きで禁止**

主キー (`route_id`)

[routes.txt](#routestxt) に含まれるルートをネットワークに割り当てます。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `network_id` | `networks.network_id` を参照する外部ID | **必須** | 1つまたは複数の `route_id` が属するネットワークを識別します。1つの `route_id` は1つの `network_id` にのみ定義することができます。 |
| `route_id` | `routes.route_id` を参照する外部ID | **必須** | ルートを識別します。 |

### shapes.txt {: #shapestxt}

ファイル: **任意**

主キー (`shape_id`, `shape_pt_sequence`)

Shape は、車両がルートの経路に沿って走行する経路を表し、ファイル shapes.txt で定義されます。Shape は Trip に関連付けられ、車両が通過する一連の点で構成されます。Shape は Stop の位置を正確に通過する必要はありませんが、Trip 上のすべての Stop は、その Trip の Shape からわずかな距離内、すなわち Shape の点を結ぶ直線セグメントの近くに位置するべきです。shapes.txt ファイルは、ゾーンベースのデマンド型サービスではなく、ルートベースのサービスすべてに含めるべきです。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `shape_id` | ID | **必須** | Shape を識別します。 |
| `shape_pt_lat` | 緯度 | **必須** | Shape の点の緯度です。[shapes.txt](#shapestxt) の各レコードは、Shape を定義するために使用される Shape の点を表します。 |
| `shape_pt_lon` | 経度 | **必須** | Shape の点の経度です。 |
| `shape_pt_sequence` | 非負の整数 | **必須** | Shape の点が接続されて Shape を形成する順序です。値は Trip に沿って増加しなければなりませんが、連続している必要はありません。<hr>*例: Shape「A_shp」が3つの点で定義されている場合、[shapes.txt](#shapestxt) ファイルには次のレコードが含まれ、Shape を定義します:* <br> `shape_id,shape_pt_lat,shape_pt_lon,shape_pt_sequence` <br> `A_shp,37.61956,-122.48161,0` <br> `A_shp,37.64430,-122.41070,6` <br> `A_shp,37.65863,-122.30839,11` |
| `shape_dist_traveled` | 非負の浮動小数点数 | 任意 | 最初の Shape の点から、このレコードで指定された点までの Shape に沿った実際の走行距離です。経路検索システムが地図上で正しい Shape の区間を表示するために使用されます。値は `shape_pt_sequence` に沿って増加しなければなりません。ルート上を逆方向に走行することを示すために使用してはいけません。距離の単位は [stop_times.txt](#stop_timestxt) で使用される単位と一致していなければなりません。<br><br>ループや重複経路（車両が1つの Trip 内で同じ経路の一部を横断または再走行する場合）があるルートでは推奨されます。<br><img src="../../../assets/inlining.svg" width=200px style="display: block; margin-left: auto; margin-right: auto;"> <br>車両が Trip の途中でルートの経路を再走行または交差する場合、`shape_dist_traveled` は [shapes.txt](#shapestxt) の点の部分が [stop_times.txt](#stop_timestxt) のレコードとどのように対応するかを明確にするために重要です。<hr>*例: バスが上記の3点を通る A_shp の経路を走行する場合、追加の `shape_dist_traveled` 値（ここではキロメートル単位）は次のようになります:* <br> `shape_id,shape_pt_lat,shape_pt_lon,shape_pt_sequence,shape_dist_traveled`<br>`A_shp,37.61956,-122.48161,0,0`<br>`A_shp,37.64430,-122.41070,6,6.8310` <br> `A_shp,37.65863,-122.30839,11,15.8765` |

### frequencies.txt {: #frequenciestxt}


ファイル: **任意**

主キー (`trip_id`, `start_time`)

[Frequencies.txt](#frequenciestxt) は、一定の運行間隔（便間の時間）で運行される便(trip)を表します。このファイルは、2種類のサービス形態を表すために使用することができます。

* 頻度ベースのサービス（`exact_times`=`0`）: 一日の中で固定された時刻表に従わないサービスです。代わりに、運行事業者はあらかじめ定められた運行間隔を厳密に維持するよう努めます。
* 時刻表ベースのサービスの圧縮表現（`exact_times`=`1`）: 指定された時間帯において、便が同一の運行間隔で運行される場合です。時刻表ベースのサービスでは、運行事業者は時刻表を厳密に遵守するよう努めます。


| フィールド名 | 型 | 出現 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `trip_id` | `trips.trip_id` を参照する外部ID | **必須** | 指定された運行間隔が適用される便(trip)を識別します。 |
|  `start_time` | 時刻 | **必須** | 指定された運行間隔で、最初の車両が便の最初の停留所(stop)を出発する時刻です。 |
|  `end_time` | 時刻 | **必須** | 便の最初の停留所(stop)において、運行間隔が変更（または運行が終了）する時刻です。 |
|  `headway_secs` | 正の整数 | **必須** | 指定された `start_time` と `end_time` の時間帯において、同一の停留所(stop)からの出発間隔（運行間隔）を秒単位で表します。同一の便(trip)に対して複数の運行間隔を定義することができますが、重複してはいけません。新しい運行間隔は、前の運行間隔が終了する時刻と同時に開始することができます。 |
|  `exact_times` | 列挙型 | 任意 | 便(trip)のサービス形態を示します。詳細はファイルの説明を参照してください。有効な値は以下の通りです。<br><br>`0` または空欄 - 頻度ベースの便。<br>`1` - 一日の中で同一の運行間隔を持つ時刻表ベースの便。この場合、`end_time` の値は、最後に運行したい便の `start_time` より大きく、かつその `start_time` + `headway_secs` より小さくなければなりません。 |

### transfers.txt {: #transferstxt}


ファイル: **任意**

主キー (`from_stop_id`, `to_stop_id`, `from_trip_id`, `to_trip_id`, `from_route_id`, `to_route_id`)

旅程(journey)を計算する際、GTFS を利用するアプリケーションは、許容される時間および停留所等(stop)の近接性に基づいて乗り換えを補間します。 [Transfers.txt](#transferstxt) は、特定の乗り換えに対する追加ルールおよび上書きを指定します。

`from_trip_id`、`to_trip_id`、`from_route_id`、および `to_route_id` フィールドは、乗り換えルールに対してより高い特異性を持たせることができます。`from_stop_id` および `to_stop_id` とあわせて、特異性の順位は次の通りです。

1. 両方の `trip_id` が定義されている場合: `from_trip_id` および `to_trip_id`。
2. 1つの `trip_id` と 1つの `route_id` セットが定義されている場合: (`from_trip_id` と `to_route_id`) または (`from_route_id` と `to_trip_id`)。
3. 1つの `trip_id` が定義されている場合: `from_trip_id` または `to_trip_id`。
4. 両方の `route_id` が定義されている場合: `from_route_id` および `to_route_id`。
5. 1つの `route_id` が定義されている場合: `from_route_id` または `to_route_id`。
6. `from_stop_id` および `to_stop_id` のみが定義されている場合: route または trip に関連するフィールドは設定されていません。

到着便と出発便の順序付きペアに対して、これら2つの便の間で適用される最も特異性の高い乗り換えが選択されます。任意の便のペアに対して、同等に最大の特異性を持つ2つの乗り換えが適用されることがあってはなりません。

| フィールド名 | 型 | 出現 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `from_stop_id` | `stops.stop_id` を参照する外部 ID | **条件付き必須** | route 間の接続が始まる停留所(`location_type=0`)または駅(`location_type=1`)を識別します。このフィールドが駅を参照する場合、乗り換えルールはそのすべての子停留所に適用されます。`transfer_type` が `4` または `5` の場合は、停留所(`location_type=0`)を参照しなければなりません。<br><br>条件付き必須:<br>- **必須**: `transfer_type` が `1`、`2`、または `3` の場合。<br>- 任意: `transfer_type` が `4` または `5` の場合。 |
|  `to_stop_id` | `stops.stop_id` を参照する外部 ID | **条件付き必須** | route 間の接続が終わる停留所(`location_type=0`)または駅(`location_type=1`)を識別します。このフィールドが駅を参照する場合、乗り換えルールはそのすべての子停留所に適用されます。`transfer_type` が `4` または `5` の場合は、停留所(`location_type=0`)を参照しなければなりません。<br><br>条件付き必須:<br>- **必須**: `transfer_type` が `1`、`2`、または `3` の場合。<br>- 任意: `transfer_type` が `4` または `5` の場合。 |
| `from_route_id`  | `routes.route_id` を参照する外部 ID | 任意 | 接続が始まる route を識別します。<br><br>`from_route_id` が定義されている場合、乗り換えは指定された `from_stop_id` におけるその route の到着便に適用されます。<br><br>`from_trip_id` と `from_route_id` の両方が定義されている場合、`trip_id` は `route_id` に属していなければならず、`from_trip_id` が優先されます。 |
| `to_route_id`  | `routes.route_id` を参照する外部 ID | 任意 | 接続が終わる route を識別します。<br><br>`to_route_id` が定義されている場合、乗り換えは指定された `to_stop_id` におけるその route の出発便に適用されます。<br><br>`to_trip_id` と `to_route_id` の両方が定義されている場合、`trip_id` は `route_id` に属していなければならず、`to_trip_id` が優先されます。 |
| `from_trip_id`  | `trips.trip_id` を参照する外部 ID | **条件付き必須** | route 間の接続が始まる便を識別します。<br><br>`from_trip_id` が定義されている場合、乗り換えは指定された `from_stop_id` における到着便に適用されます。<br><br>`from_trip_id` と `from_route_id` の両方が定義されている場合、`trip_id` は `route_id` に属していなければならず、`from_trip_id` が優先されます。<br><br>条件付き必須:<br>- **必須**: `transfer_type` が `4` または `5` の場合。<br>- それ以外は任意。 |
| `to_trip_id`  | `trips.trip_id` を参照する外部 ID | **条件付き必須** | route 間の接続が終わる便を識別します。<br><br>`to_trip_id` が定義されている場合、乗り換えは指定された `to_stop_id` における出発便に適用されます。<br><br>`to_trip_id` と `to_route_id` の両方が定義されている場合、`trip_id` は `route_id` に属していなければならず、`to_trip_id` が優先されます。<br><br>条件付き必須:<br>- **必須**: `transfer_type` が `4` または `5` の場合。<br>- それ以外は任意。 |
|  `transfer_type` | Enum | **必須** | 指定された (`from_stop_id`, `to_stop_id`) ペアに対する接続の種類を示します。有効なオプションは次の通りです。<br><br>`0` または空欄 - route 間の推奨乗り換え地点。<br>`1` - 2つの route 間の時刻調整された乗り換え地点。出発車両は到着車両を待機し、乗客が乗り換えできる十分な時間を確保することが期待されます。<br>`2` - 接続を確実にするために、到着と出発の間に最小限の時間が必要な乗り換え。必要な時間は `min_transfer_time` で指定します。<br>`3` - この場所では route 間の乗り換えは不可能です。<br>`4` - 乗客は同じ車両に乗ったまま、1つの便から別の便に乗り換えることができます（「車内乗り換え」）。このタイプの乗り換えの詳細は[以下](#linked-trips)を参照してください。<br>`5` - 連続する便の間で車内乗り換えは許可されません。乗客は一度下車し、再乗車しなければなりません。このタイプの乗り換えの詳細は[以下](#linked-trips)を参照してください。 |
|  `min_transfer_time` | 非負整数 | 任意 | 指定された停留所間で route 間の乗り換えを可能にするために必要な時間（秒単位）です。`min_transfer_time` は、各 route の時刻のばらつきを考慮し、典型的な乗客が2つの停留所間を移動するのに十分な余裕時間を含むようにするべきです。 |

#### 連結された便(linked trips) {: #linked-trips}

以下の内容は、便を連結するために使用される `transfer_type=4` および `=5` に適用されます。これらは、座席に座ったままの乗り継ぎを伴う場合と伴わない場合の両方に対応します。

連結された便は、同一の車両によって運行されなければなりません。その車両は、他の車両と連結または切り離されることがあります。

連結された便の乗り継ぎと `block_id` の両方が指定され、結果が矛盾する場合は、連結された便の乗り継ぎを使用しなければなりません。

`from_trip_id` の最終停留所(stop)は、`to_trip_id` の最初の停留所(stop)と地理的に近い位置にあるべきです。また、`from_trip_id` の最終到着時刻は、`to_trip_id` の最初の出発時刻よりも前で、かつ近い時刻であるべきです。`to_trip_id` の便が翌運行日(service day)に運行される場合には、`from_trip_id` の最終到着時刻が `to_trip_id` の最初の出発時刻よりも後になることもあります。

通常のケースでは、便は1対1で連結することができますが、より複雑な便の継続を表現するために、1対n、n対1、またはn対nで連結することもできます。例えば、2つの列車便（下図の便Aおよび便B）が、共通の駅での車両連結操作後に1つの列車便（便C）に統合される場合があります。

- 1対nの継続においては、各 `to_trip_id` の `trips.service_id` は同一でなければなりません。
- n対1の継続においては、各 `from_trip_id` の `trips.service_id` は同一でなければなりません。
- n対nの継続は、上記の両方の制約を満たさなければなりません。
- `trip.service_id` がいかなる運行日(service day)においても重複しない限り、便は複数の異なる継続の一部として連結することができます。

<pre>
便A
───────────────────\
                    \    便C
                     ─────────────
便B                 /
───────────────────/
</pre>

### pathways.txt {: #pathwaystxt}


ファイル: **任意**

主キー (`pathway_id`)

[pathways.txt](#pathwaystxt) および [levels.txt](#levelstxt) ファイルは、ノードが位置を、エッジが構内通路(pathway)を表すグラフ表現を使用して、地下鉄駅や鉄道駅を記述します。

駅の出入口（`location_type=2` で表される位置）からプラットフォーム（`location_type=0` または空で表される位置）へ移動する際、乗客は通路、改札口、階段などの構内通路(pathway)として表されるエッジを通過します。汎用ノード（`location_type=3` で表されるノード）は、駅構内の通路を接続するために使用することができます。

構内通路(pathway)は、駅構内の内部アクセスグラフを完全に定義することを目的としています。駅内で構内通路が定義されている場合、データ利用者はその駅内のすべての関連する接続が記述されていると想定するべきです。ただし、`stops.txt` の任意フィールドである `stop_access` を使用して、停留所(stop)が街路ネットワークから直接アクセス可能か、または駅内の定義済み構内通路を通じてアクセスするかを明示的に定義することができます。したがって、以下のガイドラインが適用されます。

- 孤立した位置を作らない: 駅内のいずれかの位置に構内通路がある場合、その駅内のすべての位置に構内通路が存在するべきです。ただし、以下を除きます。
    - 乗車エリア（`location_type=4`、下記ガイドライン参照）を持つプラットフォーム
    - `stops.stop_access=1` の停留所（`location_type=0` または空）
- 乗車エリアを持つプラットフォームに構内通路を設定しない: 乗車エリア（`location_type=4`）を持つプラットフォーム（`location_type=0` または空）は、点ではなく親オブジェクトとして扱われます。この場合、プラットフォーム自体に構内通路を割り当ててはいけません。すべての構内通路は、プラットフォームの各乗車エリアに割り当てる必要があります。
- 閉鎖されたプラットフォームを作らない: 駅内のいずれかの位置に構内通路がある場合、各プラットフォーム（`location_type=0` または空）または乗車エリア（`location_type=4`）は、少なくとも1つの出入口（`location_type=2`）に構内通路の連鎖を通じて接続されていなければなりません。ただし、以下の場合を除きます。
    - 停留所（`location_type=0` または空）が `stops.stop_access=1` と明示的にマークされている場合。この場合、その停留所は街路ネットワークから直接アクセス可能であるとみなされます。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `pathway_id` | 一意のID | **必須** | 構内通路を識別します。システムがレコードの内部識別子として使用します。データセット内で一意でなければなりません。<br><br>異なる構内通路が同じ `from_stop_id` および `to_stop_id` の値を持つことがあります。<hr>_例: 2つのエスカレーターが逆方向に並んでいる場合、または階段とエレベーターが同じ場所を結んでいる場合、異なる `pathway_id` が同じ `from_stop_id` および `to_stop_id` の値を持つことがあります。_|
| `from_stop_id` | `stops.stop_id` を参照する外部ID | **必須** | 構内通路の始点となる位置。<br><br>`stop_id` には、プラットフォーム（`location_type=0` または空）、出入口（`location_type=2`）、汎用ノード（`location_type=3`）、または乗車エリア（`location_type=4`）を識別する値を含めなければなりません。<br><br>駅（`location_type=1`）を識別する `stop_id`、または `stop_access=1` の停留所（`location_type=0` または空）を識別する値は使用してはいけません。|
| `to_stop_id` | `stops.stop_id` を参照する外部ID | **必須** | 構内通路の終点となる位置。<br><br>`stop_id` には、プラットフォーム（`location_type=0` または空）、出入口（`location_type=2`）、汎用ノード（`location_type=3`）、または乗車エリア（`location_type=4`）を識別する値を含めなければなりません。<br><br>駅（`location_type=1`）を識別する `stop_id`、または `stop_access=1` の停留所（`location_type=0` または空）を識別する値は使用してはいけません。|
| `pathway_mode` | 列挙型 | **必須** | 指定された (`from_stop_id`, `to_stop_id`) ペア間の構内通路の種類。使用可能な値は以下の通りです。<br><br>`1` - 通路。<br>`2` - 階段。<br>`3` - 動く歩道。<br>`4` - エスカレーター。<br>`5` - エレベーター。<br>`6` - 改札口（または支払いゲート）: 通過するために支払い証明が必要な駅エリアに入る構内通路。改札口は、駅の有料エリアと無料エリアを分ける場合や、同一駅内の異なる支払いエリアを分ける場合があります。この情報は、乗客が不要な支払いを伴う経路（例: 地下鉄プラットフォームを通ってバス乗り場に行くような経路）を案内されないようにするために使用できます。<br>`7` - 出口ゲート: 有料エリアから無料エリアへ出る構内通路で、通過に支払い証明が不要なもの。|
| `is_bidirectional` | 列挙型 | **必須** | 構内通路の通行方向を示します。<br><br>`0` - 一方向通行。`from_stop_id` から `to_stop_id` のみ通行可能。<br>`1` - 双方向通行。両方向に通行可能。<br><br>出口ゲート（`pathway_mode=7`）は双方向にしてはいけません。|
| `length` | 非負の浮動小数点数 | 任意 | 構内通路の始点（`from_stop_id` で定義）から終点（`to_stop_id` で定義）までの水平距離（メートル単位）。<br><br>このフィールドは、通路（`pathway_mode=1`）、改札口（`pathway_mode=6`）、出口ゲート（`pathway_mode=7`）に対して推奨されます。|
| `traversal_time` | 正の整数 | 任意 | 構内通路の始点（`from_stop_id` で定義）から終点（`to_stop_id` で定義）まで歩行するのに必要な平均時間（秒単位）。<br><br>このフィールドは、動く歩道（`pathway_mode=3`）、エスカレーター（`pathway_mode=4`）、エレベーター（`pathway_mode=5`）に対して推奨されます。|
| `stair_count` | 非NULL整数 | 任意 | 構内通路の階段の段数。<br><br>正の `stair_count` は、乗客が `from_stop_id` から `to_stop_id` に向かって上ることを意味します。負の `stair_count` は、乗客が `from_stop_id` から `to_stop_id` に向かって下ることを意味します。<br><br>このフィールドは、階段（`pathway_mode=2`）に対して推奨されます。<br><br>段数が推定値しか提供できない場合、1階あたり約15段で近似することが推奨されます。|
| `max_slope` | 浮動小数点数 | 任意 | 構内通路の最大勾配比。使用可能な値は以下の通りです。<br><br>`0` または空 - 勾配なし。<br>`Float` - 構内通路の勾配比。上りは正、下りは負。<br><br>このフィールドは、通路（`pathway_mode=1`）および動く歩道（`pathway_mode=3`）でのみ使用するべきです。<hr>_例: 米国では、手動車椅子の最大勾配比は0.083（8.3%）であり、これは1m進むごとに0.083m（8.3cm）上昇することを意味します。_|
| `min_width` | 正の浮動小数点数 | 任意 | 構内通路の最小幅（メートル単位）。<br><br>最小幅が1メートル未満の場合、このフィールドの指定が推奨されます。|
| `signposted_as` | テキスト | 任意 | 乗客に見える実際の案内標識に記載された公開向けテキスト。<br><br>乗客への案内文（例: 「〜の案内標識に従ってください」など）を提供するために使用することができます。`signposted_as` のテキストは、標識に印字されている内容と正確に一致していなければなりません。<br><br>物理的な標識が多言語表記の場合、このフィールドは `feed_info.feed_lang` のフィールド定義における `stops.stop_name` の例に従って入力および翻訳することができます。|
| `reversed_signposted_as` | テキスト | 任意 | `signposted_as` と同様ですが、構内通路を `to_stop_id` から `from_stop_id` の方向に使用する場合の表記です。|

### levels.txt {: #levelstxt}

ファイル: **条件付き必須**

主キー (`level_id`)

駅構内の階層を記述します。[pathways.txt](#pathwaystxt) と併せて使用すると便利です。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `level_id` | 一意のID | **必須** | 駅構内の階層を識別します。 |
| `level_index` | 浮動小数点数 | **必須** | 階層の相対的な位置を示す数値インデックスです。<br><br>地上階はインデックス `0` とし、地上より上の階は正の値、地下階は負の値で示します。 |
| `level_name` | テキスト | 任意 | 建物や駅構内で乗客が目にする階層の名称です。<hr>_例: エレベーターで「メザニン」や「プラットフォーム」または「-1」へ移動します。_ |

### location_groups.txt {: #location_groupstxt}


ファイル: **任意**

主キー (`location_group_id`)

location group（ロケーショングループ）を定義します。これは、乗客が乗車または降車をリクエストできる停留所(stop)のグループです。

| フィールド名 | 型 | 出現 | 説明 |
| ---------- | ---- | ------------ | ----------- |
| `location_group_id` | 一意のID | **必須** | location group を識別します。IDは、すべての `stops.stop_id`、locations.geojson の `id`、および `location_groups.location_group_id` の値の中で一意でなければなりません。<br><br>location group は、乗客が乗車または降車をリクエストできる場所を示す停留所(stop)のグループです。 | 
| `location_group_name` | テキスト | 任意 | 乗客に表示される location group の名称です。 |

### location_group_stops.txt {: #location_group_stopstxt}

ファイル: **任意**

主キー（`*`）

stops.txt に含まれる停留所(stop)をロケーショングループに割り当てます。

| フィールド名 | 型 | 出現 | 説明 |
| ---------- | ---- | ------------ | ----------- |
| `location_group_id` | `location_groups.location_group_id` を参照する外部ID | **必須** | 1つまたは複数の `stop_id` が属するロケーショングループを識別します。同じ `stop_id` が複数の `location_group_id` に定義されることがあります。 | 
| `stop_id` | `stops.stop_id` を参照する外部ID | **必須** | ロケーショングループに属する停留所(stop)を識別します。 |

### locations.geojson {: #locationsgeojson}


ファイル: **任意**

乗客がオンデマンドサービスによって乗車または降車をリクエストできるゾーンを定義します。これらのゾーンは GeoJSON ポリゴンとして表現されます。

- このファイルは、[RFC 7946](https://tools.ietf.org/html/rfc7946) で定義されている GeoJSON 形式のサブセットを使用します。
- 各ポリゴンは、[OpenGIS Simple Features Specification, section 6.1.11](http://www.opengis.net/doc/is/sfa/1.2.1) の定義に従って有効でなければなりません。
- `locations.geojson` ファイルには `FeatureCollection` を含めなければなりません。
- `FeatureCollection` は、乗客が乗車または降車をリクエストできるさまざまな停留所等(stop)の位置を定義します。
- すべての GeoJSON `Feature` には `id` が必要です。`id` は、すべての `stops.stop_id`、locations.geojson の `id`、および `location_group_id` の値の中で一意でなければなりません。
- すべての GeoJSON `Feature` は、以下の表に従ってオブジェクトおよび関連するキーを持つべきです。

|  フィールド名 | 型 | 出現 | 説明 |
|  ------ | ------ | ------ | ------ |
| -&nbsp;`type` | 文字列 | **必須** | ロケーションの `"FeatureCollection"`。 |
| -&nbsp;`features` | 配列 | **必須** | ロケーションを記述する `"Feature"` オブジェクトのコレクション。 |
| &nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`type` | 文字列 | **必須** | `"Feature"` |
| &nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`id` | 文字列 | **必須** | ロケーションを識別します。ID は、すべての `stops.stop_id`、locations.geojson の `id`、および `location_groups.location_group_id` の値の中で一意でなければなりません。 |
| &nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`properties` | オブジェクト | **必須** | ロケーションのプロパティキー。 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`stop_name` | 文字列 | 任意 | 乗客に表示されるロケーションの名称を示します。 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`stop_desc` | 文字列 | 任意 | 乗客の位置把握を助けるためのロケーションの説明。 |
| &nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`geometry` | オブジェクト | **必須** | ロケーションのジオメトリ。 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`type` | 文字列 | **必須** | 次のいずれかの型でなければなりません:<br>-&nbsp;`"Polygon"`<br>-&nbsp;`"MultiPolygon"` |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\-&nbsp;`coordinates` | 配列 | **必須** | ロケーションのジオメトリを定義する地理座標（緯度および経度）。 |

### booking_rules.txt {: #booking_rulestxt}


ファイル: **任意**

主キー (`booking_rule_id`)

乗客がリクエストするサービスの予約ルールを定義します。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `booking_rule_id` | 一意のID | **必須** | ルールを識別します。 |
| `booking_type` | Enum | **必須** | どの程度前もって予約ができるかを示します。有効なオプションは以下の通りです:<br><br>`0` - リアルタイム予約。<br>`1` - 当日予約（事前通知あり）。<br>`2` - 前日までの予約。 |
| `prior_notice_duration_min` | 整数 | **条件付き必須** | 旅行の何分前までにリクエストを行う必要があるかを示します。<br><br>**条件付き必須**:<br>- `booking_type=1` の場合は **必須**。<br>- それ以外の場合は **禁止**。 |
| `prior_notice_duration_max` | 整数 | **条件付き禁止** | 旅行の何分前までに予約リクエストを行うことができるかの最大値を示します。<br><br>**条件付き禁止**:<br>- `booking_type=0` および `booking_type=2` の場合は **禁止**。<br>- `booking_type=1` の場合は任意。|
| `prior_notice_last_day` | 整数 | **条件付き必須** | 旅行の何日前までに予約リクエストを行う必要があるかを示します。<br><br>例: 「乗車は1日前の午後5時までに予約する必要がある」は `prior_notice_last_day=1` としてエンコードされます。<br><br>**条件付き必須**:<br>- `booking_type=2` の場合は **必須**。<br>- それ以外の場合は **禁止**。 |
| `prior_notice_last_time` | 時刻 | **条件付き必須** | 旅行の前日で予約リクエストを行うことができる最終時刻を示します。<br><br>例: 「乗車は1日前の午後5時までに予約する必要がある」は `prior_notice_last_time=17:00:00` としてエンコードされます。<br><br>**条件付き必須**:<br>- `prior_notice_last_day` が定義されている場合は **必須**。<br>- それ以外の場合は **禁止**。 |
| `prior_notice_start_day` | 整数 | **条件付き禁止** | 旅行の何日前から予約リクエストを行うことができるかの最も早い日を示します。<br><br>例: 「乗車は最も早く1週間前の午前0時から予約できる」は `prior_notice_start_day=7` としてエンコードされます。<br><br>**条件付き禁止**:<br>- `booking_type=0` の場合は **禁止**。<br> - `booking_type=1` で `prior_notice_duration_max` が定義されている場合は **禁止**。<br> - それ以外の場合は任意。 |
| `prior_notice_start_time` | 時刻 | **条件付き必須** | 旅行の最も早い日に予約リクエストを行うことができる最も早い時刻を示します。<br><br>例: 「乗車は最も早く1週間前の午前0時から予約できる」は `prior_notice_start_time=00:00:00` としてエンコードされます。<br><br>**条件付き必須**:<br>- `prior_notice_start_day` が定義されている場合は **必須**。<br>- それ以外の場合は **禁止**。 |
| `prior_notice_service_id` | `calendar.service_id` を参照する外部ID | **条件付き禁止** | `prior_notice_last_day` または `prior_notice_start_day` がどの運行日(service day)に基づいて数えられるかを示します。<br><br>例: 空の場合、`prior_notice_start_day=2` は2暦日前を意味します。平日のみ（祝日を除く）を含む `service_id` が定義されている場合、`prior_notice_start_day=2` は2営業日前を意味します。<br><br>**条件付き禁止**:<br> - `booking_type=2` の場合は任意。<br> - それ以外の場合は **禁止**。 |
| `message` | テキスト | 任意 | オンデマンドの乗車・降車を予約する際に、`stop_time` でサービスを利用する乗客に表示されるメッセージです。乗客がサービスを利用するために取るべき行動について、ユーザーインターフェース内で伝達される最小限の情報を提供することを目的としています。 |
| `pickup_message` | テキスト | 任意 | `message` と同様に機能しますが、オンデマンドの乗車のみを行う場合に使用されます。 |
| `drop_off_message` | テキスト | 任意 | `message` と同様に機能しますが、オンデマンドの降車のみを行う場合に使用されます。 |
| `phone_number` | 電話番号 | 任意 | 予約リクエストを行うために電話をかける番号です。 |
| `info_url` | URL | 任意 | 予約ルールに関する情報を提供するURLです。 |
| `booking_url` | URL | 任意 | 予約リクエストを行うことができるオンラインインターフェースまたはアプリのURLです。 |

### translations.txt {: #translationstxt}


ファイル: **任意**

主キー (`table_name`, `field_name`, `language`, `record_id`, `record_sub_id`, `field_value`)

複数の公用語を持つ地域では、交通事業者は通常、言語ごとに異なる名称やウェブページを持っています。これらの地域の乗客に最適な情報を提供するために、データセットに言語依存の値を含めることが有用です。

同じ値を翻訳するために、参照方法（`record_id`, `record_sub_id`）と `field_value` の両方が2つの異なる行で使用されている場合、(`record_id`, `record_sub_id`) による翻訳が優先されます。

| フィールド名 | 型 | 出現 | 説明 |
|  ------ | ------ | ------ | ------ |
|  `table_name` | Enum | **必須** | 翻訳対象のフィールドを含むテーブルを定義します。許可される値は以下の通りです:<br><br>- `agency`<br>- `stops`<br>- `routes`<br>- `trips`<br>- `stop_times`<br>- `pathways`<br>- `levels`<br>- `feed_info`<br>- `attributions`<br><br> GTFS に追加されるすべてのファイルは、上記のように `.txt` 拡張子を除いたファイル名と同じ `table_name` 値を持ちます。 |
|  `field_name` | Text | **必須** | 翻訳対象のフィールド名を指定します。型が `Text` のフィールドは翻訳可能です。型が `URL`、`Email`、`Phone number` のフィールドも、適切な言語のリソースを提供するために「翻訳」することができます。それ以外の型のフィールドは翻訳するべきではありません。 |
|  `language` | Language code | **必須** | 翻訳の言語を指定します。<br><br>`feed_info.feed_lang` と同じ言語の場合、そのフィールドの元の値は、特定の翻訳が存在しない言語で使用されるデフォルト値と見なされます（`default_lang` が別途指定されていない場合）。<hr>_例: スイスでは、公的に二言語の州にある都市は公式には “Biel/Bienne” と呼ばれますが、フランス語では “Bienne”、ドイツ語では “Biel” と呼ばれます。_ |
|  `translation` | Text または URL または Email または Phone number | **必須** | 翻訳された値です。 |
|  `record_id` | Foreign ID | **条件付き必須** | 翻訳対象のフィールドを含むレコードを定義します。`record_id` の値は、各テーブルの主キー属性および以下で定義されるテーブルの主キーの最初または唯一のフィールドでなければなりません:<br><br>- [agency.txt](#agencytxt) の場合は `agency_id`<br>- [stops.txt](#stopstxt) の場合は `stop_id`<br>- [routes.txt](#routestxt) の場合は `route_id`<br>- [trips.txt](#tripstxt) の場合は `trip_id`<br>- [stop_times.txt](#stop_timestxt) の場合は `trip_id`<br>- [pathways.txt](#pathwaystxt) の場合は `pathway_id`<br>- [levels.txt](#levelstxt) の場合は `level_id`<br>- [attributions.txt](#attributionstxt) の場合は `attribution_id`<br><br>上記に定義されていないテーブルのフィールドは翻訳するべきではありません。ただし、プロデューサーが公式仕様外の追加フィールドを設ける場合があり、これらの非公式フィールドは翻訳することができます。その場合の推奨される `record_id` の使用方法は以下の通りです:<br><br>- [calendar.txt](#calendartxt) の場合は `service_id`<br>- [calendar_dates.txt](#calendar_datestxt) の場合は `service_id`<br>- [fare_attributes.txt](#fare_attributestxt) の場合は `fare_id`<br>- [fare_rules.txt](#fare_rulestxt) の場合は `fare_id`<br>- [shapes.txt](#shapestxt) の場合は `shape_id`<br>- [frequencies.txt](#frequenciestxt) の場合は `trip_id`<br>- [transfers.txt](#transferstxt) の場合は `from_stop_id`<br><br>条件付き必須:<br>- `table_name` が `feed_info` の場合は **禁止**。<br>- `field_value` が定義されている場合は **禁止**。<br>- `field_value` が空の場合は **必須**。 |
|  `record_sub_id` | Foreign ID | **条件付き必須** | 一意の ID を持たないテーブルにおいて、翻訳対象のフィールドを含むレコードを特定するために使用します。したがって、`record_sub_id` の値は以下のテーブルで定義される二次 ID です:<br><br>- [agency.txt](#agencytxt): なし<br>- [stops.txt](#stopstxt): なし<br>- [routes.txt](#routestxt): なし<br>- [trips.txt](#tripstxt): なし<br>- [stop_times.txt](#stop_timestxt): `stop_sequence`<br>- [pathways.txt](#pathwaystxt): なし<br>- [levels.txt](#levelstxt): なし<br>- [attributions.txt](#attributionstxt): なし<br><br>上記に定義されていないテーブルのフィールドは翻訳するべきではありません。ただし、プロデューサーが公式仕様外の追加フィールドを設ける場合があり、これらの非公式フィールドは翻訳することができます。その場合の推奨される `record_sub_id` の使用方法は以下の通りです:<br><br>- [calendar.txt](#calendartxt): なし<br>- [calendar_dates.txt](#calendar_datestxt): `date`<br>- [fare_attributes.txt](#fare_attributestxt): なし<br>- [fare_rules.txt](#fare_rulestxt): `route_id`<br>- [shapes.txt](#shapestxt): なし<br>- [frequencies.txt](#frequenciestxt): `start_time`<br>- [transfers.txt](#transferstxt): `to_stop_id`<br><br>条件付き必須:<br>- `table_name` が `feed_info` の場合は **禁止**。<br>- `field_value` が定義されている場合は **禁止**。<br>- `table_name=stop_times` かつ `record_id` が定義されている場合は **必須**。 |
|  `field_value` | Text または URL または Email または Phone number | **条件付き必須** | `record_id` および `record_sub_id` を使用して翻訳対象のレコードを定義する代わりに、このフィールドを使用して翻訳対象の値を定義することができます。この場合、`table_name` および `field_name` で特定されるフィールドが `field_value` に定義された値と完全に一致する場合に翻訳が適用されます。<br><br>フィールドは `field_value` に定義された値と**完全に**一致していなければなりません。値の一部のみが一致する場合、翻訳は適用されません。<br><br>同じレコードに対して2つの翻訳ルール（1つは `field_value`、もう1つは `record_id`）が一致する場合、`record_id` のルールが優先されます。<br><br>条件付き必須:<br>- `table_name` が `feed_info` の場合は **禁止**。<br>- `record_id` が定義されている場合は **禁止**。<br>- `record_id` が空の場合は **必須**。 |

### feed_info.txt {: #feed_infotxt}


ファイル: **条件付き必須**

主キー（なし）

このファイルには、データセットが記述するサービスそのものではなく、データセット自体に関する情報が含まれます。場合によっては、データセットの発行者が、いずれの事業者とも異なる組織であることがあります。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `feed_publisher_name` | Text | **必須** | データセットを発行する組織の正式名称です。これは `agency.agency_name` の値のいずれかと同じである場合があります。 |
| `feed_publisher_url` | URL | **必須** | データセットを発行する組織のウェブサイトの URL です。これは `agency.agency_url` の値のいずれかと同じである場合があります。 |
| `feed_lang` | 言語コード | **必須** | このデータセット内のテキストで使用される既定の言語です。この設定により、GTFS 利用者はデータセットに対して大文字小文字の規則やその他の言語固有の設定を選択することができます。テキストを既定の言語以外に翻訳する必要がある場合は、`translations.txt` ファイルを使用することができます。<br><br>データセット内の元のテキストが複数の言語で構成されている場合、既定の言語は多言語であってもかまいません。その場合、`feed_lang` フィールドには ISO 639-2 で定義された言語コード `mul` を指定し、データセット内で使用されている各言語に対して `translations.txt` に翻訳を提供する必要があります。データセット内のすべての元のテキストが同一言語である場合は、`mul` を使用してはいけません。<hr>_例: スイスのような多言語国のデータセットを考えます。`stops.stop_name` フィールドには、各停留所の地理的な位置における主要言語に従って停留所名が入力されています。例えば、フランス語圏の都市ジュネーブでは `Genève`、ドイツ語圏のチューリッヒでは `Zürich`、二言語都市ビール/ビエンヌでは `Biel/Bienne` です。この場合、データセットの `feed_lang` は `mul` とし、`translations.txt` に以下の翻訳を提供します。ドイツ語では `Genf`、`Zürich`、`Biel`、フランス語では `Genève`、`Zurich`、`Bienne`、イタリア語では `Ginevra`、`Zurigo`、`Bienna`、英語では `Geneva`、`Zurich`、`Biel/Bienne` です。_ |
| `default_lang` | 言語コード | 任意 | データ利用者が乗客の言語を知らない場合に使用すべき言語を定義します。多くの場合、`en`（英語）になります。 |
| `feed_start_date` | 日付 | 推奨 | データセットは、`feed_start_date` の日付の始まりから `feed_end_date` の日付の終わりまでの期間において、完全で信頼できる運行スケジュール情報を提供します。どちらの日付も、利用できない場合は空欄にすることができます。両方の日付が指定されている場合、`feed_end_date` は `feed_start_date` より前であってはいけません。データセット提供者は、この期間外のスケジュールデータを提供して将来の運行予定を示すことが推奨されますが、データセット利用者はその情報が公式なものではないことを考慮して扱うべきです。`feed_start_date` または `feed_end_date` が [calendar.txt](#calendartxt) および [calendar_dates.txt](#calendar_datestxt) で定義された有効なカレンダー日を超える場合、データセットは `feed_start_date` から `feed_end_date` の範囲内で、有効なカレンダー日に含まれない日付には運行がないことを明示的に示しています。 |
| `feed_end_date` | 日付 | 推奨 | （上記参照） |
| `feed_version` | Text | 推奨 | 現在の GTFS データセットのバージョンを示す文字列です。GTFS を利用するアプリケーションはこの値を表示し、データセット発行者が最新のデータセットが反映されているかどうかを確認するのに役立てることができます。 |
| `feed_contact_email` | Email | 任意 | GTFS データセットおよびデータ公開に関する連絡用のメールアドレスです。`feed_contact_email` は GTFS を利用するアプリケーション向けの技術的な連絡先です。顧客サービスの連絡先情報は [agency.txt](#agencytxt) で提供してください。`feed_contact_email` または `feed_contact_url` の少なくとも一方を提供することが推奨されます。 |
| `feed_contact_url` | URL | 任意 | GTFS データセットおよびデータ公開に関する連絡用の情報、ウェブフォーム、サポートデスク、またはその他のツールの URL です。`feed_contact_url` は GTFS を利用するアプリケーション向けの技術的な連絡先です。顧客サービスの連絡先情報は [agency.txt](#agencytxt) で提供してください。`feed_contact_url` または `feed_contact_email` の少なくとも一方を提供することが推奨されます。 |

### attributions.txt {: #attributionstxt}

ファイル: **任意**

主キー (`attribution_id`)

このファイルは、データセットに適用される帰属情報を定義します。

| フィールド名 | 型 | 出現 | 説明 |
| ------ | ------ | ------ | ------ |
| `attribution_id` | 一意のID | 任意 | データセットまたはその一部に対する帰属情報を識別します。これは主に翻訳に役立ちます。 |
| `agency_id` | `agency.agency_id` を参照する外部ID | 任意 | 帰属情報が適用される事業者です。<br><br>`agency_id`、`route_id`、または `trip_id` のいずれか1つの帰属情報が定義されている場合、他のものは空でなければなりません。いずれも指定されていない場合、帰属情報はデータセット全体に適用されます。 |
| `route_id` | `routes.route_id` を参照する外部ID | 任意 | `agency_id` と同様に機能しますが、帰属情報はルート・路線系統(route)に適用されます。同じルートに複数の帰属情報を適用することができます。 |
| `trip_id` | `trips.trip_id` を参照する外部ID | 任意 | `agency_id` と同様に機能しますが、帰属情報は便(trip)に適用されます。同じ便に複数の帰属情報を適用することができます。 |
| `organization_name` | テキスト | **必須** | データセットの帰属先となる組織の名称です。 |
| `is_producer` | 列挙型 | 任意 | 組織の役割がプロデューサーであることを示します。有効な値は以下の通りです。<br><br>`0` または空欄 - 組織はこの役割を持ちません。<br>`1` - 組織はこの役割を持ちます。<br><br>`is_producer`、`is_operator`、または `is_authority` のうち少なくとも1つは `1` に設定するべきです。 |
| `is_operator` | 列挙型 | 任意 | `is_producer` と同様に機能しますが、組織の役割がオペレーターであることを示します。 |
| `is_authority` | 列挙型 | 任意 | `is_producer` と同様に機能しますが、組織の役割が運輸当局であることを示します。 |
| `attribution_url` | URL | 任意 | 組織のURLです。 |
| `attribution_email` | メールアドレス | 任意 | 組織のメールアドレスです。 |
| `attribution_phone` | 電話番号 | 任意 | 組織の電話番号です。 |
