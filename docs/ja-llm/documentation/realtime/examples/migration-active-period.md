## 移行ガイド - active_period から communication_period および impact_period への移行と影響 {: #migration-guide-transition-from-active_period-to-communication_period-and-impact_period}


GTFS-realtime の `alert.active_period` は、*「運行情報(alert)をユーザーに表示するべき時刻。存在しない場合、運行情報(alert)はフィードに存在する限り表示されます。複数の範囲が指定されている場合、運行情報(alert)はそれらすべての期間中に表示されます。」* と定義されていました。

この定義には、「運行情報(alert)をユーザーに表示するべき時刻」が、以下の両方として解釈できるという曖昧さがありました。
- ユーザーに運行情報(alert)が通知される時刻。
- 運行情報(alert)によって生じるサービスの混乱が有効となる時刻。

この曖昧さを解消するため、`communication_period` および `impact_period` が導入されます。
- `communication_period`: 情報提供のみを目的として、運行情報(alert)をユーザーに表示するべき時刻。
- `impact_period`: サービスが運行情報(alert)の影響を受ける時刻。

後方互換性を維持しつつ、新しいフィールドの実装を確実にし、インフラストラクチャコストを考慮するため、コミュニティは `active_period` を非推奨として指定しつつ、`communication_period` および `impact_period` と共存できるようにすることに合意しました。

この移行ガイドでは、3つすべてのフィールドが共存する場合の解釈方法を定義し、新しいフィールドである `communication_period` および `impact_period` へ段階的に移行するための手順を概説します。目標は、プロデューサーとコンシューマーに対し、`active_period` の代わりに `communication_period` および `impact_period` を段階的に使用し始めるよう促すことです。

## プロデューサー {: #producers}

プロデューサーは、引き続き3つのフィールドすべてを同じ運行情報(alert)に含めることができます。可能な限り多くの運行情報(alert)、特にNO_SERVICEを伴う運行情報(alert)で、`communication_period`および`impact_period`を指定するようにしてください。 

**新しいフィールドからactive_periodを分離するために、運行情報(alert)を複製してはいけません！** 実際のサービス障害ごとに1つの運行情報(alert)を設定し、`communication_period`、`impact_period`、および`active_period`を指定することができます。

運行情報(alert)に`communication_period`および`impact_period`が指定されている場合、`active_period`は含めないことが推奨されます（すでに任意のフィールドです）。

ベストプラクティスを促進するため、可能な限り`communication_period`および`impact_period`を一緒に指定することが推奨されます。

以下の例はすべて有効です。

### 推奨されるオプション {: #the-recommended-option}


````
alert {
“communication_period”: [{ "start": …, "end": … } ], ← ユーザーに運行情報(alert)が通知される時刻。
"impact_period": [ { "start": …, "end": … } ], ← 運行情報(alert)によるサービス中断が有効となる時刻。サービス中断が繰り返される場合は、複数の期間にすることができます。
...
}
````

### 推奨されないその他の有効なオプション {: #other-valid-options-that-are-not-recommended}


````
alert {
“active_period”: [{ "start": …, "end": … } ],
“communication_period”: [{ "start": …, "end": … } ],
"impact_period": [ { "start": …, "end": … } ],
...
} 
````


````
alert {
“active_period”: [{ "start": …, "end": … } ],
"impact_period": [ { "start": …, "end": … } ],
...
} 
````


````
alert {
“active_period”: [{ "start": …, "end": … } ],
“communication_period”: [{ "start": …, "end": … } ],
...
} 
````


````
alert {
“active_period”: [{ "start": …, "end": … } ],
...
} 
````


````
alert {
“communication_period”: [{ "start": …, "end": … } ],
...
} 
````


````
alert {
“impact_period”: [{ "start": …, "end": … } ],
...
} 
````

`active_period` の使用が設定された期限までに非推奨となること、およびコンシューマーが代わりに `commnication_period` と `impact_period` の使用を開始するべきであることを、既存のコンシューマーに通知することが提案されます（例: 開発者メーリングリスト経由）。この移行ガイドを含め、特に *「Consumer」* セクションを強調するべきです。期限を過ぎた後は、フィードから `active_period` エンティティを削除し、`commnication_period` と `impact_period` のみを公開することができます。

## コンシューマ {: #consumers}


コンシューマは、仕様定義に基づいてフィールドを解釈することができます。
- `active_period` が `communication_period` および `impact_period` とともに存在する場合、**`active_period` を無視し**、他の2つのフィールドを使用してください。

- `active_period` が `impact_period` とともに存在する場合、**`active_period` を無視し**、これを `communication_period` として解釈してはいけません。

- `active_period` が `communication_period` とともに存在する場合、**`active_period` を無視し**、これを `impact_period` として解釈してはいけません。

- `active_period` が単独で存在する場合は、それを使用してください。
