# データセットの帰属情報 {: #dataset-attributions}

## 集約された GTFS データセットにおけるデータ作成者へのデータの帰属 {: #attribute-data-to-a-data-producer-in-an-aggregated-gtfs-dataset}


一部の GTFS データセットには、同じ管轄区域でサービスを提供する異なるサービス提供者など、複数のソースから集約されたデータが含まれています。場合によっては、[agency.txt](../../reference/#agencytxt) に記載されている事業者を、作成者、運行事業者、または当局として分類する必要があります。 

例えば、Rejseplanen はデンマークの鉄道およびバスサービスの検索エンジンです。同社は、以下の [agency.txt](../../reference/#agencytxt) に示すように、複数の事業者および提供者からのデータを含む GTFS データセットを公開しています。 

[**agency.txt**](../../reference/#agencytxt)

```
agency_id,agency_name,agency_url,agency_timezone,agency_lang
202,Bornholms Trafik,https://bat.dk,Europe/Berlin,da
204,FYNBUS,https://fynbus.dk,Europe/Berlin,
206,NT,https://www.nordjyllandstrafikselskab.dk,Europe/Berlin,
276,Rejseplanen,https://www.rejseplanen.dk,Europe/Berlin,
```

Rejseplanen をデータ作成者として帰属させるために、[attributions.txt](../../reference/#attributionstxt) ファイルを使用します。このファイルでは、組織の名前および URL とともに帰属 ID を定義します。以下に示すように、`is_producer`、`is_operator`、および `is_authority` フィールドを使用して、Rejseplanen を分類します。 

[**attributions.txt**](../../reference/#attributionstxt)

```
attribution_id,organization_name,attribution_url,is_producer,is_operator,is_authority
rp,Rejseplanen,https://www.rejseplanen.dk,1,,
```
