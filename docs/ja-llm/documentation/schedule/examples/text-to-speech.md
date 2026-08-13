# 読み上げ用 {: #text-to-speech}

## 略語、特殊な発音、大きな数字および序数 {: #abbreviations-unusual-pronunciations-large-digits-and-ordinals}


略語、特殊な発音および大きな数字は、GTFSのテキストフィールドで一般的に使用されます。以下のTriMEtの例では、読み上げ用フィールド(text-to-speech field)をどのように使用するべきかを確認できます。

- 略語は完全に綴り出します。例: 「SW」は「southwest」になり、「Ave」は「avenue」になります。
- 発音は、ソフトウェアが正しく読み上げるように綴ります。例: 「Orenco」は「orrainkoe」になり、「Merlo」は「murlo」になります。
- 大きな数字は、発話されるとおりに綴り出します。「3300」は「thirty-three hundred」になります。
そうしない場合、ソフトウェアは「3300」を「three thousand three hundred」と読み上げます。
- 1st、2nd、3rdなどの序数は、綴り出すべきです。例: 「1st」は「first」になります。

[**stops.txt**](../../reference/#stopstxt)

| stop_id | stop_name | tts_stop_name |
| ---- | ---- | ---- |
| 9163 | SW 125th & Longhorn | southwest one hundred twenty fifth & longhorn |
| 9836 | Orenco MAX Station | orrainkoe max station |
| 9828 | Merlo Rd/SW 158th Ave MAX Station | murlo road southwest one hundred fifty eighth avenue max station |
| 10074 | 3300 Block NW 35th | thirty-three-hundred block northwest thirty fifth |

## 頭字語 {: #acronyms}


文字ごとに参照される頭字語については、文字の後にピリオドを付けるか、文字をスペースで区切るべきです。これにより、その頭字語を単語としてではなく、文字ごとに読むべきであることが明確になります。

Tampa では、行先表示(headsign)「North to UATC」には、個々の文字で発音される頭字語が含まれています。読み上げ用の曖昧性解消は次のようになります。

[**trips.txt**](../../reference/#tripstxt)

| trip_headsign | tts_trip_headsign |
| ---- | ---- |
| North to UATC | north to u.a.t.c. |

または

| trip_headsign | tts_trip_headsign |
| ---- | ---- |
| North to UATC | north to u a t c |

一方で、一部の頭字語は単語として読むべきです。例: NATO、NASA。読み上げ用フィールド(text-to-speech field)にはこれを反映するべきです。

!!! 注記

    フィールド `trips.tts_trip_headsign` は、まだ仕様上正式なものではありません。

## 複数の意味を持つ略語の明確化 {: #clarifying-abbreviations-with-multiple-meanings}


「St」という略語には、「street」、「saint」、「station」、および「first」を意味する「1st」という複数の意味があります。読み上げ用フィールド(text-to-speech field)では、正しい単語を完全に綴り、かつTTS softwareが読み取れる形式で記述することで、これらの二重の意味に対応することができます。
