<div class="hollow-nested" markdown="1">

# ガイドライン {: #guidelines}


このセクションでは、Service Alerts feed の使用に関する一般的な実践を提案します。

## 一般ガイドライン {: #general-guidelines}


* 影響を受けるサービスが同一のインシデントに起因し、同一の影響下にある場合、1つの運行情報(alert)に複数の影響対象サービス（ルート・路線系統(route)、停留所等(stop)など）を含めることができます（例：同一のイベントにより運休となった複数のルート・路線系統(route)に対する同一の `NO_SERVICE` 運行情報(alert)、または同一の工事により一括して移設された複数の停留所等(stop)に対する同一の `STOP_MOVED` 運行情報(alert)）。これにより、同一の運行情報(alert)の下で乗客向けの情報を集約できます。  
* `communication_period`、`impact_period`、`cause`、および `effect` は必須フィールドではありませんが、運行情報(alert)に関する情報を可能な限り多く提供するため、すべての運行情報(alert)にこれらのフィールドを含めることが推奨されます。  
* 運行情報(alert)の原因に関する追加情報や影響範囲など、より詳細な情報が存在する場合は、`cause_detail` および `effect_detail` を使用して詳述することができます（注：`cause_detail` および `effect_detail` は[実験的フィールド](https://gtfs.org/community/governance/gtfs-realtime-amendment-process/#experimental-fields)です）。  
* 迂回経路の詳細な図など、運行情報(alert)の詳細を掲載した Web ページが存在する場合は、`url` フィールドに含めることができます。  
* サービスの中断が事前に判明している場合は、開始時刻より前にフィードへ追加してください。  
* 運行情報(alert)がまだ有効になっていない場合は、*「今後予定：」* などの注記をヘッダーに追加することを検討してください。乗客が確認する時点で常に正確となるよう、ヘッダーを更新することを忘れないでください。  
* 運行情報(alert)の `communication_period`（または `active_period`）が終了したら、フィードから運行情報(alert)を削除してください。インシデントが終了したことを乗客に通知する `NO_EFFECT` の影響を持つ運行情報(alert)を設定することもできます。ただし、これは必須ではありません。

## GTFS Schedule の優先 {: #prioritization-of-gtfs-schedule}


* サービスの中断が計画されている場合は、サービス変更を [GTFS Schedule feed](https://gtfs.org/documentation/schedule/reference/) に組み込むようにしてください（例: 停留所等(stop)が再び運用されるまで、該当する便(trip)の [`stop_times.txt`](https://gtfs.org/documentation/schedule/reference/#stop_timestxt) から閉鎖された停留所等(stop)を削除します）。今後予定されている中断または調整をユーザーに通知するための補完として、GTFS Realtime Service Alerts を引き続き使用することができます。  
* 運行情報(alert)が計画外の短期的な事象（通常は1週間未満）によるものである場合、対応する GTFS Schedule を調整する必要はありません。  
* 計画外の運行情報(alert)がより長期間継続する場合は、GTFS Realtime Service Alerts を引き続き使用し、その事象が1か月を超えて継続すると予想される場合は、サービス変更を GTFS Schedule feed に組み込むことを検討してください。

GTFS Schedule と GTFS-RT のトレードオフに関するより詳細な情報については、**[Realtime vs Schedule Best Practices document](../../../resources/mobilitydata-recommendations/gtfs-schedule-vs-gtfs-realtime.md)** を参照してください。

## 原因と影響 {: #cause-effect}


* 運行情報のヘッダーまたは説明にその情報が含まれている場合でも、`cause` および `effect` は空にするべきではありません。  
* 可能な場合は、具体的な影響を含めてください。不明または一般的な影響（`UNKNOWN_EFFECT`、`OTHER_EFFECT`）の数を最小限に抑えるようにしてください。これにより、旅程検索ツールはヘッダーまたは説明を分析することなく、サービス中断の性質を抽出できます。

## 通知期間および影響期間（以前の有効期間） {: #communication-period-and-impact-period-previously-active-period}


!!! 注記
    フィールド `active_period` は、運行情報の対象となる期間を指定するために使用されていました。しかし、その使用に関する曖昧さを解消するため、[PR\#546](https://github.com/google/transit/pull/546) で非推奨となりました。後方互換性の理由から、引き続き `active_period` を使用することができます。

    `communication_period` および `impact_period` の使用を推奨します。円滑に移行するため、[こちら](https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/examples/migration-active-period.md)の移行ガイドを参照してください。

`communication_period` および `impact_period` に固有のガイドライン:

* 通知期間は、純粋に情報提供を目的として運行情報をユーザーに表示するべき時間です。  
* 影響期間は、実際に現地でサービスが運行情報の影響を受ける時間です。 
* サービスの中断が有効であるすべての時間に運行情報が乗客に通知されるよう、各影響期間が少なくとも1つの通知期間に含まれていることを常に確認してください。

一般的な期間のガイドライン:

* 運行情報の終了時刻が判明している場合は、それを影響期間/有効期間に含めてください。そうでない場合は、終了時刻を空のままにし、判明した時点で必ず含めてください。  
* 運行情報が繰り返される場合は、複数の時間間隔を作成してください。`communication_period` および `impact_period` を使用する場合は、単一の連続した通知期間と、影響期間内の複数の時間間隔を維持することができます。これは、中断が発生していない場合でも乗客が情報を利用できるようにするためです。

## 通知対象エンティティ {: #informed-entity}


!!! Tip

    この理論的なセクションを実践的な例で補足するには、次のセクションの通知対象エンティティに関する[実際のユースケース](../../../../resources/mobilitydata-recommendations/gtfs-realtime-service-alerts/real-life-use-cases)を参照してください。


* インシデントが発生した場合は、まず可能な限り詳細なエンティティを指定して運行情報(alert)を設定することを検討し、その後、より上位レベルのエンティティに対する追加の運行情報(alert)が必要かどうかを評価してください。  
<br>**例**: 複数の地下鉄ルート・路線系統(route)が乗り入れる駅が、保守作業のため、地下鉄ルート・路線系統(route)の1つについてのみ閉鎖されます。その特定のルート・路線系統(route)に対応するプラットフォームのみが閉鎖されます。

    * まず、`informed_entity` に地下鉄プラットフォームの `stop_id` と地下鉄の `route_id` を含めた `NO_SERVICE` 運行情報(alert)を作成してください。
    * 次に、他の影響を持つ追加の運行情報(alert)を作成できる場合があります。例えば、上記と同じ例では、保守作業について乗客に通知する追加の `MODIFIED_SERVICE` または `OTHER_EFFECT` 運行情報(alert)を作成し、`informed_entity` に駅の `stop_id` を含めることができます。

* **最も詳細なエンティティを指定しない場合は、運行情報(alert)の影響に注意し、`NO_SERVICE` に設定してはいけません。これは、多くのコンシューマーが実際に `NO_SERVICE` 運行情報(alert)を使用して影響を受けるサービスを提案しない可能性があり、ユーザーに対する不正確な旅程(journey)の提案につながるためです。**  
    <br>**例**: 複数のルート・路線系統(route)が乗り入れる停留所等(stop)が、そのうち1つのルート・路線系統(route)によってのみ通過されます。`route_id` を含めずに `informed_entity` で `stop_id` のみを指定する場合、影響として `NO_SERVICE` を設定してはいけません。これは、特定のコンシューマーが `informed_entity` で提供された情報に基づき、すべてのルート・路線系統(route)について停留所等(stop)を閉鎖すると判断する可能性があるためです。`MODIFIED_SERVICE` などの影響を設定できます。

* 通知対象エンティティが可能な限り詳細であることを確認してください。  
    * 運行情報(alert)が事業者全体に関するものである場合は、`agency_id` を含めてください。  
    * 運行情報(alert)が特定のルート・路線系統(route)に関するものである場合は、`route_id` を含めてください。  
    * 運行情報(alert)が特定の方向に関するものである場合は、`direction_id` を含めてください。  
    * 運行情報(alert)が特定の停留所等(stop)に関するものである場合は、`stop_id` を含めてください。  
    * 運行情報(alert)が特定の方向に関するものであり、影響を受ける停留所等(stop)が GTFS 内で複数の方向に対応している場合は、`stop_id` と `direction_id` の両方を含めてください。  
    * 運行情報(alert)が特定の便(trip)に関するものである場合は、[TripDescriptor](https://gtfs.org/documentation/realtime/reference/#message-tripdescriptor) を使用して `trip_id` を含めてください。

!!! Info

    現在、`direction_id` は依然として実験的なフィールドです。

* 運行情報(alert)の `impact_period`/`active_period` より後に運行期間が開始する、または運行情報(alert)の `impact_period`/`active_period` より前に運行期間が終了するルート・路線系統(route)の `route_ids` を含めてはいけません。

Service Alerts の主要なユースケースと対応する `informed_entity` を含む決定木を、以下のボードに示します。

<div style="position: relative; width: 100%; height: 0; padding-top: 56.25%;
 padding-bottom: 0; box-shadow: 0 2px 8px 0 rgba(63,69,81,0.16); margin-top: 1.6em; margin-bottom: 0.9em; overflow: hidden;
 border-radius: 8px; will-change: transform;">
  <iframe loading="lazy" style="position: absolute; width: 100%; height: 100%; top: 0; left: 0; border: none; padding: 0;margin: 0;"
    src="https://www.canva.com/design/DAGvUIUG_YQ/XrgB7cCqySAB0H4OlMDdjg/view?embed" allowfullscreen="allowfullscreen" allow="fullscreen">
  </iframe>
</div>
<a href="https:&#x2F;&#x2F;www.canva.com&#x2F;design&#x2F;DAGvUIUG_YQ&#x2F;XrgB7cCqySAB0H4OlMDdjg&#x2F;view?utm_content=DAGvUIUG_YQ&amp;utm_campaign=designshare&amp;utm_medium=embeds&amp;utm_source=link" target="_blank" rel="noopener">[MobilityData][Public] informed_entities の決定木</a>

!!! Info

    将来、`pathway_id` などの他のエンティティを含められるように、Informed Entity が拡張される可能性があります。この拡張は、主に [transit repo](https://github.com/google/transit) における継続的なコミュニティの議論、または[ワーキンググループ](https://community.mobilitydata.org/working-groups#events)で開始され、transit repo への提案につながる議論の対象となっています。

## ヘッダーテキスト {: #header-text}


* メッセージを伝えつつ、ヘッダーは可能な限り短くしてください。  
* ヘッダーでは、影響を直接的に記載してください。  
* ヘッダーが長くなりすぎない場合は、運行情報(alert)の期間および原因をヘッダーに追加することができます。  
* ヘッダーテキストに複数の運行情報(alert)を列挙してはいけません。  
* HTML および Markdown の文字とタグは UTF-8 ですが、仕様では現在ヘッダーテキストをプレーンテキストとして定義しているため、ヘッダーテキストでこれらの文字を使用することは推奨されません。  
* 同様に、「en-html」のような言語コードは BCP-47 ではないため、含めないでください。  
* **ヘッダーの例** 
    * ✅ *"運行中断"* ← 改善できます  
    * ✅ *"Blue Line: Snowdon と Acadie 間は運休"* 
    * ✅ *"Orbit Earth Detour between Rio Salado Pkwy./Packard Dr. and Tempe Transportation Center \- Reggae Festival"*  
    * ❌ *"Subway Line A は運休、Subway Line K はバス運行"* ← 複数の運行情報(alert)

## 説明テキスト {: #description-text}


* 同じ運行情報の説明に複数の運行情報を列挙してはいけません。  
* HTML および Markdown の文字とタグは UTF-8 ですが、仕様では現在、説明テキストをプレーンテキストとして定義しているため、説明テキスト内でこれらの文字を使用することは推奨されません。  
* 同様に、「en-html」のような言語コードは BCP-47 ではないため、含めてはいけません。  
* 説明は十分に短く保ちつつ、可能な限り詳細にしてください。

良い例（Halifax Transit）

```json
"descriptionText": {
 "translation": [
   { 
     "text": "停留所等(stop)閉鎖のお知らせ:\r\nルート・路線系統(route): 4 Universities Inbound\r\n場所: Robie St Before Cedar St (8196)\r\n原因:  工事による停留所等(stop)閉鎖\r\n\r\n影響を受ける停留所等(stop):\r\nRobie St Before Cedar St (8196)",
     "language": "en"
   }
 ] 
} 
```

悪い例（MBTA）: 運行情報はヘッダー内で必要なすべての情報に言及しており、説明には方向のみを残しています。

```json
"headerText": {
 "translation": [
   { 
     "text": "Haverhill Line Train 1231（North Station 発 10:25 am）は、信号の問題により、North Station と Reading の間で予定より 10～20 分遅れて運行しています。",
     "language": "en"
   }
 ]
},
"descriptionText": {
 "translation": [
   {
     "text": "影響を受ける方向: Outbound",
     "language": "en"
   }
 ]
}
```

## URL {: #url}


* URLが、運行情報(alert)の詳細を示すページ、またはそれに直接関連する情報を含むページに遷移することを確認してください。例えば、URLは迂回経路または移設された停留所等(stop)の図を含むページに遷移します。  
* 一般的なURLや、ユーザーが運行情報(alert)の詳細を見つけるためにさらに操作しなければならないページに遷移するURLは避けてください。

## 画像（実験的） {: #image-experimental}


* image フィールドには、運行情報(alert)をさらに詳述する画像へのリンクとなる URL が含まれます。例には、地図上の迂回経路のルート形状(shape)、移設された停留所等(stop)の新しい位置などが含まれます。  
* 画像には、過度に多くのテキストを含めてはいけません。  
* 画像には、運行情報(alert)のヘッダーまたは説明に記載されていない新しいテキスト情報を含めるべきではありません。

## 疑わしい運行情報および曖昧な運行情報の取り扱い {: #handling-suspicious-and-ambiguous-alerts}


一部の運行情報(alert)は曖昧または不完全です。これらの運行情報の例には、以下が含まれます。

* 運行サービスの中断の開始時刻および／または予見可能な終了時刻を示す `impact_period`/ `active_period` がない運行情報。  
* informed entity がない、または事業者全体の `agency_id` を informed entity とする `NO_SERVICE` 運行情報。  
* 区間の閉鎖を説明しているものの、informed entity に `route_id` のみが含まれる `NO_SERVICE` 運行情報。  
* プラットフォームの閉鎖を説明しているものの、駅全体の `stop_id` が提供されている `NO_SERVICE` 運行情報。  
* 一部の便(trip)の運休を説明しているものの、informed entity に `route_id` のみが含まれる `NO_SERVICE` 運行情報。  
* 停留所等(stop)の位置が変更されているものの、informed entity に `route_id` のみが含まれる `DETOUR` 運行情報。

このようなケースは、消費者によるデータの解釈を妨げる不完全な情報を示しています。

* プロデューサーは、不完全または曖昧な解釈につながる慣行を避け、可能な限り詳細な informed entity および active period を含めることが推奨されます。  
* プロデューサーが運行情報に最も詳細な informed entity を含めない場合、effect として `NO_SERVICE` を使用しないことが推奨されます。**これは、一部の消費者が `NO_SERVICE` を含む運行情報を使用して経路案内の提案に影響を与えることを決定するためです。この慣行は、詳細でない informed entity と組み合わさることで、誤った運行サービスの停止につながる可能性があります。**  
* 消費者は、旅程(journey)計画に影響を与えるために Service Alerts を使用することを決定する場合、十分に注意することが推奨されます。消費者が経路案内に影響を与えるために運行情報を使用する場合、運行情報の説明を監視し、運行情報に不完全な情報が含まれている場合は事業者に連絡するべきです。  
* **消費者が経路案内に影響を与えるために Service Alerts を使用することを選択する場合、`NO_SERVICE` 以外の運行情報の effect を使用して経路案内に影響を与えることは、重大なリスクを伴い意図しない結果につながる可能性があるため、強く推奨されません。**  
* プロデューサーは、[TripModifications](https://gtfs.org/documentation/realtime/feed-entities/trip-modifications/) を実装し、すべての迂回をそこに含めることが推奨されます。

</div>
