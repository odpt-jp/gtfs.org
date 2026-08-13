# :material-escalator: 構内通路(pathway) {: #material-escalator-pathways}


構内通路(pathway)機能は、大規模な交通駅をモデル化し、乗客を駅の入口および出口から、交通車両に乗車または降車する場所まで案内できます。これらの機能の一部により、経路の物理的特性や推定移動時間、ならびに駅で採用されている実世界の案内システムを伝達できます。

## 構内通路の接続 {: #pathway-connections}


構内通路(pathway)は、その基本的なレベルにおいて、駅構内のLocation Typesで定義された主要エリアを接続する基本機能を提供します。これらの接続は構内通路を形成し、利用者が正確な経路案内（例：入口から乗車エリアまで）を取得できるようにします。これは特に、大規模で複雑な交通駅構内を移動する際に有用です。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[pathways.txt](../../../documentation/schedule/reference/#pathwaystxt)|`pathway_id`, `from_stop_id`, `to_stop_id`, `pathway_mode`, `is_bidirectional` |

**前提条件**:

- [基本機能](../base)
- [Location Types](../base_add-ons/#location-types)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルでは、事前に設定された場所（停留所等(stop)として定義）間の複数の接続（構内通路(pathway)とも呼ばれます）を定義しています：通路（`pathway_mode=1`）、階段（`pathway_mode=2`）、および運賃ゲート（`pathway_mode=6`）。双方向性も指定されています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#pathwaystxt"><b>pathways.txt</b></a> <br>
        </p>

        | pathway_id | from_stop_id | to_stop_id | pathway_mode | is_bidirectional |
        |------------|--------------|------------|--------------|------------------|
        | MainSt-001 | A102_E01     | A102_S01   |            1 |                1 |
        | MainSt-002 | A102_S01     | A102_S02   |            2 |                1 |
        | MainSt-003 | A102_S02     | A102_F02   |            1 |                1 |
        | MainSt-004 | A102_F02     | A102_F01   |            6 |                1 |
        | MainSt-005 | A102_F01     | A102_S03   |            1 |                1 |
        | MainSt-006 | A102_S03     | A102_S04   |            2 |                1 |
        | MainSt-007 | A102_F01     | A102_S05   |            1 |                1 |
        | MainSt-008 | A102_S05     | A102_S06   |            2 |                1 |
        | MainSt-009 | A102_S04     | A102_B01   |            1 |                1 |
        | MainSt-010 | A102_S06     | A102_B02   |            1 |                1 |

## 構内通路の詳細 {: #pathway-details}


駅の構内通路の物理的特性について、長さ、幅、勾配（スロープの場合）、または段数（階段の場合）などの詳細を追加できます。これにより、乗客は通行する必要がある構内通路の状況およびアクセシビリティを事前に把握できます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[pathways.txt](../../../documentation/schedule/reference/#pathwaystxt)|`max_slope`, `min_width`, `length`, `stair_count`|

**前提条件**:

- [基本機能](../base)
- [構内通路の接続](#pathway-connections)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルでは、あらかじめ設定された構内通路に対して、最小幅、階段の段数、通路の長さおよび最大勾配を含む追加の詳細を定義しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#pathwaystxt"><b>pathways.txt</b></a> <br>
        </p>

        | pathway_id | max_slope | min_width | length | stair_count |
        |------------|-----------|-----------|--------|-------------|
        | MainSt-001 |         0 |       4.3 |    3.6 |             |
        | MainSt-002 |           |       2.2 |        |          15 |
        | MainSt-003 |      0.06 |         4 |    3.1 |             |
        | MainSt-004 |           |       0.9 |    2.9 |             |
        | MainSt-005 |         0 |       3.5 |      5 |             |
        | MainSt-006 |           |       2.2 |        |          18 |
        | MainSt-007 |         0 |       3.5 |      5 |             |
        | MainSt-008 |           |       2.2 |        |          18 |
        | MainSt-009 |         0 |         6 |      2 |             |
        | MainSt-010 |         0 |         6 |      2 |             |

## 階層 {: #levels}


階層は、駅構内のすべての異なる階層を一覧化するために使用でき、利用者に停留所等(stop)に関する追加の情報レイヤーを提供します。この機能により、構内通路(pathway)接続機能と組み合わせてエレベーターを使用することも可能になります。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[levels.txt](../../../documentation/schedule/reference/#levelstxt)|`level_id`、`level_index`、`level_name`|
|[stops.txt](../../../documentation/schedule/reference/#stopstxt)|`level_id`|

**前提条件**:

- [基本機能](../base)
- [位置タイプ](../base_add-ons/#location-types)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、駅構内の異なる階層を示しています。位置（stopsとして定義）は、その対応する階層に割り当てられます。 
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#stopstxt"><b>stops.txt</b></a> <br>
        </p>

        | level_id          | level_index | level_name |
        |-------------------|-------------|------------|
        | level_0_street    |           0 | 地上  |
        | level_-1_lobby    |          -1 | ロビー      |
        | level_-2_platform |          -2 | プラットフォーム   |


    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#pathwaystxt"><b>pathways.txt</b></a> <br>
        </p>

        | stop_id      | level_id |
        |--------------|----------|
        | Station_A102 |          |
        | A102_B01     |       -2 |
        | A102_B02     |       -2 |
        | A102_E01     |        0 |
        | A102_S01     |        0 |
        | A102_S02     |       -1 |
        | A102_S03     |       -1 |
        | A102_S04     |       -2 |
        | A102_S05     |       -1 |
        | A102_S06     |       -2 |
        | A102_F01     |       -1 |
        | A102_F02     |       -1 |

## 駅構内移動時間 {: #in-station-traversal-time}


駅構内移動時間は、駅構内案内に追加の詳細情報を提供し、利用者が駅を移動するために必要な推定時間を示すことで、より良い経路案内と所要時間を実現します。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[pathways.txt](../../../documentation/schedule/reference/#pathwaystxt)|`traversal_time`|

**前提条件**: 

- [基本機能](../base)
- [構内通路接続](#pathway-connections)

??? note "サンプルデータ"

    <p style="font-size:16px">
    次のサンプルは、各構内通路(pathway)を移動するために必要な推定所要時間（秒）を示しています。
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#pathwaystxt"><b>pathways.txt</b></a> <br>
        </p>

        | pathway_id | traversal_time |
        |------------|----------------|
        | MainSt-001 |              3 |
        | MainSt-002 |             20 |
        | MainSt-003 |              2 |
        | MainSt-004 |              2 |
        | MainSt-005 |              4 |
        | MainSt-006 |             25 |
        | MainSt-007 |              4 |
        | MainSt-008 |             25 |
        | MainSt-009 |              2 |
        | MainSt-010 |              2 |

## 構内通路標識 {: #pathway-signs}


構内通路標識は、経路検索ツールに表示される情報と現実世界の標識を結び付けることができます。これがフィードで表現されている場合、経路検索ツールは「〜への標識に従ってください」のような案内を提供できます。

| 含まれるファイル                   | 含まれるフィールド   |
|----------------------------------|-------------------|
|[pathways.txt](../../../documentation/schedule/reference/#pathwaystxt)|`signposted_as`, `reversed_signposted_as`|

**前提条件**:

- [基本機能](../base)
- [構内通路接続](#pathway-connections)

??? note "サンプルデータ"

    <p style="font-size:16px">
    以下のサンプルは、あらかじめ設定された構内通路に関連するナビゲーション情報を指定しており、駅の物理的な標識に表示されるテキストを反映しています。 
    </p>
    !!! note ""
        <p style="font-size:16px">
        <a href="../../../documentation/schedule/reference/#pathwaystxt"><b>pathways.txt</b></a> <br>
        </p>

        | pathway_id | signposted_as    | reversed_signposted_as |
        |------------|------------------|------------------------|
        | MainSt-001 | ロビーへ         | 出口                   |
        | MainSt-002 |                  |                        |
        | MainSt-003 | プラットフォームへ | 出口                   |
        | MainSt-004 |                  |                        |
        | MainSt-005 | 西行き列車       | 出口                   |
        | MainSt-006 |                  |                        |
        | MainSt-007 | 東行き列車       | 出口                   |
        | MainSt-008 |                  |                        |
        | MainSt-009 | 西行き列車       | ロビーへ               |
        | MainSt-010 | 東行き列車       | ロビーへ               |
