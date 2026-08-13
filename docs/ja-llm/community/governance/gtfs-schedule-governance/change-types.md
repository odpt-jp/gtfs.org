# 変更の種類 {: #change-types}


仕様に対して提案された変更は、その機能および解釈への影響に基づいて分類されます。各カテゴリは、GTFS に正式に採用される前に、特定のプロセスに従います。

<img class="center" width="1000" height="100%" src="../../../../../assets/governance-change-types.svg">

## 機能変更 {: #functional-changes}


機能変更とは、仕様の機能に重大な影響を与える修正を指します。これらの変更は通常、テストを必要とし、新しい要素（ファイル、フィールド、列挙型）の追加または既存の要素の変更を含みます。このような変更が正式に採用されるためには、完全な仕様の[変更プロセス](change-process.md)に従わなければなりません。

過去の例:

* [PR #405](https://github.com/google/transit/pull/405) は、2つの新しいファイル networks.txt および route_networks.txt を追加しています。  
* [PR #385](https://github.com/google/transit/pull/385) は、fare_media.txt のフィールド fare_media_type に新しい選択肢を追加しています。

## 非機能的な変更 {: #non-functional-changes}


非機能的な変更とは、機能に大きな影響を与えない仕様の重要な更新を指します。これらの変更は通常、テストを必要とせず、要件の更新、ベストプラクティスの追加または変更、およびガバナンスフレームワークに対する編集上の変更以外のあらゆる変更を含みます。

### 要件の更新 {: #requirement-updates}


既存の機能に実質的な変更を導入しないものの、仕様内の現行要素の実装および準拠に影響を与える変更です。これには、ファイルおよびフィールドの存在種別の更新、ならびにファイル要件の調整が含まれます。 

* 過去の例:   
    * [PR #472](https://github.com/google/transit/pull/472) は、stops.txt の存在種別を必須から条件付き必須に変更しています。  
    * [PR #379](https://github.com/google/transit/pull/379) は、すべての GTFS データセットに対する新しい要件を追加しています。

### ガバナンス {: #governance}


仕様の管理を統括するプロセスおよびガイドラインの変更には、[Change Process](change-process.md) の更新、[Roles and Responsibilities](roles.md) および [Change Types](change-types.md) の修正、[Guiding Principles](guiding-principles.md) の改訂、ならびに[ガバナンスフレームワーク](introduction.md)に関連するその他の文書の調整が含まれます。これらの更新により、明確かつ効果的な管理慣行が確保されます。

* 過去の例:   
    * [PR #387](https://github.com/google/transit/pull/387) は、スイスの営業日から暦日のみに切り替えるため、仕様変更プロセスを更新しています。

### ベストプラクティス {: #best-practices}


「should」文を用いて推奨事項を取り入れることで、仕様の特定部分を強化する変更です。これらの更新は、仕様の適用における柔軟性を維持しつつ、望ましいアプローチまたは方法に関する指針を提供し、ユーザーがベストプラクティスに従うことを促します。

* 過去の例:  
    * [PR #485](https://github.com/google/transit/pull/485) は、行先表示(headsign)が推奨であることを追加する新しいベストプラクティスを作成しています。  
    * [PR #406](https://github.com/google/transit/pull/406) は、すべてのファイルに対するデータセット公開ガイドラインおよび実践推奨事項を仕様に導入しています。

## ドキュメントの保守 {: #documentation-maintenance}


ドキュメントの保守に重点を置く仕様変更は、仕様の意味論または機能を変更することなく、仕様の明確性、正確性、および表現を改善するために行われます。これらの更新には、仕様の現在の解釈を維持しつつ、より明確な説明を提供するために既存の内容を拡充することが含まれる場合があります。これらの変更にはレビューが必要ですが、投票を必要とせずに採用することができます。

### 編集 {: #editorial}


これには、文法、スペル、句読点、誤植、古いリンクの修正が含まれます。また、改訂履歴への採用済み変更の記録および書式の調整も含まれます。ガバナンス関連ファイルに対する編集上の更新も、このカテゴリに含まれます。

* 過去の例:  
    * [PR #506](https://github.com/google/transit/pull/505) 2024年9月の改訂履歴を更新します。  
    * [PR #361](https://github.com/google/transit/pull/361) fare_rules.txt 内の壊れたリンクを修正します。  
    * [PR #412](https://github.com/google/transit/pull/412) 既存の誤った場所に配置された要件の位置を変更します。

### 明確化と例 {: #clarifications-examples}


明確化は、理解を深めるために特定の点を詳述し、必要な文脈を追加し、仕様の適用方法を示す例を提供します。これらの更新により、ユーザーは意図された機能を変更することなく、仕様を正しく解釈して従うことができます。

* 過去の例:  
    * [PR #443](https://github.com/google/transit/pull/443) は、リファレンス文書の情報を補足する例のページへのリンクを追加しています  
    * [PR #426](https://github.com/google/transit/pull/426) は、意図された用途を反映するためにチケット商品の定義を変更しています。
