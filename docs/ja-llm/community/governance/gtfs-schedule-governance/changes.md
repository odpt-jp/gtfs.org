!!! warning "**ガバナンスフレームワークの移行**"

	**2025年7月7日以降**、すべての新規Pull Requestは、**[新しいGTFS Scheduleガバナンスフレームワーク](introduction.md)**の対象となります。
	<br>- **2025年7月7日より前**に作成されたPull Requestは、このセクションで説明する**旧ガバナンス**に引き続き従います。
	<br>- これには、次のPRが含まれます: [567](https://github.com/google/transit/pull/567), [561](https://github.com/google/transit/pull/561), [556](https://github.com/google/transit/pull/556), [546](https://github.com/google/transit/pull/546), [545](https://github.com/google/transit/pull/545), [533](https://github.com/google/transit/pull/533), [515](https://github.com/google/transit/pull/515), [502](https://github.com/google/transit/pull/502), [498](https://github.com/google/transit/pull/498), [483](https://github.com/google/transit/pull/483), [423](https://github.com/google/transit/pull/423)。
	<br>- 2025年7月7日より前に作成されたすべてのPRが解決されると、このページおよびここで説明するプロセスは完全に廃止されます。

GTFS仕様は不変のものではありません。むしろ、GTFSを利用する交通事業者、開発者、その他の関係者のコミュニティによって開発・維持されるオープンな仕様です。GTFSデータの生成者および利用者からなるこのコミュニティが、新しい機能を実現するために仕様を拡張する提案を行うことが想定されています。このプロセスの管理を支援するため、以下の手順およびガイドラインが定められています。

### 仕様改定プロセス {: #specification-amendment-process}


!!! note ""

	公式の仕様、リファレンスおよびドキュメントは英語で記述されています。異なる言語への翻訳が英語の原文と異なる場合、英語の原文が優先されます。すべてのコミュニケーションは英語で行われます。

1. プロトコル定義、仕様およびドキュメントファイルの関連するすべての部分（翻訳を除く）を更新した git branch を作成します。
1. https://github.com/google/transit で pull request を作成します。pull request には、パッチの詳細な説明を含めなければなりません。pull request の作成者は _advocate_ となります。
1. pull request が登録されたら、その advocate は pull request へのリンクを含めて [GTFS Changes mailing list](https://groups.google.com/forum/#!forum/gtfs-changes) で告知しなければなりません。告知されると、pull request は提案と見なされます。相互参照を容易にするため、pull request も編集して Google Groups の告知へのリンクを含めるべきです。
  	- advocate は貢献者であるため、pull request が受理される前に [Contributor License Agreement](https://github.com/google/transit/blob/master/CONTRIBUTING.md) に署名しなければなりません。
1. 提案についての議論を行います。pull request のコメントを唯一の議論フォーラムとして使用するべきです。
  	- 議論は advocate が必要と感じる限り継続しますが、少なくとも7暦日間でなければなりません。
  	- advocate は、同意したコメントに基づいて提案（すなわち pull request）を適時更新する責任を負います。
  	- advocate はいつでも提案の放棄を宣言することができます。
1. advocate は、議論に必要な最初の7日間の期間の後であれば、いつでも提案のバージョンについて投票を呼びかけることができます。
  	- 投票を呼びかける前に、少なくとも1つの GTFS producer と1つの GTFS consumer が提案された変更を実装するべきです。GTFS producer は公開 GTFS feed に変更を含め、pull request のコメント内でそのデータへのリンクを提供すること、また GTFS consumer は変更を自明でない方法で利用しているアプリケーション（すなわち、新規または改善された機能をサポートしているもの）へのリンクを pull request のコメント内で提供することが期待されます。
1. 投票は、完全な14暦日間を満たすのに十分な最短期間継続します。投票は UTC の 23:59:59 に終了します。
  	- advocate は投票開始時に具体的な終了時刻を告知するべきです。
  	- 投票期間中は、提案に対する編集上の変更のみが許可されます（誤字、文言は意味を変更しない限り変更することができます）。
  	- 誰でも pull request へのコメントの形式で賛成／反対に投票することができ、投票期間の終了まで投票を変更することができます。
    投票者が投票を変更する場合、元の投票コメント内で投票に取り消し線を引き、新しい投票を記載して更新することが推奨されます。
  	- 投票期間の開始前の投票は考慮されません。
	- 投票の開始および終了は、[GTFS Changes mailing list](https://groups.google.com/forum/#!forum/gtfs-changes) で告知しなければなりません。
1. 少なくとも3票の全会一致の賛成がある場合、提案は受理されます。
  	- 提案者の投票は、合計3票には数えません。たとえば、提案者 X が pull request を作成して賛成票を投じ、ユーザー Y と Z が賛成票を投じた場合、これは合計2票の賛成票として数えられます。
  	- 反対票には理由を付けなければならず、可能であれば実行可能なフィードバックを提供するべきです。
  	- 投票が否決された場合、advocate は提案に関する作業を継続するか、提案を放棄するかを選択することができます。
    advocate のいずれの決定も、[GTFS Changes mailing list](https://groups.google.com/forum/#!forum/gtfs-changes) で告知しなければなりません。
  	- advocate が提案に関する作業を継続する場合、いつでも新たな投票を呼びかけることができます。
1. 30暦日間活動がない pull request は閉じられます。pull request が閉じられると、対応する提案は放棄されたものと見なされます。advocate は、議論を継続または維持したい場合、いつでも pull request を再開することができます。
1. 提案が受理された場合:
  	- Google は、投票されたバージョンの pull request をマージすること（貢献者が [CLA](https://github.com/google/transit/blob/master/CONTRIBUTING.md) に署名していることを条件とします）、および5営業日以内に pull request を実行することを約束します。
  	- 翻訳を元の pull request に含めてはいけません。
    Google は最終的にサポート対象言語の関連する翻訳を更新する責任を負いますが、コミュニティからの純粋な翻訳 pull request は歓迎され、すべての編集上のコメントに対応され次第受理されます。
1. pull request の最終結果（受理または放棄）は、pull request が当初告知された同じ Google Groups のスレッドで告知するべきです。

### 指針 {: #guiding-principles}

GTFSの当初の理念を維持するため、仕様を拡張する際に考慮すべき多数の指針が定められています。

#### フィードは作成および編集が容易であるべきです {: #feeds-should-be-easy-to-create-and-edit}

小規模な事業者にとって役立つ、スプレッドシートプログラムやテキストエディタを使用して容易に表示および編集できるため、仕様の基礎としてCSVを選択しました。また、ほとんどのプログラミング言語やデータベースから容易に生成できるため、大規模なフィードの公開者にとっても適しています。

#### フィードは解析しやすくするべきです {: #feeds-should-be-easy-to-parse}

フィードの読み取り側は、可能な限り少ない作業で、必要な情報を抽出できるべきです。フィードへの変更および追加は、フィードの読み取り側が実装する必要のあるコードパスの数を最小限に抑えるため、可能な限り幅広く有用であるべきです。（ただし、最終的にはフィードの読み取り側よりもフィードの公開側の方が多くなるため、作成を容易にすることを優先するべきです。）

#### この仕様は乗客情報に関するものです {: #the-spec-is-about-passenger-information}

GTFS は主として乗客情報に関するものです。すなわち、この仕様には、何よりもまず乗客向けツールを支えるのに役立つ情報を含めるべきです。交通事業者がシステム間で内部的に送信したい運行指向の情報は、潜在的に大量に存在します。GTFS はその目的を意図したものではなく、より適切な可能性のある他の運行指向のデータ標準が存在します。

#### 仕様への変更は後方互換性を持つべきです {: #changes-to-the-spec-should-be-backwards-compatible}

仕様に機能を追加する際には、既存のフィードを無効にする変更を避けたいと考えています。既存のフィード公開者がフィードに機能を追加したいと考えるまで、追加の作業を発生させたくありません。また、可能な限り、既存のパーサーが新しいフィードの古い部分を引き続き読み取れるようにしたいと考えています。

#### 推測的な機能は推奨されません {: #speculative-features-are-discouraged}

新しい機能はすべて、フィードの作成と読み取りに複雑さを加えます。したがって、有用であることが分かっている機能のみを追加するよう注意したいと考えています。理想的には、あらゆる提案は、新機能を使用する実際の交通システム向けのデータを生成し、それを読み取って表示するソフトウェアを作成することで、テスト済みであるべきです。GTFS では、公式のパーサーおよびバリデーターによって無視される追加の列やファイルを追加することで、形式を容易に拡張できます。そのため、提案は既存のフィード上で容易にプロトタイプ化およびテストできます。
