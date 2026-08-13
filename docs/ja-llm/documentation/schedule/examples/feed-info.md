# フィード情報 {: #feed-information}

## GTFS データセットに関する情報を提供する {: #provide-information-about-the-gtfs-dataset}


事業者およびそのサービスに関する情報の提供に加えて、ファイル [feed_info.txt](../../reference/#feed_infotxt) を使用して GTFS データセットに関する情報を提供することができます。これには以下が含まれます。

- 発行者の詳細
- フィードの言語
- フィードの有効期間 
- バージョン

以下は Transport for Cairo の例です。

[**feed_info.txt**](../../reference/#feed_infotxt)

```
feed_publisher_name,feed_publisher_url,feed_lang,feed_start_date,feed_end_date,feed_version
Transport for Cairo,http://transportforcairo.com/,en,20160101,20161201,0.5
```

<sup>[例のソース](https://github.com/transportforcairo/Metro-GTFS/archive/master.zip#Metro-GTFS-master)</sup>
