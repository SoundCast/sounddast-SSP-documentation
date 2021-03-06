# SSP Documentation

This guide covers the latest version of the SoundCast Real-time Bidding Protocol based on the latest [OpenRTB Protocol Specification V2.5](https://wiki.iabtechlab.com/index.php?title=OpenRTB_2.5). 

## Request

The invitation to participate in an auction is sent using the POST method in JSON format (Content-Type: application/json) encoded in GZIP (Accept-Encoding: gzip).

### Header

The request header contains the OpenRTB Protocol Version (x-openrtb-version: 2.5).

### Body

The request body contains the Bid Request object. Its parameters describe the site, endpoint device, and consumer. These characteristics help the DSP select an ad and a bid.

## Bid Request Model (V2.5)

Bid request model for OpenRTB Protocol 2.5 is shown below.

### Bid Object

This is the top level object that is sent to the Buyer. Each bid request sent from SoundCast to a Buyer will contain the following fields.

| Key | Type | Description |
| --- | ---- | ----------- |
| id | string | Unique ID of the bid request. |
| cur | array of strings | Array of allowed currencies for bids on this bid request using ISO-4217 alpha codes. |
| allimps | integer  | A flag to indicate if the Supplier can verify that the impressions offered represent all of the impressions available in context to support road-blocking. |
| tmax | integer | Maximum time in milliseconds the exchange allows for bids to be received to avoid timeout, including internet latency |
| bcat | array of strings | Blocked Advertiser Categories, using the IAB taxonomy. |
| at | integer | Auction type. |
| device | object | 	Device object with details about the device to which the impression will be delivered, for more information, see the [Device Object](#device-object) section. |
| user | object | User Object which describes the user, for more information, see the [User Object](#user-object) section. |
| source | object | Indicates the entity responsible for the final impression sale decision, for more information, see the [Source Object](#source-object). |
| ext | object | ext is an json object used to extend the data |
| imp | array of objects | Array of objects representing the impressions offered, for more information, see the [Impression Object](#impression-object) section. |
| site | object | The Site Object describing the site, for more information, see the [Site Object](#site-object) section.  |


#### Device Object


| Key | Type | Description |
| --- | ---- | ----------- |
| js | integer | 1 if the device supports JavaScript; otherwise 0. |
| ip | string | Specifies the IPv4 address closest to the device. |
| geo | object | Geo Object as derived from the device’s location services, or supplied by the Supplier if the device IP is missing. For more information, see the [Geo Object](#geo-object) section. |
| language | string | Alpha-2/ISO 639-1 code of browser language. |
| ua | string | Browser or application user agent string. |
| osv | string | Device operating system version |
| devicetype | integer | Device type as defined by OpenRTB. |
| w | integer | Physical height of the screen in pixels. |
| h | integer | Physical width of the screen in pixels. |
| pxratio | float | The ratio of physical pixels to device independent pixels. |
| ipv6 | string | IP address in IPv6. |


##### Geo Object


| Key | Type | Description |
| --- | ---- | ----------- |
| type | integer | Source of location data as defined by OpenRTB. |
| lat | double | Latitude from -90 to 90. South is negative. |
| lon | double | Longitude from -180 to 180. West is negative. |
| region | string | Region using ISO-3166-2 or FIPS region codes. |
| country | string | Country using ISO-3166-1 Alpha-2. |
| city | string | City name as provided by GeoIP. |
| zip | integer | Zip/postal code. |
| utcoffset | integer | Local time as the number +/- of minutes from UTC. |


#### User Object


| Key | Type | Description |
| --- | ---- | ----------- |
| id | string | Unique SoundCast ID of this user |
| consent | string | The binary encoding scheme that is passed in base64 URL/web safe format known as daisybit. |


#### Source Object


| Key | Type | Description |
| --- | ---- | ----------- |
| fd | integer | Indicates the entity responsible for the final impression sale decision |


#### Impression object


| Key | Type | Description |
| --- | ---- | ----------- |
| id | string | ID of the impression being shown, unique within the bid request. |
| bidfloor | float | Bid floor in CPM as set by the Supplier |
| bidfloorcur | string | Bid floor currency specified using ISO-4217 alpha codes. |
| instl | integer | Specifies if the ad is an interstitial. |
| exp | integer | Impression expiry timeout, in seconds. |
| displaymanager | string | Name of the ad mediation partner, SDK technology. |
| displaymanagerver | string | Version of the ad mediation partner, SDK technology. |
| tagid | integer | Identifier for specific ad placement or ad tag that was used to initiate the auction. |
| audio | object | An [Audio Object](#audio-object) |
| pmp | object | An [PrivateMarket Object](#privatemarket-object) |
| secure | integer | Specifies if the page is SSL compliant. |


##### Audio Object


| Key | Type | Description |
| --- | ---- | ----------- |
| mimes | string array | Array of content MIME types supported (e.g., “audio/mp4”) |
| minduration | integer | Minimum audio ad duration in seconds. |
| maxduration | integer | Maximum audio ad duration in seconds. |
| startdelay | integer | Indicates the start delay in seconds for pre-roll, mid-roll, or post-roll ad placements. see the [Start Delay Values](#start-delay-values) section. |
| sequence | integer | If multiple ad impressions are offered in the same bid request, the sequence number will allow for the coordinated delivery of multiple creatives. |
| battr | integer array | Blocked creative attributes. see the [Battr Values](#battr-values) |
| maxextended | integer | Maximum extended ad duration if extension is allowed. If blank or 0, extension is not allowed. If -1, extension is allowed, and there is no time limit imposed. If greater than 0, then the value represents the number of seconds of extended play supported beyond the maxduration value. |
| minbitrate | integer | Minimum bit rate in Kbps. |
| maxbitrate | integer | Maximum bit rate in Kbps. |
| delivery | integer array | Supported delivery methods (e.g., streaming, progressive). If none specified, assume all are supported. See [Content Delivery Methods](#content-delivery-methods-values) |
| companionad | object array | Array of [Banner Object](#banner-object) when companionad will be available.|
| companiontype | integer array | Supported DAAST companion ad types (see [Companion Types Values](#companion-types-values)). Recommended if companion [Banner Object](#banner-object) are included via the companionad array.|
| maxseq | integer | The maximum number of ads that can be played in an ad pod. |
| nvol | integer | Volume normalization mode. Refer to List [Volume Normalization Modes Values](#volume-normalization-modes-values) |
| stitched | integer | Indicates if the ad is stitched with audio content or delivered independently, where 0 = no, 1 = yes. |
| ext | object | Placeholder for exchange-specific extensions to OpenRTB.
| feed | integer | Type of audio feed. See [Feed Types Values](#feed-types-values) |

###### Start Delay Values


| Value | Description |
| ----- | ----------- |
| > 0 | Mid-Roll (value indicates start delay in second) |
| 0 | Pre-Roll |
| -1 | Generic Mid-Roll |
| -2 | Generic Post-Roll |

###### Battr Values


| Value | Description |
| ----- | ----------- |
| 1 | Audio Ad (Auto-Play) |
| 2 | Audio Ad (User Initiated) |
| 3 | Expandable (Automatic) |
| 4 | Expandable (User Initiated - Click) |
| 5 | Expandable (User Initiated - Rollover) |
| 6 | In-Banner Video Ad (Auto-Play) |
| 7 | In-Banner Video Ad (User Initiated) |
| 8 | Pop (e.g., Over, Under, or Upon Exit) |
| 9 | Provocative or Suggestive Imagery |
| 10 | Shaky, Flashing, Flickering, Extreme Animation, Smileys |
| 11 | Surveys |
| 12 | Text Only |
| 13 | User Interactive (e.g., Embedded Games) |
| 14 | Windows Dialog or Alert Style |
| 15 | Has Audio On/Off Button |
| 16 | Ad Provides Skip Button (e.g. VPAID-rendered skip button on pre-roll video) |
| 17 Adobe Flash |

###### Delivery Values


| Value | Description |
| ----- | ----------- |
| 1 | Streaming |
| 2 | Progressive |
| 3 | Download |

###### Companion Types Values


| Value | Description |
| ----- | ----------- |
| 1 | Static Resource |
| 2 | HTML Resource |
| 3 | iframe Resource |

###### Content Delivery Methods Values


| Value | Description |
| ----- | ----------- |
| 1 | Streaming |
| 2 | Progressive |
| 3 Download |

######  Volume Normalization Modes Values


| Value | Description |
| ----- | ----------- |
| 0 | None |
| 1 | Ad Volume Average Normalized to Content |
| 2 | Ad Volume Peak Normalized to Content |
| 3 | Ad Loudness Normalized to Content |
| 4 | Custom Volume Normalization |

###### Feed Types Values


| Value | Description |
| ----- | ----------- |
| 1 | Music Service |
| 2 | FM/AM Broadcast |
| 3 | Podcast |

##### Banner Object (companion Ads only)


| Key | Type | Description |
| --- | ---- | ----------- |
| id | string | Unique identifier for the banner object |
| topframe | integer | Indicates if the banner is in the top frame as opposed to an iframe. |
| battr | array of integers | Blocked creative attributes as defined in the OpenRTB protocol. |
| btype | array of integers | Blocked banner ad types as defined in the OpenRTB protocol. |
| pos | integer | Ad Position as defined in the OpenRTB protocol. |
| mimes | array of strings | Specifies the content MIME types supported. |
| h | integer | Height of the impression in pixels. |
| w | integer | Width of the impression in pixels. |
| format | array of objects | Array of objects denoting the alternative sizes that may be used for bidding, for more information, see the [BannerFormat Object](#bannerformat-object) section. |

###### BannerFormat Object (companion Ads only)


| Key | Type | Description |
| --- | ---- | ----------- |
| h | integer | Height of the impression in pixels. |
| w | integer | Width of the impression in pixels. |

##### PrivateMarket Object


| Key | Type | Description |
| --- | ---- | ----------- |
| deals | array of objects | Array of Deal objects, for more information, see the [Deal Object](#deal-object) section. |
| private_auction | integer | A value of 1 indicates that only bids submitted inside pmp.deals will take part in the auction. A value of 0 indicates that bids without deal information may also be considered for serving. |


###### Deal Object


| Key | Type | Description |
| --- | ---- | ----------- |
| id | string | Deal ID |
| bidfloorcur | string | Bid floor currency specified using ISO-4217 alpha codes. |
| bidfloor | float | Deal price in CPM. |
| at | integer | Auction type. |


#### Site Object


| Key | Type | Description |
| --- | ---- | ----------- |
| id | string | An exchange specific identifier |
| publisher | object | Publisher object, for more information, see the [Publisher Object](#publisher-object) section. |
| domain | string | Domain of the site, used for advertiser side blocking. |
| ref | string | Referrer URL that caused navigation to the current page. |
| page | string | URL of the page where the impression will be shown. |
| name | string | Site name. |


##### Publisher Object


| Key | Type | Description |
| --- | ---- | ----------- |
| id | string | An exchange specific identifier. |
| domain | string | The highest level domain of the publisher. |
| name | string | Publisher name. |


## Bid Request Sample

```
{
  "id": "01E33AQJHG814RSJX1ZGMEBW3P",
  "imp": [
    {
      "id": "01E33AQJHGTFDP8SBNDQ5QZ05G",
      "audio": {
        "mimes": [
          "audio/mpeg",
          "audio/mp3",
          "audio/mpeg3",
          "audio/x-mpeg-3"
        ],
        "minduration": 10,
        "maxduration": 30,
        "protocols": [
          2,
          9,
          10
        ],
        "startdelay": 1,
        "sequence": 1,
        "battr": [
          1,
          2
        ],
        "minbitrate": 128,
        "maxbitrate": 320,
        "api": [
          3,
          5,
          4,
          1,
          2
        ],
        "feed": 3,
        "nvol": 3
      },
      "pmp": {
        "deals": [
          {
            "id": "5e3ac0d83a6d0",
            "bidfloor": 1,
            "bidfloorcur": "EUR",
            "at": 1,
            "ext": {
              "data_src": "Soundcast SSP",
              "Soundcast": null
            }
          }
        ]
      },
      "displaymanager": "Soundcast SSP",
      "displaymanagerver": "1.0",
      "tagid": "5c505f593adaa",
      "secure": 1,
      "exp": 120
    }
  ],
  "site": {
    "id": "5c505f0ea5ceb",
    "name": "Auto Moto ",
    "domain": "www.automoto.com",
    "cat": [
      "1"
    ],
    "sectioncat": [
      "1"
    ],
    "pagecat": [
      "1"
    ],
    "publisher": {
      "id": "5e3301babefba-5c505f0ea5ceb",
      "name": "Auto Moto ",
      "cat": [
        "1"
      ],
      "domain": "www.automoto.com"
    },
    "content": {
      "id": "01E33AQJHG3TBZE5GSF7DPR2N9",
      "title": "Podcast - Auto-Moto",
      "url": "www.automoto.com",
      "cat": [
        "1"
      ],
      "language": "fr"
    },
    "keywords": "95, Arabie Saoudite, brent, carburant, gazole, moyenne, opep, pompe, prix de l'essence, russie, sans-plomb",
    "mobile": 1
  },
  "device": {
    "ua": "Mozilla/5.0 (Linux; Android 7.0; Q8S55IN4G2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.112 Mobile Safari/537.36",
    "geo": {
      "lat": 43.3242,
      "lon": 2.7375,
      "type": 2,
      "accuracy": 20,
      "country": "FRA",
      "region": "OCC",
      "metro": "0",
      "city": "Azillanet",
      "utcoffset": 3600
    },
    "ip": "xxx.xxx.xxx.xxx",
    "devicetype": 4,
    "os": "Android 7.0",
    "osv": "7.0"
  },
  "user": {
    "id": "01E33AQJHG3TBZE5GSF7DPR2N9",
    "geo": {
      "lat": 43.3242,
      "lon": 2.7375,
      "type": 2,
      "accuracy": 20,
      "country": "FRA",
      "region": "OCC",
      "metro": "0",
      "city": "Azillanet",
      "utcoffset": 3600
    },
    "ext": {
      "consent": "BOu923LOu924gAHABBFRC--AAAAuhr_7__7-_9_-_f__9uj3Or_v_f__30ccL59v_B_zv-_7fi_20jV4u_1vft9yfk1-5ctDztp507iakivXmqdeb9v_nz3_5pxP78k89r7337Ew_v8_v8b7BCIJAAAAAA"
    }
  },
  "at": 1,
  "tmax": 900,
  "wseat": [
    "1"
  ],
  "wlang": [
    "fr"
  ],
  "cur": [
    "EUR"
  ],
  "bcat": [
    "IAB26",
    "IAB25",
    "IAB24",
    "IAB23",
    "IAB14-3",
    "IAB13-2"
  ],
  "source": {
    "fd": 1
  },
  "regs": {
    "ext": {
      "gdpr": 1
    }
  },
  "pmp": {
    "private_auction": 1
  }
}

```

## Any Questions
Please contact our support team.

