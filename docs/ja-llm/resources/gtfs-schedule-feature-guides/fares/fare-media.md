# チケットメディア {: #fare-media}


*主要ファイル: fare_media.txt、fare_products.txt*  
*例: [Translink (Vancouver)](../intro/#translink-vancouver)*

!!! info "リマインダー" 

    チケット商品は、公共交通機関への乗車時にそれらを検証するために使用される異なるチケットメディア内に保存されます。詳細については、Introduction ページの [Features section](../intro/#fares-features-and-their-files) を再確認してください。

## チケットメディアの作成 {: #create-fare-media}


チケットメディアのエントリは、次のように `fare_media.txt` で作成します。

1. **fare_media_id** 列に、チケットメディアを識別する一意の ID を入力します。  
    * これは、`fare_products.txt` と関連付けるために使用する Primary Key です。  
2. **fare_media_name** 列に、乗客向けのチケットメディア名を入力します。  
3. **fare_media_type** 列に、適切な enum（0=なし、1=物理的な紙のチケット、2=物理的な交通カード、3=cEMV、4=モバイルアプリ）を入力します。

チケット商品についての詳細は、[ドキュメントを参照してください](https://gtfs.org/documentation/schedule/reference)。

この例では、5つの異なるチケットメディアを作成し、それぞれに ID、名前、およびメディアのタイプを割り当てます。たとえば、Compass Card は物理的な交通カードであるため、`fare_media_type=2` を割り当てます。

[**fare_media.txt**](../../../reference/#fare_mediatxt)

| fare_media_id | fare_media_name | fare_media_type |
| :---- | :---- | :---- |
| cash | Cash | 0 |
| contactless | Contactless | 3 |
| compass_card | Compass Card | 2 |
| compass_ticket | Compass Ticket | 1 |
| wallet | Mobile Wallet | 3 |

## （代替）cEMV サポートを指定する {: #alternative-specify-cemv-support}


GTFS Fares (v2) を作成する能力がない GTFS プロデューサーは、Fares (v2) を持たずに cEMV サポートを指定することができます。これにより、コンシューマーは Fares (v2) を実装しなくても、事業者（または特定のルート・路線系統(route)）が contactless EMV を使用した支払いをサポートしているかどうかを表示できます。

!!! tip "ヒント" 

    cEMV サポートだけでなく包括的な運賃情報を表示できるように、Fares (v2) を実装することを推奨します。

1. cEMV 情報を指定するために、`agency.txt` または `routes.txt` のいずれかを選択します。  
    * 事業者全体で、すべての便(trip)にわたり同じ cEMV サポートがある場合は、`agency.txt` を使用します。  
    * cEMV サポートがルート・路線系統(route)ごとに異なる場合は、\routes.txt` を使用します。  
2. `agency.txt` または `routes.txt` の **cemv_support** フィールドに、適切な列挙値を適切な列挙値で入力します。
    * 0: cEMV 情報なし。
    * 1: 乗客は、事業者（またはルート・路線系統(route)）に関連付けられたすべての便(trip)で、cEMV をチケットメディアとして使用することができます。
    * 2: 事業者（またはルート・路線系統(route)）に関連付けられたすべての便(trip)で、cEMV はチケットメディアとしてサポートされていません。

cEMV サポートに関する詳細は、[ドキュメントを参照してください](./../../reference/)。

[Translink](../intro/#translink-vancouver) では、cEMV はすべてのルート・路線系統(route)でサポートされています。したがって、**cemv_support** は `agency.txt` で定義できます。

[**agency.txt**](../../../reference/#agencytxt)

| agency_id | agency_name | cemv_support | … |
| :---- | :---- | :---- | :---- |
| TL | TransLink | 1 | … |
