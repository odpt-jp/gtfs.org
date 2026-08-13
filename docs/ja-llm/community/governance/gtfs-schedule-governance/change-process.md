# 変更プロセス {: #change-process}

## 概要 {: #overview}


仕様変更プロセスは、コミュニティが[GTFS Repository](https://github.com/google/transit/pulls)において仕様への変更を提案、レビュー、および採用する方法を示します。 

仕様変更プロセスは、**2つの主要な段階**に分かれており、3つの[変更タイプ](change-types.md)、すなわち[機能的変更](change-types.md/#functional-changes)、[非機能的変更](change-types.md/#non-functional-changes)、および[ドキュメント保守](change-types.md/#documentation-maintenance)に応じて、**3つのトラック**に分類されます。 

<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-overview.svg">

### ステージ1: Issue {: #stage-1-issue}


Issueステージは、新しいアイデアの議論、ニーズの特定、および仕様の改善提案を目的としています。Issueは、変更の必要性と支持を評価するのに役立つとともに、Pull Requestステージへ進むために必要なリソースを整理します。 

新しいアイデアについて合意形成を図るため、Issueステージから開始することが推奨されます。ただし、提案の範囲がすでに明確に定義されている場合は、Pull Requestステージから直接開始することが適切です。

### ステージ2: プルリクエスト {: #stage-2-pull-request}


プルリクエストステージでは、Issueステージでのアイデアを仕様として開発・実装します。このステージは、変更の種類に応じて3つのトラックに分かれています。 

プロセス全体は [GitHub google/transit repository](https://github.com/google/transit/pulls) 内で行われ、採用される前にすべての変更が徹底的に評価されることを保証します。

## プロセストラック {: #process-tracks}


提案された変更の種類に応じて、変更プロセスには異なるトラックが適用されます。Issue Stageは3つのトラックすべてで同じままですが、Pull Request Stageはトラックごとに異なります。

### トラックA: 機能的変更 {: #track-a-functional-changes}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-functional.svg">

このプロセスは、コミュニティが[GTFS Repository](https://github.com/google/transit/pulls)内の仕様に対する[機能的変更](change-types.md/#functional-changes)を提案、レビュー、および採用する方法を示します。 

* GTFS RepositoryでPull Requestを作成することで、提案が提出されます。   
* コミュニティは、提案を改善するための議論を行います。この期間は少なくとも7日間継続しなければなりません。
* [Contributors](roles.md/#contributors)および[Maintainer](roles.md/#maintainer)は、提案された変更をレビューします。この期間は少なくとも7日間継続しなければなりません。  
* テストの前に、コミュニティは提案について全会一致の合意を確認するための投票を行います。これは、投票に参加するすべての投票者が賛成しなければならないことを意味します。投票を有効とするには、少なくとも5人のcontributorを含み、そのうち最低2人のProducerおよび2人のConsumerを含まなければなりません。投票期間は少なくとも14日間継続しなければなりません。 
* [First Adopters](roles.md/#first-adopter)は、提案された変更をテストします。   
* コミュニティは、変更を正式に採用するべきかどうかを決定するための投票を行います。この投票は80%多数決ルールに従います。つまり、可決されるには少なくとも80%の票が賛成でなければなりません。有効とするには、投票には少なくとも5人のcontributorを含み、そのうち最低2人のProducerおよび2人のConsumerを含まなければなりません。投票期間は少なくとも14日間継続しなければなりません。
* 最後に、変更が仕様に実装されます。

### トラックB: 非機能的変更 {: #track-b-non-functional-changes}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-non-functional.svg">

このプロセスは、コミュニティが[GTFS Repository](https://github.com/google/transit/pulls)において、仕様に対する[非機能的変更](change-types.md/#non-functional-changes)を提案、レビュー、および採用する方法を示します。

* GTFS RepositoryでPull Requestを作成することにより、提案が提出されます。   
* コミュニティは、提案を改善するための議論を行います。この期間は少なくとも7日間継続しなければなりません。
* [Contributors](roles.md/#contributors)および[Maintainer](roles.md/#maintainer)は、提案された変更をレビューします。この期間は少なくとも7日間継続しなければなりません。   
* コミュニティは、変更を正式に採用するべきかどうかを決定するために投票を行います。この投票は80%多数決ルールに従います。つまり、可決されるには少なくとも80%の票が賛成でなければなりません。有効となるためには、投票には少なくとも5人のcontributorが含まれ、そのうち最低2人のProducerおよび2人のConsumerが含まれなければなりません。投票期間は少なくとも14日間継続しなければなりません。
* 最後に、変更が仕様に実装されます。

### トラックC: ドキュメントの保守 {: #track-c-documentation-maintenance}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-documentation.svg">

このプロセスは、コミュニティが [GTFS Repository](https://github.com/google/transit/pulls) における[ドキュメントを保守するための変更](change-types.md/#documentation-maintenance)を提案、レビュー、および採用する方法を示します。

* GTFS Repository で Pull Request を作成することで、提案が提出されます。   
* コミュニティは、提案を改善するための議論に参加します。この期間は少なくとも7日間継続しなければなりません。
* [Contributors](roles.md/#contributors) および [Maintainer](roles.md/#maintainer) が、提案された変更をレビューします。この期間は少なくとも7日間継続しなければなりません。  
* 最後に、変更が仕様に実装されます。

## プロセスのステップ {: #process-steps}


IssueおよびPull Requestステージにおけるすべてのステップを以下に示します。すべてのステップを利用するのはTrack Aのみであることに留意してください。Track BおよびTrack Cでは、短縮版のプロセスを利用します。

|  | Track A: 機能的変更 | Track B: 非機能的変更 | Track C: ドキュメント保守 |
| ----- | :---: | :---: | :---: |
| **[ステップ1.1: Issueの公開](#step-11-issue-publication)** | ✓ | ✓ | ✓ |
| **[ステップ1.2: Issueの議論](#step-12-issue-discussion)** | ✓ | ✓ | ✓ |
| **[ステップ2.1: Pull Requestの公開](#step-21-pull-request-publication)** | ✓ | ✓ | ✓ |
| **[ステップ2.2: Pull Requestの議論](#step-22-pull-request-discussion)** | ✓ | ✓ | ✓ |
| **[ステップ2.3: Pull Requestのレビュー](#step-23-pull-request-review)** | ✓ | ✓ | ✓ |
| **[ステップ2.4: テストへの投票](#step-24-vote-to-test)** | ✓ |  |  |
| **[ステップ2.5: テスト](#step-25-testing)** | ✓ |  |  |
| **[ステップ2.6: 採用への投票](#step-26-vote-to-adopt)** | ✓ | ✓ |  |
| **[ステップ2.7: 採用](#step-27-adoption)** | ✓ | ✓ | ✓ |

### ステップ1.1: Issueの公開 {: #step-11-issue-publication}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-step-1.1.svg">

[Contributor](roles.md/#contributors)は、[GTFS Repository](https://github.com/google/transit/pulls)でIssueを作成することにより、仕様を改善するためのアイデアを共有します。 

* 誰でも、議論を開始するためにIssueを作成することができます。

**<ins>アクション</ins>** 

1. **Issueの提出**

    * [Contributor](roles.md/#contributors)は、アイデアと、それによって解決される問題を説明するIssueを投稿します。

**<ins>提案</ins>** 

| 提案 | 詳細 |
| :---- | :---- |
| **仕様変更テンプレートを使用する** | [提供されているテンプレート](https://github.com/google/transit/issues/new?assignees=&labels=spec-change&projects=&template=spec_change.yml)を使用して、フィールドに概要レベルの説明を記入します。 |
| **議論を促進する** | 内容は完璧である必要はありません。会話が進むにつれて、議論を促し、変化していくべきです。 |
| **関心のあるContributorにタグ付けする** | 議論に関心を持つ可能性のある他のContributorにタグ付けし、関連するプラットフォームでIssueを共有します。 |

### ステップ1.2: Issueの議論 {: #step-12-issue-discussion}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-step-1.2.svg">

コミュニティは、仕様を変更するための提案の策定を支援するために議論を行います。この提案は、次の段階でPull Requestとして提出されます。

**<ins>アクション</ins>** 

1. **Issueの議論**

    * [Contributors](roles.md/#contributors)は、元のIssue投稿に返信し、フィードバックを共有します。

2. **ワーキンググループの提案**

    * 必要に応じて、任意の[Contributor](roles.md/#contributors)は、ビデオ会議ソフトウェアを使用してすべての関係者間の議論を促進するため、ワーキンググループの設置を提案することができます。  
    * ワーキンググループは、任意の[Contributor](roles.md/#contributors)または[Maintainer](roles.md/#maintainer)が組織することができます。  
    * ワーキンググループ会議で行われた議論は、Issueコメントに要約するべきです。

**<ins>提案</ins>** 

| 提案 | 詳細 |
| :---- | :---- |
| **議論の範囲を明確化する** | 提案の範囲を明確化することに議論を集中させます。 |
| **要件を確認する** | 提案を策定するために必要なすべての要件が確認されていることを確実にします。 |
| **意見を収集する** | 複数のcontributorからフィードバックを収集し、提案に対する全体的な支持を評価します。 |
| **議論を要約する** | 合意に達した事項、合意された範囲、advocateおよび／またはテストに関心を持つ関係者の告知など、最新の議論の要点を元の投稿に定期的に更新します。  |
| **候補となるAdvocateを特定する** | 完全な提案を策定し、Pull Request StageにおいてAdvocateの役割を担う意思のあるcontributorを特定します。 |

### ステップ2.1: Pull Request の公開 {: #step-21-pull-request-publication}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-step-2.1.svg">

> 注記: すべてのトラックに適用されます

仕様を変更する提案は、[GTFS Repository](https://github.com/google/transit/pulls) で Pull Request を作成することにより公開されます。提案を公開する[提唱者](roles.md/#advocate)は、単一の変更に焦点を当てなければなりません。変更の提案は誰でも行うことができます。

**<ins>アクション</ins>** 

1. **変更の適用**

    * [提唱者](roles.md/#advocate)は、元の [GTFS Repository](https://github.com/google/transit/pulls) の [fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo#forking-a-repository) を、自身の個人アカウントまたは組織のアカウントに作成します。  
    * [提唱者](roles.md/#advocate)は、自身の fork にブランチを作成し、提案された変更を適用します。 

2. **Pull Request の提出**

    * [提唱者](roles.md/#advocate)は、自身の fork から [GTFS Repository](https://github.com/google/transit/pulls) に [Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork) を作成します。 

**<ins>要件</ins>**

| 要件 | 詳細 |
| :---- | :---- |
| **単一の変更** | Pull Request は、一度に単一の変更に焦点を当てるべきです。「単一の変更」とは、無関係な更新をまとめることなく、1つの概念、機能、またはルールに対処する、焦点を絞った範囲を持つ自己完結した変更です。 |
| **拡張説明**  | Pull Request には、提案された変更の拡張説明を含めなければなりません。提供されている [Pull Request template](https://github.com/google/transit/blob/master/.github/PULL_REQUEST_TEMPLATE.md) に従うことが推奨されます。 |
| **変更タイプ** | 提唱者は、Pull Request の最初の投稿で変更タイプ（Functional、Non-Functional、または Documentation Maintenance）を指定しなければなりません。  <br>- 正しい採用トラックに従うことを確実にするため、すべての貢献者はいつでも誤分類された変更を指摘することができます。<br>- 合意に達しない場合、メンテナーは明確化を提供し、適切なトラックを推奨することができます。 |
| **提案する議論期間** | 提唱者は、提案された変更の範囲に基づき、最小限の推定議論期間を指定するべきです。  <br>- 例: 「全員が提案について議論するための十分な時間を確保できるよう、少なくとも1か月を議論のために確保することを推奨します。」 |
| **メーリングリストでの告知** | 提唱者は、[GTFS Changes mailing list](https://groups.google.com/g/gtfs-changes) で Pull Request の作成を告知しなければなりません。これには、変更の簡単な説明および Pull Request へのリンクを含めます。 |

### ステップ 2.2: Pull Request の議論 {: #step-22-pull-request-discussion}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-step-2.2.svg">
  
> 注: すべてのトラックに適用されます

コミュニティは、提案の改善および策定を支援するために議論を行います。

**<ins>アクション</ins>** 

1. **提案の議論**

    * [Contributors](roles.md/#contributors) は、Pull Request のコメント欄で提案について議論します。

2. **提案の更新**

    * [Advocate](roles.md/#advocate) は、受け取ったコメントに基づいて提案の内容を更新します。

3. **Working Group の提案**

    * 必要に応じて、任意の [Contributor](roles.md/#contributors) は、ビデオ会議ソフトウェアを使用してすべての関係者間の議論を促進するため、Working Group の設置を提案することができます。  
    * Working Group は、[Advocate](roles.md/#advocate) または [Maintainer](roles.md/#maintainer) のいずれかが組織することができます。  
    * Working Group 会議で行われた議論は、Pull Request のコメントに要約するべきです。

**<ins>要件</ins>** 

| 要件 | 詳細 |
| :---- | :---- |
| **最小議論期間**  | 議論は Advocate が必要と判断する期間継続しますが、少なくとも暦日で丸7日間でなければなりません。 |
| **Contributor License Agreement** | 提案を編集する Advocate を含むすべての contributors は、[Contributor License Agreement](https://github.com/google/transit/blob/master/CONTRIBUTING.md#before-you-contribute) に署名しなければなりません。 |

### ステップ2.3: Pull Request レビュー {: #step-23-pull-request-review}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-step-2.3.svg">

> 注記: すべてのトラックに適用されます

コミュニティは、提案をテスト用に準備するため、[Advocate](roles.md/#advocate) にフィードバックを提供します。

**<ins>アクション</ins>**

1. **レビュー期間の告知**

    * [Advocate](roles.md/#advocate) は、Pull Request のコメント欄でレビュー期間の開始を告知します。

2. **Maintainer のレビュー**

    * [Maintainer](roles.md/#maintainer) は、用語が現行の仕様と整合していることを確認するため、Pull Request をレビューします。   
    * [Maintainer](roles.md/#maintainer) は、コメントで変更を提案するか、または文言が正しいことを確認できます。これにより、[Advocate](roles.md/#advocate) は必要に応じて調整し、次のステップに進みます。

3. **Contributor のフィードバック**

    * [Contributors](roles.md/#contributors) もこの期間中に Pull Request をレビューし、テスト前に[Advocate](roles.md/#advocate) が最終調整を行うためのフィードバックを提供できます。

**<ins>要件</ins>**

| 要件 | 詳細 |
| :---- | :---- |
| **最小レビュー期間**  | レビューは Advocate が必要と判断する期間継続しますが、少なくとも暦日で丸7日間でなければなりません。<br>- Documentation Maintenance Track には最小レビュー期間の要件はありません。 |

### ステップ2.4: テスト実施のための投票 {: #step-24-vote-to-test}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-step-2.4.svg">

> 注記: トラックB: 非機能的変更およびトラックC: ドキュメント保守には適用されません

コミュニティは、提案の範囲についての合意を確認し、テストに進むのに十分な技術的妥当性があることを確認するために投票します。

**<ins>アクション</ins>** 

1. **投票の告知**

    * [Advocate](roles.md/#advocate)は、Pull Requestのコメント欄で投票の開始を告知し、投票の終了時刻を指定します。  
    * [Advocate](roles.md/#advocate)は、[GTFS Changes mailing list](https://groups.google.com/g/gtfs-changes)のディスカッションスレッドで、Pull Requestのコメントへのリンクおよび投票の終了時刻を示して投票を告知します。

2. **投票プロセス**  
     
    * [Contributors](roles.md/#contributors)は、Pull Requestのコメント欄で投票しなければなりません。

3. **編集および取消し**  
     
    * [Advocate](roles.md/#advocate)は、投票期間中、編集上の目的に限り提案を編集することができます。その他の変更には、投票プロセスの再開が必要です。  
    * [Advocate](roles.md/#advocate)は、いつでも投票を取り消すことができます。  
    
4. **投票終了の告知**  
     
    * [Advocate](roles.md/#advocate)は、Pull Requestのコメント欄で投票の終了を告知し、結果を含めます。  
    * [Advocate](roles.md/#advocate)は、[GTFS Changes mailing list](https://groups.google.com/g/gtfs-changes)のディスカッションスレッドでも、結果を含めて投票の終了を告知します。

5. **否決された投票**  
     
    * 投票が否決された場合、[Advocate](roles.md/#advocate)は以下を選択することができます。  
        1. 提案の作業を継続する、または  
        2. 提案を放棄する。  
    * [Advocate](roles.md/#advocate)は、Pull Requestのコメント欄および[GTFS Changes mailing list](https://groups.google.com/g/gtfs-changes)のディスカッションスレッドで、その決定を告知しなければなりません。

**<ins>要件</ins>** 

投票は、以下の条件を満たさなければなりません。

| 要件 | 詳細 |
| :---- | :---- |
| **承認ルール** | すべてのcontributorが+1に投票した場合にのみ、投票は可決されます（全会一致の合意）。|
| **投票形式** | 投票は以下の形式でなければなりません。<br>- *“+1 または \-1、組織名、Contributor Type（Consumer、Producer、またはGeneral Contributor）、作成したFeedまたは利用アプリケーションへのリンク”* |
| **反対票** | 反対票（-1）を投じるcontributorは、実行可能なフィードバックを提供しなければなりません。<br>- 実行可能なフィードバックとは、特定された問題の解決に役立つ具体的な所見または提案を提供する、実践的かつ建設的なフィードバックです。<br> - “*この提案はGTFSの後方互換性の原則を尊重していないため、代わりに別のファイルを作成することを提案します。*” |
| **最低投票数** | 少なくとも5票が投じられなければなりません。 |
| **参加者構成** | 少なくとも2つのGTFS consumer **および** 2つのGTFS producerが投票に参加しなければなりません。<br>- これらのcontributorは、両方の役割を代表できる場合であっても、Consumer **または** Producerのいずれかとしてのみ扱うことができます。<br>- 提案のテストを意図するFirst Adopterは、提案のAdvocateとして行動していない場合、投票し、この要件の対象として数えることができます。 |
| **Advocateの投票** | Advocateは、自身の提案に投票することはできません。 |
| **無効な投票** | 以下の場合、投票は無効とみなされます。<br>- contributorが公式の投票期間外（開始前または終了後）に投票した場合。<br>- 個人または組織が複数回投票した場合（個人または組織ごとに許可される投票は1票のみです）。<br>- contributorが実行可能なフィードバックを含めずに提案に反対票を投じた場合。  |
| **最低投票期間**  | 投票期間は少なくとも**14暦日間**継続し、**23:59:59 UTC**に終了しなければなりません。 |

### ステップ2.5: テスト {: #step-25-testing}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-step-2.5.svg">

> 注記: トラックB: 非機能的変更およびトラックC: ドキュメント保守には適用されません

1つのGTFS [Producer](roles.md/#producers) と1つのGTFS [Consumer](roles.md/#consumers) が、テストのために提案された変更を実装する [First Adopters](roles.md/#first-adopter) として協力を申し出ます。

**<ins>アクション</ins>** 

1. **テスターの確認**  
     
    * [Advocate](roles.md/#advocate) は、変更をテストし、Pull Requestのコメント欄でコメントを提供する [First Adopters](roles.md/#first-adopter) の身元を確認します。
    
2. **テスト**

    * [First Adopters](roles.md/#first-adopter) は、公開環境で変更を適用し、テストします。Producerの場合、これは公開GTFSフィードを意味します。Consumerの場合、これはアプリケーションの公開されている本番バージョンを意味します。
    * テストは、採択のための投票を呼びかける前に、すべての要件が満たされていることを確認するために必要な期間継続します。

3. **テストの証明**

    * [First Adopters](roles.md/#first-adopter) は、pull requestのコメントで実装された変更へのリンクを共有することにより、テストの証明を示します。

**<ins>要件</ins>** 

[Advocate](roles.md/#advocate) は、テスト期間のすべての要件が完了した後にのみ、採択のための投票（[ステップ2.6](#step-26-vote-to-adopt)）に進むことができます。

| 要件 | 詳細 |
| :---- | :---- |
| **最小テスト期間**  | テスト期間は、**少なくとも7暦日間**継続しなければなりません。 |
| **テスターの参加** | 少なくとも1つのConsumerおよび1つのProducerを含む、少なくとも2人の貢献者が、提案された変更を適用しテストしなければなりません。 |
| **テスト中の問題の特定** | 変更をテストするFirst Adoptersは、Advocateが提案に必要な調整を行えるよう、特定された問題を、理想的には解決策の提案とともに、Pull Requestにコメントして報告しなければなりません。<br>- 変更が提案の範囲に重大な影響を与える場合、任意のContributorがそれを指摘することができ、その場合Advocateは提案を議論ステップ（[ステップ2.2](#step-22-pull-request-discussion)）に戻すか、または取り下げを検討する必要があります。 |
| **テストの証明** | First Adoptersは、公開環境で変更を適用、テスト、および共有しなければなりません。<br>- Producersの場合は公開GTFSフィードへのリンク<br>- Consumersの場合はGTFSを利用するアプリケーションへの公開リンク。 |

### ステップ2.6: 採択の投票 {: #step-26-vote-to-adopt}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-step-2.6.svg">

> 注: Track C: Documentation Maintenance には適用されません

コミュニティは、提案された変更を仕様に正式に採択するかどうかを確認するために投票します。

**<ins>アクション</ins>** 

1. **投票の告知**  
     
    * [Advocate](roles.md/#advocate) は、投票の終了時刻を明記して、Pull Request のコメント欄で投票の開始を告知します。  
    * [Advocate](roles.md/#advocate) は、Pull Request のコメントへのリンクおよび投票の終了時刻を記載して、[GTFS Changes mailing list](https://groups.google.com/g/gtfs-changes) のディスカッションスレッドで投票を告知します。  
    
2. **投票プロセス**  
     
    * [Contributors](roles.md/#contributors) は、Pull Request のコメント欄で投票しなければなりません。  
    
3. **編集および取消し**  
     
    * [Advocate](roles.md/#advocate) は、投票期間中、編集上の目的に限り提案を編集することができます。  
    * [Advocate](roles.md/#advocate) は、いつでも投票を取り消すことができます。  
    
4. **投票終了の告知**  
     
    * [Advocate](roles.md/#advocate) は、Pull Request のコメント欄で投票の終了を告知し、結果を含めます。  
    * [Advocate](roles.md/#advocate) は、結果を含めて、[GTFS Changes mailing list](https://groups.google.com/g/gtfs-changes) のディスカッションスレッドでも投票の終了を告知します。

5. **否決された投票**  
     
    * 投票が否決された場合、[Advocate](roles.md/#advocate) は以下のいずれかを選択することができます。  
        1. 提供された実行可能なフィードバックに基づいて提案を調整し、再度投票を実施する、  
        2. ディスカッションのステップ（[ステップ2.2](#step-22-pull-request-discussion)）に戻り、スコープを再定義する、または  
        3. 提案を放棄する。  
    * [Advocate](roles.md/#advocate) は、Pull Request のコメント欄および[GTFS Changes mailing list](https://groups.google.com/g/gtfs-changes)のディスカッションスレッドで、その決定を告知しなければなりません。

**<ins>要件</ins>** 

投票は、以下の条件を満たさなければなりません。

| 要件 | 詳細 |
| :---- | :---- |
| **承認ルール** | contributors の80%以上が+1に投票した場合、投票は可決されます（特別多数決）。 |
| **投票形式** | 投票は以下の形式でなければなりません。<br>- *“+1 または \-1、組織名、Contributor Type（Consumer、Producer、またはGeneral Contributor）、作成したFeedまたは利用するApplicationへのリンク”* |
| **反対票** | 反対票（-1）を投じる contributors は、実行可能なフィードバックを提供しなければなりません。<br>- 実行可能なフィードバックとは、特定された問題の解決に役立つ具体的な所見または提案を提供する、実践的かつ建設的なフィードバックです。<br> - “*この提案はGTFSの後方互換性の原則を尊重していないため、代わりに別のファイルを作成することを提案します。*” |
| **最低投票数** | 少なくとも5票が投じられなければなりません。 |
| **参加者構成** | 少なくとも2つのGTFS consumers **および** 2つのGTFS producers が投票に参加しなければなりません。<br>- これらの contributors は、両方の役割を代表できる場合であっても、Consumer **または** Producer のいずれかとしてのみ考慮されます。<br>- 提案をテストした First Adopters は、提案のAdvocateとして行動していない場合、投票でき、この要件に算入されます。 |
| **Advocateの投票** | Advocate は、自身の提案に投票できません。 |
| **無効な投票** | 以下の場合、投票は無効と見なされます。<br>- contributor が公式の投票期間外（開始前または終了後）に投票した場合。<br>- contributor が複数回投票した場合（contributor ごとに許可される投票は1票のみです）。<br>- contributor が実行可能なフィードバックを含めずに提案に反対票を投じた場合。 |
| **最低投票期間**  | 投票期間は少なくとも**14暦日間すべて**継続し、**23:59:59 UTC**に終了しなければなりません。 |

### ステップ 2.7: 採用 {: #step-27-adoption}


<img class="center" width="1000" height="100%" src="../../../../../assets/governance-process-step-2.7.svg">
  
> 注記: すべてのトラックに適用されます。

[Maintainer](roles.md/#maintainer) は、投票が成功した後に正式に採用された変更を実装します。

**<ins>アクション</ins>** 

1. **実装**

    * 投票が可決された場合、[Contributors](roles.md/#contributors) が Contributor License Agreement に署名していることを条件として、[Maintainer](roles.md/#maintainer) は投票対象となった Pull Request を14暦日以内にマージしなければなりません。

2. **Revision History の更新**

    * [Maintainer](roles.md/#maintainer) は、投票が成功した後に採用されたすべての変更を、月に1回 Revision History に記録します。

**<ins>要件</ins>** 

| 要件 | 詳細 |
| :---- | :---- |
| **実装** | Maintainer は、正式に採用されたすべての変更を14暦日以内にマージするべきです |
| **Revision History** | Maintainer は、Specification の Revision History を毎月更新し、最近採用されたすべての変更について、簡単な説明および関連する議論へのリンクとともに記録するべきです。Documentation Maintenance の変更はこの要件の対象外ですが、価値があると判断された場合は Revision History に追加することができます。 |
