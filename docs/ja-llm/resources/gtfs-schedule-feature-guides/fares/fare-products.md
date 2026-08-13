# チケット商品 {: #fare-products}


*主なファイル: fare_products.txt*  
*例: [Translink（Vancouver）](../intro/#translink-vancouver)*

!!! info "リマインダー" 

    チケット商品は、サービスを利用するために交通事業者が提供する運賃の種類（例: 1回乗車運賃、月間パス、乗換料金など）です。詳細については、Introduction ページの[機能のセクション](../intro/#fares-features-and-their-files)を再確認してください。

!!! Note

    * このセクションでは、チケット商品の概要を説明します。後続のセクションでは、ユースケースに基づいてより具体的に定義します。  
    * このセクションには、異なる種類のパスを表すチケット商品（不定期利用パスおよび1日パス）が含まれます。後続のセクションでは、例を簡潔に保つため、不定期利用パスのみを使用します。

## チケット商品を作成する {: #create-fare-products}


同じ旅程に対して、異なるチケット商品が関連する場合があります。チケット商品は、次のように `fare_products.txt` で作成します。

1. **fare_product_id** 列に、チケット商品を識別する一意の ID を入力します。  
2. **fare_product_name** 列に、乗客向けのチケット商品の名称（例: Bus Flat Fare、Bus Flat Fare Monthly）を入力します。  
3. **amount** 列および **currency** 列に、運賃の料金とその通貨（[通貨コード](https://en.wikipedia.org/wiki/ISO_4217#Active_codes)）を入力します。  
4. **fare_media_id** 列に、このチケット商品を保存および使用できるチケットメディアを入力します。  
    * これは、`fare_media.txt` の **fare_media_id** を参照する外部キーです（[チケットメディア](../../../reference/#fare_mediatxt)）。  
    * 同じチケット商品に複数のチケットメディアを関連付けることができ、料金が異なる可能性があります。  
    * **fare_media_id** が空の場合、チケットメディアは不明であることを意味します。

チケット商品の詳細については、[ドキュメント](../../../reference/#fare_productstxt)を参照してください。

この例では、`bus_flat_fare` と `bus_flat_fare_daily` の2つのチケット商品が定義されています。どちらも Translink のバス便で使用できます。`Bus_flat_fare` は、時折発生する短時間の旅程に適しています。`bus_flat_fare_daily` は、同じ日の複数時間にわたる旅程により適しています。

[**fare_products.txt**](../../../reference/#fare_productstxt)

| fare_product_id | fare_product_name | amount | currency | fare_media_id |
| :---- | :---- | :---- | :---- | :---- |
| bus_flat_fare | Bus Flat Fare | 3.20 | CAD | contactless |
| bus_flat_fare | Bus Flat Fare | 3.20 | CAD | cash |
| bus_flat_fare | Bus Flat Fare | 2.60 | CAD | compass_card |
| bus_flat_fare_daily | Daily pass | 11.50 | CAD | compass_card |
| bus_flat_fare_daily | Daily pass | 11.50 | CAD | compass_ticket |
| … | … | … | … | … |
