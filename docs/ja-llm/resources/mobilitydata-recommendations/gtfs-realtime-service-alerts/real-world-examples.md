# 実世界の例とその修正方法 {: #real-world-examples-and-how-to-fix-them}


この例では、実世界の事業者向けに作成された Service Alerts の例を紹介します。これらの運行情報(alert)に関する問題を検討し、それらの改善案を提示します。

## 1) Toronto Transit Commission {: #1-toronto-transit-commission}


公開日 28-03-2025

* ヘッダーの形式が不適切です。説明文まで続いているように見えます。
* ヘッダーは *"Line 1 Yonge-University: No service between St George and St Andrew"* に変更するべきです。
* 説明文を完成させるべきです: *"Service will resume…"*
* effect は `MODIFIED_SERVICE` に変更するべきです。
  * `NO_SERVICE` をこの effect として設定してはいけません。これは現在、一般的に停留所等(stop)およびプラットフォームの閉鎖に関するものです。この場合、便(trip)は実質的に途中で2つの別々の便(trip)に分割されているように見えます。
* cause は `MAINTENANCE` に変更するべきです。
* informed entities として含まれている `route_ids` が多すぎます。言及するべき唯一のルート・路線系統(route)は Yonge-University Line (`route_id=1`) です。また、St George と St Andrew の間にあるすべての駅の stop_ids に言及するべきです。
* 運行情報(alert)には期間が関連付けられていません。

```json
{
  "id": "1",
  "alert": {
    "informedEntity": [
      {
        "agencyId": "1",
        "routeId": "508"
      },
      {
        "agencyId": "1",
        "routeId": "13"
      },
      {
        "agencyId": "1",
        "routeId": "503"
      },
      {
        "agencyId": "1",
        "routeId": "504"
      },
      {
        "agencyId": "1",
        "routeId": "505"
      },
      {
        "agencyId": "1",
        "routeId": "501"
      },
      {
        "agencyId": "1",
        "routeId": "94"
      },
      {
        "agencyId": "1",
        "routeId": "97"
      },
      {
        "agencyId": "1",
        "routeId": "304"
      },
      {
        "agencyId": "1",
        "routeId": "19"
      },
      {
        "agencyId": "1",
        "routeId": "26"
      },
      {
        "agencyId": "1",
        "routeId": "300"
      },
      {
        "agencyId": "1",
        "routeId": "301"
      },
      {
        "agencyId": "1",
        "routeId": "303"
      },
      {
        "agencyId": "1",
        "routeId": "305"
      }
    ],
    "cause": "UNKNOWN_CAUSE",
    "effect": "UNKNOWN_EFFECT",
    "headerText": {
      "translation": [
        {
          "text": "Line 1 Yonge-University: There i",
          "language": "en"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "There is no subway service on Line 1 Yonge-University between St George and St Andrew stations due to planned track work. Shuttle buses will not operate. Follow the station signs or speak to a TTC employee for help. Service will resume",
          "language": "en"
        }
      ]
    }
  }
}
```

###改善された運行情報(alert)

```json hl_lines="8 13 19 24 28 29 30-41 45 53"
{
  "id": "1",
  "alert": {
    "informedEntity": [
      {
        "agencyId": "1",
        "routeId": "1",
        "stopId": "14445", // St George Station - southbound platform
      },
      {
        "agencyId": "1",
        "routeId": "1",
        "stopId": "14446", // Museum Station - southbound platform
      },
      ...
      {
        "agencyId": "1",
        "routeId": "1",
        "stopId": "14421", // St Andrew Station - southbound platform
      },
      {
        "agencyId": "1",
        "routeId": "1",
        "stopId": "14422", // Osgoode Station - southbound platform
      },
      ...
    ],
    "cause": "MAINTENANCE",
    "effect": "NO_SERVICE",
    "communicationPeriod": [
      {
        "start": ...
        "end": ...
      }, ...
    ],
    "impactPeriod": [
      {
        "start": ...
        "end": ...
      }, ...
    ],
    "headerText": {
      "translation": [
        {
          "text": "Line 1 Yonge-University: No service between St George and St Andrew",
          "language": "en"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "There is no subway service on Line 1 Yonge-University between St George and St Andrew stations due to planned track work. Shuttle buses will not operate. Follow the station signs or speak to a TTC employee for help. Service will resume at 3AM.",
          "language": "en"
        }
      ]
    }
  }
}
```

Mobility Database のフィードには[こちら](https://mobilitydatabase.org/feeds/gtfs_rt/tld-6879-sa)からアクセスしてください。

## 2) BC Transit {: #2-bc-transit}


公開日: 28-03-2025

* ヘッダーは次のように拡張するべきです: *「労働争議によりCowichan Valleyでは運行していません。」*
* 原因は `STRIKE` に変更するべきです。
* 影響は `NO_SERVICE` に変更するべきです。
* 説明に記載されているURLは、`url` フィールドに含めるべきです。
* `active_period` の代わりに、フィールド `communication_period` および `impact_period` を使用するべきです。

```json
{
  "id": "ft-TX1173397",
  "alert": {
    "activePeriod": [
      {
        "start": "1739001600"
      }
    ],
    "informedEntity": [
      {
        "routeId": "2-COW"
      },
      {
        "routeId": "3-COW"
      },
      ...
    ],
    "cause": "UNKNOWN_CAUSE",
    "effect": "OTHER_EFFECT",
    "headerText": {
      "translation": [
        {
          "text": "No Service"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "Please be advised that all services in the Cowichan Valley are suspended due to labour action.\r\n\r\nWe apologize for any inconvenience this may cause. \r\n\r\nFor updates please sign up for alerts, please visit\u202fhttps://www.bctransit.com/cowichan-valley  "
        }
      ]
    }
  }
}
```

###改善された運行情報(alert)

```json hl_lines="9-20 30 31 35 46"
{
  "id": "ft-TX1173397",
  "alert": {
    "activePeriod": [  <-- このフィールドは引き続き保持することができます。GTFS referenceの移行ガイドを参照してください。
      {
        "start": "1739001600"
      }
    ],
    "communicationPeriod": [
      {
        "start": ...
        "end": ...
      }, ...
    ],
    "impactPeriod": [
      {
        "start": ...
        "end": ...
      }, ...
    ],
    "informedEntity": [
      {
        "routeId": "2-COW"
      },
      {
        "routeId": "3-COW"
      },
      ...
    ],
    "cause": "STRIKE",
    "effect": "NO_SERVICE",
    "headerText": {
      "translation": [
        {
          "text": "労働争議によりCowichan Valleyでは運行していません"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "Please be advised that all services in the Cowichan Valley are suspended due to labour action.\r\n\r\nWe apologize for any inconvenience this may cause. \r\n\r\nFor updates please sign up for alerts, please visit\u202fhttps://www.bctransit.com/cowichan-valley"
        }
      ]
    },
    "url": "https://www.bctransit.com/cowichan-valley",
  }
}
```

Mobility Databaseでフィードにアクセスするには、[こちら](https://mobilitydatabase.org/feeds/gtfs_rt/mdb-1380)をご覧ください。

## 3) MBTA {: #3-mbta}


公開日: 02-04-2025

* `informed_entity` は、正確な便(trip)および方向を含め、適切に入力されています。
* ヘッダーには、説明に移すことができる情報が含まれています。
* 運行情報(alert)には `SIGNIFICANT_DELAYS` の effect を使用できます。
* `active_period` の代わりに、フィールド `communication_period` および `impact_period` を使用するべきです。

```json
{
  "id": "634445",
  "alert": {
    "activePeriod": [
      {
        "start": "1743086220",
        "end": "1743091200"
      }
    ],
    "informedEntity": [
      {
        "agencyId": "1",
        "routeId": "CR-Haverhill",
        "routeType": 2,
        "trip": {
          "tripId": "SPRING2025V2-728425-1231-HaverhillBradfordVan",
          "routeId": "CR-Haverhill",
          "directionId": 0
        }
      }
    ],
    "cause": "UNKNOWN_CAUSE",
    "effect": "OTHER_EFFECT",
    "headerText": {
      "translation": [
        {
          "text": "Haverhill Line Train 1231 (10:25 am from North Station) is operating 10-20 minutes behind schedule between North Station and Reading due to a signal issue.",
          "language": "en"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "Affected direction: Outbound",
          "language": "en"
        }
      ]
    },
    "severityLevel": "WARNING"
  }
}
```

###改善された運行情報(alert)

```json hl_lines="10-21 35 39 47"
{
  "id": "634445",
  "alert": {
    "activePeriod": [  <-- このフィールドは引き続き保持できます。GTFS reference の移行ガイドを参照してください。
      {
        "start": "1743086220",
        "end": "1743091200"
      }
    ],
    "communicationPeriod": [
      {
        "start": ...
        "end": ...
      }, ...
    ],
    "impactPeriod": [
      {
        "start": ...
        "end": ...
      }, ...
    ],
    "informedEntity": [
      {
        "agencyId": "1",
        "routeId": "CR-Haverhill",
        "routeType": 2,
        "trip": {
          "tripId": "SPRING2025V2-728425-1231-HaverhillBradfordVan",
          "routeId": "CR-Haverhill",
          "directionId": 0
        }
      }
    ],
    "cause": "UNKNOWN_CAUSE",
    "effect": "SIGNIFICANT_DELAYS",
    "headerText": {
      "translation": [
        {
          "text": "Haverhill Line Train 1231 running behind schedule.",
          "language": "en"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "Haverhill Line Train 1231 (10:25 am from North Station) is operating 10-20 minutes behind schedule between North Station and Reading due to a signal issue. Affected direction: Outbound",
          "language": "en"
        }
      ]
    },
    "severityLevel": "WARNING"
  }
}
```

Mobility Database のフィードには[こちら](https://mobilitydatabase.org/feeds/gtfs_rt/mdb-1603)からアクセスしてください。

## 4) AMB-Mobilitat {: #4-amb-mobilitat}


公開日: 28-03-2025

* 影響を受ける停留所等(stop)（判明している場合）は、description と informed entities の両方に追加するべきです。
* &nbsp; や \n のような文字は、プレーンテキストの対応する文字に置き換えるべきです。
* `active_period` の代わりに、フィールド `communication_period` および `impact_period` を使用するべきです。

```json
{
  "id": "24950",
  "alert": {
    "activePeriod": [
      {
        "start": "1743724800",
        "end": "1743811140"
      }
    ],
    "informedEntity": [
      {
        "routeId": "302"
      },
      {
        "routeId": "303"
      },
      {
        "stopId": "107046"
      }
    ],
    "cause": "MAINTENANCE",
    "effect": "DETOUR",
    "url": {
      "translation": [
        {
          "text": "http://www.ambmobilitat.cat/Principales/Noticia.aspx?incidencia=24950&idioma=1",
          "language": "ca"
        },
        {
          "text": "http://www.ambmobilitat.cat/Principales/Noticia.aspx?incidencia=24950&idioma=2",
          "language": "es"
        }
      ]
    },
    "headerText": {
      "translation": [
        {
          "text": "Desviament provisional al carrer Prat de la Riba afectant parades",
          "language": "ca"
        },
        {
          "text": "Desv\u00edo provisional en la calle Prat de la Riba afectando paradas",
          "language": "es"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "Divendres&nbsp;4&nbsp;d&rsquo;abril de 2025 durant tot el servei,&nbsp;es modifica el recorregut habitual al carrer Prat de la Riba de&nbsp;Santa Coloma de Gramenet amb l&rsquo;anul&middot;laci&oacute; provisional de les parades afectades.\n\nLes l&iacute;nies seran desviades tal i com s&rsquo;indica al pl&agrave;nol seg&uuml;ent:\n\n\n",
          "language": "ca"
        },
        {
          "text": "Viernes 4&nbsp;de abril de 2025 durante todo&nbsp;el servicio, se&nbsp;modifica el recorrido&nbsp;habitual en la calle&nbsp;Prat de la Riba de&nbsp;Santa Coloma de Gramenet con la anulaci&oacute;n&nbsp;provisional de las paradas afectadas.\n\nLas lineas&nbsp;ser&aacute;n desviadas tal y como se indica en el siguiente plano:\n\n:\n",
          "language": "es"
        }
      ]
    }
  }
}
```

Mobility Database のフィードには[こちら](https://mobilitydatabase.org/feeds/gtfs_rt/mdb-2369)からアクセスしてください。

## 5) King County Metro {: #5-king-county-metro}


公開日: 28-03-2025

* cause は `OTHER_CAUSE` に変更するべきです。
* 説明を追加するべきです。
* `active_period` の代わりに、フィールド `communication_period` および `impact_period` を使用するべきです。

```json
{
  "id": "18180",
  "alert": {
    "activePeriod": [
      {
        "start": "1657226640"
      }
    ],
    "informedEntity": [
      {
        "agencyId": "1",
        "routeId": "102698",
        "routeType": 3
      }
    ],
    "cause": "UNKNOWN_CAUSE",
    "effect": "NO_SERVICE",
    "url": {
      "translation": [
        {
          "text": "https://svtbus.org/duvall-monroe-shuttle/",
          "language": "en"
        }
      ]
    },
    "headerText": {
      "translation": [
        {
          "text": "Duvall-Monroe Shuttle service is suspended until further notice due to driver shortage.\n\n",
          "language": "en"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "",
          "language": "en"
        }
      ]
    },
    "severityLevel": "SEVERE"
  }
}
```

Mobility Database のフィードには[こちら](https://mobilitydatabase.org/feeds/gtfs_rt/tld-1093-sa)からアクセスしてください。

## 6) Ruter - OSLO {: #6-ruter-oslo}


公開日: 02-04-2025

* 原因または影響が記載されていません。少なくとも `MODIFIED_SERVICE` の影響を追加するべきです。
  * `NO_SERVICE` を設定してはいけません。これは現在、一般的に停留所等(stop)およびプラットフォームの閉鎖に関する影響として使用されています。この場合、便(trip)は実質的に途中で2つの別個の便(trip)に分割されているようです。
* 地下鉄の `route_id` は、運休区間の駅とともに `informed_entity` に含めるべきです。
* 代替バスサービスが GTFS Schedule に存在しない場合は、GTFS Schedule に追加するべきです。代替バスサービスが GTFS 内のルート・路線系統(route)に対応しており、GTFS Schedule で追加の便(trip)を定義できない場合は、`TripDescriptor = NEW` を使用して `tripUpdates` 内に作成してください。
* 言語コードは、header および description のテキストとともに提供する必要があります。
* `active_period` ではなく、`communication_period` および `impact_period` のフィールドを使用するべきです。

```json
{
  "id": "RUT:SituationNumber:732042",
  "alert": {
    "activePeriod": [
      {
        "start": "1743130800",
        "end": "1762218000"
      }
    ],
    "url": {},
    "headerText": {
      "translation": [
        {
          "text": "Buss for T-bane mellom Borgen og Majorstuen"
        },
        {
          "text": "Bus replacement service between Borgen and Majorstuen "
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "Buss erstatter T-banen mellom Borgen og Majorstuen. T-banen kj\u00f8rer Kols\u00e5s\u2013Borgen og Stortinget\u2013Mortensrud."
        },
        {
          "text": "Bus replaces the Metro between Borgen and Majorstuen. The Metro runs between Kols\u00e5s and Borgen, and between Stortinget and Mortensrud."
        }
      ]
    }
  }
}
```

Mobility Database のフィードには[こちら](https://mobilitydatabase.org/feeds/gtfs_rt/mdb-1799)からアクセスしてください。

## 7) Calgary Transit {: #7-calgary-transit}


公開日: 02-04-2025

* GTFS Schedule にはフィールド `agency_id` が存在しません。そのため、informed entities 内の agency_id は agency_name を指しています。GTFS Schedule feed には1つの事業者（Calgary Transit）しか存在しないため、`informed_entity` には `route_id` のみを保持する方がよいです。
* ヘッダーを追加するべきです。
* 説明文はプレーンテキストにするべきです。
* 説明には、運行情報(alert)の原因と影響が含まれているようです。
  * 原因は `WEATHER` として追加するべきです。
  * 言及されている影響は `SIGNIFICANT_DELAYS` です。しかし、説明は閉鎖された停留所等(stop)または迂回の可能性を示唆しています。したがって、影響とともに説明を明確化するべきです。
  * 可能であれば、運行情報(alert)を複数の運行情報(alert)に分割してください。1つは停留所等(stop)の閉鎖または迂回用、もう1つはそれによって発生する可能性のある重大な遅延用です。
  * `active_period` の代わりに、フィールド `communication_period` および `impact_period` を使用するべきです。

```json
{
  "id": "143607",
  "alert": {
    "activePeriod": [
      {
        "start": "1743605700",
        "end": "1743832740"
      }
    ],
    "informedEntity": [
      {
        "agencyId": "Calgary Transit",
        "routeId": "123"
      }
    ],
    "headerText": {
      "translation": [
        {
          "text": "",
          "language": "en"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "<div data-headertext=\"Major service delay\" data-cause=\"WEATHER\" data-effect=\"SIGNIFICANT_DELAYS\"></div><p>Due to current weather conditions, we are unable to serve Hidden Creek Drive N.W.</b>\n<p><b>Stops temporarily closed:</b>\n<p>Northbound: #9340, #9341, #9342 and #9705\n<p><b>Buses running to North Point will travel:</b>\n<ul>\n<li>From Hidden Valley Link N.W.\n<li>South on Beddington Trail N.W.\n<li>East on Country Hills Boulevard N.W.\n<li>North on Panorma Hills Boulevard N.W.\n<li>West on Panamount Boulevard N.W.\n<li>East on Pantella Boulevard N.W. to regular route\n</ul>\n<p>For more information on snow detours, please visit calgarytransit.com/snowdetours",
          "language": "en"
        }
      ]
    }
  }
}
```

Mobility Database の feed には[こちら](https://mobilitydatabase.org/feeds/gtfs_rt/tld-1080-sa)からアクセスしてください。

## 8) MTA New York City Transit {: #8-mta-new-york-city-transit}


公開日: 02-04-2025

* ヘッダーは説明よりもかなり長くなっています。ヘッダーの後半部分である *" - use the stops on Lexington Ave at E 53rd St or E 41st St instead"* は、説明に移すことができます。
* Lexington Ave at E 46th St の `stop_id` を informed entities 内の各 entity に追加し、運行情報(alert)が「Lexington Ave at E 46th St 」停留所等(stop)における影響を受けるルート・路線系統(route)にのみ適用されることを指定するべきです。影響を受けるルート・路線系統(route)がその停留所等(stop)を利用する唯一のルート・路線系統(route)である場合、`route_ids` を含める必要はありません。
* 原因は `CONSTRUCTION` として追加するべきです。
* 影響は `NO_SERVICE` として追加するべきです。
* "</b>" のような HTML entities は、プレーンテキストの代替表現に置き換えるべきです。
* "en-html" 翻訳は削除するべきです。
* `active_period` の代わりに、フィールド `communication_period` および `impact_period` を使用するべきです。

```json
{
  "id": "lmm:planned_work:21840",
  "alert": {
    "activePeriod": [
      {
        "start": "1733288400",
        "end": "1759104000"
      }
    ],
    "informedEntity": [
      {
        "agencyId": "MTA NYCT",
        "routeId": "SIM11"
      },
      {
        "agencyId": "MTA NYCT",
        "routeId": "SIM6"
      }
    ],
    "headerText": {
      "translation": [
        {
          "text": "Southbound SIM6 and SIM11 buses are skipping the stop on Lexington Ave at E 46th St - use the stops on Lexington Ave at E 53rd St or E 41st St instead",
          "language": "en"
        },
        {
          "text": "<p>Southbound <b>SIM6</b> and <b>SIM11</b> buses are skipping the stop on Lexington Ave at E 46th St - use the stops on Lexington Ave at E 53rd St or E 41st St instead</p>",
          "language": "en-html"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "See a map of the bypass.\n\nWhat's happening?\nWater main construction",
          "language": "en"
        },
        {
          "text": "<p><a title=\"\" href=\"https://files.mta.info/s3fs-public/2024-12/Southbound%20%E2%80%8CSIM6%E2%80%8C%20and%20%E2%80%8CSIM11%E2%80%8C%20stop%20on%20Lexington%20Ave%20at%20E%2046th%20St%20is%20being%20skipped.png\" rel=\"noopener noreferrer nofollow\" data-link-auto=\"\" target=\"_blank\">See a map</a> of the bypass.</p><p></p><p><strong>What's happening?</strong></p><p>Water main construction</p>",
          "language": "en-html"
        }
      ]
    }
  }
}
```

Mobility Database のフィードには[こちら](https://mobilitydatabase.org/feeds/gtfs_rt/tld-5978-sa)からアクセスできます。

## 9) MTA Bus Company {: #9-mta-bus-company}


公開日: 02-04-2025

* effect は `ACCESSIBILITY_ISSUE` に変更するべきです。
* cause は `MAINTENANCE` または `CONSTRUCTION` に変更するべきです。
* `informed_entity` では、[7] プラットフォームの `stop_id` に加えて、駅についても記載するべきです。
* 「accessibility icon」は削除し、「&nbsp;」のような HTML エンティティはプレーンテキストの代替表現に置き換えるべきです。
* 「en-html」翻訳は削除するべきです。
* `active_period` の代わりに、フィールド `communication_period` および `impact_period` を使用するべきです。

```json
{
  "id": "lmm:planned_work:22052",
  "alert": {
    "activePeriod": [
      {
        "start": "1734411600",
        "end": "1758672000"
      }
    ],
    "informedEntity": [
      {
        "agencyId": "MTABC",
        "routeId": "Q70+"
      }
    ],
    "headerText": {
      "translation": [
        {
          "text": "74 St-Broadway/Jackson Hts-Roosevelt Av [E][F][M][R][7] Station elevators from the street level to mezzanine and to/from the [7] platform are closed",
          "language": "en"
        },
        {
          "text": "<p><b>74 St-Broadway</b>/<b>Jackson Hts-Roosevelt Av</b> [E][F][M][R][7] Station elevators from the street level to mezzanine and to/from the [7] platform are closed</p>",
          "language": "en-html"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "accessibility icon The nearest accessible subway station connecting with the Q70-SBS is 61 St-Woodside accessibility icon [7]. \n\nFor additional travel alternatives, plan your trip at mta.info, use the MTA app (download the app for iOS or Android), or check our Elevator & Escalator Status page.\n\nWhat's happening?We're replacing the elevators",
          "language": "en"
        },
        {
          "text": "<p>[accessibility icon] The nearest accessible subway station connecting with the <b>Q70-SBS</b> is <b>61 St-Woodside</b> [accessibility icon] [7]. </p><p style=\"min-height:10px\"></p><p>For additional travel alternatives, plan your trip at <a title=\"\" href=\"https://new.mta.info/\" rel=\"noopener noreferrer nofollow\" data-link-auto=\"\" target=\"_blank\">mta.info</a>, use the MTA app (download the app for <a title=\"\" href=\"https://apps.apple.com/us/app/mymta/id1297605670\" rel=\"noopener noreferrer nofollow\" target=\"_blank\">iOS</a> or <a title=\"\" href=\"https://play.google.com/store/apps/details?id=info.mta.mymta&amp;hl=en_US&amp;gl=US\" rel=\"noopener noreferrer nofollow\" target=\"_blank\">Android</a>), or check our Elevator &amp; Escalator Status page.</p><p style=\"min-height:10px\"></p><p><strong>What's happening?</strong><br><a title=\"\" href=\"https://new.mta.info/project/station-accessibility-upgrades/elevator-replacements\" rel=\"noopener noreferrer nofollow\" data-link-auto=\"\" target=\"_blank\">We're replacing the elevators</a></p>",
          "language": "en-html"
        }
      ]
    }
  }
}
```

Mobility Database のフィードには[こちら](https://mobilitydatabase.org/feeds/gtfs_rt/mdb-1628)からアクセスしてください。

## 10) BigBlueBus {: #10-bigbluebus}


公開日: 02-04-2025

* 運行情報(alert)には、ヘッダー内にroute 7を含めることもできます。停留所等(stop)がroute 7によってのみ通過される場合は、informed entitiesにroute 7を含めることもできます。
* 代替の停留所等(stop)は、運行情報(alert)の説明に記載するべきです。
* URLは、この運行情報(alert)に関連する具体的な情報ではなく、事業者の一般的な運行情報(alert)ページへのリンクであるように見えます。URLは、乗客を運行情報(alert)の詳細を示すページ、またはそれに直接関連する情報を含むページへ誘導する必要があります。
* `active_period`の代わりに、`communication_period`および`impact_period`フィールドを使用するべきです。

```json
{
  "id": "3f0a0d8a-f751-4d79-843a-98227f597a7d",
  "alert": {
    "activePeriod": [
      {
        "start": "1736185080"
      }
    ],
    "informedEntity": [
      {
        "stopId": "1717"
      }
    ],
    "cause": "MAINTENANCE",
    "effect": "NO_SERVICE",
    "url": {
      "translation": [
        {
          "text": "https://www.bigbluebus.com/Newsroom/Content.aspx?type=Alerts"
        }
      ]
    },
    "headerText": {
      "translation": [
        {
          "text": "Pico Blvd.およびStanley Ave.の停留所閉鎖"
        }
      ]
    },
    "descriptionText": {
      "translation": [
        {
          "text": "Stanley Ave.のWB Pico Blvd.に位置するroute 7のバス停は、追って通知があるまで閉鎖されます。代替の停留所等(stop)については、bigbluebus.com/servicealertsをご覧ください。"
        }
      ]
    }
  }
}
```

Mobility Databaseでフィードにアクセスするには[こちら](https://mobilitydatabase.org/feeds/gtfs_rt/mdb-1437)をご覧ください。
