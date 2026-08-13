# 乗客カテゴリ {: #rider-categories}


*主要ファイル: rider_categories.txt、fare_products.txt*  
*例: [Translink (Vancouver)](../intro/#translink-vancouver)*

!!! info "リマインダー"

    乗客カテゴリは、高齢者、学生、子ども、成人など、年齢、属性、またはニーズに基づいて特定の運賃率の対象となる、異なる乗客のグループを表します。詳細については、Introduction ページの[機能のセクション](../intro/#fares-features-and-their-files)を再確認してください。

乗客カテゴリを定義することで、経路探索ツール（GTFS コンシューマー）は、年齢、アクセシビリティのニーズ、社会的状況などの乗客の属性に基づいて異なる運賃を表示でき、各グループに適切な運賃が表示されるようになります。

## 乗客カテゴリを指定する {: #specify-rider-categories}


各乗客カテゴリは、以下のように `rider_categories.txt` で作成します。

1. **rider_category_id** に乗客カテゴリの一意の ID を入力します  
2. **rider_category_name** に、乗客向けのカテゴリ名（Adult、Student など）を入力します  
3. **is_default_fare_cateory;** に、カテゴリがデフォルトのものである場合はこのフィールドを 1 に設定し、それ以外の場合は 0 に設定するか空欄のままにします。  
4. 利用可能な場合は、**eligibility_url** に乗客カテゴリの条件に関する情報を含む Web ページを入力します。それ以外の場合、列全体は任意です。

乗客カテゴリに関する詳細は、[ドキュメントを参照してください](../../../reference/#rider_categoriestxt)。

この例では、[Translink](../intro/#translink-vancouver) に対して adult と concession の 2 つのカテゴリを指定しています。adult はデフォルトのカテゴリです。また、concession カテゴリに含まれる乗客を説明する **eligibility_url** も追加されています。

[**rider_categories.txt**](../../../reference/#rider_categoriestxt)

| rider_category_id | rider_category_name | is_default_fare_category | eligibility_url |
| :---- | :---- | :---- | :---- |
| adult | Adult | 1 |  |
| concession | Concession |  | https://www.translink.ca/transit-fares/pricing-and-fare-zones#fare-pricing |

## チケット商品との関連付け {: #associate-with-fare-products}


各チケット商品は、1つまたは複数の乗客カテゴリの対象となることができます。この対象資格を示すため、乗客カテゴリは次のように `fare_products.txt` 内のチケット商品に関連付けられます。

1. 各チケット商品について、乗車区間(leg)の料金を決定する乗客カテゴリの ID を **rider_category_id** に入力します。  
   * これは、`rider_categories.txt` の **rider_category_id** を参照する Foreign Key です。  
   * `fare_products.txt` の **rider_category_id** が空の場合、そのチケット商品は任意の乗客カテゴリの対象であることを意味します。

この例では、成人向けおよび割引カテゴリ向けの均一バス運賃について、2つの異なるバスチケット商品が指定されています。`bus_flat_fare` が成人乗客カテゴリに関連付けられている場合、金額は CAD 3.20 です。`bus_flat_fare` が割引乗客カテゴリに関連付けられている場合、金額は CAD 2.15 です。

[**fare_products.txt**](../../../reference/#fare_productstxt)

| fare_product_id | fare_product_name | amount | currency | fare_media_id | rider_category_id |
| :---- | :---- | :---- | :---- | :---- | :---- |
| bus_flat_fare | 均一バス運賃 | 3.20 | CAD | contactless | adult |
| bus_flat_fare | 均一バス運賃 | 2.15 | CAD | contactless | concession |
