# GTFS Realtime {: #gtfs-realtime}


GTFS Realtime Specification は固定されたものではありません。代わりに、GTFS Realtime を利用する交通事業者、開発者、その他の関係者のコミュニティによって開発・維持されるオープンな仕様です。GTFS Realtime データのプロデューサーおよびコンシューマーからなるこのコミュニティは、新たな機能を実現するために仕様を拡張する提案を行うことが期待されています。そのプロセスの管理を支援するため、以下の手順およびガイドラインが定められています。

!!! note ""

	公式の仕様、リファレンスおよびドキュメントは英語で記述されています。異なる言語への翻訳が英語の原文と異なる場合、英語の原文が優先されます。すべてのコミュニケーションは英語で行われます。

# GTFS Realtime への新しいフィールドの追加 {: #adding-new-fields-to-gtfs-realtime}


プロデューサーまたはコンシューマーが GTFS Realtime 仕様に新しいフィールドを追加することに関心がある場合、提案するフィールドを説明する新しい issue を [GTFS Realtime GitHub repository](https://github.com/google/transit) で作成し、[GTFS Realtime mailing list](https://groups.google.com/forum/#!forum/gtfs-realtime) でこの新しいフィールド（issue へのリンクを含む）を告知するべきです。

## *Experimental* フィールド {: #experimental-fields}

1. コミュニティが、(a) 提案されたフィールドが有用であると思われること、および (b) フィールドの型（`optional` 対 `repeated`、`string` 対 `int` 対 `bool`）について合意に達することができた場合、GTFS Realtime メッセージ内にフィールド番号が割り当てられ、将来変更される可能性がある *experimental* フィールドである旨が [.proto file](../../../documentation/realtime/proto/) およびドキュメントに記載されます。
      - 合意は、以下の[仕様改定プロセス](#specification-amendment-process)と同じ議論および投票プロセスを通じて達成されますが、全会一致の同意ではなく、承認には賛成票の80%のみが必要です。
      - 新しい *experimental* フィールドを使用したい GTFS Realtime のプロデューサーおよびコンシューマーは、新しいフィールドを含む .proto file を使用してライブラリを再生成し（例: Google は [gtfs-realtime-bindings library](https://github.com/google/gtfs-realtime-bindings) を更新します）、ライブデータでフィールドの設定および解析を開始します。
      - *experimental* フィールドに価値があり、プロデューサーとコンシューマーの両方がそのフィールドを使用していることに満足した時点で、以下の[仕様改定プロセス](#specification-amendment-process)に従って、そのフィールドを正式に仕様へ追加します。
      - *experimental* フィールドとして承認されてから2年以内に、[仕様改定プロセス](#specification-amendment-process)を通じて *experimental* フィールドが採用されなかった場合、[.proto file](../../../documentation/realtime/proto/) ファイル内のフィールド値の横に `[deprecated=true]` を追加することで非推奨となります。`RESERVED` ではなく `[deprecated=true]` を使用することで、すでにフィールドを採用しているプロデューサーおよびコンシューマーは、その使用を削除する必要がありません。さらに、[仕様改定プロセス](#specification-amendment-process)に従う後続の投票で承認された場合（例: 追加のプロデューサーおよび/またはコンシューマーがフィールドの使用を開始した場合）、将来そのフィールドは「非推奨解除」されることがあります。
 
1. 新しいフィールドが単一のプロデューサーに固有であると見なされる場合、またはデータ型について争いがある場合、そのプロデューサーに[カスタム拡張](#extensions)を割り当て、そのプロデューサー自身のフィードでフィールドを使用できるようにします。可能な場合は拡張を避け、仕様のさまざまな拡張をコンシューマーがサポートすることによる断片化および追加作業を回避するため、多くの事業者に有用なフィールドを主要な仕様に追加するべきです。

## 仕様改定プロセス {: #specification-amendment-process}

1. プロトコル定義、仕様、およびドキュメントファイル（翻訳を除く）の関連するすべての部分を更新した git branch を作成します。
1. https://github.com/google/transit で pull request を作成します。pull request には、パッチの詳細な説明を含めなければなりません。pull request の作成者が _advocate_ となります。
1. pull request が登録されたら、その advocate が [GTFS Realtime mailing list](https://groups.google.com/forum/#!forum/gtfs-realtime) で告知しなければなりません。告知されると、pull request は提案と見なされます。
  	- advocate は貢献者であるため、pull request が受理される前に [Contributor License Agreement](https://github.com/google/transit/blob/master/CONTRIBUTING.md) に署名しなければなりません。
1. 提案についての議論を行います。pull request のコメントを唯一の議論フォーラムとして使用するべきです。
  	- 議論は advocate が必要と感じる限り継続しますが、少なくとも暦日で7日間でなければなりません。
  	- advocate は、同意するコメントに基づいて提案（すなわち pull request）を適時更新する責任を負います。
  	- advocate はいつでも提案の放棄を宣言することができます。
1. advocate は、議論に必要な最初の7日間の期間の後であれば、いつでも提案のバージョンについて投票を呼びかけることができます。
    - 投票を呼びかける前に、少なくとも1つの GTFS-realtime producer と1つの GTFS-realtime consumer が提案された変更を実装するべきです。GTFS-realtime producer は、公開されている GTFS-realtime feed に変更を含め、pull request のコメント内でそのデータへのリンクを提供すること、また GTFS-realtime consumer は、変更を自明でない方法で利用しているアプリケーション（すなわち、新規または改善された機能をサポートしているもの）へのリンクを pull request のコメント内で提供することが期待されます。
    - 投票を呼びかける際、advocate は、その投票がフィールドを仕様に正式採用するためのものか、実験的フィールドのためのものかを明確に述べるべきです。
1. 投票は、暦日で完全な7日間およびスイスの営業日で完全な5日間を満たすために十分な最短期間継続します。投票は UTC の 23:59:59 に終了します。
  	- advocate は、投票開始時に具体的な終了時刻を告知するべきです。
  	- 投票期間中は、提案に対する編集上の変更のみが許可されます（誤字、文言は意味を変更しない限り変更できます）。
  	- 誰でも pull request へのコメントの形式で賛成／反対に投票することができ、投票期間の終了まで投票を変更できます。
    投票者が投票を変更する場合、元の投票コメント内で投票に取り消し線を引き、新しい投票を記載して更新することが推奨されます。
  	- 投票期間の開始前の投票は考慮されません。
    - 投票の開始および終了は、[GTFS Realtime mailing list](https://groups.google.com/forum/#!forum/gtfs-realtime) で告知しなければなりません。
1. 少なくとも3票の全会一致の賛成がある場合、提案は受理されます。
  	- 提案者の投票は、合計3票には数えません。たとえば、提案者 X が pull request を作成して賛成票を投じ、ユーザー Y と Z が賛成票を投じた場合、これは合計2票の賛成票として数えられます。
  	- 反対票には理由を付けなければならず、可能であれば実行可能なフィードバックを提供するべきです。
  	- 投票が否決された場合、advocate は提案に対する作業を継続するか、提案を放棄するかを選択できます。
    advocate のいずれの決定も、[GTFS Realtime mailing list](https://groups.google.com/forum/#!forum/gtfs-realtime) で告知しなければなりません。
  	- advocate が提案に対する作業を継続する場合、いつでも新たな投票を呼びかけることができます。
1. 暦日で30日間活動がない pull request はすべて閉じられます。pull request が閉じられると、対応する提案は放棄されたものと見なされます。advocate は、議論を継続または維持したい場合、いつでも pull request を再開できます。 
 	- advocate は、公式仕様の一部ではなく [custom extension](#extensions) として機能を実装することを選択できる点に注意してください。
1. 提案が受理された場合:
  	- Google は、貢献者が [CLA](https://github.com/google/transit/blob/master/CONTRIBUTING.md) に署名していることを条件として、投票対象となったバージョンの pull request をマージし、5営業日以内に pull request を実行することを約束します。
  	- Google は、[https://github.com/google/gtfs-realtime-bindings](https://github.com/google/gtfs-realtime-bindings) リポジトリを適時更新することを約束します。提案の結果として行われる gtfs-realtime-bindings への commit は、その提案の pull request を参照するべきです。
  	- 翻訳を元の pull request に含めてはいけません。
    Google は、最終的にサポート対象言語の関連する翻訳を更新する責任を負いますが、コミュニティによる純粋な翻訳 pull request は歓迎され、すべての編集上のコメントに対応され次第受理されます。

## 指針となる原則 {: #guiding-principles}

GTFS Realtime の当初のビジョンを維持するため、仕様を拡張する際に考慮すべき指針となる原則がいくつか定められています。

**フィードは realtime で効率的に生成および利用できるべきです。**

Realtime 情報は、必然的に効率的な処理を必要とする、継続的かつ動的なデータストリームです。開発者にとっての使いやすさとデータ送信の効率性の両面で優れたトレードオフを提供するため、仕様の基盤として Protocol Buffers を選択しました。GTFS とは異なり、多くの事業者が GTFS Realtime フィードを手作業で編集することは想定していません。Protocol Buffers の選択は、ほとんどの GTFS Realtime フィードがプログラムによって生成および利用されるという結論を反映しています。

**仕様は乗客情報に関するものです。**

GTFS と同様に、GTFS Realtime は主として乗客情報に関するものです。すなわち、仕様には、何よりもまず乗客向けツールを支援できる情報を含めるべきです。交通事業者がシステム間で内部的に送信したい運行指向の情報は、大量に存在する可能性があります。GTFS Realtime はその目的を意図したものではなく、より適切な運行指向のデータ標準が他に存在する可能性があります。

**仕様への変更は後方互換性を持つべきです。**

仕様に機能を追加する際には、既存のフィードを無効にする変更を避けたいと考えています。既存のフィード公開者がフィードに機能を追加したいと考えるまで、追加の作業を発生させたくありません。また、可能な限り、既存のパーサーが新しいフィードの古い部分を引き続き読み取れるようにしたいと考えています。Protocol Buffers を拡張するための規約は、ある程度まで後方互換性を保証します。しかし、後方互換性を損なう可能性がある既存フィールドの意味的な変更も避けたいと考えています。

**推測に基づく機能は推奨されません。**

新しい機能はすべて、フィードの作成および読み取りに複雑さを加えます。したがって、有用であると分かっている機能のみを追加するよう注意したいと考えています。理想的には、あらゆる提案は、新機能を使用する実際の交通システム向けにデータを生成し、それを読み取り表示するソフトウェアを作成することで、検証されているべきです。

## 拡張機能 {: #extensions}

プロデューサーが GTFS Realtime フィードにカスタム情報を追加できるようにするため、[Protocol Buffers の Extensions 機能](https://developers.google.com/protocol-buffers/docs/proto#extensions)を活用します。Extensions により、Protocol Buffer メッセージ内に名前空間を定義でき、サードパーティ開発者は元の proto 定義を変更することなく追加フィールドを定義できます。

可能な場合は拡張機能を避け、仕様のさまざまな拡張機能をコンシューマーがサポートする際の断片化や追加作業を避けるため、多くの事業者にとって有用なフィールドを主要仕様に追加するべきです。拡張 ID を要求する前に、プロデューサーはそのフィールドを仕様に追加することを提案するべきです（[GTFS Realtime への新しいフィールドの追加](#adding-new-fields-to-gtfs-realtime)を参照してください）。

9000～9999 の範囲にある拡張 ID は、GTFS-rt プロデューサーによる私的利用のために予約されています。これらの ID は、組織内で情報を伝達するためにのみ使用するべきです。この範囲の拡張機能を公開フィードで使用してはいけません。 

新しい拡張機能を作成するために、1000 から始まり昇順に並ぶ番号のリストから次に利用可能な拡張 ID をプロデューサーに割り当てます。このリストは、以下の Extension Registry セクションに記載されています。

これらの割り当てられた拡張 ID は、各 GTFS Realtime メッセージ定義の「extension」名前空間で利用可能なタグ ID に対応します。開発者は拡張 ID を割り当てられると、任意のすべての GTFS Realtime メッセージを拡張する際にその ID を使用します。開発者が単一のメッセージのみを拡張する予定であっても、割り当てられた拡張 ID はすべてのメッセージのために予約されます。

仕様を拡張する開発者にとっては、拡張 ID を使用して「string」や「int32」などの単一フィールドを追加するのではなく、「MyTripDescriptorExtension」のような新しいメッセージを定義し、新しいメッセージで基盤となる GTFS Realtime メッセージを拡張して、すべての新しいフィールドをそこに配置するモデルが推奨されます。これには、マスターリストから新しい拡張 ID を予約する必要なく、拡張メッセージ内のフィールドを任意の方法で管理できるという利点があります。

```
message MyTripDescriptorExtension {
  optional string some_string = 1;
  optional bool some_bool = 2;
  ...
}
extend transit_realtime.TripDescriptor {
  optional MyTripDescriptorExtension my_trip_descriptor = YOUR_EXTENSION_ID;
}
```

拡張機能を作成する際、開発者は [Protocol Buffers Language Guide](https://developers.google.com/protocol-buffers/docs/proto) に従うべきです。よくある誤りは、拡張フィールド番号を再利用することです。[Assigning Field Numbers セクション](https://developers.google.com/protocol-buffers/docs/proto#assigning-field-numbers)で、Language Guide は次のように述べています。

> メッセージ定義内の各フィールドには一意の番号があります。これらの番号は、メッセージのバイナリ形式でフィールドを識別するために使用され、メッセージ型が使用され始めた後は変更するべきではありません。

例えば、最初の例では `some_string` にフィールド番号 `1` が割り当てられています。開発者が `some_string` を使用しなくなった場合、または `some_string` が公式 GTFS Realtime 仕様に採用されて拡張機能が不要になった場合、開発者は新しいフィールドにフィールド番号 `1` を再利用できません。代わりに、開発者はフィールドを非推奨にし、新しいフィールドには新しい番号を使用するべきです。
```
message MyTripDescriptorExtension {
  optional string some_string = 1 [deprecated=true];
  optional bool some_bool = 2;
  optional string some_new_string = 3;
  ...
}
```

## 拡張レジストリ {: #extension-registry}


|拡張ID|開発者|連絡先|詳細|
|------------|---------|-------|-------|
|1000|OneBusAway|[onebusaway-developers](http://groups.google.com/group/onebusaway-developers)|https://github.com/OneBusAway/onebusaway/wiki/GTFS-Realtime-Resources|
|1001|New York City MTA|[mtadeveloperresources](http://groups.google.com/group/mtadeveloperresources)|http://mta.info/developers/|
|1002|Google|[transit-realtime-partner-support@google.com](mailto:transit-realtime-partner-support@google.com)|Google Maps Live Transit Updates|
|1003|OVapi|gtfs-rt at ovapi.nl|http://gtfs.ovapi.nl|
|1004|Metra|[William Ashbaugh <w.l.ashbaugh@gmail.com>](mailto:w.l.ashbaugh@gmail.com)|
|1005|Metro-North Railroad|[John Larsen](mailto:mnrappdev@mnr.org)|
|1006|realCity|[David Varga](mailto:transit@realcity.io)|http://realcity.io|
|1007|Transport for NSW|[timetable@transport.nsw.gov.au](mailto:timetable@transport.nsw.gov.au)|[グループディスカッション](https://groups.google.com/forum/#!msg/gtfs-realtime/WYwIs4Hd_E0/PbkMnELUAwAJ)|
|1008|SEPTA - Southeastern Pennsylvania Transportation Authority|[Gregory Apessos](mailto:GApessos@septa.org)|https://github.com/septadev|
|1009|Swiftly|[mike@goswift.ly](mailto:mike@goswift.ly)|[グループディスカッション](https://groups.google.com/forum/#!msg/gtfs-realtime/mmnZV6L-2ls/wVWdknhLBwAJ)|
|1010|IBI Group|[Ritesh Warade](mailto:transitrealtime@ibigroup.com)|[Service Alerts の新しいタイムスタンプに関する GitHub 提案](https://github.com/google/transit/pull/134)|
|1013|MITFAHR\|DE\|ZENTRALE (MFDZ)|[Holger Bruch](mailto:holger.bruch@mfdz.de)|[グループディスカッション](https://groups.google.com/g/gtfs-realtime/c/IxYh-beoNoo)|
|9000-9999|予約済み - 内部使用のみ|[GTFS Community](https://groups.google.com/forum/#!forum/gtfs-realtime)|[グループディスカッション](https://groups.google.com/g/gtfs-realtime/c/IxYh-beoNoo)|
