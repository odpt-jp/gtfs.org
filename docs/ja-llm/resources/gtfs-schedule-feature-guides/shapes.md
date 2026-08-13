# ルート形状(shape) {: #shapes}

## Shape データのガイダンス {: #shapes-data-guidance}


shapes.txt ファイルに含まれるデータは、公共交通サービスの表現において重要な役割を果たします。適切に構築された shape は、経路検索アプリケーションにおける便(trip)の可視化の正確性を向上させ、乗客にシームレスな体験を提供します。以下の推奨プラクティスは、車両の移動を正確に表現し、実際の走行経路と一致する高品質な shape データを作成するためのガイダンスを提供します。

1. 停留所等(stop)間の走行経路が直線ではない場合、2つのポイントのみで shape を定義することは避けてください。shape が車両の走行経路を正確に反映していることを確認してください。

    <img class="center" width="1500" height="100%" src="../../../../assets/shapes-1.png">


2. shape が出発駅から始まり、到着駅で終わることを確認してください。短すぎる、または長すぎる shape は避けてください。

    <img class="center" width="1500" height="100%" src="../../../../assets/shapes-2a.png">

    <img class="center" width="1500" height="100%" src="../../../../assets/shapes-2b.png">

3. 単一の shape_id で定義された shape 内では、非論理的な逆戻りや不要なポイントの折り返しを避けてください。

    <img class="center" width="1500" height="100%" src="../../../../assets/shapes-3.png">

4. shape が WGS84 座標系において車両が走行する経路に沿っていることを確認し、異なる座標系によって生じるずれを避けてください。

    - 道路上のサービス（例: バス）の場合、車両が走行する道路の中心線に沿うべきです。指定車線がない場合は道路の中心線、車線が指定されている場合は進行方向側の中心線のいずれかとすることができます。

        <img class="center" width="1500" height="100%" src="../../../../assets/shapes-4a.png">

    - 鉄道サービス（例: 地下鉄、列車、ライトレール）の場合、shape は列車が走行する線路に沿うべきです。便(trip)が特定の区域において常に特定の線路を走行するとは限らず、複数の線路が存在する場合、shape が列車の運行可能な線路の範囲内に収まることを確認してください。

        <img class="center" width="1500" height="100%" src="../../../../assets/shapes-4b.png">

5. 車両が曲線に沿って走行する場合、乗客にとって視覚的に滑らかな表示を可能にするため、shape のポイントは十分に密であるべきです。

    <img class="center" width="1500" height="100%" src="../../../../assets/shapes-5.png">

6. 経路は、縁石側の停留所等(stop)、プラットフォーム、または乗車場所へ「ぎざぎざ」に接続してはいけません。

    <img class="center" width="1500" height="100%" src="../../../../assets/shapes-6.png">
