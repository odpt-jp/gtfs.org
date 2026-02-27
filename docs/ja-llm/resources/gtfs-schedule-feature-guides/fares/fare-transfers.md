# 運賃の乗り継ぎ（Fare Transfers） {: #fare-transfers}


*主なファイル: fare_leg_rules.txt, fare_transfer_rules.txt*  
*例: [Translink (バンクーバー)](../intro/#translink-vancouver)*

!!! info "リマインダー"

    運賃の乗り継ぎ（Fare Transfers）は、乗車区間(leg)間の乗り継ぎに適用されるルールを定義するために使用されます。旅程(journey)の合計運賃は、個々の乗車区間(leg)およびそれらを接続する乗り継ぎに適用されるチケット商品(fare product)から算出されます。詳細については、イントロダクションページの[機能セクション](../intro/#fares-features-and-their-files)を参照してください。

!!! info "リマインダー"

    TransLink の場合:

    * 乗り継ぎは運賃の認証後90分間有効です。  
    * この時間内のバス間の乗り継ぎは無料です。  
    * 同一運賃ゾーン番号内（元の運賃でサポートされている範囲）での乗り継ぎも無料です。  
    * 乗客がより高い運賃ゾーンに乗り継ぐ場合は、差額（AddFare と呼ばれます）のみを支払います。

!!! Note

    このセクションには、非接触型運賃（Contactless fares）のみの例が含まれています。他のチケットメディア(fare media)タイプをサポートするには、該当する `fare_products.txt` の行を複製し、金額および `fare_media_id` フィールドを適宜更新してください。

## 乗り換えチケット商品の作成 {: #create-transfer-fare-products}

乗り換えチケット商品は、有料の乗り換えにかかる費用を表します。無料の乗り換えについては作成する必要はありません。乗り換えチケット商品は、`fare_products.txt` に次のように作成します。

1. **fare_product_id** 列に、チケット商品を識別する一意のIDを入力します。  
2. **fare_product_name** 列に、乗客向けのチケット商品名を入力します（例: Zone 1 to Zone 2 Transfer Upgrade、Bus to Zone 1 Transfer Upgrade）。  
3. **amount** および **currency** 列に、乗り換えの費用とその通貨を入力します（[通貨コード](https://en.wikipedia.org/wiki/ISO_4217#Active_codes)）。  
4. **fare_media_id** 列に、このチケット商品を保存および利用できるチケットメディアを入力します。  
    * これは、`fare_media.txt` の **fare_media_id** を参照する外部キーです（[Fare Media](../../../reference/#fare_mediatxt)）。  
    * 同じチケット商品に対して、複数のチケットメディアを異なる価格で関連付けることができます。  
    * **fare_media_id** が空の場合は、チケットメディアが不明であることを意味します。

チケット商品に関する詳細は、[ドキュメントを参照してください](../../../reference/#fare_productstxt)。

[Translink](../intro/#translink-vancouver) の場合、3つの乗り換えチケット商品が作成されます。

* `1_zone_to_2_zone_upgrade` は、1ゾーン運賃から2ゾーン運賃への乗り換え費用を表します。これは CAD 4.65 - CAD 3.20 = CAD 1.45（または Sea Island から始まる乗車区間の場合 CAD 9.65 - CAD 8.20 = CAD 1.45）です。  
* `2_zone_to_3_zone_upgrade` は、2ゾーン運賃から3ゾーン運賃への乗り換え費用を表します。これは CAD 6.35 - CAD 4.65 = CAD 1.70（または Sea Island から始まる乗車区間の場合 CAD 11.35 - CAD 9.65 = CAD 1.70）です。  
* `1_zone_to_3_zone_upgrade` は、1ゾーン運賃から3ゾーン運賃への乗り換え費用を表します。これは CAD 6.35 - CAD 3.20 = CAD 3.15 です。

**fare_products.txt**

| fare_product_id | fare_product_name | amount | currency | fare_media_id |
| :---- | :---- | :---- | :---- | :---- |
| … | … | … | … | … |
| 1_zone_to_2_zone_upgrade | 1 zone から 2 zones へのアップグレード | 1.45 | CAD | contactless |
| 2_zone_to_3_zone_upgrade | 2 zones から 3 zones へのアップグレード | 1.70 | CAD | contactless |
| 1_zone_to_3_zone_upgrade | 1 zone から 3 zones へのアップグレード | 3.15 | CAD | contactless |

## 乗り換えルールの作成 {: #create-transfer-rules}

乗り換えルールは、旅程(journey)の対象となる乗車区間(leg)に適用可能な乗り換えを定義します。これらは `fare_transfer_rules.txt` に次のように作成します。

1. **from_leg_group_id** および **to_leg_group_id** を入力します。  
    * これらは `fare_leg_rules.txt` の **leg_group_id** を参照する外部キーです。それぞれ、乗り換え前の乗車区間グループと乗り換え後の乗車区間グループを参照します。  
2. **duration_limit** に、乗り換えが有効である秒数を入力します。時間制限がない場合は空欄のままにします。  
   **duration_limit** が空でない場合は **duration_limit_type** を入力します。**duration_limit_type** は **duration_limit** の計算方法を説明します。  
3. **fare_transfer_type** を入力し、運賃の計算方法を説明します。  
4. 乗り換え回数に制限がある場合は **transfer_count** を入力します。このフィールドは **from_leg_group_id**=**to_leg_group_id** の場合に必須であり、それ以外の場合は入力してはいけません。

乗り換えルールのさまざまな値について詳しくは、[ドキュメントを参照してください](../../../reference/#fare_transfer_rulestxt)。

この例では、Translink のすべての可能な乗車区間グループ間で複数の乗り換えが定義されています。すべての乗り換えは 5400 秒（180 分）間有効です。**duration_limit_type** は 1 に設定されています。これは、乗客が **flat_fare_leg** 運賃区間で任意のルートに乗車した時点から乗り換え時間が開始し、別の運賃区間に乗車した時点で終了することを意味します。

**fare_transfer_type** は 0 に設定されています。これは「A + AB 乗り換え」とも呼ばれ、乗客が最初の区間運賃（A）に加えて、B への乗り換え運賃（AB）を支払うことを意味します。最後に、**transfer_count** は -1 に設定されており、乗り換え回数に制限がないことを示しています。

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

## 非連続乗り換えを有効にする {: #enable-nonconsecutive-transfers}


!!! Warning

    非連続乗り換えは現在も開発中であり、まだ公式仕様の一部ではありません。しかし、Translink の運賃実装を包括的に理解するために、このガイドに含まれています。

    非連続乗り換えの詳細については、関連する [pull request](https://github.com/google/transit/pull/498) および [提案書](https://docs.google.com/document/d/1HBskmMx32W7whP-fQGlNlv0c1rgIe0jS8Dm4Rp3R_2I/edit?usp=sharing) を参照してください。

非連続乗り換えは、旅程(journey)のある乗車区間(leg)が、後の非隣接の乗車区間に影響を与える場合に必要です。非連続乗り換えが乗り換えルールに対して有効化されている場合、その乗り換えルールは連続する乗車区間と非連続の乗車区間の両方に適用することができます。これにより、すべての可能な乗り換えの組み合わせが考慮されることが保証されます。

非連続乗り換えは、旅程のある乗車区間が後の非隣接の乗車区間に影響を与える場合に必要です。

非連続乗り換えは、`fare_transfer_rules.txt` において次のように有効化します。

1. **nonconsecutive_transfers_allowed** に 1 を設定すると、その乗り換えルールが非連続の乗車区間間で発生することを許可します。そうでない場合は 0 を設定するか、空欄のままにします。

!!! info "リマインダー"

    Translink では有効期間が 90 分です。例：乗客が Zone 1 から Zone 2 へ移動（2ゾーン運賃を支払い）、次に Zone 2 から Zone 2 へ移動（乗り換え料金 0）、最後に Zone 2 から Zone 1 に戻る場合、乗客は最初の 2 ゾーン運賃のみを支払います。

[Translink](../intro/#translink-vancouver) のシステムでは、乗客が以下のような 80 分間の旅程を複数のサービスをまたいで行うことがあります。

* **乗車区間 1**: Lonsdale Quay（Zone 2）から Waterfront Station（Zone 1）まで Seabus で移動 — 2 ゾーン運賃が必要です。

* **乗車区間 2**: Waterfront Station（Zone 1）から Commercial-Broadway Station（Zone 1）まで SkyTrain で移動 — 1 ゾーン運賃が必要です。

* **乗車区間 3**: Commercial-Broadway Station（Zone 1）から Lake City Way Station（Zone 2）まで SkyTrain で移動 — 2 ゾーン運賃が必要です。

乗車区間 1 の運賃は、Zone 1 と Zone 2 間のすべての 2 ゾーン移動を 90 分以内でカバーする必要があります。したがって、合計金額は次のように計算されます。

* 乗車区間 1（2 ゾーン運賃） + 乗車区間 1 から 2 への乗り換え + 乗車区間 1 から 3 への乗り換え = CAD 4.65 + CAD 0.00 + CAD 0.00 = CAD 4.65

この計算は、`fare_transfer_rules.txt` の `nonconsecutive_transfers_allowed` を 1 に設定することで機能し、最初の乗車区間で通過したゾーンが、複数乗車区間の旅程全体における後続の乗り換えに影響を与えるようにします。

非連続乗り換えを有効にしない場合、運賃は誤って次のように計算されます。

* 乗車区間 1 + 乗車区間 1 から 2 への乗り換え + 乗車区間 2 から 3 への乗り換え = CAD 4.65 + CAD 0.00 + CAD 1.65 = CAD 6.30

この 2 番目の計算結果は、実際の運賃体系の意図を反映していません。したがって、非連続乗り換えを有効にすることで、すべての有効な乗り換えの可能性を考慮し、正確な運賃計算を保証します。

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
