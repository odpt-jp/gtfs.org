# Python GTFS-realtime 言語バインディング {: #python-gtfs-realtime-language-bindings}


[![PyPI バージョン](https://badge.fury.io/py/gtfs-realtime-bindings.svg)](http://badge.fury.io/py/gtfs-realtime-bindings)

[GTFS-realtime](https://github.com/google/transit/tree/master/gtfs-realtime) Protocol Buffer 仕様から生成された Python クラスを提供します。これらのクラスを使用すると、バイナリ Protocol Buffer GTFS-realtime データフィードを Python オブジェクトに解析できます。

## 依存関係を追加する {: #add-the-dependency}


自身のプロジェクトで `gtfs-realtime-bindings` クラスを使用するには、まず [PyPI repository](https://pypi.python.org/pypi/gtfs-realtime-bindings) からモジュールをインストールする必要があります。

```
# easy_install を使用する場合
easy_install --upgrade gtfs-realtime-bindings

# pip を使用する場合
pip install --upgrade gtfs-realtime-bindings
```

## コード例 {: #example-code}


以下のコードスニペットは、特定の URL から GTFS-realtime データフィードをダウンロードし、それを FeedMessage（GTFS-realtime スキーマのルート型）として解析し、結果を反復処理する方法を示しています。

```python
from google.transit import gtfs_realtime_pb2
import requests

feed = gtfs_realtime_pb2.FeedMessage()
response = requests.get('URL OF YOUR GTFS-REALTIME SOURCE GOES HERE')
feed.ParseFromString(response.content)
for entity in feed.entity:
  if entity.HasField('trip_update'):
    print(entity.trip_update)
```

[gtfs-realtime.proto](https://github.com/google/transit/blob/master/gtfs-realtime/proto/gtfs-realtime.proto) から生成される Python クラスの命名規則の詳細については、Protocol Buffers 開発者サイトの [Python Generated Code](https://developers.google.com/protocol-buffers/docs/reference/python-generated) セクションを参照してください。
