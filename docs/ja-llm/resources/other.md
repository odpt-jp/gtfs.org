# その他のリソース {: #other-resources}

## コミュニティ {: #community}


質問をしたり、その他のコミュニティリソースを見つけたりできる場所です。

- [MobilityData Slack chat](https://share.mobilitydata.org/slack) - #gtfs、#gtfs-validators、#mobility-database、#gtfs-realtime、#gtfs_best-practices、#gtfs-pathways、#gtfs-fares、#gtfs-flex、#trb-transit-data のチャンネルを含むチャットルームです。
- [Transit Developers mailing list](https://groups.google.com/forum/#!forum/transit-developers)
- [OpenTripPlanner](https://github.com/opentripplanner/OpenTripPlanner) コミュニティ
    - [OpenTripPlanner User mailing list](https://groups.google.com/forum/#!forum/opentripplanner-users)
    - [OpenTripPlanner Developers mailing list](https://groups.google.com/forum/#!forum/opentripplanner-dev)
- OneBusAway
    - [OneBusAway Developers mailing list](http://groups.google.com/group/onebusaway-developers)
    - [OneBusAway API mailing list](http://groups.google.com/group/onebusaway-api)

### 地域・ローカルグループ {: #local-and-regional-groups}

- [Transit Techies NYC](https://transittechies.nyc/) - NYCを拠点とする、対面とオンラインのハイブリッド形式のミートアップです。[登壇者リスト](https://transittechies.nyc/past)には、このリポジトリへの多くの貢献者が含まれています。
- [German Open Transport Meetup](https://github.com/transportkollektiv/meetup/wiki) - ドイツ語圏のオープン交通コミュニティによる[隔週](https://hackmd.okfn.de/opentransportmeetup#)のオンラインミートアップです。
- [German Open Transport Data Quality Meetup](https://github.com/transportkollektiv/meetup/wiki) - データ品質に特化した、ドイツ語圏のオープン交通コミュニティによる隔月のオンラインミートアップです。

## 調査と解説 {: #research-and-commentary}


オープンな公共交通データに関連するブログ記事およびレポート。

### ブログ記事 {: #blog-posts}


- [私のバスはいつ（頃）来ますか？ データとコード](https://github.com/mjskay/when-ish-is-my-bus) - 「私のバスはいつ（頃）来ますか？」の背景にあるデータとコード（R）です。データには、3日分の過去の車両位置情報と調査結果が含まれます。
- [「レガシーAVLシステムですか？ 大丈夫です、仲間に加わりましょう。」Kurt Raschke著](https://kurtraschke.com/2015/01/legacy-avl-export) - レガシーAVLシステムのデータをGTFS-realtime形式へ変換するための選択肢についての議論です。
- [「GTFS Best Practicesが利用可能になりました！」Sean Barbeau著](https://medium.com/@sjbarbeau/gtfs-best-practices-now-available-88ac67194233) - GTFSのようなオープンデータ形式における課題の一部と、データ品質への対応を支援するために2017年初頭に公開されたGTFS Best Practicesについて論じています。
- [「GTFS-realtime v2.0の新機能」Sean Barbeau著](https://medium.com/@sjbarbeau/whats-new-in-gtfs-realtime-v2-0-cd45e6a861e9) - GTFS-realtime v1.0の不足点と、v2.0における改善について論じています。
- [「初心者向けAVL、CAD、およびリアルタイム乗客情報」Tony Laidig著](http://transitdata.net/avl-cad-and-real-time-passenger-info-for-beginners/) - 車両を追跡するために使用される技術について、一般的な導入を提供しています。
- [「より良い交通を可視化する：データとツール」Steve Pepple著](https://medium.com/@stevepepple/visualizing-better-transportation-data-tools-e48b8317a21c) - サンフランシスコのARUPで開催された2018年Transit Weekイベントで当初収集・議論された、サンフランシスコ・ベイエリアおよび北米のその他の都市に関する交通関連データとツールのコレクションです。
- [「GTFSデータを使用して公共交通車両をリアルタイムで追跡する方法」Tom Camp著](https://www.ably.io/blog/gtfs-data-track-transit-vehicles-realtime) - GTFSおよびGTFS Realtimeを使用して、継続的なリアルタイム更新を提供します。

### 学術論文 {: #academic-papers}


- [Tang et al. - 「Ridership effects of real-time bus information system: A case study in the City of Chicago」](https://www.sciencedirect.com/science/article/pii/S0968090X12000022) - イリノイ州シカゴでの実験では、乗客がテキストメッセージまたはメールを通じてリアルタイム情報にアクセスできる場合、利用者数がわずかに増加することが示されました。
- [Kay et al. - 「When(ish) is my bus? User-centered Visualizations of Uncertainty in Everyday, Mobile Predictive Systems」](https://www.mjskay.com/papers/chi_2016_uncertain_bus.pdf) - 本論文は、「交通機関の予測における不確実性をどのように伝えるか」という問いへの回答を試みています。問題、既存の解決策を説明し、[利用者にバス停へ到着すべき時刻を知らせるためのより良いインターフェース](https://github.com/mjskay/when-ish-is-my-bus/blob/master/quantile-dotplots.md#quantile-dotplots)を設計しています。
- [Watkins et al. - 「Where Is My Bus? Impact of mobile real-time information on the perceived and actual wait time of transit riders」](https://www.sciencedirect.com/science/article/pii/S0965856411001030) - ワシントン州シアトルでの実験では、乗客はモバイルアプリを通じてリアルタイム情報にアクセスできる場合、バスの待ち時間をより短く感じることが示されました。
- [Brakewood et al. - 「An experiment evaluating the impacts of real-time transit information on bus riders in Tampa, Florida」](https://www.sciencedirect.com/science/article/pii/S0965856414002146) - フロリダ州タンパでの統制実験では、モバイルアプリを通じてリアルタイム情報にアクセスできる乗客は、リアルタイム情報を持たない乗客と比較して、待ち時間がほぼ2分短縮されたと感じることが示されました。リアルタイム情報を持つ乗客では、不安と苛立ちも減少し、事業者に対する受け止め方も改善しました。
- [Brakewood et al. - 「The impact of real-time information on bus ridership in New York City」](https://www.sciencedirect.com/science/article/pii/S0968090X15000297) - ニューヨーク市での実験では、リアルタイム情報が乗客に提供された場合、長距離ルート・路線系統(route)で利用者数が増加することが示されました。
- [Brakewood and Watkins - 「A literature review of the passenger benefits of real-time transit information」](https://www.tandfonline.com/doi/full/10.1080/01441647.2018.1472147?scroll=top&needAccess=true) (2018) - リアルタイム交通情報の利点を検討した多くの異なる研究の概要です。
- [Gramacki et al. - 「gtfs2vec - Learning GTFS Embeddings for comparing Public Transport Offer in Microregions」](2021) - UberのH3空間インデックスと機械学習を使用して、都市内で公共交通サービス品質が「類似」する地域を特定する手法です。ソースコードは[GitHub](https://github.com/pwr-inf/gtfs2vec)で利用できます。
- [Higgins et al. - 「Calculating place-based transit accessibility: Methods, tools and algorithmic dependence」 (2022)](https://doi.org/10.5198/jtlu.2022.2012) - ArcGIS Pro、Emme、R5R、OpenTripPlannerを含む、徒歩および公共交通によるアクセシビリティを計算するソフトウェアツールを比較しています。
- [Aemmer et al. - 「Measurement and classification of transit delays using GTFS-RT data」](https://link.springer.com/article/10.1007/s12469-022-00291-7) - General Transit Feed SpecificationのReal-Time (GTFS-RT) コンポーネントから交通機関の運行パフォーマンス指標を抽出し、それらを道路区間に集約する手法を提示しています。[Transit Vis](https://github.com/zackAemmer/transit_vis)とともに使用され、[こちら](https://www.transitvis.com/)で閲覧できます。

### 政府報告書 {: #government-reports}

- [APTA Policy Development and Research - Public Transportation Embracing Open Data](http://www.apta.com/resources/reportsandpublications/Documents/APTA-Embracing-Open-Data.pdf) - オープンな公共交通データの利点と課題に関するAPTAの考察（以下のTCRP報告書の短い要約です）。
- [TCRP Synthesis 115 - Open Data: Challenges and Opportunities for Transit Agencies](http://onlinepubs.trb.org/Onlinepubs/tcrp/tcrp_syn_115.pdf) (2015) - オープンな公共交通データの利点と課題を包括的に検討した報告書です。
- [TCRP Research Report 213: Data Sharing Guidance for Public Transit Agencies – Now and in the Future](http://www.trb.org/Main/Blurbs/180188.aspx) (2020) - 利点、コスト、リスクの評価方法を含め、事業者がデータ共有に関する意思決定を行うのを支援するために作成された報告書です。
- [TCRP G-16 Development of Transactional Data Specifications for Demand-Responsive Transportation (In progress)](http://apps.trb.org/cmsfeed/TRBNetProjectDisplay.asp?ProjectID=4120) - この研究の目的は、デマンド型サービスの提供に関与する組織向けに、トランザクションデータの技術仕様を策定することです。完了予定日は2018年後半です。

### コミュニティ管理リスト {: #community-maintained-lists}

- [GTFS 作成・保守サービスを提供するベンダー](https://docs.google.com/spreadsheets/u/1/d/1Gc9mu4BIYC8ORpv2IbbVnT3q8VQ3xkeY7Hz068vT_GQ/pubhtml) - 新しいベンダーを追加するには[こちら](http://goo.gl/forms/YDbPSPmufS)。
- [交通ソフトウェア開発コンサルティングサービスを提供する組織](https://docs.google.com/spreadsheets/u/1/d/1n44CNMCK1vt1nyrsdYz-KD_hYxUMNIm6Me69M6ROBIg/pubhtml) - 新しい組織を追加するには[こちら](http://goo.gl/forms/cc6kcVERuP)。
