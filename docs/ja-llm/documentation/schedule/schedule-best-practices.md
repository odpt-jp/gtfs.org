# GTFS Schedule ベストプラクティス {: #gtfs-schedule-best-practices}

これは、[General Transit Feed Specification (GTFS)](../reference) において公共交通サービスを記述するための推奨プラクティスです。これらのプラクティスは、[GTFS ベストプラクティス作業部会](#gtfs-best-practices-working-group) のメンバーの経験および [アプリケーション固有の GTFS プラクティス推奨事項](http://www.transitwiki.org/TransitWiki/index.php/Best_practices_for_creating_GTFS) に基づいてまとめられたものです。

さらに詳しい背景については、[よくある質問 (FAQ)](#frequently-asked-questions-faq) をご覧ください。

## ドキュメント構成 {: #document-structure}

実践事項は、主に次の4つのセクションに整理されています。

* __[データセット公開および一般的な実践](#dataset-publishing-general-practices)：__ これらの実践は、GTFSデータセット全体の構造およびGTFSデータセットの公開方法に関するものです。
* __[ファイル別の推奨実践](#practice-recommendations-organized-by-file)：__ 推奨事項は、公式のGTFSリファレンスとの対応を容易にするため、GTFS内のファイルおよびフィールドごとに整理されています。
* __[ケース別の推奨実践](#practice-recommendations-organized-by-case)：__ ループ路線などの特定のケースでは、複数のファイルやフィールドにわたって実践を適用する必要がある場合があります。そのような推奨事項は、このセクションにまとめられています。

## データセットの公開および一般的な運用慣行 {: #dataset-publishing-general-practices}

* データセットは、zipファイル名を含む公開かつ永続的なURLで公開するべきです（例: www.agency.org/gtfs/gtfs.zip）。理想的には、ファイルにアクセスするためにログインを必要とせず、直接ダウンロード可能なURLとすることで、利用するソフトウェアアプリケーションによるダウンロードを容易にするべきです。GTFSデータセットをオープンにダウンロード可能とすることが推奨され（また最も一般的な運用です）、データ提供者がライセンスやその他の理由でGTFSへのアクセスを制御する必要がある場合は、自動ダウンロードを容易にするためにAPIキーを使用してGTFSデータセットへのアクセスを制御することが推奨されます。
* GTFSデータは反復的に公開され、安定した場所にある単一のファイルが常に交通事業者（または複数の事業者）の最新の公式運行情報を含むようにします。
* 可能な限り、データの反復間で `stop_id`、`route_id`、および `agency_id` の永続的な識別子(idフィールド)を維持してください。
* 1つのGTFSデータセットには、現在および今後の運行情報（「マージされたデータセット」と呼ばれることもあります）を含めるべきです。Google transitfeedツールの[merge関数](https://github.com/google/transitfeed/wiki/Merge)を使用して、2つの異なるGTFSフィードからマージされたデータセットを作成することができます。
    * 公開されるGTFSデータセットは、常に少なくとも今後7日間は有効であるべきであり、理想的には、運行事業者がスケジュールの継続運行に自信を持てる期間まで有効であるべきです。
    * 可能であれば、GTFSデータセットは少なくとも今後30日間の運行をカバーするべきです。
* 古い運行情報（有効期限切れのカレンダー）はフィードから削除してください。
* 7日以内に運行変更が発効する場合は、静的なGTFSデータセットではなく、[GTFS-realtime](../../realtime/reference)フィード（運行情報または便の更新）を通じてこの運行変更を表現してください。
* GTFSデータをホスティングするWebサーバーは、ファイルの更新日時を正しく報告するように設定しなければなりません（[HTTP/1.1 - Request for Comments 2616](https://tools.ietf.org/html/rfc2616#section-14.29)のセクション14.29を参照）。

## ファイル別の推奨実践事項 {: #practice-recommendations-organized-by-file}

このセクションでは、[GTFS リファレンス](../reference)に沿って、ファイルおよびフィールドごとに整理された実践事項を示します。

### すべてのファイル {: #all-files}


| フィールド名 | 推奨事項 |
| --- | --- |
| Mixed Case | すべての利用者向けテキスト文字列（停留所名、路線名、行先表示などを含む）は、すべて大文字(ALL CAPS)ではなく、Mixed Case（大文字・小文字混在）を使用するべきです。これは、小文字を表示できるディスプレイにおいて、地名の大文字・小文字の使用に関する地域の慣習に従うものとします。 |
| | 例: |
| | Brighton Churchill Square |
| | Villiers-sur-Marne |
| | Market Street |
| 略語 | フィード全体で、名称やその他のテキストに略語（例: Street を St. とするなど）を使用することは避けるべきです。ただし、場所が略称で一般的に呼ばれている場合（例: “JFK Airport”）はこの限りではありません。略語は、スクリーンリーダーソフトウェアや音声ユーザーインターフェースによるアクセシビリティに問題を引き起こす可能性があります。利用側のソフトウェアは、完全な単語を略語に変換して表示するように設計することができますが、略語から完全な単語に変換する場合は、誤りが発生するリスクが高くなります。 |

### agency.txt {: #agencytxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| `agency_phone` | 顧客サービス用の電話番号が存在しない場合を除き、含めるべきです。 |
| `agency_email` | 顧客サービス用のメールアドレスが存在しない場合を除き、含めるべきです。 |
| `agency_fare_url` | 事業者が完全に運賃無料でない限り、含めるべきです。 |

__例:__

- バスサービスはいくつかの小規模なバス事業者によって運行されています。しかし、スケジューリングとチケット発行を担当し、利用者の視点からバスサービス全体の責任を負う大きな事業者が1つ存在します。この大きな事業者をフィード内の agency として定義するべきです。データが内部的に複数の小規模バス運行会社に分割されていたとしても、フィード内で定義される agency は1つだけであるべきです。
  
- フィード提供者がチケットポータルを運営していますが、実際にサービスを運行し、利用者から責任を持つと認識されている複数の事業者が存在します。この場合、実際にサービスを運行している事業者をフィード内の agency として定義するべきです。

### stops.txt {: #stopstxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| `stop_name` | 公開された停留所名が存在しない場合は、フィード全体で一貫した停留所命名規則に従うべきです。 | |
| | デフォルトでは、`stop_name` に「Station」や「Stop」などの一般的または冗長な単語を含めるべきではありませんが、いくつかの例外が認められます。<ul><li>それが実際に名称の一部である場合（Union Station、Central Station など）</li><li>`stop_name` があまりに一般的である場合（都市名など）。この場合、「Station」や「Terminal」などの単語を加えることで意味が明確になります。</li></ul> |
| `stop_lat` & `stop_lon` | 停留所の位置情報は可能な限り正確であるべきです。実際の停留所位置と比較して、誤差は4メートル以内でなければなりません。 |
| | 停留所の位置は、乗客が乗車する歩行者通行エリアのすぐ近く（すなわち、正しい道路側）に配置するべきです。 |
| | 停留所の位置が複数のデータフィード間で共有されている場合（例：2つの事業者がまったく同じ停留所／乗車施設を使用している場合）、両方の停留所でまったく同じ `stop_lat` および `stop_lon` を使用して、共有されていることを示すべきです。 |
| `parent_station` & `location_type` | 多くの駅やターミナルには複数の乗車施設があります（交通モードによっては、バスベイ、プラットフォーム、桟橋、ゲートなどと呼ばれます）。このような場合、フィード作成者は駅、乗車施設（子停留所とも呼ばれます）、およびそれらの関係を記述するべきです。<ul><li>駅またはターミナルは、`stops.txt` 内で `location_type = 1` のレコードとして定義するべきです。</li><li>各乗車施設は、`location_type = 0` の停留所として定義し、`parent_station` フィールドでその乗車施設が属する駅の `stop_id` を参照するべきです。</li></ul> |
| | 駅および子停留所の命名にあたっては、乗客に広く認識されており、駅および乗車施設（バスベイ、プラットフォーム、桟橋、ゲートなど）を識別しやすい名称を設定するべきです。<table class='example'><thead><tr><th>親駅名</th><th>子停留所名</th></tr></thead><tbody><tr><td>Chicago Union Station</td><td>Chicago Union Station Platform 19</td></tr><tr><td>San Francisco Ferry Building Terminal</td><td>San Francisco Ferry Building Terminal Gate E</td></tr><tr><td>Downtown Transit Center</td><td>Downtown Transit Center Bay B</td></tr></tbody></table> |

### routes.txt {: #routestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| `route_long_name` | 仕様書の定義: <q>この名称は一般的に <code>route_short_name</code> よりも説明的であり、多くの場合、ルートの目的地または停留所(stop)を含みます。<code>route_short_name</code> または <code>route_long_name</code> の少なくとも一方を指定しなければなりません。適切であれば両方を指定することもできます。ルートに長い名称がない場合は、<code>route_short_name</code> を指定し、このフィールドの値として空文字列を使用してください。</q><br>以下に長い名称の種類の例を示します。<table class='example'><thead><tr><th colspan='3'>主要な運行経路または回廊</th></tr><tr><th>ルート名</th><th>形式</th><th>事業者</th></tr></thead><tbody><tr><td><a href='https://www.sfmta.com/getting-around/transit/routes-stops/n-judah'>“N”/“Judah”</a></td><td><code>route_short_name</code>/<br><code>route_long_name</code></td><td><a href='https://www.sfmta.com/'>Muni</a>（サンフランシスコ）</td></tr><tr><td><a href='https://trimet.org/schedules/r006.htm'>“6“/“ML King Jr Blvd“</a></td><td><code>route_short_name</code>/<br><code>route_long_name</code></td><td><a href='https://trimet.org/'>TriMet</a>（オレゴン州ポートランド）</td></tr><tr><td><a href='http://www.ratp.fr/informer/pdf/orienter/f_plan.php?nompdf=m6'>“6”/“Nation - Étoile”</a></td><td><code>route_short_name</code>/<br><code>route_long_name</code></td><td><a href='http://www.ratp.fr/'>RATP</a>（フランス・パリ）</td></tr><tr><td><a href='http://www.bvg.de/images/content/linienverlaeufe/LinienverlaufU2.pdf'>“U2”-“Pankow – Ruhleben”</a></td><td><code>route_short_name</code>-<br><code>route_long_name</code></td><td><a href='http://www.bvg.de/'>BVG</a>（ドイツ・ベルリン）</td></tr></tbody></table><table class='example'><thead><tr><th>サービスの説明</th></tr></thead><tbody><tr><td><a href='https://128bc.org/schedules/rev-bus-hartwell-area/'>“Hartwell Area Shuttle“</a></td></tr></tbody></table>        
| | `route_long_name` には `route_short_name` を含めるべきではありません。 |
| | `route_long_name` を入力する際には、サービスのブランド名を含む完全な名称を記載してください。例:<table class='example'><thead><tr><th>サービスブランド</th><th>推奨事項</th><th>例</th></tr></thead><tbody><tr><td>"MAX Light Rail"<br>TriMet（オレゴン州ポートランド）</td><td><code>route_long_name</code> にはブランド名（MAX）と特定のルート名を含めるべきです。</td><td>"MAX Red Line" "MAX Blue Line"</td></tr><tr><td>"Rapid Ride"<br>ABQ Ride（ニューメキシコ州アルバカーキ）</td><td><code>route_long_name</code> にはブランド名（Rapid Ride）と特定のルート名を含めるべきです。</td><td>"Rapid Ride Red Line"<br>"Rapid Ride Blue Line"</td></tr></tbody></table>
| `route_id` | 同一の名称を持つルート上のすべての便(trip)は、同じ `route_id` を参照するべきです。<li>ルートの異なる方向を別々の `route_id` に分けてはいけません。</li><li>ルートの異なる運行時間帯を別々の `route_id` に分けてはいけません。すなわち、「Downtown AM」および「Downtown PM」サービスのために `routes.txt` に異なるレコードを作成してはいけません。</li> |
| | ルートグループに明確に名前の異なる枝（例: 1A および 1B）が含まれる場合は、`route_short_name` および `route_long_name` を決定するために [branches](#branches) の推奨事項に従ってください。 |
| `route_color` および `route_text_color` | 標識、印刷物、オンラインの利用者向け情報と一貫しているべきです（他の場所に存在しない場合は含めるべきではありません）。 |

### trips.txt {: #tripstxt}


* __ループ系統の特別なケースを参照してください:__ ループ系統とは、便(trip)が同じ停留所(stop)で始まり、同じ停留所(stop)で終わるケースを指します。これに対し、線形系統は2つの異なる終点を持ちます。ループ系統は、特定の手順に従って記述しなければなりません。[ループ系統のケースを参照してください。](#loop-routes)
* __ラッソ系統の特別なケースを参照してください:__ ラッソ系統とは、線形とループの形状を組み合わせたもので、ルートの一部のみで車両がループを走行するものです。ラッソ系統は、特定の手順に従って記述しなければなりません。[ラッソ系統のケースを参照してください。](#lasso-routes)

| フィールド名 | 推奨事項 |
| --- | --- |
| `trip_headsign` | `trip_headsign` または `stop_headsign` フィールドに、`route_short_name` や `route_long_name` に一致するルート名を記載してはいけません。 |
| | 行先、方向、または便(trip)を区別するために使用されるその他の表示テキストを、車両の行先表示(headsign)に示される内容として含めるべきです。GTFS データセットで提供される行先表示(headsign)を決定する際の主な目的は、車両に表示される方向情報との一貫性を保つことです。この主目的を損なわない限りにおいて、その他の情報を含めることができます。便(trip)の途中で行先表示(headsign)が変わる場合は、`trip_headsign` を `stop_times.stop_headsign` で上書きしてください。以下にいくつかのケースにおける推奨事項を示します。 |
| | <table class="example"><thead><tr><th>ルートの説明</th><th>推奨事項</th></tr></thead><tbody><tr><td>2A. 行先のみ</td><td>終点の行先を記載します。例: "Transit Center"、"Portland City Center"、または "Jantzen Beach"。</td></tr><tr><td>2B. 経由地を含む行先</td><td>&lt;行先&gt; via &lt;経由地&gt; 例: “Highgate via Charing Cross”。車両が経由地を通過した後、乗客に表示される行先表示(headsign)から経由地が削除される場合は、<code>stop_times.stop_headsign</code> を使用して更新後の行先表示を設定してください。</td></tr><tr><td>2C. 地域名とその地域内の停留所</td><td>目的地の都市または地区内に複数の停留所(stop)がある場合は、目的地の都市に到達した時点で <code>stop_times.stop_headsign</code> を使用してください。</td></tr><tr><td>2D. 方向のみ</td><td>“Northbound”（北行き）、“Inbound”（内回り）、“Clockwise”（時計回り）などの方向を示す語を使用してください。</td></tr><tr><td>2E. 方向と行先</td><td>&lt;方向&gt; to &lt;終点名&gt; 例: “Southbound to San Jose”。</td></tr><tr><td>2F. 方向、経由地、行先</td><td>&lt;方向&gt; via &lt;経由地&gt; to &lt;行先&gt; 例: “Northbound via Charing Cross to Highgate”。</td></tr></tbody></table> |
| | 行先表示(headsign)を “To” または “Towards” という語で始めてはいけません。 |
| `direction_id` | データセット全体で値 0 および 1 を一貫して使用してください。すなわち、<ul><li>1 = Red ルートの外回りの場合、1 = Green ルートの外回りとする</li><li>1 = Route X の北行きの場合、1 = Route Y の北行きとする</li><li>1 = Route X の時計回りの場合、1 = Route Y の時計回りとする</li></ul> |
| `bikes_allowed` | フェリー便(trip)については、自転車の持ち込みが可能かどうかを明示的に指定してください。データが欠落しているためにフェリー便を避けると、大きな迂回が発生することが多いためです。 |

### stop_times.txt {: #stop_timestxt}


ループ系統(Loop routes): ループ系統は特別な `stop_times` の考慮が必要です。（参照: [ループ系統のケース](#loop-routes)）

| フィールド名 | 推奨事項 |
| --- | --- |
| `pickup_type` & `drop_off_type` | 乗客サービスを提供しない回送便（non-revenue / deadhead trips）は、すべての `stop_times` 行で `pickup_type` および `drop_off_type` の値を `1` に設定する必要があります。 |
| | 収益便（revenue trips）において、運行実績の監視などのための内部的な「時刻管理点（timing points）」や、乗客が乗車できない車庫などの場所は、`pickup_type = 1`（乗車不可）および `drop_off_type = 1`（降車不可）としてマークするべきです。 |
| `arrival_time` & `departure_time` | `arrival_time` および `departure_time` フィールドは、可能な限り時刻値を指定するべきです。これには、拘束力のない推定時刻や時刻管理点間の補間時刻も含まれます。 |
| `stop_headsign` | 一般的に、行先表示(headsign)の値は駅の案内表示と一致しているべきです。<br><br>以下のケースでは、「Southbound」という表記は駅の案内表示で使用されていないため、利用者を誤解させるおそれがあります。 |
| | <table class="example"><thead><tr><th colspan="2">ニューヨーク市（NYC）において、2系統が南行きの場合:</th></tr><tr><th><code>stop_times.txt</code> の行:</th><th>使用する <code>stop_headsign</code> の値:</th></tr></thead><tbody><tr><td>マンハッタンに到達するまで</td><td><code>Manhattan & Brooklyn</code></td></tr><tr><td>ダウンタウンに到達するまで</td><td><code>Downtown & Brooklyn</code></td></tr><tr><td>ブルックリンに到達するまで</td><td><code>Brooklyn</code></td></tr><tr><td>ブルックリンに到達した後</td><td><code>Brooklyn (New Lots Av)</code></td></tr></tbody></table> |
| | <table class="example"><thead><tr><th colspan="2">ボストンにおいて、レッドライン南行き（Braintree支線）の場合:</th></tr><tr><th><code>stop_times.txt</code> の行:</th><th>使用する <code>stop_headsign</code> の値:</th></tr></thead><tbody><tr><td>ダウンタウンに到達するまで</td><td><code>Inbound to Braintree</code></td></tr><tr><td>ダウンタウンに到達した後</td><td><code>Braintree</code></td></tr><tr><td>ダウンタウン通過後</td><td><code>Outbound to Braintree</code></td></tr></tbody></table> |

### calendar.txt {: #calendartxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | 仕様には採用されていませんが、`calendar.service_name` フィールドを含めることで、GTFS の可読性を高めることができます。 |

### calendar_dates.txt {: #calendar_datestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | `calendar.service_name` フィールドを含めることで、仕様には採用されていないものの、GTFS の可読性を高めることができます。|

### fare_attributes.txt {: #fare_attributestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| | 運賃システムを正確にモデル化できない場合は、さらなる混乱を避けるために空欄のままにしてください。 |
| | 運賃（`fare_attributes.txt` および `fare_rules.txt`）を含め、可能な限り正確にモデル化してください。運賃を正確にモデル化できない特殊なケースでは、乗客が運賃不足で乗車しようとしないように、実際よりも高い運賃として表現するべきです。大部分の運賃を正しくモデル化できない場合は、フィードに運賃情報を含めないでください。 |

### fare_rules.txt {: #fare_rulestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | 運賃システムを正確にモデル化できない場合は、さらなる混乱を避けるために空欄のままにしてください。 |
| | 運賃（`fare_attributes.txt` および `fare_rules.txt`）を含め、可能な限り正確にモデル化してください。運賃を正確にモデル化できない特殊なケースでは、乗客が運賃不足で乗車しようとしないように、実際よりも高い運賃として表現するべきです。大部分の運賃を正しくモデル化できない場合は、フィードに運賃情報を含めないでください。 |

### shapes.txt {: #shapestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | 共有される経路（例：Route 1 と Route 2 が同じ道路または線路の区間を走行する場合）については、共有部分の経路が完全に一致することが理想的です。これは、高品質な交通地図作成を促進するのに役立ちます。 <!-- (77) -->
| | 経路は、車両が走行する道路の中心線に沿うべきです。これは、専用レーンがない場合は道路の中心線、または車両の進行方向側の車線の中心線のいずれかになります。<br><br>経路は、縁石停留所、プラットフォーム、または乗車位置に「ジグザグ」してはいけません。 |
| `shape_dist_traveled` | `shape_dist_traveled` フィールドは、事業者が `stop_times.txt` ファイル内の停留所がそれぞれの shape にどのように対応するかを正確に指定することを可能にします。`shape_dist_traveled` フィールドに一般的に使用される値は、車両が走行した shape の始点からの距離（いわば走行距離計の値のようなもの）です。<li>`shapes.txt` 内のルート経路は、便が停車する停留所位置から 100 メートル以内であるべきです。</li><li>`shapes.txt` に不要な点が含まれないように経路を簡略化してください（すなわち、直線区間上の余分な点を削除します。線の簡略化問題に関する議論を参照してください）。</li>

### frequencies.txt {: #frequenciestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | `frequencies.txt` で参照される便(trip)については、実際の停車時刻(stop time)は無視され、停留所間の走行時間間隔のみが重要となります。明確さおよび人間の可読性のために、`frequencies.txt` で参照される便の最初の停車時刻(stop time)は 00:00:00（最初の `arrival_time` の値が 00:00:00）から始まることが推奨されます。 |
| `block_id` | 頻度ベースの便に対して指定することができます。 |

### transfers.txt {: #transferstxt}

`transfers.transfer_type` は、[GTFS で定義されている](../reference/#transferstxt)4つの値のいずれかを取ることができます。これらの `transfer_type` の定義は、以下に _イタリック体_ で示す GTFS 仕様書からの引用であり、実務上の推奨事項を付記しています。

| フィールド名 | 推奨事項 |
| --- | --- |
| `transfer_type` | <q>0 または (空): これはルート間の推奨される乗り換え地点です。</q><br>複数の乗り換え機会があり、その中により優れた選択肢（例: 追加の設備を備えた交通センターや、隣接または接続された乗車施設／プラットフォームを持つ駅）が含まれる場合は、推奨される乗り換え地点を指定してください。 |
| | <q>1: これは2つのルート間の時刻調整された乗り換え地点です。出発する車両は到着する車両を待つことが期待され、乗客がルート間を乗り換えるのに十分な時間が確保されています。</q><br>この乗り換えタイプは、確実に乗り換えを行うために必要な間隔を上書きします。例えば、Google Maps では、乗客が安全に乗り換えを行うために3分が必要であると仮定しています。他のアプリケーションでは、異なるデフォルト値を仮定している場合があります。 |
| | <q>2: この乗り換えには、接続を確実に行うために到着と出発の間に最小限の時間が必要です。乗り換えに必要な時間は <code>min_transfer_time</code> で指定されます。</q><br>停留所間の移動時間を増加させる障害物やその他の要因がある場合は、最小乗り換え時間を指定してください。 |
| | <q>3: この場所ではルート間の乗り換えはできません。</q><br>物理的な障害がある場合、または困難な道路横断や歩行ネットワークの欠如により乗り換えが安全でない、または複雑になる場合は、この値を指定してください。 |
| | 座席に座ったままの（ブロック）乗り換えが便間で許可されている場合、到着便の最終停留所は出発便の最初の停留所と同一でなければなりません。 |

## ケース別の実践推奨事項 {: #practice-recommendations-organized-by-case}

このセクションでは、複数のファイルやフィールドに影響を及ぼす特定のケースについて説明します。

### ループ系統 {: #loop-routes}

ループ系統では、車両の便(trip)は同じ場所（しばしば交通センターや乗換センター）で開始し、終了します。車両は通常、連続的に運行し、乗客は車両がループを続ける間、車内に留まることができます。

<img src="https://raw.githubusercontent.com/MobilityData/GTFS_Schedule_Best-Practices/master/en/loop-route.svg" width=200px style="display: block; margin-left: auto; margin-right: auto;">

そのため、乗客に車両の進行方向を示すために、行先表示(headsign)の推奨事項を適用するべきです。

進行方向の変化を示すために、`stop_times.txt` ファイル内で `stop_headsigns` を指定してください。`stop_headsign` は、その停留所(stop)から出発する便の進行方向を表します。便の各停留所に `stop_headsigns` を追加することで、便の途中で行先表示(headsign)の情報を変更することができます。

2つの終点間を往復運行する系統（同じバスが行き来する場合など）については、`stop_times.txt` ファイル内で1つの循環便を定義してはいけません。その代わりに、便を2つの異なる方向の便に分割してください。
  
__循環便のモデリング例:__

- 各停留所で行先表示(headsign)が変化する循環便

| trip_id | arrival_time | departure_time | stop_id | stop_sequence | stop_headsign |
|---------|--------------|----------------|---------|---------------|---------------|
| trip_1  | 06:10:00     | 06:10:00       | stop_A  | 1             | "B"           |
| trip_1  | 06:15:00     | 06:15:00       | stop_B  | 2             | "C"           |
| trip_1  | 06:20:00     | 06:20:00       | stop_C  | 3             | "D"           |
| trip_1  | 06:25:00     | 06:25:00       | stop_D  | 4             | "E"           |
| trip_1  | 06:30:00     | 06:30:00       | stop_E  | 5             | "A"           |
| trip_1  | 06:35:00     | 06:35:00       | stop_A  | 6             | ""            |
 
- 2種類の行先表示(headsign)を持つ循環便

| trip_id | arrival_time | departure_time | stop_id | stop_sequence | stop_headsign |
|---------|--------------|----------------|---------|---------------|---------------|
| trip_1  | 06:10:00     | 06:10:00       | stop_A  | 1             | "outbound"    |
| trip_1  | 06:15:00     | 06:15:00       | stop_B  | 2             | "outbound"    |
| trip_1  | 06:20:00     | 06:20:00       | stop_C  | 3             | "outbound"    |
| trip_1  | 06:25:00     | 06:25:00       | stop_D  | 4             | "inbound"     |
| trip_1  | 06:30:00     | 06:30:00       | stop_E  | 5             | "inbound"     |
| trip_1  | 06:35:00     | 06:35:00       | stop_F  | 6             | "inbound"     |
| trip_1  | 06:40:00     | 06:40:00       | stop_A  | 7             | ""            |

| フィールド名 | 推奨事項 |
| --- | --- |
| `trips.trip_id` | ループ全体の往復を1つの便(trip)としてモデリングしてください。 |
| `stop_times.stop_id` | ループ便の場合、`stop_times.txt` に最初と最後の停留所(stop)を2回含めてください。以下の例を参照してください。多くの場合、ループ系統にはループ全体を走行しない始発便や最終便が含まれることがあります。これらの便も含めてください。 <table class="example"><thead><tr><th><code>trip_id</code></th><th><code>stop_id</code></th><th><code>stop_sequence</code></th></tr></thead><tbody><tr><td>9000</td><td>101</td><td>1</td></tr><tr><td>9000</td><td>102</td><td>2</td></tr><tr><td>9000</td><td>103</td><td>3</td></tr><tr><td>9000</td><td>101</td><td>4</td></tr></tbody></table> |
| `trips.direction_id` | ループが反対方向（例：時計回りと反時計回り）に運行する場合は、`direction_id` を `0` または `1` として指定してください。 |
| `trips.block_id` | 同一の `block_id` を使用して、連続するループ便を示してください。 |

### ラッソ型ルート {: #lasso-routes}

ラッソ型ルートは、循環ルートと方向性のあるルートの両方の特徴を組み合わせたものです。

<img src="https://raw.githubusercontent.com/MobilityData/GTFS_Schedule_Best-Practices/master/en/lasso-route.svg" width=140px style="display: block; margin-left: auto; margin-right: auto;">

| 例: |
| -------- |
| 地下鉄ルート（[シカゴ](https://www.transitchicago.com/assets/1/6/ctamap_Lsystem.pdf)） |
| 郊外から都心へのバスルート（[セント・アルバート](https://stalbert.ca/uploads/PDF-infosheets/RideGuide-201-207_Revised_Oct_2017.pdf) または [エドモントン](http://webdocs.edmonton.ca/transit/route_schedules_and_maps/future/RT039.pdf)） |
| CTA ブラウンライン（[CTA公式サイト](http://www.transitchicago.com/brownline/) および [TransitFeeds](https://transitfeeds.com/p/chicago-transit-authority/165/latest/route/Brn)） |

| フィールド名 | 推奨事項 |
| --- | --- |
| `trips.trip_id` | 「車両の往復運行」（上記の図を参照）全体は、A から B、B から再び A への移動で構成されます。車両の往復運行全体は、次のいずれかの方法で表現することができます。<li>__1つの__ `trip_id` 値／`trips.txt` 内のレコード</li><li>`block_id` によって連続運行を示す、__複数の__ `trip_id` 値／`trips.txt` 内のレコード</li> |
| `stop_times.stop_headsign` | A-B 区間の停留所等(stop)は、両方向で通過します。`stop_headsign` は運行方向を区別するのに役立ちます。そのため、これらの便(trip)については `stop_headsign` を提供することが推奨されます。example_table: <table class="example"><thead>  <tr><th>例:</th></tr></thead><tbody><tr><td>"A via B"</td></tr><tr><td>"A"</td></tr></tbody></table><table class="example"><thead><tr><th>シカゴ交通局（Chicago Transit Authority）の <a href="http://www.transitchicago.com/purpleline/">パープルライン</a></th></tr></thead><tbody><tr><td>"Southbound to Loop"</td></tr><tr><td>"Northbound via Loop"</td></tr><tr><td>"Northbound to Linden"</td></tr></tbody></table><table class="example"><thead><tr><th>エドモントン交通局（Edmonton Transit Service）のバス路線、例として <a href="http://webdocs.edmonton.ca/transit/route_schedules_and_maps/future/RT039.pdf">39番</a></th></tr></thead><tbody><tr><td>"Rutherford"</td></tr><tr><td>"Century Park"</td></tr></tbody></table> |
| `trip.trip_headsign` | trip headsign は、時刻表に表示されるような便(trip)全体の概要を示すものであるべきです。例として、「Linden to Linden via Loop」（シカゴの例）や「A to A via B」（一般的な例）などが考えられます。 |

### 分岐 {: #branches}

一部のルート・路線系統(route)には分岐が含まれる場合があります。これらの分岐はルート形状(shape)や停留所等(stop)を共有しますが、それぞれが独自の停留所やルート形状区間を持ちます。分岐間の関係は、以下の追加ガイドラインに従って、ルート名、行先表示(headsign)、および便の短縮名(trip short name)によって示すことができます。

<img src="https://raw.githubusercontent.com/MobilityData/GTFS_Schedule_Best-Practices/master/en/branching.svg" width=250px style="display: block; margin-left: auto; margin-right: auto;">

| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | 分岐ルートの命名においては、他の乗客向け情報資料に従うことが推奨されます。以下に2つのケースの説明と例を示します。 |
| | 時刻表や街頭の案内表示が、明確に異なる名前のルート（例: 1A と 1B）として表現している場合は、GTFS においても同様に `route_short_name` および/または `route_long_name` フィールドを使用して表現してください。例: GoDurham Transit の [routes 2, 2A, and 2B](https://gotriangle.org/sites/default/files/brochure/godurham-route2-2a-2b_1.pdf) は、ルートの大部分で共通のルート形状を共有していますが、いくつかの点で異なります。<ul><li>Route 2 は主要なサービスであり、ほとんどの時間帯に運行します。</li><li>Route 2 は夜間、日曜日、祝日に Main Street 上で経路変更を行います。</li><li>Routes 2A および 2B は月曜日から土曜日の日中に運行します。</li><li>Route 2B は、共有ルート形状の経路から分岐した区間に追加の停留所等(stop)を設けています。</li></ul> |
| | 事業者が提供する情報で、分岐が同一名称のルートとして説明されている場合は、`trips.trip_headsign`、`stop_times.stop_headsign`、および/または `trips.trip_short_name` フィールドを使用してください。例: GoTriangle の [route 300](https://gotriangle.org/sites/default/files/route_300_v.1.19.pdf) は、時間帯によって異なる場所へ運行します。通勤ピーク時には、標準ルートに追加の乗車区間(leg)が加えられ、市内に出入りする労働者に対応しています。 |

## よくある質問 (FAQ) {: #frequently-asked-questions-faq}

### これらの GTFS ベストプラクティスが重要である理由 {: #why-are-these-gtfs-best-practices-important}

GTFS ベストプラクティスの目的は以下の通りです。

* 公共交通アプリにおけるエンドユーザーの利用体験を向上させること  
* ソフトウェア開発者がアプリケーション、製品、サービスを容易に展開・拡張できるよう、幅広いデータの相互運用性を支援すること  
* GTFS を（当初の旅程計画への焦点を超えて）さまざまなアプリケーションカテゴリで活用できるようにすること  

GTFS ベストプラクティスが調整されていない場合、GTFS を利用するさまざまなアプリケーションが、統一されていない形で要件や期待値を設定してしまい、結果として要件の分岐やアプリケーション固有のデータセットが生じ、相互運用性が低下します。ベストプラクティスが公開される以前は、正しく構成された GTFS データとは何かについて、より多くの曖昧さや意見の相違が存在していました。

### どのように策定されたのか？誰が策定したのか？ {: #how-were-they-developed-who-developed-them}


これらのベストプラクティスは、GTFS に関わる 17 の組織からなるワーキンググループによって策定されました。このグループには、アプリ提供者やデータ利用者、交通事業者、そして GTFS に深く関与しているコンサルタントが含まれています。ワーキンググループは [Rocky Mountain Institute](http://www.rmi.org/mobility) によって招集・運営されました。

ワーキンググループのメンバーは、それぞれのベストプラクティスについて投票を行いました。ほとんどのベストプラクティスは全会一致で承認されましたが、一部のケースでは大多数の組織による賛成で承認されました。

### なぜ単に GTFS リファレンスを変更しないのですか？ {: #why-not-just-change-the-gtfs-reference}

良い質問です！仕様、データの利用状況およびニーズを検討する過程で、実際に仕様にいくつかの変更が加えられました（[GitHub のクローズ済みプルリクエスト](https://github.com/google/transit/pulls?q=is%3Apr+is%3Aclosed)を参照してください）。  
仕様リファレンスの修正は、ベストプラクティスよりも厳格な精査とコメントの対象となります。特定のベストプラクティスは、その採用状況やコミュニティの合意に基づいて仕様に統合されています。最終的には、すべての GTFS ベストプラクティスがコアの GTFS リファレンスの一部となる可能性があります。

### これらのベストプラクティスへの準拠を確認する方法 {: #how-to-check-for-conformance-with-these-best-practices}

Canonical GTFS Schedule Validator は、これらのベストプラクティスへの準拠を確認します。この検証ツールの詳細については、[validate ページ](../../../getting-started/validate)をご覧ください。

### 私は交通事業者を代表しています。ソフトウェアサービス提供者やベンダーがこれらのベストプラクティスに従うようにするには、どのような手順を踏めばよいですか？ {: #i-represent-a-transit-agency-what-steps-can-i-take-so-that-our-software-service-providers-and-vendors-follow-these-best-practices}

ベンダーまたはソフトウェアサービス提供者に、これらのベストプラクティスを参照するよう案内してください。GTFS データを生成するソフトウェアの調達においては、GTFS ベストプラクティスの URL およびコア仕様リファレンスを参照することを推奨します。

### GTFS データフィードがこれらのベストプラクティスに準拠していないことに気づいた場合、どうすればよいですか？ {: #what-should-i-do-if-i-notice-a-gtfs-data-feed-does-not-conform-to-these-best-practices}

*feed_info.txt* 内に存在する場合は、[提案されている feed_contact_email または feed_contact_url](https://github.com/google/transit/pull/31/files) フィールドを使用してフィードの連絡先を特定するか、交通事業者またはフィード作成者のウェブサイトで連絡先情報を調べてください。フィード作成者に問題を伝える際には、議論している特定の GTFS ベストプラクティスへのリンクを提示してください。（「[このドキュメントへのリンク](#linking-to-this-document)」を参照してください。）

### どのように参加できますか？ {: #how-do-i-get-involved}


[specifications@mobilitydata.org](mailto:specifications@mobilitydata.org) にメールをお送りください。

## このドキュメントについて {: #about-this-document}

### 目的 {: #objectives}

GTFS ベストプラクティスを維持する目的は次の通りです。

* 交通データの相互運用性を高めることを支援する  
* 公共交通アプリにおけるエンドユーザーの利用体験を向上させる  
* ソフトウェア開発者がアプリケーション、製品、サービスを展開・拡張しやすくする  
* GTFS を（当初の旅程計画への焦点を超えて）さまざまなアプリケーション分野で活用しやすくする

### 公開されている GTFS ベストプラクティスを提案または修正する方法 {: #how-to-propose-or-amend-published-gtfs-best-practices}

ベストプラクティスは現在、仕様への統合が進行中です。新しいベストプラクティスを提案したい場合は、[GTFS Reference GitHub リポジトリ](https://github.com/google/transit/) にアクセスして issue を作成するか、PR を作成するか、または [specifications@mobilitydata.org](mailto:specifications@mobilitydata.org) までご連絡ください。

### 本ドキュメントへのリンク {: #linking-to-this-document}

GTFS データを正しく作成するためのガイダンスをフィード作成者に提供する目的で、本ドキュメントへのリンクを設定してください。各推奨事項にはアンカーリンクが付与されています。推奨事項をクリックすると、ページ内アンカーリンクの URL を取得できます。

GTFS を利用するアプリケーションが、ここで説明されていない GTFS データの運用に関する要件や推奨事項を設ける場合は、これらの共通ベストプラクティスを補足する形で、その要件や推奨事項を記載したドキュメントを公開することが推奨されます。

### GTFS ベストプラクティス ワーキンググループ {: #gtfs-best-practices-working-group}

GTFS ベストプラクティス ワーキンググループは、GTFS データに関する共通の実践と期待を定義するために、2016〜2017 年に [Rocky Mountain Institute](http://rmi.org/) によって招集されました。このグループは、公共交通事業者、GTFS を利用するアプリケーションの開発者、コンサルタント、および学術機関で構成されていました。  
このワーキンググループのメンバーには以下が含まれます。

* [Cambridge Systematics](https://www.camsys.com/)
* [Capital Metro](https://www.capmetro.org/)
* [Center for Urban Transportation Research at University of South Florida](https://www.cutr.usf.edu/)
* [Conveyal](http://conveyal.com/)
* [Google](https://www.google.com/)
* [IBI Group](http://www.ibigroup.com/)
* [Mapzen](https://mapzen.com/)
* [Microsoft](https://www.microsoft.com/)
* [Moovel](https://www.moovel.com/)
* [Oregon Department of Transportation](http://www.oregon.gov/odot/)
* [Swiftly](https://goswift.ly/)
* [Transit](https://transitapp.com/)
* [Trillium](http://trilliumtransit.com/)
* [TriMet](https://trimet.org/)
* [World Bank](http://www.worldbank.org/)

現在、この文書は [MobilityData](http://mobilitydata.org/) によって維持されています。
