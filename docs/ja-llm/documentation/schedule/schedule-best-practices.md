# GTFS Schedule ベストプラクティス {: #gtfs-schedule-best-practices}


これらは、[General Transit Feed Specification (GTFS)](../reference) で公共交通サービスを記述するための推奨プラクティスです。これらのプラクティスは、[GTFS Best Practices ワーキンググループ](#gtfs-best-practices-working-group)のメンバーの経験および[アプリケーション固有の GTFS プラクティス推奨事項](http://www.transitwiki.org/TransitWiki/index.php/Best_practices_for_creating_GTFS)から統合されています。 

詳細な背景については、[よくある質問](#frequently-asked-questions-faq)を参照してください。

## 文書構造 {: #document-structure}


実践事項は、4つの主要なセクションに整理されています。

* __[データセットの公開および一般的な実践事項](#dataset-publishing-general-practices):__ これらの実践事項は、GTFSデータセット全体の構造およびGTFSデータセットの公開方法に関するものです。
* __[ファイル別に整理された実践事項の推奨](#practice-recommendations-organized-by-file):__ 公式GTFSリファレンスへの実践事項の対応付けを容易にするため、推奨事項はGTFS内のファイルおよびフィールドごとに整理されています。
* __[ケース別に整理された実践事項の推奨](#practice-recommendations-organized-by-case):__ ループ路線などの特定のケースでは、複数のファイルおよびフィールドにわたって実践事項を適用する必要がある場合があります。そのような推奨事項は、このセクションに集約されています。

## データセットの公開と一般的な実践 {: #dataset-publishing-general-practices}


* データセットは、zipファイル名を含む公開かつ恒久的なURLで公開するべきです（例: www.agency.org/gtfs/gtfs.zip）。利用するソフトウェアアプリケーションによるダウンロードを容易にするため、理想的には、ファイルへのアクセスにログインを要求せず、URLから直接ダウンロードできるべきです。GTFSデータセットを公開ダウンロード可能にすることが推奨され（かつ最も一般的な実践です）、データ提供者がライセンスその他の理由でGTFSへのアクセスを制御する必要がある場合は、自動ダウンロードを容易にするAPIキーを使用してGTFSデータセットへのアクセスを制御することが推奨されます。
* GTFSデータは反復的に公開されるため、安定した場所にある単一のファイルには、常に交通事業者（または複数の事業者）の運行に関する最新の公式記述が含まれます。
* 可能な限り、データの反復間で`stop_id`、`route_id`、および`agency_id`の永続的な識別子（idフィールド）を維持してください。
* 1つのGTFSデータセットには、現在および今後の運行を含めるべきです（「統合」データセットと呼ばれることもあります）。Google transitfeed toolの[merge function](https://github.com/google/transitfeed/wiki/Merge)を使用して、2つの異なるGTFSフィードから統合データセットを作成できます。
    * 公開されるGTFSデータセットは、常に少なくとも今後7日間有効であるべきであり、理想的には、運行事業者がダイヤが継続して運行されると確信している期間と同じ長さであるべきです。
    * 可能であれば、GTFSデータセットは少なくとも今後30日間の運行を対象とするべきです。
* フィードから古い運行（期限切れのcalendar）を削除してください。
* 運行変更が7日以内に発効する場合は、静的GTFSデータセットではなく、[GTFS-realtime](../../realtime/reference)フィード（運行情報または便の更新(trip update)）を通じてこの運行変更を表現してください。
* GTFSデータをホストするweb-serverは、ファイルの更新日を正しく報告するように設定するべきです（[HTTP/1.1 - Request for Comments 2616](https://tools.ietf.org/html/rfc2616#section-14.29)のSection 14.29を参照してください）。

## ファイル別に整理された実践上の推奨事項 {: #practice-recommendations-organized-by-file}


このセクションでは、[GTFS reference](../reference)に沿って、ファイルおよびフィールド別に整理された実践を示します。

### すべてのファイル {: #all-files}


| フィールド名 | 推奨事項 |
| --- | --- |
| Mixed Case | 停留所等(stop)名、ルート・路線系統(route)名、行先表示(headsign)を含む、すべての顧客向けテキスト文字列では、英小文字を表示できる表示機器における地名の大文字化に関する地域の慣例に従い、ALL CAPS ではなく Mixed Case を使用するべきです。 |
| | 例: |
| | Brighton Churchill Square |
| | Villiers-sur-Marne |
| | Market Street |
| Abbreviations | 場所が略称で呼ばれている場合（例: 「JFK Airport」）を除き、名称およびその他のテキストについて、フィード全体で略語（例: Street に対する St.）の使用を避けるべきです。略語は、スクリーンリーダーソフトウェアおよび音声ユーザーインターフェースによるアクセシビリティに問題を生じさせることがあります。利用ソフトウェアは、完全な単語を表示用の略語へ確実に変換できるように設計できますが、略語から完全な単語への変換は、より高い誤りのリスクを伴います。 |

### agency.txt {: #agencytxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| `agency_phone` | 該当するカスタマーサービス電話番号が存在しない場合を除き、含めるべきです。 |
| `agency_email` | 該当するカスタマーサービスメールアドレスが存在しない場合を除き、含めるべきです。 |
| `agency_fare_url` | 事業者が完全に運賃無料である場合を除き、含めるべきです。 |

__例:__

- バスサービスは複数の小規模なバス事業者によって運行されています。しかし、スケジューリングと発券を担当し、利用者の観点からバスサービスの責任を負う1つの大規模な事業者があります。この1つの大規模な事業者を、フィード内の事業者として定義するべきです。データが内部的に異なる小規模なバス運行事業者ごとに分割されている場合でも、フィード内で定義する事業者は1つだけにするべきです。
  
- フィード提供者が発券ポータルを運営していますが、実際にサービスを運行し、利用者から責任を負う事業者として認識されている異なる事業者が存在します。実際にサービスを運行している事業者を、フィード内の事業者として定義するべきです。

### stops.txt {: #stopstxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| `stop_name` | 公開されている停留所等(stop)名がない場合は、フィード全体で一貫した停留所等(stop)の命名規則に従うべきです。  | |
| | デフォルトでは、`stop_name` に「Station」や「Stop」のような一般的または冗長な語を含めるべきではありませんが、一部の例外は許容されます。<ul><li>実際に名称の一部である場合（Union Station、Central Station）<li>`stop_name` が一般的すぎる場合（都市名である場合など）。「Station」、「Terminal」、またはその他の語により意味が明確になります。</ul> |
| `stop_lat` & `stop_lon` | 停留所等(stop)の位置は、可能な限り正確であるべきです。実際の停留所等(stop)位置と比較した場合、停留所等(stop)の位置の誤差は4メートルを__超えてはなりません__。 |
| | 停留所等(stop)の位置は、乗客が乗車する歩行者の通行権がある場所の非常に近くに配置するべきです（すなわち、道路の正しい側）。 |
| | 停留所等(stop)の位置が別々のデータフィード間で共有されている場合（すなわち、2つの事業者がまったく同じ停留所等(stop)／乗車施設を使用する場合）、両方の停留所等(stop)にまったく同じ`stop_lat`および`stop_lon`を使用して、停留所等(stop)が共有されていることを示すべきです。 |
| `parent_station` & `location_type` | 多くの駅またはターミナルには複数の乗車施設があります（交通手段に応じて、バスベイ、プラットフォーム、埠頭、ゲート、または別の用語で呼ばれる場合があります）。このような場合、フィード作成者は駅、乗車施設（子停留所等(stop)とも呼ばれます）、およびそれらの関係を記述するべきです。 <ul><li>駅またはターミナルは、`location_type = 1`を持つ`stops.txt`内のレコードとして定義するべきです。</li><li>各乗車施設は、`location_type = 0`を持つ停留所等(stop)として定義するべきです。`parent_station`フィールドは、乗車施設が属する駅の`stop_id`を参照するべきです。</li></ul> |
| | 駅および子停留所等(stop)に名前を付ける際は、乗客によく認識され、乗客が駅および乗車施設（バスベイ、プラットフォーム、埠頭、ゲートなど）を識別するのに役立つ名称を設定するべきです。<table class='example'><thead><tr><th>親駅名</th><th>子停留所等(stop)名</th></tr></thead><tbody><tr><td>Chicago Union Station</td><td>Chicago Union Station Platform 19</td></tr><tr><td>San Francisco Ferry Building Terminal</td><td>San Francisco Ferry Building Terminal Gate E</td></tr><tr><td>Downtown Transit Center</td><td>Downtown Transit Center Bay B</td></tr></tbody></table> |

### routes.txt {: #routestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| `route_long_name` | Specification reference における定義: <q>この名称は一般に <code>route_short_name</code> よりも説明的であり、多くの場合、ルート・路線系統(route)の目的地または停留所等(stop)を含みます。<code>route_short_name</code> または <code>route_long_name</code> の少なくとも一方を指定しなければならず、適切な場合は両方を指定することもできます。ルート・路線系統(route)に長い名称がない場合は、<code>route_short_name</code> を指定し、このフィールドの値として空文字列を使用してください。</q><br>長い名称の種類の例を以下に示します:<table class='example'><thead><tr><th colspan='3'>主要な移動経路または回廊</th></tr><tr><th>ルート・路線系統(route)名</th><th>形式</th><th>事業者</th></tr></thead><tbody><tr><td><a href='https://www.sfmta.com/getting-around/transit/routes-stops/n-judah'>“N”/“Judah”</a></td><td><code>route_short_name</code>/<br><code>route_long_name</code></td><td><a href='https://www.sfmta.com/'>Muni</a>、サンフランシスコ</td></tr><tr><td><a href='https://trimet.org/schedules/r006.htm'>“6“/“ML King Jr Blvd“</a></td><td><code>route_short_name</code>/<br><code>route_long_name</code></td><td><a href='https://trimet.org/'>TriMet</a>、オレゴン州ポートランド</td></tr><tr><td><a href='http://www.ratp.fr/informer/pdf/orienter/f_plan.php?nompdf=m6'>“6”/“Nation - Étoile”</a></td><td><code>route_short_name</code>/<br><code>route_long_name</code></td><td><a href='http://www.ratp.fr/'>RATP</a>、フランス・パリ</td></tr><tr><td><a href='http://www.bvg.de/images/content/linienverlaeufe/LinienverlaufU2.pdf'>“U2”-“Pankow – Ruhleben”</a></td><td><code>route_short_name</code>-<br><code>route_long_name</code></td><td><a href='http://www.bvg.de/'>BVG</a>、ドイツ・ベルリン</td></tr></tbody></table><table class='example'><thead><tr><th>サービスの説明</th></tr></thead><tbody><tr><td><a href='https://128bc.org/schedules/rev-bus-hartwell-area/'>“Hartwell Area Shuttle“</a></td></tr></tbody></table>        
| | `route_long_name` に `route_short_name` を含めるべきではありません。 |
| | `route_long_name` を設定する際は、サービス識別子を含む完全な名称を含めてください。例:<table class='example'><thead><tr><th>サービス識別子</th><th>推奨事項</th><th>例</th></tr></thead><tbody><tr><td>"MAX Light Rail"<br>オレゴン州ポートランドのTriMet</td><td><code>route_long_name</code> には、ブランド（MAX）および特定のルート・路線系統(route)指定を含めるべきです</td><td>"MAX Red Line" "MAX Blue Line"</td></tr><tr><td>"Rapid Ride"<br>ニューメキシコ州アルバカーキのABQ Ride</td><td><code>route_long_name</code> には、ブランド（Rapid Ride）および特定のルート・路線系統(route)指定を含めるべきです</td><td>"Rapid Ride Red Line"<br>"Rapid Ride Blue Line"</td></tr></tbody></table>
| `route_id` | 特定の名称付きルート・路線系統(route)上のすべての便(trip)は、同じ `route_id` を参照するべきです。<li>ルート・路線系統(route)の異なる方向を、異なる `route_id` 値に分けるべきではありません。</li><li>ルート・路線系統(route)の異なる運行時間帯を、異なる `route_id` 値に分けるべきではありません。すなわち、「Downtown AM」サービスと「Downtown PM」サービスについて、`routes.txt` に異なるレコードを作成しないでください。</li> |
| | ルート・路線系統(route)グループに明確に名称が異なる分岐（例: 1Aおよび1B）が含まれる場合、`route_short_name` および `route_long_name` を決定するために、ルート・路線系統(route)の[分岐](#branches)ケースにおける推奨事項に従ってください。 |
| `route_color` & `route_text_color` | 標識、印刷物、およびオンラインの利用者向け情報と一貫しているべきです（したがって、他の場所に存在しない場合は含めるべきではありません）。 |

### trips.txt {: #tripstxt}


* __ループ路線の特別なケースを参照してください:__ ループ路線は、2つの異なる終点を持つ線形路線とは対照的に、便が同じ停留所等(stop)で開始・終了するケースです。ループ路線は、特定の慣行に従って記述しなければなりません。[ループ路線のケースを参照してください。](#loop-routes)
* __投げ縄型路線の特別なケースを参照してください:__ 投げ縄型路線は、車両がルート・路線系統(route)の一部でのみループを走行する、線形とループの形状を組み合わせたものです。投げ縄型路線は、特定の慣行に従って記述しなければなりません。[投げ縄型路線のケースを参照してください。](#lasso-routes)

| フィールド名 | 推奨事項 |
| --- | --- |
| `trip_headsign` | `trip_headsign` または `stop_headsign` フィールドに、ルート名（`route_short_name` および `route_long_name` と一致するもの）を指定しないでください。 |
| | ルート・路線系統(route)内の便(trip)を区別するために使用できる、車両の行先表示(headsign)に表示される目的地、方向、および／またはその他の便の識別テキストを含めるべきです。GTFS データセットで提供される行先表示(headsign)を決定する際の、第一かつ最優先の目標は、車両に表示される方向情報との一貫性です。この第一の目標を損なわない場合にのみ、その他の情報を含めるべきです。便(trip)の途中で行先表示(headsign)が変わる場合は、`trip_headsign` を `stop_times.stop_headsign` で上書きしてください。以下に、いくつかの想定されるケースに対する推奨事項を示します。 |
| | <table class="example"><thead><tr><th>ルート・路線系統(route)の説明</th><th>推奨事項</th></tr></thead><tbody><tr><td>2A. 目的地のみ</td><td>終点の目的地を指定してください。例: "Transit Center"、 “Portland City Center”、または “Jantzen Beach”> </td></tr><tr><td>2B. 経由地を含む目的地</td><td>&lt;destination&gt; via &lt;waypoint&gt; “Highgate via Charing Cross”。車両がそれらの経由地を通過した後に乗客に表示される行先表示(headsign)から経由地を削除する場合は、<code>stop_times.stop_headsign</code> を使用して更新後の行先表示(headsign)を設定してください。> </td></tr><tr><td>2C. 地域名と地域内の停留所等(stop)</td><td>目的地の市または区内に複数の停留所等(stop)がある場合は、目的地の市に到着した時点で <code>stop_times.stop_headsign</code> を使用してください。> </td></tr><tr><td>2D. 方向のみ</td><td>“Northbound”、“Inbound”、“Clockwise” などの方向を示す用語を使用して示してください。></td></tr><tr><td>2E. 目的地を伴う方向</td><td>&lt;direction&gt; to &lt;terminus name&gt; 例: “Southbound to San Jose”></td></tr><tr><td>2F. 目的地および経由地を伴う方向</td><td>&lt;direction&gt; via &lt;waypoint&gt; to &lt;destination&gt;（“Northbound via Charing Cross to Highgate”）。></td></tr></tbody></table> |
| | 行先表示(headsign)を “To” または “Towards” という語で始めないでください。 |
| `direction_id` | データセット全体で値 0 および 1 を一貫して使用してください。すなわち、<ul><li>Red ルート・路線系統(route)で 1 = Outbound の場合、Green ルート・路線系統(route)でも 1 = Outbound</li><li>Route X で 1 = Northbound の場合、Route Y でも 1 = Northbound</li><li>Route X で 1 = clockwise の場合、Route Y でも 1 = clockwise</li></ul> |
| `bikes_allowed` | フェリー便(trip)については、自転車の持ち込みが許可されているか（または許可されていないか）を明示してください。データ欠落によりフェリー便(trip)を回避すると、通常は大幅な迂回につながるためです。 |

### stop_times.txt {: #stop_timestxt}


環状ルート: 環状ルートには、特別な `stop_times` の考慮事項が必要です。（参照: [環状ルートのケース](#loop-routes)）

| フィールド名 | 推奨事項 |
| --- | --- |
| `pickup_type` & `drop_off_type` | 乗客サービスを提供しない営業外（回送）便(trip)は、すべての `stop_times` 行について `pickup_type` および `drop_off_type` の値を `1` としてマークするべきです。
| | 営業便では、運行パフォーマンスの監視のための内部「時刻管理地点」や、乗客が乗車できない車庫などのその他の場所を、`pickup_type = 1`（乗車不可）および `drop_off_type = 1`（降車不可）としてマークするべきです。  |
| `arrival_time` & `departure_time` | `arrival_time` および `departure_time` フィールドは、時刻管理地点間の拘束力のない推定時刻または補間時刻を含め、可能な限り時刻値を指定するべきです。  |
| `stop_headsign` | 一般に、行先表示(headsign)の値は、駅の案内表示にも対応するべきです。<br><br>以下のケースでは、「Southbound」は駅の案内表示で使用されていないため、乗客を誤解させることになります。
| | <table class="example"><thead><tr><th colspan="2">NYCにおいて、南行きの2の場合:</th></tr><tr><th><code>stop_times.txt</code> 行:</th><th>使用する <code>stop_headsign</code> 値:</th></tr></thead><tbody><tr><td>Manhattanに到達するまで</td><td><code>Manhattan & Brooklyn</code></td></tr><tr><td>Downtownに到達するまで</td><td><code>Downtown & Brooklyn</code></td></tr><tr><td>Brooklynに到達するまで</td><td><code>Brooklyn</code></td></tr><tr><td>Brooklynに到達した後</td><td><code>Brooklyn (New Lots Av)</code></td></tr></tbody></table> |
| | <table class="example"><thead><tr><th colspan="2">Bostonにおいて、南行きのRed LineのBraintree支線の場合:</th></tr><tr><th><code>stop_times.txt</code> 行:</th><th>使用する <code>stop_headsign</code> 値:</th></tr></thead><tbody><tr><td>Downtownに到達するまで</td><td><code>Inbound to Braintree</code></td></tr><tr><td>Downtownに到達した後</td><td><code>Braintree</code></td></tr><tr><td>Downtownの後</td><td><code>Outbound to Braintree</code></td>  </tr></tbody></table> |

### calendar.txt {: #calendartxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | `calendar.service_name` フィールドを含めることも、GTFS の人間による可読性を向上させることができますが、これは仕様には採用されていません。 |

### calendar_dates.txt {: #calendar_datestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | `calendar.service_name` フィールドを含めることで、仕様には採用されていませんが、GTFSの人間による可読性も向上させることができます。|

### fare_attributes.txt {: #fare_attributestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| | 運賃システムを正確にモデル化できない場合は、さらなる混乱を避けるため、空欄のままにしてください。 |
| | 運賃（`fare_attributes.txt` および `fare_rules.txt`）を含め、可能な限り正確にモデル化してください。運賃を正確にモデル化できないエッジケースでは、乗客が不足した運賃で乗車しようとしないよう、運賃はより安価ではなく、より高額に表現するべきです。運賃の大部分を正しくモデル化できない場合は、フィードに運賃情報を含めないでください。 |

### fare_rules.txt {: #fare_rulestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | 運賃体系を正確にモデル化できない場合は、さらなる混乱を避けるため、空欄のままにするべきです。 |
| | 運賃（`fare_attributes.txt` および `fare_rules.txt`）を含め、可能な限り正確にモデル化するべきです。運賃を正確にモデル化できないエッジケースでは、乗客が不足した運賃で乗車しようとしないよう、運賃はより安価ではなく、より高価に表現するべきです。大多数の運賃を正しくモデル化できない場合は、フィードに運賃情報を含めるべきではありません。 |

### shapes.txt {: #shapestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | 理想的には、共有される経路（すなわち、ルート・路線系統(route) 1および2が道路または線路の同じ区間を運行する場合）については、共有される経路部分が完全に一致するべきです。これにより、高品質な公共交通地図作成を促進できます。<!-- (77) --> |
| | 経路は、車両が走行する通行権の中心線に従うべきです。これは、指定車線がない場合は道路の中心線、または車両の進行方向に走行する道路側の中心線のいずれかになります。<br><br>経路は、縁石の停留所等(stop)、プラットフォーム、または乗車場所へ「ギザギザ」にしてはいけません。 |
| `shape_dist_traveled` | `shape_dist_traveled` フィールドにより、事業者は `stop_times.txt` ファイル内の停留所等(stop)がそれぞれのルート形状(shape)にどのように正確に適合するかを指定できます。`shape_dist_traveled` フィールドに使用する一般的な値は、車両が走行したルート形状(shape)の始点からの距離です（走行距離計の読み取り値のようなものと考えてください）。<li>ルート・路線系統(route)の経路（`shapes.txt` 内）は、便(trip)が運行する停留所等(stop)の位置から100メートル以内にあるべきです。</li><li>`shapes.txt` に余分なポイントが含まれないように経路を簡略化するべきです（すなわち、直線区間上の余分なポイントを削除します。線の簡略化問題に関する議論を参照してください）。</li> |

### frequencies.txt {: #frequenciestxt}


| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | `frequencies.txt` で参照される便(trip)では、実際の停車時刻(stop_time)は無視されます。頻度ベースの便(trip)では、停留所等(stop)間の移動時間間隔のみが重要です。明確性および人間による可読性のため、`frequencies.txt` で参照される便(trip)の最初の停車時刻(stop_time)は00:00:00（最初の `arrival_time` 値を00:00:00）で開始することが推奨されます。 |
| `block_id` | 頻度ベースの便(trip)に提供することができます。 |

### transfers.txt {: #transferstxt}


`transfers.transfer_type` は、[GTFS で定義されている](../reference/#transferstxt)4つの値のいずれかにすることができます。以下に、GTFS Specification から引用したこれらの `transfer_type` の定義を、_イタリック体_ で示し、追加の実務上の推奨事項を記載します。

| フィールド名 | 推奨事項 |
| --- | --- |
| `transfer_type` | <q>0 または（空）：これはルート・路線系統(route)間の推奨乗換地点です。</q><br>より優れた選択肢（すなわち、追加の設備を備えた交通センター、または隣接もしくは接続された乗車施設／プラットフォームを持つ駅）を含む複数の乗換機会がある場合は、推奨乗換地点を指定するべきです。 |
| | <q>1：これは2つのルート・路線系統(route)間の時刻調整された乗換地点です。出発車両は到着車両を待機することが期待され、乗客がルート・路線系統(route)間を乗り換えるための十分な時間が確保されます。</q><br>この乗換タイプは、確実に乗り換えるために必要な時間間隔を上書きします。例として、Google Maps は乗客が安全に乗り換えるために3分を必要とすると想定しています。他のアプリケーションでは、異なるデフォルト値を想定する場合があります。 |
| | <q>2：この乗換には、接続を確保するために到着と出発の間に最低限の時間が必要です。乗換に必要な時間は <code>min_transfer_time</code> で指定されます。</q><br>障害物または停留所等(stop)間の移動時間を増加させるその他の要因がある場合は、最小乗換時間を指定するべきです。 |
| | <q>3：この場所ではルート・路線系統(route)間の乗換はできません。</q><br>物理的な障壁により乗換が不可能な場合、または困難な道路横断や歩行者ネットワークの欠落により乗換が危険または複雑になる場合は、この値を指定するべきです。 |
| | 便(trip)間で座席に座ったままの（block）乗換が許可される場合、到着便(trip)の最後の停留所等(stop)は出発便(trip)の最初の停留所等(stop)と同じでなければなりません。 |

## ケース別に整理された実践上の推奨事項 {: #practice-recommendations-organized-by-case}


このセクションでは、ファイルおよびフィールド全体に影響を及ぼす特定のケースを扱います。

### ループ・路線系統(route) {: #loop-routes}


ループ・路線系統(route)では、車両の便(trip)は同じ場所（場合によっては交通センターまたは乗換センター）で開始・終了します。車両は通常、継続的に運行し、車両がループを継続する間、乗客は車内にとどまることができます。

<img src="https://raw.githubusercontent.com/MobilityData/GTFS_Schedule_Best-Practices/master/en/loop-route.svg" width=200px style="display: block; margin-left: auto; margin-right: auto;">

したがって、車両が向かう方向を乗客に示すために、行先表示(headsign)に関する推奨事項を適用するべきです。

進行方向の変化を示すには、`stop_times.txt` ファイルで `stop_headsigns` を指定します。`stop_headsign` は、それが定義されている停留所等(stop)から出発する便(trip)の方向を説明します。便(trip)の各停留所等(stop)に `stop_headsigns` を追加すると、便(trip)の途中で行先表示(headsign)情報を変更できます。

2つの終点間を運行するルート・路線系統(route)（同じバスが往復する場合など）について、stop_times.txt ファイル内で単一の循環便(trip)を定義してはいけません。代わりに、便(trip)を2つの別々の運行方向に分割してください。
  
__循環便(trip)のモデリング例:__

- 各停留所等(stop)で行先表示(headsign)が変わる循環便(trip)

| trip_id | arrival_time | departure_time | stop_id | stop_sequence | stop_headsign |
|---------|--------------|----------------|---------|---------------|---------------|
| trip_1  | 06:10:00     | 06:10:00       | stop_A  | 1             | "B"           |
| trip_1  | 06:15:00     | 06:15:00       | stop_B  | 2             | "C"           |
| trip_1  | 06:20:00     | 06:20:00       | stop_C  | 3             | "D"           |
| trip_1  | 06:25:00     | 06:25:00       | stop_D  | 4             | "E"           |
| trip_1  | 06:30:00     | 06:30:00       | stop_E  | 5             | "A"           |
| trip_1  | 06:35:00     | 06:35:00       | stop_A  | 6             | ""            |
 
- 2つの行先表示(headsign)を持つ循環便(trip)

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
| `trips.trip_id `| ループの完全な往復便(trip)を、単一の便(trip)としてモデル化します。 |
| `stop_times.stop_id` | ループである便(trip)について、`stop_times.txt` に最初／最後の停留所等(stop)を2回含めます。以下の例を参照してください。多くの場合、ループ・路線系統(route)には、ループ全体を走行しない最初と最後の便(trip)が含まれることがあります。これらの便(trip)も含めてください。<table class="example"><thead><tr><th><code>trip_id</code></th><th><code>stop_id</code></th><th><code>stop_sequence</code></th></tr></thead><tbody><tr><td>9000</td><td>101</td><td>1</td></tr><tr><td>9000</td><td>102</td><td>2</td></tr><tr><td>9000</td><td>103</td><td>3</td></tr><tr><td>9000</td><td>101</td><td>4</td></tr></tbody></table> |
| `trips.direction_id` | ループが反対方向（すなわち時計回りと反時計回り）に運行する場合、`direction_id` を `0` または `1` として指定します。 |
| `trips.block_id` | 継続するループ便(trip)を同じ `block_id` で示します。 |

### 投げ縄型ルート {: #lasso-routes}


投げ縄型ルートは、環状ルートと方向別ルートの特徴を組み合わせたものです。

<img src="https://raw.githubusercontent.com/MobilityData/GTFS_Schedule_Best-Practices/master/en/lasso-route.svg" width=140px style="display: block; margin-left: auto; margin-right: auto;">

| 例: |
| -------- |
| 地下鉄ルート・路線系統(route)（[Chicago](https://www.transitchicago.com/assets/1/6/ctamap_Lsystem.pdf)） |
| 郊外から都心へのバスルート・路線系統(route)（[St. Albert](https://stalbert.ca/uploads/PDF-infosheets/RideGuide-201-207_Revised_Oct_2017.pdf) または [Edmonton](http://webdocs.edmonton.ca/transit/route_schedules_and_maps/future/RT039.pdf)） |
| CTA Brown Line（[CTA Website](http://www.transitchicago.com/brownline/) および [TransitFeeds](https://transitfeeds.com/p/chicago-transit-authority/165/latest/route/Brn)） |

| フィールド名 | 推奨事項 |
| --- | --- |
| `trips.trip_id` | 「車両往復運行」の全範囲（[上記](#lasso-routes)の図を参照）は、AからB、Bを経由してAへ戻る移動で構成されます。車両往復運行全体は、以下によって表現することができます。<li>`trips.txt` 内の __単一の__ `trip_id` 値/レコード</li><li>`trips.txt` 内の __複数の__ `trip_id` 値/レコード。連続した移動は `block_id` によって示されます。</li> |
| `stop_times.stop_headsign` | A-B区間に沿った停留所等(stop)は、両方向で通過します。`stop_headsign` は移動方向の区別を容易にします。したがって、これらの便(trip)には `stop_headsign` を提供することが推奨されます。example_table: <table class="example"><thead>  <tr><th>例:</th></tr></thead><tbody><tr><td>"A via B"</td></tr><tr><td>"A"</td></tr></tbody></table><table class="example"><thead><tr><th>Chicago Transit Authority の <a href="http://www.transitchicago.com/purpleline/">Purple Line</a></th></tr></thead><tbody><tr><td>"Southbound to Loop"</td></tr><tr><td>"Northbound via Loop"</td></tr><tr><td>"Northbound to Linden"</td></tr></tbody></table><table class="example"><thead><tr><th>Edmonton Transit Service Bus Lines、ここでは <a href="http://webdocs.edmonton.ca/transit/route_schedules_and_maps/future/RT039.pdf">39</a></th></tr></thead><tbody><tr><td>"Rutherford"</td></tr><tr><td>"Century Park"</td></tr></tbody></table>
| `trip.trip_headsign` | 便(trip)の行先表示(headsign)は、時刻表に表示されるような便(trip)の全体的な説明であるべきです。これは「Linden to Linden via Loop」（Chicagoの例）または「A to A via B」（一般的な例）とすることができます。 |

### 分岐 {: #branches}


一部のルート・路線系統(route)には分岐が含まれる場合があります。これらの分岐間では経路と停留所等(stop)が共有されますが、それぞれが固有の停留所等(stop)および経路区間にも運行します。分岐間の関係は、以下の追加ガイドラインを用いて、ルート・路線系統(route)名、行先表示(headsign)、および便(trip)の短縮名で示すことができます。

<img src="https://raw.githubusercontent.com/MobilityData/GTFS_Schedule_Best-Practices/master/en/branching.svg" width=250px style="display: block; margin-left: auto; margin-right: auto;">

| フィールド名 | 推奨事項 |
| --- | --- |
| すべてのフィールド | 分岐するルート・路線系統(route)の命名では、他の乗客向け情報資料に従うことが推奨されます。以下に、2つのケースの説明と例を示します。 |
| | 時刻表および路上の案内表示が、明確に異なる名称の2つのルート・路線系統(route)（例: 1Aおよび1B）を示している場合、`route_short_name` フィールドおよび/または `route_long_name` フィールドを使用して、GTFSでもそのように示します。例: GoDurham Transitの[ルート・路線系統(route) 2、2A、および2B](https://gotriangle.org/sites/default/files/brochure/godurham-route2-2a-2b_1.pdf)は、ルート・路線系統(route)の大部分にわたって共通の経路を共有していますが、いくつかの異なる点があります。<ul><li>ルート・路線系統(route)2は、ほとんどの時間帯に運行する基幹サービスです。</li><li>ルート・路線系統(route)2には、夜間、日曜日、および休日にMain Streetへの迂回が含まれます。</li><li>ルート・路線系統(route)2Aおよび2Bは、月曜日から土曜日の日中時間帯に運行します。</li><li>ルート・路線系統(route)2Bは、共有経路の迂回区間にある追加の停留所等(stop)に運行します。</li></ul> |
| | 事業者が提供する情報で分岐が同じ名称のルート・路線系統(route)として説明されている場合、`trips.trip_headsign`、`stop_times.stop_headsign`、および/または `trips.trip_short_name` フィールドを使用します。例: GoTriangleの[ルート・路線系統(route) 300](https://gotriangle.org/sites/default/files/route_300_v.1.19.pdf)は、時間帯に応じて異なる場所へ運行します。通勤ピーク時間帯には、市内に入る、または市外へ出る通勤者に対応するため、標準ルート・路線系統(route)に追加の乗車区間(leg)が加えられます。 |

## よくある質問（FAQ） {: #frequently-asked-questions-faq}

### これらの GTFS Best Practices が重要である理由 {: #why-are-these-gtfs-best-practices-important}


GTFS Best Practices の目的は以下のとおりです。

* 公共交通アプリにおけるエンドユーザーの顧客体験を向上させること
* ソフトウェア開発者がアプリケーション、製品、サービスを容易に導入および拡張できるよう、幅広いデータ相互運用性を支援すること
* さまざまなアプリケーションカテゴリにおける GTFS の利用を促進すること（本来の焦点である経路探索を超えて）

協調された GTFS Best Practices がなければ、さまざまな GTFS を利用するアプリケーションが、要件および期待を協調されていない方法で定める可能性があり、その結果、要件の乖離、アプリケーション固有のデータセット、および相互運用性の低下につながります。Best Practices の公開以前は、正しく構成された GTFS データを構成するものについて、より大きな曖昧さと意見の不一致がありました。

### これらはどのように策定されましたか？誰が策定しましたか？ {: #how-were-they-developed-who-developed-them}


これらのベストプラクティスは、アプリ提供者およびデータ利用者、交通事業者、ならびにGTFSに幅広く関与しているコンサルタントを含む、GTFSに関わる17の組織からなるワーキンググループによって策定されました。ワーキンググループは、[Rocky Mountain Institute](http://www.rmi.org/mobility)によって招集および進行されました。

ワーキンググループのメンバーは、各ベストプラクティスについて投票しました。ほとんどのベストプラクティスは、全会一致の投票により承認されました。少数のケースでは、ベストプラクティスは組織の大多数によって承認されました。

### GTFS Reference を単に変更しないのはなぜですか？ {: #why-not-just-change-the-gtfs-reference}


良い質問です！Specification、データの利用状況およびニーズを検討するプロセスは、実際にSpecificationへのいくつかの変更を促しました（[GitHub のクローズ済み pull request](https://github.com/google/transit/pulls?q=is%3Apr+is%3Aclosed)を参照してください）。
Specification reference の改訂には、Best Practices よりも高い水準の精査とコメントが求められます。一部の Best Practices は、その採用度およびコミュニティの合意に基づいて spec に統合されています。最終的には、すべての GTFS Best Practices が中核となる GTFS Reference の一部になる可能性があります。

### これらのベストプラクティスへの適合性を確認する方法 {: #how-to-check-for-conformance-with-these-best-practices}


Canonical GTFS Schedule Validator は、これらのベストプラクティスへの準拠を確認します。この検証ツールの詳細は、[validate page](../../../getting-started/validate) で確認できます。

### 私は交通事業者を代表しています。ソフトウェアサービスプロバイダーおよびベンダーがこれらのベストプラクティスに従うようにするために、どのような手順を取ることができますか？ {: #i-represent-a-transit-agency-what-steps-can-i-take-so-that-our-software-service-providers-and-vendors-follow-these-best-practices}


ベンダーまたはソフトウェアサービスプロバイダーに、これらのベストプラクティスを参照するよう案内してください。GTFS を生成するソフトウェアの調達においては、GTFS Best Practices URL および中核となる Spec Reference を参照することを推奨します。

### GTFS データフィードがこれらのベストプラクティスに準拠していないことに気付いた場合、どうすればよいですか？ {: #what-should-i-do-if-i-notice-a-gtfs-data-feed-does-not-conform-to-these-best-practices}


存在する場合は *feed_info.txt* の[提案されている feed_contact_email または feed_contact_url](https://github.com/google/transit/pull/31/files) フィールドを使用するか、交通事業者またはフィード作成者のウェブサイトで連絡先情報を調べて、フィードの連絡先を特定してください。フィード作成者に問題を伝える際は、議論対象となっている特定の GTFS Best Practice へのリンクを含めてください。（[「このドキュメントへのリンク」](#linking-to-this-document)を参照してください）。

### どのように参加できますか？ {: #how-do-i-get-involved}


[specifications@mobilitydata.org](mailto:specifications@mobilitydata.org) にメールしてください。

## この文書について {: #about-this-document}

### 目的 {: #objectives}


GTFS Best Practices を維持する目的は、以下のとおりです。

* 交通データの相互運用性をより高めること
* 公共交通アプリにおけるエンドユーザーの顧客体験を改善すること
* ソフトウェア開発者がアプリケーション、製品、およびサービスを導入・拡張しやすくすること
* さまざまなアプリケーションカテゴリ（本来の主眼である経路検索を超えるもの）での GTFS の利用を促進すること

### 公開済みの GTFS Best Practices を提案または改訂する方法 {: #how-to-propose-or-amend-published-gtfs-best-practices}


Best Practices は仕様への統合が進められています。新しいベストプラクティスを提案したい場合は、[GTFS Reference GitHub repository](https://github.com/google/transit/) にアクセスして issue を開くか PR を作成するか、[specifications@mobilitydata.org](mailto:specifications@mobilitydata.org) までお問い合わせください。

### このドキュメントへのリンク {: #linking-to-this-document}


GTFS データを正しく作成するためのガイダンスをフィード提供者に提供するには、ここにリンクしてください。個々の推奨事項にはアンカーリンクがあります。推奨事項をクリックすると、ページ内アンカーリンクの URL を取得できます。

GTFS を利用するアプリケーションが、ここで説明されていない GTFS データの実践に関する要件または推奨事項を設ける場合、これらの共通ベストプラクティスを補完するために、それらの要件または推奨事項を記載したドキュメントを公開することが推奨されます。

### GTFS ベストプラクティス作業部会 {: #gtfs-best-practices-working-group}


GTFS ベストプラクティス作業部会は、GTFS データに関する共通の慣行および期待事項を定義するために、公共交通事業者、GTFS を利用するアプリケーションの開発者、コンサルタント、および学術組織で構成され、2016～17年に [Rocky Mountain Institute](http://rmi.org/) によって招集されました。 
この作業部会のメンバーには、以下が含まれていました。

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
