# 運賃乗換 {: #fare-transfers}


*主なファイル: fare_leg_rules.txt、fare_transfer_rules.txt*  
*例: [Translink（バンクーバー）](../intro/#translink-vancouver)*

!!! info "注意"

    運賃乗換は、乗車区間(leg)間の乗換時に適用されるルールを定義するために使用されます。旅程(journey)の総費用は、個々の乗車区間(leg)に適用されるチケット商品と、それらを接続する乗換に適用されるチケット商品から算出されます。詳細については、Introductionページの[機能のセクション](../intro/#fares-features-and-their-files)を再確認してください。

!!! info "注意"

    TransLinkの場合:

    * 乗換は、運賃認証後90分間有効です。  
    * この時間内のバスからバスへの乗換は無料です。  
    * 同じ運賃ゾーン番号内（元の運賃で対応している場合）の乗換も無料です。  
    * 乗客がより高い運賃のゾーンへ乗り換える場合、AddFareとして知られる差額のみを支払います。

!!! Note

    このセクションには、Contactless運賃のみの例が含まれています。他のチケットメディアの種類をサポートするには、該当する`fare_products.txt`の行を複製し、amountおよび`fare_media_id`フィールドを適宜更新してください。

## 乗換チケット商品を作成する {: #create-transfer-fare-products}


乗換チケット商品は、無料ではない乗換の費用を表します。無料乗換を作成する必要はありません。乗換チケット商品は、以下のように `fare_products.txt` で作成します。

1. **fare_product_id** 列に、チケット商品を識別する一意の ID を入力します。  
2. **fare_product_name** 列に、乗客向けのチケット商品名（例: Zone 1 to Zone 2 Transfer Upgrade、Bus to Zone 1 Transfer Upgrade）を入力します。  
3. **amount** 列および **currency** 列に、乗換の費用とその通貨（[通貨コード](https://en.wikipedia.org/wiki/ISO_4217#Active_codes)）を入力します。  
4. **fare_media_id** 列に、このチケット商品を保存および使用できるチケットメディアを入力します。  
    * これは、`fare_media.txt` の **fare_media_id** を参照する外部キーです（[チケットメディア](../../../reference/#fare_mediatxt)）。  
    * 複数のチケットメディアを同じチケット商品に関連付けることができ、価格が異なる可能性があります。  
    * **fare_media_id** が空の場合、チケットメディアは不明であることを意味します。

チケット商品の詳細については、[ドキュメント](../../../reference/#fare_productstxt)を参照してください。

[Translink](../intro/#translink-vancouver) の場合、3つの乗換チケット商品が作成されます。

* `1_zone_to_2_zone_upgrade` は、1ゾーン運賃から2ゾーン運賃への乗換費用を表します。これは CAD 4.65 - CAD 3.20 = CAD 1.45 です（または、Sea Island から開始する乗車区間(leg)の場合は CAD 9.65 - CAD 8.20 = CAD 1.45 です）。  
* `2_zone_to_3_zone_upgrade` は、2ゾーン運賃から2ゾーン運賃への乗換費用を表します。これは CAD 6.35 - CAD 4.65 = CAD 1.70 です（または、Sea Island から開始する乗車区間(leg)の場合は CAD 11.35 - CAD 9.65 = CAD 1.70 です）。  
* `1_zone_to_3_zone_upgrade` は、1ゾーン運賃から2ゾーン運賃への乗換費用を表します。これは CAD 6.35 - CAD 3.20 = CAD 3.15 です。

**fare_products.txt**

| fare_product_id | fare_product_name | amount | currency | fare_media_id |
| :---- | :---- | :---- | :---- | :---- |
| … | … | … | … | … |
| 1_zone_to_2_zone_upgrade | 1 zone から 2 zones へのアップグレード | 1,45 | CAD | contactless |
| 2_zone_to_3_zone_upgrade | 2 zones から 3 zones へのアップグレード | 1.70 | CAD | contactless |
| 1_zone_to_3_zone_upgrade | 1 zone から 3 zones へのアップグレード | 3.15 | CAD | contactless |

## 乗換ルールの作成 {: #create-transfer-rules}


乗換ルールにより、旅程(journey)の対象となる乗車区間(leg)に適用可能な乗換を設定できます。これらは、次のように `fare_transfer_rules.txt` で作成します。

1. **from_leg_group_id** と **to_leg_group_id** を入力します。  
    * これらは `fare_leg_rules.txt` の **leg_group_id** を参照する外部キーです。それぞれ、乗換前の乗車区間グループと乗換後の乗車区間グループを参照します。  
2. 時間制限がある場合は、乗換が有効である秒数を **duration_limit** に入力します。時間制限がない場合は空欄のままにします。  
   **duration_limit** が空欄でない場合は、**duration_limit_type** を入力します。**duration_limit_type** は、**duration_limit** の範囲を計算する方法を説明します。  
3. 運賃の計算方法を説明するために、**fare_transfer_type** を入力します。  
4. 乗換回数に制限がある場合は、**transfer_count** を入力します。このフィールドは、**from_leg_group_id**=**to_leg_group_id** である乗換では必須であり、それ以外では使用してはいけません。

乗換ルールのさまざまな値について詳しくは、[ドキュメントを参照してください](../../../reference/#fare_transfer_rulestxt)。

この例では、Translink のすべての可能な乗車区間グループ間に複数の乗換が定義されています。すべての乗換は 5400 秒（180 分）有効です。**duration_limit_type** は 1 に設定されています。これは、乗換時間が **flat_fare_leg** 運賃乗車区間の任意のルート・路線系統(route)で乗客が出発した時点から開始し、別の運賃乗車区間で出発した時点で終了するためです。

**fare_transfer_type** は 0 に設定されています。これは A + AB 乗換とも呼ばれ、乗客は最初の乗車区間運賃（A）に加えて、B への乗換運賃（AB）を支払います。最後に、許可される乗換回数に制限がないため、**transfer_count** は -1 に設定されています。

**fare_transfer_rules.txt**

| from_leg_group_id | from_leg_group_id | duration_limit | duration_limit_type | transfer_count | fare_transfer_type | fare_product_id |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| flat_fare_leg | flat_fare_leg | 5400 | 1 | -1 | 0 |  |
| ZN1_ZN1 | flat_fare_leg | 5400 | 1 | -1 | 0 |  |
| flat_fare_leg | ZN1_ZN1 | 5400 | 1 | -1 | 0 |  |
| ZN1_ZN2 | flat_fare_leg | 5400 | 1 | -1 | 0 |  |
| flat_fare_leg | ZN1_ZN2 | 5400 | 1 | -1 | 0 | 1_zone_to_2_zone_upgrade |
| ZN2_ZN2 | flat_fare_leg | 5400 | 1 | -1 | 0 |  |
| flat_fare_leg | ZN2_ZN2 | 5400 | 1 | -1 | 0 |  |
| … | … | … | … | … | … | … |
| ZN1_ZN1 | ZN1_ZN2 | 5400 | 1 | -1 | 0 | 1_zone_to_2_zone_upgrade |
| ZN1_ZN1 | ZN1_ZN3 | 5400 | 1 |  | 0 | 1_zone_to_3_zone_upgrade |
| ZN1_ZN2 | ZN2_ZN2 | 5400 | 1 |  | 0 |  |
| ZN1_ZN2 | ZN1_ZN2 | 5400 | 1 |  | 0 |  |
| ZN1_ZN2 | ZN2_ZN3 | 5400 | 1 |  | 0 | 2_zone_to_3_zone_upgrade |
| … | … | … | … |  | … | … |

## 非連続乗換を有効にする {: #enable-nonconsecutive-transfers}


!!! Warning

    非連続乗換は現在も開発中であり、まだ公式仕様の一部ではありません。ただし、Translink向けの運賃実装の包括的な概要を提供するため、このガイドに含めています。

    非連続乗換の詳細については、関連する[プルリクエスト](https://github.com/google/transit/pull/498)および[提案](https://docs.google.com/document/d/1HBskmMx32W7whP-fQGlNlv0c1rgIe0jS8Dm4Rp3R_2I/edit?usp=sharing)を参照してください。

旅程(journey)のある乗車区間(leg)が、後続する隣接していない乗車区間(leg)に影響する場合、非連続乗換が必要です。乗換ルールに対して非連続乗換が有効になっている場合、その乗換ルールは連続する乗車区間(leg)間と非連続の乗車区間(leg)間の両方に適用できます。これにより、可能なすべての乗換の組み合わせが考慮されます。

旅程(journey)のある乗車区間(leg)が、後続する隣接していない乗車区間(leg)に影響する場合、非連続乗換が必要です。

非連続乗換は、次のように`fare_transfer_rules.txt`で有効にします。

1. 乗換ルールが非連続の乗車区間(leg)間で発生できる場合は、**nonconsecutive_transfers_allowed** に1を入力します。それ以外の場合は0に設定するか、空欄のままにします。

!!! info "リマインダー"

    Translinkの有効期間は90分です。例えば、乗客がZone 1からZone 2への旅程(journey)を開始し（2ゾーン運賃を支払い）、次にZone 2からZone 2へ移動し（乗換料金は0）、最後にZone 2からZone 1へ戻る場合、乗客が支払うのは最初の2ゾーン運賃のみです。

[Translink](../intro/#translink-vancouver)のシステムでは、乗客はさまざまなサービスをまたぐ次の80分間の旅程(journey)を利用する場合があります。

* **乗車区間(leg) 1**: Seabusを利用してLonsdale Quay（Zone 2）からWaterfront Station（Zone 1）へ移動します。この場合、2ゾーン運賃が必要です。

* **乗車区間(leg) 2**: SkyTrainを利用してWaterfront Station（Zone 1）からCommercial-Broadway Station（Zone 1）へ移動します。この場合、1ゾーン運賃が必要です。

* **乗車区間(leg) 3**: SkyTrainを利用してCommercial-Broadway Station（Zone 1）からLake City Way Station（Zone 2）へ移動します。この場合、2ゾーン運賃が必要です。

乗車区間(leg) 1の運賃は、90分の時間枠内におけるZone 1とZone 2間のすべての2ゾーン移動をカバーするべきです。したがって、合計費用は次のように計算されます。

* 乗車区間(leg) 1（2ゾーン運賃）+ 乗車区間(leg) 1から乗車区間(leg) 2への乗換 + 乗車区間(leg) 1から乗車区間(leg) 3への乗換 = CAD 4.65 + CAD 0.00 + CAD 0.00 = CAD 4.65

この計算は、`fare_transfer_rules.txt`で`nonconsecutive_transfers_allowed`を1に設定することで機能し、最初の乗車区間(leg)で通過したゾーンが、複数の乗車区間(leg)からなる旅程(journey)のすべての乗車区間(leg)にわたる後続の乗換に影響することを保証します。

非連続乗換を有効にしない場合、運賃は次のように誤って計算されます。

* 乗車区間(leg) 1 + 乗車区間(leg) 1から乗車区間(leg) 2への乗換 + 乗車区間(leg) 2から乗車区間(leg) 3への乗換 = CAD 4.65 + CAD 0.00 + CAD 1.65 = CAD 6.30

2番目の運賃は、実際に意図された運賃の仕組みを反映しません。したがって、非連続乗換を有効にすることで、すべての有効な乗換の可能性を考慮し、正確な運賃計算が保証されます。

**fare_transfer_rules.txt**

| from_leg_group_id | from_leg_group_id | duration_limit | duration_limit_type | transfer_count | fare_transfer_type | fare_product_id | nonconsecutive_transfers_allowed |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| flat_fare_leg | flat_fare_leg | 5400 | 1 | -1 | 0 |  | 1 |
| ZN1_ZN1 | flat_fare_leg | 5400 | 1 | -1 | 0 |  | 1 |
| flat_fare_leg | ZN1_ZN1 | 5400 | 1 | -1 | 0 |  | 1 |
| ZN1_ZN2 | flat_fare_leg | 5400 | 1 | -1 | 0 |  | 1 |
| flat_fare_leg | ZN1_ZN2 | 5400 | 1 | -1 | 0 | 1_zone_to_2_zone_upgrade | 1 |
| ZN2_ZN2 | flat_fare_leg | 5400 | 1 | -1 | 0 |  | 1 |
| flat_fare_leg | ZN2_ZN2 | 5400 | 1 | -1 | 0 |  | 1 |
| … | … | … | … | … | … | … | 1 |
| ZN1_ZN1 | ZN1_ZN2 | 5400 | 1 | -1 | 0 | 1_zone_to_2_zone_upgrade | 1 |
| ZN1_ZN1 | ZN1_ZN3 | 5400 | 1 | -1 | 0 | 1_zone_to_3_zone_upgrade | 1 |
| ZN1_ZN2 | ZN2_ZN2 | 5400 | 1 | -1 | 0 |  | 1 |
| ZN1_ZN2 | ZN1_ZN2 | 5400 | 1 | -1 | 0 |  | 1 |
| ZN1_ZN2 | ZN2_ZN3 | 5400 | 1 | -1 | 0 | 2_zone_to_3_zone_upgrade | 1 |
| … | … | … | … | -1 | … | … | 1 |
