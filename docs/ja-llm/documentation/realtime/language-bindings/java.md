# Java GTFS-realtime 言語バインディング {: #java-gtfs-realtime-language-bindings}


![Maven Central Version](https://img.shields.io/maven-central/v/org.mobilitydata/gtfs-realtime-bindings.svg)

[GTFS-realtime](https://github.com/google/transit/tree/master/gtfs-realtime) Protocol Buffer 仕様から生成された Java クラスを提供します。これらのクラスにより、バイナリ Protocol Buffer GTFS-realtime データフィードを Java オブジェクトに解析できます。

## 依存関係を追加する {: #add-the-dependency}


自身のプロジェクトで `gtfs-realtime-bindings` クラスを使用するには、適切な依存関係を追加する必要があります。モジュールは [Maven Central Repository](http://search.maven.org/) に公開されているため、Maven、Ivy、Gradle などの Java ビルドツールから容易に参照できます。

[Maven](http://maven.apache.org/) の場合、`pom.xml` の dependencies セクションに以下を追加してください。

```xml
<dependency>
  <groupId>org.mobilitydata</groupId>
  <artifactId>gtfs-realtime-bindings</artifactId>
  <version>0.0.8</version>
</dependency>
```

[Gradle](https://www.gradle.org/) の場合、`build.gradle` の dependecies セクションに以下を追加してください。

```
implementation group: 'org.mobilitydata', name: 'gtfs-realtime-bindings', version: '0.0.8'
```

Maven central repository がプロジェクトから参照されていることを確認してください。

## コード例 {: #example-code}


以下のコードスニペットは、特定の URL から GTFS-realtime データフィードをダウンロードし、それを FeedMessage（GTFS-realtime schema のルート型）として解析して、結果を反復処理する方法を示しています。

```java
import java.net.URL;

import com.google.transit.realtime.GtfsRealtime.FeedEntity;
import com.google.transit.realtime.GtfsRealtime.FeedMessage;

public class GtfsRealtimeExample {
  public static void main(String[] args) throws Exception {
    URL url = new URL("URL OF YOUR GTFS-REALTIME SOURCE GOES HERE");
    FeedMessage feed = FeedMessage.parseFrom(url.openStream());
    for (FeedEntity entity : feed.getEntityList()) {
      if (entity.hasTripUpdate()) {
        System.out.println(entity.getTripUpdate());
      }
    }
  }
}
```

[gtfs-realtime.proto](https://github.com/google/transit/blob/master/gtfs-realtime/proto/gtfs-realtime.proto) から生成される Java クラスの命名規則の詳細については、Protocol Buffers 開発者サイトの [Java Generated Code](https://developers.google.com/protocol-buffers/docs/reference/java-generated) セクションを参照してください。

## プロジェクト履歴 {: #project-history}

### `0.0.4` 以前 {: #004-and-lower}

このプロジェクトは元々 Google によって作成されました。`0.0.4` およびそれ以前のバージョンは、Group ID `com.google.transit` のもとで、[Maven Central のこちら](https://search.maven.org/search?q=g:com.google.transit%20AND%20a:gtfs-realtime-bindings)からダウンロードできます。

### `0.0.5` {: #005}

MobilityData は2019年初頭にプロジェクトの保守を開始し、当初は JCenter を通じてリリース成果物を公開していました。バージョン `0.0.5` は、Group ID `io.mobilitydata.transit` のもとで、[Maven Central のこちら](https://search.maven.org/artifact/io.mobilitydata.transit/gtfs-realtime-bindings)からダウンロードできます。

### `0.0.6` および `0.0.7` {: #006-and-007}

JCenter は2021年に[終了しました](https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/)。終了前に、同期の問題によりバージョン `0.0.6` および `0.0.7` は JCenter から Maven Central へ同期されなかったため、現在これらのバージョンではアーティファクトを直接ダウンロードできません。ただし、[tags](https://github.com/MobilityData/gtfs-realtime-bindings/tags) からコマンド `mvn package` を使用して、自身でコンパイルすることができます。

### `0.0.8` 以降 {: #008-and-higher}

2022年に、MobilityData は Group ID `org.mobilitydata` の下で Maven Central に直接アーティファクトを公開する方式へ移行しました。ここでバージョン 0.0.8 以降が公開されています。
