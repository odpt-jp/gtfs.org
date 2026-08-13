# 翻訳 {: #translations}

## 駅名を翻訳する {: #translate-station-names}


一部の交通事業者は複数の言語でサービスを提供しています。その1つがベルギー国鉄会社です（現地では、オランダ語の Nationale Maatschappij der Belgische Spoorwegen またはフランス語の Société nationale des chemins de fer belges に由来して NMBS/SNCB として知られています）。同社は複数の言語で駅名を提供しており、ユーザーの言語および位置情報の設定に応じて表示されます。

NMBS/SNCB は、以下のファイルに示すようにフランス語で GTFS データを公開しています。

[**agency.txt**](../../reference/#agencytxt)

```
agency_id,agency_name,agency_url,agency_timezone,agency_lang
NMBS/SNCB,NMBS/SNCB,http://www.belgiantrain.be/,Europe/Brussels,fr
```


事業者の言語がフランス語に設定されているため、駅名は [stops.txt](../../reference/#stopstxt) にフランス語で記載されています。

[**stops.txt**](../../reference/#stopstxt)

```
stop_id,stop_code,stop_name,stop_desc,stop_lat,stop_lon,zone_id,stop_url,location_type,parent_station,platform_code
S8815040,,Bruxelles-Ouest,,50.8485600,4.32104000,,,1,,
S8821006,,Anvers-Central,,51.2172000,4.42109800,,,1,,
```


次に、ファイル [translations.txt](../../reference/#translationstxt) を使用して、駅名をデフォルトの事業者言語（この場合はフランス語）からオランダ語に翻訳します。

[**translations.txt**](../../reference/#translationstxt)

```
table_name,field_name,record_id,language,translation
stops,stop_name,S8815040,nl,Brussel-West
```

- この例では、[stops.txt](../../reference/#stopstxt) の駅名が翻訳され、[stops.txt](../../reference/#stopstxt) のレコードは `stop_id` によって識別されます。したがって:
    - `stops` はテーブル名の下に記載されます（[stops.txt](../../reference/#stopstxt) を参照）
    - 駅名が翻訳されるため、`stop_name` は `field_name` の下に記載されます
    - フランス語から翻訳する駅名の `stop_id` は `record_id` の下に記載されます（この場合は Bruxelles-Ouest の `stop_id`）
- 名前はオランダ語（nl）に翻訳されます
- 最後に、翻訳された名前が translation の下に記載されます

GTFS で [translations.txt](../../reference/#translationstxt) を使用して名前を翻訳する別の方法として、`record_id` の代わりにフィールド `field_value` を使用する方法があります。この場合、駅名を使用して [stops.txt](../../reference/#stopstxt) から翻訳対象のレコードを検索します。

[**translations.txt**](../../reference/#translationstxt)

```
table_name,field_name,field_value,language,translation
stops,stop_name,Bruxelles-Ouest,nl,Brussel-West
```
