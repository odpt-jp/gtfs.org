# JavaScript GTFS-realtime 言語バインディング {: #javascript-gtfs-realtime-language-bindings}


[![npm version](https://badge.fury.io/js/gtfs-realtime-bindings.svg)](http://badge.fury.io/js/gtfs-realtime-bindings)

[GTFS-realtime](https://github.com/google/transit/tree/master/gtfs-realtime) Protocol Buffer 仕様から生成された JavaScript クラスおよび関連する型を提供します。これらのクラスにより、バイナリ Protocol Buffer GTFS-realtime データフィードを JavaScript オブジェクトに解析できます。

これらのバインディングは [Node.js](http://nodejs.org/) 環境で使用するように設計されていますが、多少の作業を行えば、他の JavaScript 環境でも使用できる可能性があります。

JavaScript Protocol Buffer のサポートには、[ProtoBuf.js](https://github.com/dcodeIO/ProtoBuf.js) ライブラリを使用しています。

## 依存関係を追加する {: #add-the-dependency}


自身のプロジェクトで `gtfs-realtime-bindings` クラスを使用するには、まず [Node.js npm package](https://www.npmjs.com/package/gtfs-realtime-bindings) をインストールする必要があります。

```
npm install gtfs-realtime-bindings
```

## コード例 {: #example-code}


以下の Node.js コードスニペットは、特定の URL から GTFS-realtime データフィードをダウンロードし、それを FeedMessage（GTFS-realtime スキーマのルート型）として解析して、結果を反復処理する方法を示しています。

この例を動作させるには、まず NPM を使用して `node-fetch` をインストールしなければなりません。

_注: この例では ES modules（`import`/`export` 構文）を使用しており、CommonJS（`require` 構文）とは互換性がありません。`import` を `require` に変換し、`node-fetch@2` をインストールすることで CommonJS を使用できます。ES modules の詳細については[こちら](https://nodejs.org/api/esm.html)を参照してください。_

```javascript
import GtfsRealtimeBindings from "gtfs-realtime-bindings";
import fetch from "node-fetch";

(async () => {
  try {
    const response = await fetch("<GTFS-realtime source URL>", {
      headers: {
        "x-api-key": "<redacted>",
        // ご利用の GTFS-realtime source の認証トークンに置き換えてください
        // 例: x-api-key は NY の MTA GTFS APIs で使用されるヘッダー値です
      },
    });
    if (!response.ok) {
      const error = new Error(`${response.url}: ${response.status} ${response.statusText}`);
      error.response = response;
      throw error;
      process.exit(1);
    }
    const buffer = await response.arrayBuffer();
    const feed = GtfsRealtimeBindings.transit_realtime.FeedMessage.decode(
      new Uint8Array(buffer)
    );
    feed.entity.forEach((entity) => {
      if (entity.tripUpdate) {
        console.log(entity.tripUpdate);
      }
    });
  }
  catch (error) {
    console.log(error);
    process.exit(1);
  }
})();
```

[gtfs-realtime.proto](https://github.com/google/transit/blob/master/gtfs-realtime/proto/gtfs-realtime.proto) から生成される JavaScript クラスの命名規則の詳細については、Protocol Buffer serialization の処理に使用している [ProtoBuf.js project](https://github.com/dcodeIO/ProtoBuf.js/wiki)を確認してください。
