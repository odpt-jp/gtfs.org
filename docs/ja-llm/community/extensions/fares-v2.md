# GTFS-Fares v2 {: #gtfs-fares-v2}


Fares v2 は、[Fares v1](../../../documentation/schedule/examples/fares-v1/) の制限に対処することを目的とした GTFS Schedule 拡張プロジェクトです。

Fares v2 が表現することを計画している主な概念は以下のとおりです。

- チケット商品（例: 乗車券およびパス）
- 乗客カテゴリ（例: 高齢者および子ども）
- チケットメディア（例: 交通パス、紙の乗車券、非接触型銀行カード）
- 運賃上限

これらの概念により、データ作成者はゾーンベース、時間依存、および事業者間の運賃をモデル化することができます。この拡張プロジェクトは、反復的に採用されています。 

GTFS で正式に採用された内容を使用して何をモデル化できるかを示す[例はこちら](../../../documentation/schedule/examples/fares-v2)で確認できます。

2つの間に技術的な競合はないため、データ作成者は同じデータセット内で Fares v2 を Fares v1 とともに実装することができます。利用者は、他方とは独立して使用するバージョンを選択できます。Fares v2 の採用および十分な支持により、Fares v1 は将来非推奨となる可能性があります。

[完全な提案を見る](https://share.mobilitydata.org/gtfs-fares-v2){ .md-button .md-button--primary }

## 議論に参加する {: #participate-in-the-conversation}

Slack チャンネルおよび定期的なワーキンググループ会議に参加することで、Fares v2 に関する最新情報を把握し、議論に加わることができます。

[Slack で #gtfs-fares に参加する](https://share.mobilitydata.org/slack){ .md-button .md-button--primary} [会議スケジュールを見る](https://www.eventbrite.ca/e/specifications-discussions-gtfs-fares-v2-monthly-meetings-tickets-522966225057){ .md-button .md-button--primary } [会議メモを見る](https://docs.google.com/document/d/1d3g5bMXupdElCKrdv6rhFNN11mrQgEk-ibA7wdqVLTU/edit){ .md-button .md-button--primary }

## 初期採用者 {: #first-adopters}


🎉 Fares v2の初期採用者に感謝します！実験的機能を公式仕様に追加するための公開投票が開始される前に、少なくとも1つのデータ提供者と1つの利用者が、その実装に取り組むことを確約しなければなりません。これらの組織は、GTFSが継続的に進化することを確実にするため、実験的な変更に多大な時間と労力を投じています。

- 提供者: <a href="https://www.interline.io/" target="_blank">Interline</a>, <a href="https://www.mta.maryland.gov/developer-resources" target="_blank">Maryland Department of Transportation</a>, <a href="https://dot.ca.gov/cal-itp/cal-itp-gtfs" target="_blank">Cal-ITP</a>, <a href="https://trilliumtransit.com/" target="_blank">Trillium Solutions</a>, <a href="https://www.itoworld.com/" target="_blank">ITO World</a>, <a href="https://www.mbta.com/" target="_blank">MBTA</a>, <a href="http://www.pvta.com/" target="_blank">PVTA</a>
- 利用者: <a href="https://transitapp.com/" target="_blank">Transit</a>, <a href="https://www.apple.com/">Apple Maps</a>

## 導入状況トラッカー {: #adoption-tracker}

### 現在 {: #current}


<iframe class="airtable-embed" src="https://airtable.com/embed/shrZzYzPYao7iExlW?backgroundColor=red&viewControls=on" frameborder="0" onmousewheel="" width="100%" height="533" style="background: transparent; border: 1px solid #ccc;"></iframe>

[変更をリクエストする](https://airtable.com/shr8aT0K9bpncmy0V){ .md-button .md-button--primary } [組織を追加する（コンシューマー）](https://airtable.com/shr5B6Pl1r9KH9qMX){ .md-button .md-button--primary } [組織を追加する（プロデューサー）](https://airtable.com/shrn0Afa3TPNkOAEh){ .md-button .md-button--primary }

### 今後 {: #future}

<iframe class="airtable-embed" src="https://airtable.com/embed/shrUrgZTO1noUF66R?backgroundColor=red&viewControls=on" frameborder="0" onmousewheel="" width="100%" height="533" style="background: transparent; border: 1px solid #ccc;"></iframe>

[今後の計画を追加する](https://airtable.com/shrvnI40zuFXmDsQI){ .md-button .md-button--primary }

## 検討中の Fares v2 機能 {: #fares-v2-features-under-discussion}


<iframe class="airtable-embed" src="https://airtable.com/embed/appqkG4yZwMY8b1Bj/shrjS3zRpYOQ6IE8G" frameborder="0" onmousewheel="" width="100%" height="533" style="background: transparent; border: 1px solid #ccc;"></iframe>

## 履歴 {: #history}


- **2017年**: 業界調査およびデータモデリング
- **2021年10月**: <a href="https://github.com/google/transit/pull/286#issue-1026848880" target="_blank">基本実装の草案を作成し共有</a>
- **2021年12月**: <a href="https://github.com/google/transit/pull/286#issuecomment-990258396" target="_blank">公開投票 #1 → 可決されませんでした</a>
- **2022年3月**: <a href="https://github.com/google/transit/pull/286#issuecomment-1080716109" target="_blank">公開投票 #2 → 可決されませんでした</a>
- **2022年5月**: <a href="https://github.com/google/transit/pull/286#issuecomment-1121392932" target="_blank">公開投票 #3 → 可決されました</a>
- **2022年8月**: <a href="https://github.com/google/transit/issues/341" target="_blank">Fares v2 の次フェーズに関するコミュニティでの議論を開始</a>
- **2022年11月**: <a href="https://github.com/google/transit/pull/355" target="_blank">フィードバックを求めるチケットメディアの草案 pull request を公開</a>
- **2022年12月**: <a href="https://github.com/google/transit/issues/341#issuecomment-1339947915" target="_blank">コミュニティが、反復作業で優先すべき機能の優先順位を特定</a>
- **2023年3月**: <a href="https://github.com/google/transit/pull/355#issuecomment-1468326858" target="_blank">チケットメディアが可決されました</a>
- **2023年7月**: <a href="https://github.com/google/transit/pull/357#issuecomment-1653561813" target="_blank">時刻／曜日によって変動する運賃が可決されました</a>
- **2023年11月**: <a href="https://github.com/google/transit/pull/405#issuecomment-1830665141" target="_blank">ネットワークを定義するための専用ファイル</a>
