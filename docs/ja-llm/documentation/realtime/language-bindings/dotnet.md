# .NET GTFS-realtime 言語バインディング {: #net-gtfs-realtime-language-bindings}


[![NuGet version](https://badge.fury.io/nu/GtfsRealtimeBindings.svg)](http://badge.fury.io/nu/GtfsRealtimeBindings)

[GTFS-realtime](https://github.com/google/transit/tree/master/gtfs-realtime) Protocol Buffer 仕様から生成された .NET クラスを提供します。これらのクラスにより、バイナリの Protocol Buffer GTFS-realtime データフィードを C# オブジェクトとして解析できます。

## 依存関係を追加する {: #add-the-dependency}


自身のプロジェクトで `gtfs-realtime-bindings` クラスを使用するには、まず [NuGet repository](https://www.nuget.org/packages/GtfsRealtimeBindings/) からモジュールをインストールする必要があります。

```
Install-Package GtfsRealtimeBindings
```

## コード例 {: #example-code}


以下のコードスニペットは、特定の URL から GTFS-realtime データフィードをダウンロードし、それを FeedMessage（GTFS-realtime スキーマのルート型）として解析し、結果を反復処理する方法を示しています。

```csharp
using System.Net;
using ProtoBuf;
using TransitRealtime;

WebRequest req = HttpWebRequest.Create("URL OF YOUR GTFS-REALTIME SOURCE GOES HERE");
FeedMessage feed = Serializer.Deserialize<FeedMessage>(req.GetResponse().GetResponseStream());
foreach (FeedEntity entity in feed.Entities) {
  ...
}
```
