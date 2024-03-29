---
title: flutter 解析复杂的json
date: 
tags: 
- flutter 
- dart 
copyright: true
categories: flutter
---



<blockquote class="blockquote-center">时光逆转成红色的晨雾，昼夜逐渐平分。。</blockquote>

<!-- more -->

##### flutter 解析json练习 

```Java
{
  "count": 100,
  "start": 0,
  "total": 41,
  "subjects": [
    {
      "rating": {
        "max": 10,
        "average": 6.7,
        "details": {
          "1": 352.0,
          "3": 12759.0,
          "2": 2613.0,
          "5": 2337.0,
          "4": 7586.0
        },
        "stars": "35",
        "min": 0
      },
      "genres": [
        "\u559c\u5267",
        "\u52a8\u753b",
        "\u5947\u5e7b"
      ],
      "title": "\u5927\u4fa6\u63a2\u76ae\u5361\u4e18",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p48608.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p48608.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p48608.webp"
          },
          "name_en": "Ryan Reynolds",
          "name": "\u745e\u6069\u00b7\u96f7\u8bfa\u5179",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1053623\/",
          "id": "1053623"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1523860646.23.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1523860646.23.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1523860646.23.webp"
          },
          "name_en": "Justice Smith",
          "name": "\u8d3e\u65af\u8482\u65af\u00b7\u53f2\u5bc6\u65af",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1350981\/",
          "id": "1350981"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1430023168.56.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1430023168.56.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1430023168.56.webp"
          },
          "name_en": "Kathryn Newton",
          "name": "\u51ef\u745f\u7433\u00b7\u7ebd\u987f",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1174320\/",
          "id": "1174320"
        }
      ],
      "durations": [
        "104\u5206\u949f"
      ],
      "collect_count": 138189,
      "mainland_pubdate": "2019-05-10",
      "has_video": false,
      "original_title": "Pok\u00e9mon Detective Pikachu",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1446386865.82.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1446386865.82.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1446386865.82.webp"
          },
          "name_en": "Rob Letterman",
          "name": "\u7f57\u4f2f\u00b7\u83b1\u7279\u66fc",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1304840\/",
          "id": "1304840"
        }
      ],
      "pubdates": [
        "2019-05-10(\u7f8e\u56fd)",
        "2019-05-10(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555538168.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555538168.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555538168.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26835471\/",
      "id": "26835471"
    },
    {
      "rating": {
        "max": 10,
        "average": 6.9,
        "details": {
          "1": 28.0,
          "3": 943.0,
          "2": 178.0,
          "5": 255.0,
          "4": 630.0
        },
        "stars": "35",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u559c\u5267",
        "\u5bb6\u5ead"
      ],
      "title": "\u4e00\u6761\u72d7\u7684\u4f7f\u547d2",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p52.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p52.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p52.webp"
          },
          "name_en": "Dennis Quaid",
          "name": "\u4e39\u5c3c\u65af\u00b7\u594e\u5fb7",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1002679\/",
          "id": "1002679"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p26177.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p26177.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p26177.webp"
          },
          "name_en": "Kathryn Prescott",
          "name": "\u51ef\u745f\u7433\u00b7\u666e\u96f7\u65af\u79d1\u7279",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1265538\/",
          "id": "1265538"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1558029075.72.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1558029075.72.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1558029075.72.webp"
          },
          "name_en": "Henry Lau",
          "name": "\u5218\u5baa\u534e",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1335689\/",
          "id": "1335689"
        }
      ],
      "durations": [
        "108\u5206\u949f"
      ],
      "collect_count": 17417,
      "mainland_pubdate": "2019-05-17",
      "has_video": false,
      "original_title": "A Dog's Journey",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1498769826.44.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1498769826.44.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1498769826.44.webp"
          },
          "name_en": "Gail Mancuso",
          "name": "\u76d6\u5c14\u00b7\u66fc\u5e93\u7d22",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1331582\/",
          "id": "1331582"
        }
      ],
      "pubdates": [
        "2019-05-17(\u7f8e\u56fd)",
        "2019-05-17(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555859678.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555859678.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555859678.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/27074316\/",
      "id": "27074316"
    },
    {
      "rating": {
        "max": 10,
        "average": 8.6,
        "details": {
          "1": 550.0,
          "3": 12953.0,
          "2": 1383.0,
          "5": 44700.0,
          "4": 31357.0
        },
        "stars": "45",
        "min": 0
      },
      "genres": [
        "\u52a8\u4f5c",
        "\u79d1\u5e7b",
        "\u5947\u5e7b"
      ],
      "title": "\u590d\u4ec7\u8005\u8054\u76df4\uff1a\u7ec8\u5c40\u4e4b\u6218",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p56339.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p56339.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p56339.webp"
          },
          "name_en": "Robert Downey Jr.",
          "name": "\u5c0f\u7f57\u4f2f\u7279\u00b7\u5510\u5c3c",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1016681\/",
          "id": "1016681"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1359877729.49.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1359877729.49.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1359877729.49.webp"
          },
          "name_en": "Chris Evans",
          "name": "\u514b\u91cc\u65af\u00b7\u57c3\u6587\u65af",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1017885\/",
          "id": "1017885"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p15885.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p15885.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p15885.webp"
          },
          "name_en": "Mark Ruffalo",
          "name": "\u9a6c\u514b\u00b7\u9c81\u5f17\u6d1b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1040505\/",
          "id": "1040505"
        }
      ],
      "durations": [
        "181\u5206\u949f"
      ],
      "collect_count": 662996,
      "mainland_pubdate": "2019-04-24",
      "has_video": false,
      "original_title": "Avengers: Endgame",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1555672594.77.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1555672594.77.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1555672594.77.webp"
          },
          "name_en": "Anthony Russo",
          "name": "\u5b89\u4e1c\u5c3c\u00b7\u7f57\u7d20",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1321812\/",
          "id": "1321812"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1525505053.79.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1525505053.79.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1525505053.79.webp"
          },
          "name_en": "Joe Russo",
          "name": "\u4e54\u00b7\u7f57\u7d20",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1320870\/",
          "id": "1320870"
        }
      ],
      "pubdates": [
        "2019-04-24(\u4e2d\u56fd\u5927\u9646)",
        "2019-04-26(\u7f8e\u56fd)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552058346.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552058346.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552058346.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26100958\/",
      "id": "26100958"
    },
    {
      "rating": {
        "max": 10,
        "average": 9.0,
        "details": {
          "1": 46.0,
          "3": 2059.0,
          "2": 136.0,
          "5": 19507.0,
          "4": 13130.0
        },
        "stars": "45",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5"
      ],
      "title": "\u4f55\u4ee5\u4e3a\u5bb6",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pa96FEdlJe08cel_avatar_uploaded1526709219.38.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pa96FEdlJe08cel_avatar_uploaded1526709219.38.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pa96FEdlJe08cel_avatar_uploaded1526709219.38.webp"
          },
          "name_en": "Zain al-Rafeea",
          "name": "\u8d5e\u6069\u00b7\u963f\u5c14\u00b7\u62c9\u83f2\u4e9a",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1393813\/",
          "id": "1393813"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551513231.02.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551513231.02.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551513231.02.webp"
          },
          "name_en": "Yordanos Shiferaw",
          "name": "\u7ea6\u4e39\u8bfa\u65af\u00b7\u5e0c\u8d39\u7f57",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1411924\/",
          "id": "1411924"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1549962696.77.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1549962696.77.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1549962696.77.webp"
          },
          "name_en": "Boluwatife Treasure Bankole",
          "name": "\u535a\u9c81\u74e6\u8482\u592b\u00b7\u7279\u96f7\u6770\u00b7\u73ed\u79d1\u5c14",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1395545\/",
          "id": "1395545"
        }
      ],
      "durations": [
        "126\u5206\u949f",
        "117\u5206\u949f(\u4e2d\u56fd\u5927\u9646)"
      ],
      "collect_count": 210787,
      "mainland_pubdate": "2019-04-29",
      "has_video": false,
      "original_title": "\u0643\u0641\u0631\u0646\u0627\u062d\u0648\u0645",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p18917.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p18917.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p18917.webp"
          },
          "name_en": "Nadine Labaki",
          "name": "\u5a1c\u4e01\u00b7\u62c9\u5df4\u57fa",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1275745\/",
          "id": "1275745"
        }
      ],
      "pubdates": [
        "2018-05-17(\u621b\u7eb3\u7535\u5f71\u8282)",
        "2018-09-20(\u9ece\u5df4\u5ae9)",
        "2019-04-29(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555295759.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555295759.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555295759.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/30170448\/",
      "id": "30170448"
    },
    {
      "rating": {
        "max": 10,
        "average": 3.6,
        "details": {
          "1": 118.0,
          "3": 30.0,
          "2": 87.0,
          "5": 5.0,
          "4": 11.0
        },
        "stars": "20",
        "min": 0
      },
      "genres": [
        "\u7231\u60c5",
        "\u60ac\u7591"
      ],
      "title": "\u53cc\u751f",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1473508881.63.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1473508881.63.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1473508881.63.webp"
          },
          "name_en": "Haoran Liu",
          "name": "\u5218\u660a\u7136",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1336305\/",
          "id": "1336305"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1501332774.05.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1501332774.05.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1501332774.05.webp"
          },
          "name_en": "Duling Chen",
          "name": "\u9648\u90fd\u7075",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1342249\/",
          "id": "1342249"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1502175019.57.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1502175019.57.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1502175019.57.webp"
          },
          "name_en": "Rui Zhao",
          "name": "\u8d75\u82ae",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1323767\/",
          "id": "1323767"
        }
      ],
      "durations": [
        "82\u5206\u949f"
      ],
      "collect_count": 6136,
      "mainland_pubdate": "2019-05-18",
      "has_video": false,
      "original_title": "\u53cc\u751f",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551949192.18.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551949192.18.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551949192.18.webp"
          },
          "name_en": "Jin-seong Kim",
          "name": "\u91d1\u632f\u6210",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1411883\/",
          "id": "1411883"
        }
      ],
      "pubdates": [
        "2019-05-18(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554892284.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554892284.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554892284.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26715617\/",
      "id": "26715617"
    },
    {
      "rating": {
        "max": 10,
        "average": 9.1,
        "details": {
          "1": 69.0,
          "3": 5376.0,
          "2": 375.0,
          "5": 52633.0,
          "4": 26265.0
        },
        "stars": "45",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u5192\u9669",
        "\u5bb6\u5ead"
      ],
      "title": "\u6d77\u8482\u548c\u7237\u7237",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pI6v5IZ05dJscel_avatar_uploaded1468081618.44.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pI6v5IZ05dJscel_avatar_uploaded1468081618.44.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pI6v5IZ05dJscel_avatar_uploaded1468081618.44.webp"
          },
          "name_en": "Anuk Steffen",
          "name": "\u963f\u52aa\u514b\u00b7\u65af\u7279\u82ac",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1359966\/",
          "id": "1359966"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p2443.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p2443.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p2443.webp"
          },
          "name_en": "Bruno Ganz",
          "name": "\u5e03\u9c81\u8bfa\u00b7\u7518\u8328",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1000247\/",
          "id": "1000247"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1500948178.65.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1500948178.65.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1500948178.65.webp"
          },
          "name_en": "Quirin Agrippi",
          "name": "\u6606\u6797\u00b7\u827e\u683c\u5339",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1360719\/",
          "id": "1360719"
        }
      ],
      "durations": [
        "111\u5206\u949f"
      ],
      "collect_count": 125911,
      "mainland_pubdate": "2019-05-16",
      "has_video": false,
      "original_title": "Heidi",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p40705.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p40705.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p40705.webp"
          },
          "name_en": "Alain Gsponer",
          "name": "\u963f\u5170\u00b7\u845b\u65af\u5f6d\u7eb3",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1033402\/",
          "id": "1033402"
        }
      ],
      "pubdates": [
        "2015-12-10(\u5fb7\u56fd)",
        "2019-05-16(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2015",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554525534.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554525534.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554525534.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/25958717\/",
      "id": "25958717"
    },
    {
      "rating": {
        "max": 10,
        "average": 6.7,
        "details": {
          "1": 41.0,
          "3": 1299.0,
          "2": 263.0,
          "5": 224.0,
          "4": 740.0
        },
        "stars": "35",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u72af\u7f6a"
      ],
      "title": "\u4e00\u4e2a\u6bcd\u4eb2\u7684\u590d\u4ec7",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p58658.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p58658.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p58658.webp"
          },
          "name_en": "Sridevi",
          "name": "\u5e0c\u91cc\u9edb\u7389",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1054499\/",
          "id": "1054499"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p7647.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p7647.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p7647.webp"
          },
          "name_en": "Akshaye Khanna",
          "name": "\u963f\u514b\u590f\u8036\u00b7\u574e\u7eb3",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1000509\/",
          "id": "1000509"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1557548110.45.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1557548110.45.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1557548110.45.webp"
          },
          "name_en": "Sajal Ali",
          "name": "\u8428\u4f73\u00b7\u963f\u91cc",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1399864\/",
          "id": "1399864"
        }
      ],
      "durations": [
        "146\u5206\u949f"
      ],
      "collect_count": 19143,
      "mainland_pubdate": "2019-05-10",
      "has_video": false,
      "original_title": "Mom",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1535337603.88.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1535337603.88.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1535337603.88.webp"
          },
          "name_en": "Ravi Udyawar",
          "name": "\u62c9\u7ef4\u00b7\u5fb7\u8036\u74e6\u5c14",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1399862\/",
          "id": "1399862"
        }
      ],
      "pubdates": [
        "2017-07-07(\u5370\u5ea6)",
        "2019-05-10(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2017",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554613277.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554613277.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554613277.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26996734\/",
      "id": "26996734"
    },
    {
      "rating": {
        "max": 10,
        "average": 7.8,
        "details": {
          "1": 20.0,
          "3": 667.0,
          "2": 73.0,
          "5": 654.0,
          "4": 1230.0
        },
        "stars": "40",
        "min": 0
      },
      "genres": [
        "\u52a8\u753b",
        "\u5947\u5e7b",
        "\u5192\u9669"
      ],
      "title": "\u4f01\u9e45\u516c\u8def",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1484904556.03.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1484904556.03.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1484904556.03.webp"
          },
          "name_en": "Kana Kita",
          "name": "\u5317\u9999\u90a3",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1367253\/",
          "id": "1367253"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1493110606.27.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1493110606.27.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1493110606.27.webp"
          },
          "name_en": "Yu Aoi",
          "name": "\u82cd\u4e95\u4f18",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1018990\/",
          "id": "1018990"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p4971.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p4971.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p4971.webp"
          },
          "name_en": "Rie Kugimiya",
          "name": "\u9489\u5bab\u7406\u60e0",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1018687\/",
          "id": "1018687"
        }
      ],
      "durations": [
        "117\u5206\u949f"
      ],
      "collect_count": 10973,
      "mainland_pubdate": "2019-05-17",
      "has_video": false,
      "original_title": "\u30da\u30f3\u30ae\u30f3\u30fb\u30cf\u30a4\u30a6\u30a7\u30a4",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p51197.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p51197.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p51197.webp"
          },
          "name_en": "Hiroyasu Ishida",
          "name": "\u77f3\u7530\u7950\u5eb7",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1321734\/",
          "id": "1321734"
        }
      ],
      "pubdates": [
        "2018-08-17(\u65e5\u672c)",
        "2019-05-17(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555927320.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555927320.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555927320.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/30158971\/",
      "id": "30158971"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u5386\u53f2"
      ],
      "title": "\u5468\u6069\u6765\u56de\u5ef6\u5b89",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p28593.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p28593.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p28593.webp"
          },
          "name_en": "Jin Liu",
          "name": "\u5218\u52b2",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1314473\/",
          "id": "1314473"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1508739235.25.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1508739235.25.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1508739235.25.webp"
          },
          "name_en": "Guoqiang Tang",
          "name": "\u5510\u56fd\u5f3a",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1152771\/",
          "id": "1152771"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1553969670.25.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1553969670.25.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1553969670.25.webp"
          },
          "name_en": "Qi Lu",
          "name": "\u5362\u5947",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1314435\/",
          "id": "1314435"
        }
      ],
      "durations": [
        "103\u5206\u949f"
      ],
      "collect_count": 317,
      "mainland_pubdate": "2019-05-15",
      "has_video": false,
      "original_title": "\u5468\u6069\u6765\u56de\u5ef6\u5b89",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p36752.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p36752.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p36752.webp"
          },
          "name_en": "Weidong Wu",
          "name": "\u5434\u536b\u4e1c",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1316547\/",
          "id": "1316547"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p28593.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p28593.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p28593.webp"
          },
          "name_en": "Jin Liu",
          "name": "\u5218\u52b2",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1314473\/",
          "id": "1314473"
        }
      ],
      "pubdates": [
        "2019-05-15(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2553832372.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2553832372.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2553832372.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/30322432\/",
      "id": "30322432"
    },
    {
      "rating": {
        "max": 10,
        "average": 6.6,
        "details": {
          "1": 15.0,
          "3": 105.0,
          "2": 48.0,
          "5": 38.0,
          "4": 101.0
        },
        "stars": "35",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u5386\u53f2",
        "\u53e4\u88c5"
      ],
      "title": "\u8fdb\u4eac\u57ce",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1415523283.1.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1415523283.1.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1415523283.1.webp"
          },
          "name_en": "Yili Ma",
          "name": "\u9a6c\u4f0a\u740d",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1011935\/",
          "id": "1011935"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1468674227.5.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1468674227.5.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1468674227.5.webp"
          },
          "name_en": "Dalong Fu",
          "name": "\u5bcc\u5927\u9f99",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1274705\/",
          "id": "1274705"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1465876087.45.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1465876087.45.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1465876087.45.webp"
          },
          "name_en": "Jinghan Ma",
          "name": "\u9a6c\u656c\u6db5",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1320198\/",
          "id": "1320198"
        }
      ],
      "durations": [
        "113\u5206\u949f"
      ],
      "collect_count": 2381,
      "mainland_pubdate": "2019-05-10",
      "has_video": false,
      "original_title": "\u8fdb\u4eac\u57ce",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p32851.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p32851.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p32851.webp"
          },
          "name_en": "Mei Hu",
          "name": "\u80e1\u73ab",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1291776\/",
          "id": "1291776"
        }
      ],
      "pubdates": [
        "2018-06-17(\u4e0a\u6d77\u7535\u5f71\u8282)",
        "2019-05-10(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554883629.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554883629.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554883629.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26773625\/",
      "id": "26773625"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u7231\u60c5"
      ],
      "title": "\u771f\u7231\u4e0d\u8fdf\u5230",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1511511057.66.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1511511057.66.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1511511057.66.webp"
          },
          "name_en": "Hao-chi Chiu",
          "name": "\u90b1\u660a\u5947",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1355355\/",
          "id": "1355355"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1481690307.09.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1481690307.09.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1481690307.09.webp"
          },
          "name_en": "Jingge Cui",
          "name": "\u5d14\u83c1\u683c",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1365813\/",
          "id": "1365813"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p39324.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p39324.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p39324.webp"
          },
          "name_en": "Chih-Hsi Li",
          "name": "\u674e\u5fd7\u5e0c",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1314412\/",
          "id": "1314412"
        }
      ],
      "durations": [
        "101\u5206\u949f"
      ],
      "collect_count": 55,
      "mainland_pubdate": "2019-05-20",
      "has_video": false,
      "original_title": "\u771f\u7231\u4e0d\u8fdf\u5230",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1418717377.3.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1418717377.3.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1418717377.3.webp"
          },
          "name_en": "Yong Ma",
          "name": "\u9a6c\u96cd",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1339824\/",
          "id": "1339824"
        }
      ],
      "pubdates": [
        "2019-05-20(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2553093032.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2553093032.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2553093032.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/30467175\/",
      "id": "30467175"
    },
    {
      "rating": {
        "max": 10,
        "average": 6.7,
        "details": {
          "1": 25.0,
          "3": 476.0,
          "2": 98.0,
          "5": 88.0,
          "4": 354.0
        },
        "stars": "35",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u5bb6\u5ead"
      ],
      "title": "\u67d4\u60c5\u53f2",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p57744.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p57744.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p57744.webp"
          },
          "name_en": "An Nai",
          "name": "\u8010\u5b89",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1319560\/",
          "id": "1319560"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p9977.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p9977.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p9977.webp"
          },
          "name_en": "Xianmin Zhang",
          "name": "\u5f20\u732e\u6c11",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1275180\/",
          "id": "1275180"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1520910329.52.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1520910329.52.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1520910329.52.webp"
          },
          "name_en": "Mingming Yang",
          "name": "\u6768\u660e\u660e",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1324484\/",
          "id": "1324484"
        }
      ],
      "durations": [
        "117\u5206\u949f",
        "116\u5206\u949f(\u4e2d\u56fd\u5927\u9646)"
      ],
      "collect_count": 2858,
      "mainland_pubdate": "2019-05-17",
      "has_video": false,
      "original_title": "\u67d4\u60c5\u53f2",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1520910329.52.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1520910329.52.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1520910329.52.webp"
          },
          "name_en": "Mingming Yang",
          "name": "\u6768\u660e\u660e",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1324484\/",
          "id": "1324484"
        }
      ],
      "pubdates": [
        "2018-02-16(\u67cf\u6797\u7535\u5f71\u8282)",
        "2019-05-17(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554962852.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554962852.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554962852.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26959074\/",
      "id": "26959074"
    },
    {
      "rating": {
        "max": 10,
        "average": 6.6,
        "details": {
          "1": 6.0,
          "3": 53.0,
          "2": 17.0,
          "5": 19.0,
          "4": 29.0
        },
        "stars": "35",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u5386\u53f2"
      ],
      "title": "\u97f3\u4e50\u5bb6",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p20960.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p20960.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p20960.webp"
          },
          "name_en": "Jun Hu",
          "name": "\u80e1\u519b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1032540\/",
          "id": "1032540"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p20770.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p20770.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p20770.webp"
          },
          "name_en": "Quan Yuan",
          "name": "\u8881\u6cc9",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1010504\/",
          "id": "1010504"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1553794047.78.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1553794047.78.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1553794047.78.webp"
          },
          "name_en": "Berik Aitzhanov",
          "name": "\u522b\u91cc\u514b\u00b7\u827e\u7279\u5360\u8bfa\u592b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1362429\/",
          "id": "1362429"
        }
      ],
      "durations": [
        "104\u5206\u949f"
      ],
      "collect_count": 886,
      "mainland_pubdate": "2019-05-17",
      "has_video": false,
      "original_title": "\u97f3\u4e50\u5bb6",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1555471425.99.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1555471425.99.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1555471425.99.webp"
          },
          "name_en": "Xirzat Yahup",
          "name": "\u897f\u5c14\u624e\u63d0\u00b7\u4e9a\u5408\u752b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1319661\/",
          "id": "1319661"
        }
      ],
      "pubdates": [
        "2019-04-13(\u5317\u4eac\u56fd\u9645\u7535\u5f71\u8282)",
        "2019-05-17(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2556763879.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2556763879.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2556763879.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/27040921\/",
      "id": "27040921"
    },
    {
      "rating": {
        "max": 10,
        "average": 8.2,
        "details": {
          "1": 78.0,
          "3": 4263.0,
          "2": 436.0,
          "5": 7224.0,
          "4": 11690.0
        },
        "stars": "40",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u5bb6\u5ead"
      ],
      "title": "\u7f57\u9a6c",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1545114839.7.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1545114839.7.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1545114839.7.webp"
          },
          "name_en": "Yalitza Aparicio",
          "name": "\u96c5\u5229\u624e\u00b7\u963f\u5df4\u91cc\u897f\u5965",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1400464\/",
          "id": "1400464"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1545114534.81.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1545114534.81.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1545114534.81.webp"
          },
          "name_en": "Marina de Tavira",
          "name": "\u739b\u4e3d\u5a1c\u00b7\u5fb7\u00b7\u5854\u7ef4\u62c9",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1282087\/",
          "id": "1282087"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1548222736.85.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1548222736.85.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1548222736.85.webp"
          },
          "name_en": "Diego Cortina Autrey",
          "name": "\u8fed\u6208\u00b7\u79d1\u8482\u5a1c\u00b7\u5965\u7279\u91cc",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1406782\/",
          "id": "1406782"
        }
      ],
      "durations": [
        "135\u5206\u949f"
      ],
      "collect_count": 83851,
      "mainland_pubdate": "2019-05-10",
      "has_video": false,
      "original_title": "Roma",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1362388356.65.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1362388356.65.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1362388356.65.webp"
          },
          "name_en": "Alfonso Cuar\u00f3n",
          "name": "\u963f\u65b9\u7d22\u00b7\u5361\u9686",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1036409\/",
          "id": "1036409"
        }
      ],
      "pubdates": [
        "2018-08-30(\u5a01\u5c3c\u65af\u7535\u5f71\u8282)",
        "2018-12-14(\u7f8e\u56fd)",
        "2019-05-10(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555541430.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555541430.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555541430.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/1950330\/",
      "id": "1950330"
    },
    {
      "rating": {
        "max": 10,
        "average": 7.4,
        "details": {
          "1": 3.0,
          "3": 153.0,
          "2": 20.0,
          "5": 75.0,
          "4": 184.0
        },
        "stars": "40",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u513f\u7ae5",
        "\u5bb6\u5ead"
      ],
      "title": "\u8fc7\u662d\u5173",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1544268428.84.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1544268428.84.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1544268428.84.webp"
          },
          "name_en": "Taiyi Yang",
          "name": "\u6768\u592a\u4e49",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1402788\/",
          "id": "1402788"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554289629.34.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554289629.34.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554289629.34.webp"
          },
          "name_en": "",
          "name": "\u674e\u4e91\u864e ",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1413977\/",
          "id": "1413977"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554289645.47.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554289645.47.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554289645.47.webp"
          },
          "name_en": "",
          "name": "\u4e07\u4f17 ",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1413978\/",
          "id": "1413978"
        }
      ],
      "durations": [
        "93\u5206\u949f"
      ],
      "collect_count": 1580,
      "mainland_pubdate": "2019-05-20",
      "has_video": false,
      "original_title": "\u8fc7\u662d\u5173",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pzCXRp14nonwcel_avatar_uploaded1439191632.0.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pzCXRp14nonwcel_avatar_uploaded1439191632.0.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pzCXRp14nonwcel_avatar_uploaded1439191632.0.webp"
          },
          "name_en": "Meng Huo",
          "name": "\u970d\u731b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1351129\/",
          "id": "1351129"
        }
      ],
      "pubdates": [
        "2018-10-17(\u5e73\u9065\u56fd\u9645\u7535\u5f71\u5c55)",
        "2019-05-20(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2556430298.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2556430298.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2556430298.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/30206431\/",
      "id": "30206431"
    },
    {
      "rating": {
        "max": 10,
        "average": 8.3,
        "details": {
          "1": 196.0,
          "3": 10049.0,
          "2": 1102.0,
          "5": 22667.0,
          "4": 31730.0
        },
        "stars": "45",
        "min": 0
      },
      "genres": [
        "\u559c\u5267",
        "\u72af\u7f6a",
        "\u60ac\u7591"
      ],
      "title": "\u8c03\u97f3\u5e08",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1549016715.18.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1549016715.18.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1549016715.18.webp"
          },
          "name_en": "Ayushmann Khurrana",
          "name": "\u963f\u5c24\u65af\u66fc\u00b7\u5e93\u62c9\u7eb3",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1327903\/",
          "id": "1327903"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1385263384.03.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1385263384.03.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1385263384.03.webp"
          },
          "name_en": "Tabu",
          "name": "\u5854\u5e03",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1040796\/",
          "id": "1040796"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554656744.53.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554656744.53.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554656744.53.webp"
          },
          "name_en": "Radhika Apte",
          "name": "\u62c9\u8fea\u5361\u00b7\u827e\u666e\u7279",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1329473\/",
          "id": "1329473"
        }
      ],
      "durations": [
        "139\u5206\u949f"
      ],
      "collect_count": 377315,
      "mainland_pubdate": "2019-04-03",
      "has_video": true,
      "original_title": "Andhadhun",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1545725463.81.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1545725463.81.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1545725463.81.webp"
          },
          "name_en": "Sriram Raghavan",
          "name": "\u65af\u91cc\u5170\u59c6\u00b7\u62c9\u683c\u4e07",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1306786\/",
          "id": "1306786"
        }
      ],
      "pubdates": [
        "2018-10-05(\u5370\u5ea6)",
        "2019-04-03(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2551995207.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2551995207.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2551995207.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/30334073\/",
      "id": "30334073"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u72af\u7f6a",
        "\u60ac\u7591",
        "\u60ca\u609a"
      ],
      "title": "\u81f4\u547d\u68a6\u9b47",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1516090416.96.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1516090416.96.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1516090416.96.webp"
          },
          "name_en": "Junsheng Sun",
          "name": "\u5b59\u96bd\u5723",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1369604\/",
          "id": "1369604"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552293930.73.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552293930.73.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552293930.73.webp"
          },
          "name_en": "Yaxuan Fang",
          "name": "\u65b9\u96c5\u8431",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1384560\/",
          "id": "1384560"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/patAueMzzTk0cel_avatar_uploaded1550455327.2.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/patAueMzzTk0cel_avatar_uploaded1550455327.2.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/patAueMzzTk0cel_avatar_uploaded1550455327.2.webp"
          },
          "name_en": "Yue Ren",
          "name": "\u4efb\u60a6",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1410997\/",
          "id": "1410997"
        }
      ],
      "durations": [
        "86\u5206\u949f"
      ],
      "collect_count": 68,
      "mainland_pubdate": "2019-05-17",
      "has_video": false,
      "original_title": "\u81f4\u547d\u68a6\u9b47",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1467696252.13.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1467696252.13.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1467696252.13.webp"
          },
          "name_en": "Jackie Lee",
          "name": "\u674e\u9526\u4f26",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1359696\/",
          "id": "1359696"
        }
      ],
      "pubdates": [
        "2019-05-17(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554290032.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554290032.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554290032.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/27663956\/",
      "id": "27663956"
    },
    {
      "rating": {
        "max": 10,
        "average": 5.4,
        "details": {
          "1": 5.0,
          "3": 14.0,
          "2": 9.0,
          "5": 1.0,
          "4": 6.0
        },
        "stars": "30",
        "min": 0
      },
      "genres": [
        "\u559c\u5267",
        "\u52a8\u4f5c",
        "\u5947\u5e7b"
      ],
      "title": "\u6700\u540e\u7684\u52c7\u58eb",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1550063952.88.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1550063952.88.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1550063952.88.webp"
          },
          "name_en": "Viktor Horinyak",
          "name": "\u7ef4\u514b\u591a\u00b7\u970d\u6797\u96c5\u514b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1349172\/",
          "id": "1349172"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pzW1CLetw4b4cel_avatar_uploaded1515925597.06.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pzW1CLetw4b4cel_avatar_uploaded1515925597.06.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pzW1CLetw4b4cel_avatar_uploaded1515925597.06.webp"
          },
          "name_en": "Mila Sivatskaya",
          "name": "\u7c73\u62c9\u00b7\u65af\u74e6\u5947\u5361\u5a05",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1387107\/",
          "id": "1387107"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1437568956.02.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1437568956.02.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1437568956.02.webp"
          },
          "name_en": "Ekaterina Vilkova",
          "name": "\u53f6\u5361\u5a55\u7433\u5a1c\u00b7\u7ef4\u5c14\u79d1\u5a03",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1009787\/",
          "id": "1009787"
        }
      ],
      "durations": [
        "114\u5206\u949f",
        "108\u5206\u949f(\u4e2d\u56fd\u5927\u9646)"
      ],
      "collect_count": 252,
      "mainland_pubdate": "2019-05-17",
      "has_video": false,
      "original_title": "\u041f\u043e\u0441\u043b\u0435\u0434\u043d\u0438\u0439 \u0431\u043e\u0433\u0430\u0442\u044b\u0440\u044c",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1533652963.13.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1533652963.13.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1533652963.13.webp"
          },
          "name_en": "Dmitriy Dyachenko",
          "name": "\u8fea\u7c73\u7279\u91cc\u00b7\u8fea\u4e9a\u7434\u79d1",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1349432\/",
          "id": "1349432"
        }
      ],
      "pubdates": [
        "2017-10-19(\u4fc4\u7f57\u65af)",
        "2019-05-17(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2017",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555547311.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555547311.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555547311.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/27591668\/",
      "id": "27591668"
    },
    {
      "rating": {
        "max": 10,
        "average": 8.7,
        "details": {
          "1": 132.0,
          "3": 7544.0,
          "2": 802.0,
          "5": 34862.0,
          "4": 26114.0
        },
        "stars": "45",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u4f20\u8bb0",
        "\u97f3\u4e50"
      ],
      "title": "\u6ce2\u897f\u7c73\u4e9a\u72c2\u60f3\u66f2",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1860.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1860.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1860.webp"
          },
          "name_en": "Rami Malek",
          "name": "\u62c9\u7c73\u00b7\u9a6c\u96f7\u514b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1044903\/",
          "id": "1044903"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1548177691.13.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1548177691.13.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1548177691.13.webp"
          },
          "name_en": "Ben Hardy",
          "name": "\u672c\u00b7\u54c8\u8fea",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1344763\/",
          "id": "1344763"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p13772.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p13772.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p13772.webp"
          },
          "name_en": "Joseph Mazzello",
          "name": "\u7ea6\u745f\u592b\u00b7\u6885\u6cfd\u7f57",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1049517\/",
          "id": "1049517"
        }
      ],
      "durations": [
        "135\u5206\u949f",
        "131\u5206\u949f(\u4e2d\u56fd\u5927\u9646)"
      ],
      "collect_count": 294680,
      "mainland_pubdate": "2019-03-22",
      "has_video": false,
      "original_title": "Bohemian Rhapsody",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1403264395.93.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1403264395.93.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1403264395.93.webp"
          },
          "name_en": "Bryan Singer",
          "name": "\u5e03\u83b1\u6069\u00b7\u8f9b\u683c",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1049586\/",
          "id": "1049586"
        }
      ],
      "pubdates": [
        "2018-11-02(\u7f8e\u56fd)",
        "2019-03-22(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2549558913.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2549558913.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2549558913.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/5300054\/",
      "id": "5300054"
    },
    {
      "rating": {
        "max": 10,
        "average": 7.1,
        "details": {
          "1": 1.0,
          "3": 165.0,
          "2": 25.0,
          "5": 45.0,
          "4": 156.0
        },
        "stars": "35",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u559c\u5267"
      ],
      "title": "\u6b22\u8fce\u6765\u5317\u65b9II",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p3315.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p3315.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p3315.webp"
          },
          "name_en": "Dany Boon",
          "name": "\u4e39\u5c3c\u00b7\u4f2f\u6069",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1009442\/",
          "id": "1009442"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p42718.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p42718.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p42718.webp"
          },
          "name_en": "Line Renaud",
          "name": "\u4e3d\u5a1c\u00b7\u96f7\u8bfa",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1012686\/",
          "id": "1012686"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1501728090.76.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1501728090.76.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1501728090.76.webp"
          },
          "name_en": "Laurence Arn\u00e9",
          "name": "\u6d1b\u6717\u65af\u00b7\u963f\u5c14\u5185",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1341875\/",
          "id": "1341875"
        }
      ],
      "durations": [
        "107\u5206\u949f"
      ],
      "collect_count": 1596,
      "mainland_pubdate": "2019-05-10",
      "has_video": false,
      "original_title": "La ch'tite famille",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p3315.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p3315.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p3315.webp"
          },
          "name_en": "Dany Boon",
          "name": "\u4e39\u5c3c\u00b7\u4f2f\u6069",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1009442\/",
          "id": "1009442"
        }
      ],
      "pubdates": [
        "2018-02-28(\u6cd5\u56fd)",
        "2019-05-10(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555472985.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555472985.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555472985.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/27125208\/",
      "id": "27125208"
    },
    {
      "rating": {
        "max": 10,
        "average": 2.6,
        "details": {
          "1": 368.0,
          "3": 25.0,
          "2": 64.0,
          "5": 4.0,
          "4": 4.0
        },
        "stars": "15",
        "min": 0
      },
      "genres": [
        "\u7231\u60c5"
      ],
      "title": "\u4e0b\u4e00\u4efb\uff1a\u524d\u4efb",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p44400.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p44400.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p44400.webp"
          },
          "name_en": "Amber Kuo",
          "name": "\u90ed\u91c7\u6d01",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1274814\/",
          "id": "1274814"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1366015827.84.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1366015827.84.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1366015827.84.webp"
          },
          "name_en": "Kai Zheng",
          "name": "\u90d1\u607a",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1275564\/",
          "id": "1275564"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p46584.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p46584.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p46584.webp"
          },
          "name_en": "Ethan Li",
          "name": "\u674e\u4e1c\u5b66",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1317113\/",
          "id": "1317113"
        }
      ],
      "durations": [
        "90\u5206\u949f"
      ],
      "collect_count": 12651,
      "mainland_pubdate": "2019-05-01",
      "has_video": false,
      "original_title": "\u4e0b\u4e00\u4efb\uff1a\u524d\u4efb",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pWXdR4F5vqQcel_avatar_uploaded1425209153.95.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pWXdR4F5vqQcel_avatar_uploaded1425209153.95.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/pWXdR4F5vqQcel_avatar_uploaded1425209153.95.webp"
          },
          "name_en": "Hung-I Chen",
          "name": "\u9648\u9e3f\u4eea",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1348355\/",
          "id": "1348355"
        }
      ],
      "pubdates": [
        "2019-05-01(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554795775.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554795775.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554795775.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26311974\/",
      "id": "26311974"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u7eaa\u5f55\u7247"
      ],
      "title": "\u6e2f\u73e0\u6fb3\u5927\u6865",
      "casts": [],
      "durations": [
        "70\u5206\u949f"
      ],
      "collect_count": 747,
      "mainland_pubdate": "2019-05-01",
      "has_video": false,
      "original_title": "\u6e2f\u73e0\u6fb3\u5927\u6865",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552879372.01.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552879372.01.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552879372.01.webp"
          },
          "name_en": "Dong Yan",
          "name": "\u95eb\u4e1c",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1342371\/",
          "id": "1342371"
        }
      ],
      "pubdates": [
        "2019-05-01(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554017175.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554017175.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554017175.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/30230160\/",
      "id": "30230160"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u7231\u60c5"
      ],
      "title": "\u4f60\u597d\u73b0\u4efb",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1378307305.72.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1378307305.72.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1378307305.72.webp"
          },
          "name_en": "Yvonne Yung Hung",
          "name": "\u7fc1\u8679",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1162012\/",
          "id": "1162012"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p36795.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p36795.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p36795.webp"
          },
          "name_en": "Lei Feng",
          "name": "\u51af\u96f7",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1316572\/",
          "id": "1316572"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p57842.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p57842.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p57842.webp"
          },
          "name_en": "Xin Wen",
          "name": "\u6e29\u5fc3",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1324029\/",
          "id": "1324029"
        }
      ],
      "durations": [
        "90\u5206\u949f"
      ],
      "collect_count": 65,
      "mainland_pubdate": "2019-05-18",
      "has_video": false,
      "original_title": "\u4f60\u597d\u73b0\u4efb",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556609269.65.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556609269.65.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556609269.65.webp"
          },
          "name_en": "Changxin Li",
          "name": "\u674e\u957f\u6b23",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1339432\/",
          "id": "1339432"
        }
      ],
      "pubdates": [
        "2019-05-18(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555538686.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555538686.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555538686.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/33427690\/",
      "id": "33427690"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u60ac\u7591",
        "\u60ca\u609a",
        "\u6050\u6016"
      ],
      "title": "\u5348\u591c\u602a\u8c08",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1476002036.86.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1476002036.86.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1476002036.86.webp"
          },
          "name_en": "Chi Zhang",
          "name": "\u5f20\u9a70",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1363029\/",
          "id": "1363029"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1440144091.11.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1440144091.11.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1440144091.11.webp"
          },
          "name_en": "Rayna Wang",
          "name": "\u738b\u5b50\u6e05",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1334974\/",
          "id": "1334974"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1501975064.1.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1501975064.1.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1501975064.1.webp"
          },
          "name_en": "Guorong Liang",
          "name": "\u6881\u56fd\u8363",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1348342\/",
          "id": "1348342"
        }
      ],
      "durations": [
        "86\u5206\u949f"
      ],
      "collect_count": 130,
      "mainland_pubdate": "2019-05-07",
      "has_video": false,
      "original_title": "\u5348\u591c\u602a\u8c08",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1504500136.53.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1504500136.53.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1504500136.53.webp"
          },
          "name_en": "Lizhou Wei ",
          "name": "\u536b\u7acb\u6d32",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1380488\/",
          "id": "1380488"
        }
      ],
      "pubdates": [
        "2019-05-07(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554981472.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554981472.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554981472.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/33434703\/",
      "id": "33434703"
    },
    {
      "rating": {
        "max": 10,
        "average": 8.1,
        "details": {
          "1": 8.0,
          "3": 1208.0,
          "2": 91.0,
          "5": 1697.0,
          "4": 3064.0
        },
        "stars": "40",
        "min": 0
      },
      "genres": [
        "\u559c\u5267",
        "\u6218\u4e89",
        "\u72af\u7f6a"
      ],
      "title": "\u5929\u4e0a\u518d\u89c1",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1496955185.94.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1496955185.94.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1496955185.94.webp"
          },
          "name_en": "Nahuel P\u00e9rez Biscayart",
          "name": "\u7eb3\u5a01\u5c14\u00b7\u4f69\u96f7\u5179\u00b7\u6bd5\u65af\u5361\u4e9a\u7279",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1033134\/",
          "id": "1033134"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p18742.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p18742.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p18742.webp"
          },
          "name_en": "Albert Dupontel",
          "name": "\u963f\u5c14\u8d1d\u00b7\u675c\u90a6\u6cf0\u5c14",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1014278\/",
          "id": "1014278"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1498407715.06.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1498407715.06.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1498407715.06.webp"
          },
          "name_en": "Laurent Lafitte",
          "name": "\u7f57\u5170\u00b7\u62c9\u6590\u7279",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1106952\/",
          "id": "1106952"
        }
      ],
      "durations": [
        "117\u5206\u949f"
      ],
      "collect_count": 23327,
      "mainland_pubdate": "2019-04-30",
      "has_video": false,
      "original_title": "Au revoir l\u00e0-haut",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p18742.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p18742.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p18742.webp"
          },
          "name_en": "Albert Dupontel",
          "name": "\u963f\u5c14\u8d1d\u00b7\u675c\u90a6\u6cf0\u5c14",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1014278\/",
          "id": "1014278"
        }
      ],
      "pubdates": [
        "2017-10-25(\u6cd5\u56fd)",
        "2019-04-30(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2017",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554018955.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554018955.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554018955.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26731376\/",
      "id": "26731376"
    },
    {
      "rating": {
        "max": 10,
        "average": 6.3,
        "details": {
          "1": 216.0,
          "3": 4413.0,
          "2": 1219.0,
          "5": 494.0,
          "4": 2075.0
        },
        "stars": "35",
        "min": 0
      },
      "genres": [
        "\u52a8\u4f5c",
        "\u72af\u7f6a"
      ],
      "title": "\u53cd\u8d2a\u98ce\u66b44",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1421047436.79.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1421047436.79.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1421047436.79.webp"
          },
          "name_en": "Louis Koo",
          "name": "\u53e4\u5929\u4e50",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1027577\/",
          "id": "1027577"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p40550.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p40550.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p40550.webp"
          },
          "name_en": "Kevin Cheng",
          "name": "\u90d1\u5609\u9896",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1274666\/",
          "id": "1274666"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1517921928.93.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1517921928.93.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1517921928.93.webp"
          },
          "name_en": "Raymond Lam",
          "name": "\u6797\u5cef",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1037562\/",
          "id": "1037562"
        }
      ],
      "durations": [
        "100\u5206\u949f"
      ],
      "collect_count": 56716,
      "mainland_pubdate": "2019-04-04",
      "has_video": false,
      "original_title": "P\u98a8\u66b4",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1357529568.73.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1357529568.73.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1357529568.73.webp"
          },
          "name_en": "David Lam",
          "name": "\u6797\u5fb7\u7984 ",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1326068\/",
          "id": "1326068"
        }
      ],
      "pubdates": [
        "2019-04-04(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2551353482.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2551353482.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2551353482.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/27202819\/",
      "id": "27202819"
    },
    {
      "rating": {
        "max": 10,
        "average": 6.3,
        "details": {
          "1": 49.0,
          "3": 1186.0,
          "2": 377.0,
          "5": 128.0,
          "4": 595.0
        },
        "stars": "35",
        "min": 0
      },
      "genres": [
        "\u52a8\u4f5c",
        "\u72af\u7f6a",
        "\u60ac\u7591"
      ],
      "title": "\u96ea\u66b4",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1477225566.41.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1477225566.41.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1477225566.41.webp"
          },
          "name_en": "Chen Chang",
          "name": "\u5f20\u9707",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1077991\/",
          "id": "1077991"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1368598869.24.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1368598869.24.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1368598869.24.webp"
          },
          "name_en": "Ni Ni",
          "name": "\u502a\u59ae",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1315861\/",
          "id": "1315861"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1454644679.84.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1454644679.84.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1454644679.84.webp"
          },
          "name_en": "Fan Liao",
          "name": "\u5ed6\u51e1",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1007401\/",
          "id": "1007401"
        }
      ],
      "durations": [
        "111\u5206\u949f(\u91dc\u5c71\u7535\u5f71\u8282)",
        "112\u5206\u949f(\u4e2d\u56fd\u5927\u9646)"
      ],
      "collect_count": 12638,
      "mainland_pubdate": "2019-04-30",
      "has_video": false,
      "original_title": "\u96ea\u66b4",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1366860600.5.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1366860600.5.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1366860600.5.webp"
          },
          "name_en": "Siwei Cui",
          "name": "\u5d14\u65af\u97e6",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1328313\/",
          "id": "1328313"
        }
      ],
      "pubdates": [
        "2018-10-05(\u91dc\u5c71\u7535\u5f71\u8282)",
        "2019-04-30(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554545271.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554545271.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554545271.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26899146\/",
      "id": "26899146"
    },
    {
      "rating": {
        "max": 10,
        "average": 6.7,
        "details": {
          "1": 369.0,
          "3": 7350.0,
          "2": 1752.0,
          "5": 1566.0,
          "4": 5112.0
        },
        "stars": "35",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5"
      ],
      "title": "\u8001\u5e08\u00b7\u597d",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1504169127.76.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1504169127.76.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1504169127.76.webp"
          },
          "name_en": "Qian Yu",
          "name": "\u4e8e\u8c26",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1274307\/",
          "id": "1274307"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1442330240.0.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1442330240.0.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1442330240.0.webp"
          },
          "name_en": "Melody Tang",
          "name": "\u6c64\u68a6\u4f73",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1351587\/",
          "id": "1351587"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1542251128.4.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1542251128.4.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1542251128.4.webp"
          },
          "name_en": "Guangyuan Wang",
          "name": "\u738b\u5e7f\u6e90",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1403828\/",
          "id": "1403828"
        }
      ],
      "durations": [
        "111\u5206\u949f"
      ],
      "collect_count": 113891,
      "mainland_pubdate": "2019-03-22",
      "has_video": true,
      "original_title": "\u8001\u5e08\u00b7\u597d",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1550730545.46.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1550730545.46.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1550730545.46.webp"
          },
          "name_en": "Luan Zhang",
          "name": "\u5f20\u683e",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1383594\/",
          "id": "1383594"
        }
      ],
      "pubdates": [
        "2019-03-22(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2551352209.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2551352209.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2551352209.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/27663742\/",
      "id": "27663742"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u559c\u5267"
      ],
      "title": "\u641e\u602a\u5947\u5999\u591c",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1398052268.11.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1398052268.11.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1398052268.11.webp"
          },
          "name_en": "Ronald Cheng",
          "name": "\u90d1\u4e2d\u57fa",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1032940\/",
          "id": "1032940"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1500460509.41.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1500460509.41.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1500460509.41.webp"
          },
          "name_en": "Bo Cheng",
          "name": "\u7a0b\u6ce2",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1377549\/",
          "id": "1377549"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p45432.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p45432.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p45432.webp"
          },
          "name_en": "Van Fan",
          "name": "\u8303\u9038\u81e3",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1274683\/",
          "id": "1274683"
        }
      ],
      "durations": [
        "96\u5206\u949f"
      ],
      "collect_count": 73,
      "mainland_pubdate": "2019-05-17",
      "has_video": true,
      "original_title": "\u641e\u602a\u5947\u5999\u591c",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1379392451.45.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1379392451.45.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1379392451.45.webp"
          },
          "name_en": "Jinke Lei",
          "name": "\u96f7\u91d1\u514b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1334553\/",
          "id": "1334553"
        }
      ],
      "pubdates": [
        "2019-05-17(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2556693170.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2556693170.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2556693170.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/27091548\/",
      "id": "27091548"
    },
    {
      "rating": {
        "max": 10,
        "average": 6.1,
        "details": {
          "1": 32.0,
          "3": 660.0,
          "2": 217.0,
          "5": 57.0,
          "4": 216.0
        },
        "stars": "30",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u559c\u5267",
        "\u7231\u60c5"
      ],
      "title": "\u90bb\u5ea7\u7684\u602a\u540c\u5b66",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1402644225.57.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1402644225.57.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1402644225.57.webp"
          },
          "name_en": "Masaki Suda",
          "name": "\u83c5\u7530\u5c06\u6656",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1274418\/",
          "id": "1274418"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1406782114.56.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1406782114.56.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1406782114.56.webp"
          },
          "name_en": "Tao Tsuchiya",
          "name": "\u571f\u5c4b\u592a\u51e4",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1315335\/",
          "id": "1315335"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1368103986.27.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1368103986.27.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1368103986.27.webp"
          },
          "name_en": "Y\u00fbki Furukawa",
          "name": "\u53e4\u5ddd\u96c4\u8f89",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1315011\/",
          "id": "1315011"
        }
      ],
      "durations": [
        "105\u5206\u949f"
      ],
      "collect_count": 5682,
      "mainland_pubdate": "2019-05-23",
      "has_video": false,
      "original_title": "\u3068\u306a\u308a\u306e\u602a\u7269\u304f\u3093",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1500646774.95.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1500646774.95.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1500646774.95.webp"
          },
          "name_en": "Sh\u00f4 Tsukikawa",
          "name": "\u6708\u5ddd\u7fd4",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1372453\/",
          "id": "1372453"
        }
      ],
      "pubdates": [
        "2018-04-27(\u65e5\u672c)",
        "2019-05-23(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555637936.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555637936.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555637936.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/27042544\/",
      "id": "27042544"
    },
    {
      "rating": {
        "max": 10,
        "average": 8.0,
        "details": {
          "1": 220.0,
          "3": 4778.0,
          "2": 765.0,
          "5": 6488.0,
          "4": 10480.0
        },
        "stars": "40",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5",
        "\u5bb6\u5ead"
      ],
      "title": "\u5730\u4e45\u5929\u957f",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p12560.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p12560.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p12560.webp"
          },
          "name_en": "Jingchun Wang",
          "name": "\u738b\u666f\u6625",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1275934\/",
          "id": "1275934"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1469426474.73.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1469426474.73.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1469426474.73.webp"
          },
          "name_en": "Mei Yong",
          "name": "\u548f\u6885",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1276041\/",
          "id": "1276041"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p43726.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p43726.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p43726.webp"
          },
          "name_en": "Xi Qi",
          "name": "\u9f50\u6eaa",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1318634\/",
          "id": "1318634"
        }
      ],
      "durations": [
        "175\u5206\u949f"
      ],
      "collect_count": 92112,
      "mainland_pubdate": "2019-03-22",
      "has_video": false,
      "original_title": "\u5730\u4e45\u5929\u957f",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1553157534.76.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1553157534.76.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1553157534.76.webp"
          },
          "name_en": "Xiaoshuai Wang",
          "name": "\u738b\u5c0f\u5e05",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1018708\/",
          "id": "1018708"
        }
      ],
      "pubdates": [
        "2019-02-14(\u67cf\u6797\u7535\u5f71\u8282)",
        "2019-03-22(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2550208359.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2550208359.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2550208359.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26715636\/",
      "id": "26715636"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u5947\u5e7b",
        "\u5192\u9669"
      ],
      "title": "\u6349\u5996\u5b66\u9662",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556013554.59.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556013554.59.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556013554.59.webp"
          },
          "name_en": "Zezong Wang",
          "name": "\u738b\u6cfd\u5b97",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1415165\/",
          "id": "1415165"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556013588.39.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556013588.39.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556013588.39.webp"
          },
          "name_en": "Wantong Ci",
          "name": "\u6148\u5a49\u5f64",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1385236\/",
          "id": "1385236"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p54637.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p54637.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p54637.webp"
          },
          "name_en": "Kathy Chow",
          "name": "\u5468\u6d77\u5a9a",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1037908\/",
          "id": "1037908"
        }
      ],
      "durations": [
        "96\u5206\u949f"
      ],
      "collect_count": 24,
      "mainland_pubdate": "2019-04-30",
      "has_video": false,
      "original_title": "\u6349\u5996\u5b66\u9662",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556013428.84.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556013428.84.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556013428.84.webp"
          },
          "name_en": "Zicheng Tian",
          "name": "\u7530\u6893\u6a59",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1415164\/",
          "id": "1415164"
        }
      ],
      "pubdates": [
        "2019-04-30(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554386398.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554386398.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554386398.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26879542\/",
      "id": "26879542"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5"
      ],
      "title": "\u6b66\u9675\u5c71\u4e0a\u7684\u661f\u5149",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1505553419.06.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1505553419.06.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1505553419.06.webp"
          },
          "name_en": "Hanjun Fan",
          "name": "\u8303\u6d69\u519b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1364212\/",
          "id": "1364212"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552988944.18.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552988944.18.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552988944.18.webp"
          },
          "name_en": "",
          "name": "\u59dc\u7433",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1413250\/",
          "id": "1413250"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552988986.43.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552988986.43.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552988986.43.webp"
          },
          "name_en": "Shufeng Wang",
          "name": "\u738b\u6811\u98ce",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1344778\/",
          "id": "1344778"
        }
      ],
      "durations": [
        "106\u5206\u949f"
      ],
      "collect_count": 32,
      "mainland_pubdate": "2019-05-18",
      "has_video": false,
      "original_title": "\u6b66\u9675\u5c71\u4e0a\u7684\u661f\u5149",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552988881.75.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552988881.75.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552988881.75.webp"
          },
          "name_en": "",
          "name": "\u5f20\u585e\u541b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1413248\/",
          "id": "1413248"
        }
      ],
      "pubdates": [
        "2019-05-18(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2553669139.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2553669139.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2553669139.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/32568719\/",
      "id": "32568719"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [],
      "title": "\u9675\u6c34\u8c23",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1353165137.87.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1353165137.87.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1353165137.87.webp"
          },
          "name_en": "Yu Song",
          "name": "\u5b8b\u79b9",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1320287\/",
          "id": "1320287"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/f\/movie\/ca527386eb8c4e325611e22dfcb04cc116d6b423\/pics\/movie\/celebrity-default-small.png",
            "large": "https://img3.doubanio.com\/f\/movie\/63acc16ca6309ef191f0378faf793d1096a3e606\/pics\/movie\/celebrity-default-large.png",
            "medium": "https://img1.doubanio.com\/f\/movie\/8dd0c794499fe925ae2ae89ee30cd225750457b4\/pics\/movie\/celebrity-default-medium.png"
          },
          "name_en": "Yelena Drapeko",
          "name": "\u4f0a\u83b2\u5a1c\u00b7\u5fb7\u7f57\u4f69\u79d1",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1084453\/",
          "id": "1084453"
        }
      ],
      "durations": [],
      "collect_count": 5,
      "mainland_pubdate": "2019-05-19",
      "has_video": false,
      "original_title": "\u9675\u6c34\u8c23",
      "subtype": "movie",
      "directors": [],
      "pubdates": [
        "2019-05-19(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2557157878.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2557157878.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2557157878.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/33418721\/",
      "id": "33418721"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u52a8\u753b",
        "\u5192\u9669"
      ],
      "title": "\u7a7a\u5929\u6218\u961f\u4e4b\u661f\u517d\u5927\u6218",
      "casts": [],
      "durations": [
        "90\u5206\u949f"
      ],
      "collect_count": 24,
      "mainland_pubdate": "2019-05-18",
      "has_video": false,
      "original_title": "\u7a7a\u5929\u6218\u961f\u4e4b\u661f\u517d\u5927\u6218",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p46007.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p46007.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p46007.webp"
          },
          "name_en": "Jiye Tang ",
          "name": "\u6c64\u7ee7\u4e1a",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1319178\/",
          "id": "1319178"
        }
      ],
      "pubdates": [
        "2019-05-18(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552449109.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552449109.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552449109.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/30396239\/",
      "id": "30396239"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u559c\u5267"
      ],
      "title": "\u4e00\u8def\u75af\u766b",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1496901561.89.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1496901561.89.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1496901561.89.webp"
          },
          "name_en": "Duo Ba",
          "name": "\u5df4\u591a",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1315506\/",
          "id": "1315506"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p10227.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p10227.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p10227.webp"
          },
          "name_en": "Cun Xue",
          "name": "\u96ea\u6751",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1275210\/",
          "id": "1275210"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1356511983.19.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1356511983.19.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1356511983.19.webp"
          },
          "name_en": "Hui Liu",
          "name": "\u5218\u60e0",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1318058\/",
          "id": "1318058"
        }
      ],
      "durations": [
        "91\u5206\u949f"
      ],
      "collect_count": 20,
      "mainland_pubdate": "2019-05-10",
      "has_video": false,
      "original_title": "\u4e00\u8def\u75af\u766b",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556514190.94.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556514190.94.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1556514190.94.webp"
          },
          "name_en": "Shoujie Cui",
          "name": "\u5d14\u5b88\u6770",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1392548\/",
          "id": "1392548"
        }
      ],
      "pubdates": [
        "2019-05-10(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554875122.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554875122.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554875122.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/30379952\/",
      "id": "30379952"
    },
    {
      "rating": {
        "max": 10,
        "average": 7.4,
        "details": {
          "1": 2.0,
          "3": 472.0,
          "2": 46.0,
          "5": 164.0,
          "4": 570.0
        },
        "stars": "40",
        "min": 0
      },
      "genres": [
        "\u559c\u5267",
        "\u7231\u60c5"
      ],
      "title": "\u771f\u7231\u767e\u5206\u767e",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p15253.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p15253.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p15253.webp"
          },
          "name_en": "Franck Dubosc",
          "name": "\u5f17\u6717\u514b\u00b7\u8fea\u535a\u65af\u514b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1041585\/",
          "id": "1041585"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p40349.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p40349.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p40349.webp"
          },
          "name_en": "Alexandra Lamy",
          "name": "\u4e9a\u5386\u5c71\u5fb7\u62c9\u00b7\u62c9\u7c73",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1028139\/",
          "id": "1028139"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p11390.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p11390.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p11390.webp"
          },
          "name_en": "Elsa Zylberstein",
          "name": "\u827e\u5c14\u838e\u00b7\u6cfd\u8d1d\u65af\u5766",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1027994\/",
          "id": "1027994"
        }
      ],
      "durations": [
        "107\u5206\u949f"
      ],
      "collect_count": 5067,
      "mainland_pubdate": "2019-05-23",
      "has_video": false,
      "original_title": "Tout le monde debout",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p15253.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p15253.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p15253.webp"
          },
          "name_en": "Franck Dubosc",
          "name": "\u5f17\u6717\u514b\u00b7\u8fea\u535a\u65af\u514b",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1041585\/",
          "id": "1041585"
        }
      ],
      "pubdates": [
        "2018-03-14(\u6cd5\u56fd)",
        "2019-05-23(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555646002.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555646002.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2555646002.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/30170375\/",
      "id": "30170375"
    },
    {
      "rating": {
        "max": 10,
        "average": 7.5,
        "details": {
          "1": 25.0,
          "3": 747.0,
          "2": 130.0,
          "5": 515.0,
          "4": 1037.0
        },
        "stars": "40",
        "min": 0
      },
      "genres": [
        "\u52a8\u753b"
      ],
      "title": "\u9f99\u73e0\u8d85\uff1a\u5e03\u7f57\u5229",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p26240.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p26240.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p26240.webp"
          },
          "name_en": "Masako Nozawa",
          "name": "\u91ce\u6cfd\u96c5\u5b50",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1041488\/",
          "id": "1041488"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p9676.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p9676.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p9676.webp"
          },
          "name_en": "Ry\u00f4 Horikawa",
          "name": "\u5800\u5ddd\u4eae",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1008133\/",
          "id": "1008133"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p32892.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p32892.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p32892.webp"
          },
          "name_en": "Ry\u00fbsei Nakao",
          "name": "\u4e2d\u5c3e\u9686\u5723",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1008254\/",
          "id": "1008254"
        }
      ],
      "durations": [
        "100\u5206\u949f"
      ],
      "collect_count": 15573,
      "mainland_pubdate": "2019-05-24",
      "has_video": false,
      "original_title": "\u30c9\u30e9\u30b4\u30f3\u30dc\u30fc\u30eb\u8d85 \u30d6\u30ed\u30ea\u30fc",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1506337656.05.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1506337656.05.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1506337656.05.webp"
          },
          "name_en": "Tatsuya Nagamine",
          "name": "\u957f\u5cf0\u8fbe\u4e5f",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1324215\/",
          "id": "1324215"
        }
      ],
      "pubdates": [
        "2018-12-14(\u65e5\u672c)",
        "2019-05-24(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554795240.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554795240.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554795240.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/27607378\/",
      "id": "27607378"
    },
    {
      "rating": {
        "max": 10,
        "average": 6.4,
        "details": {
          "1": 7.0,
          "3": 118.0,
          "2": 32.0,
          "5": 22.0,
          "4": 50.0
        },
        "stars": "35",
        "min": 0
      },
      "genres": [
        "\u559c\u5267",
        "\u52a8\u753b",
        "\u5947\u5e7b"
      ],
      "title": "\u795e\u5947\u4e50\u56ed\u5386\u9669\u8bb0",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552877631.35.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552877631.35.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1552877631.35.webp"
          },
          "name_en": "Sofia Mali",
          "name": "\u7d22\u83f2\u4e9a\u00b7\u739b\u4e3d",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1412961\/",
          "id": "1412961"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p26079.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p26079.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p26079.webp"
          },
          "name_en": "Jennifer Garner",
          "name": "\u8a79\u59ae\u5f17\u00b7\u52a0\u7eb3",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1054512\/",
          "id": "1054512"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1548996503.57.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1548996503.57.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1548996503.57.webp"
          },
          "name_en": "Ken Hudson Campbell",
          "name": "\u80af\u00b7\u54c8\u5fb7\u68ee\u00b7\u574e\u8d1d\u5c14",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1068149\/",
          "id": "1068149"
        }
      ],
      "durations": [
        "86\u5206\u949f"
      ],
      "collect_count": 1848,
      "mainland_pubdate": "2019-04-19",
      "has_video": false,
      "original_title": "Wonder Park",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/f\/movie\/ca527386eb8c4e325611e22dfcb04cc116d6b423\/pics\/movie\/celebrity-default-small.png",
            "large": "https://img3.doubanio.com\/f\/movie\/63acc16ca6309ef191f0378faf793d1096a3e606\/pics\/movie\/celebrity-default-large.png",
            "medium": "https://img1.doubanio.com\/f\/movie\/8dd0c794499fe925ae2ae89ee30cd225750457b4\/pics\/movie\/celebrity-default-medium.png"
          },
          "name_en": "Dylan Brown",
          "name": "\u8fea\u5170\u00b7\u5e03\u6717",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1414949\/",
          "id": "1414949"
        }
      ],
      "pubdates": [
        "2019-03-15(\u7f8e\u56fd)",
        "2019-04-19(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552076937.webp",
        "large": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552076937.webp",
        "medium": "https://img1.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552076937.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26662282\/",
      "id": "26662282"
    },
    {
      "rating": {
        "max": 10,
        "average": 0,
        "details": {
          "1": 0,
          "3": 0,
          "2": 0,
          "5": 0,
          "4": 0
        },
        "stars": "00",
        "min": 0
      },
      "genres": [
        "\u559c\u5267",
        "\u52a8\u753b"
      ],
      "title": "\u609f\u7a7a\u5947\u9047\u8bb0",
      "casts": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551089156.81.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551089156.81.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551089156.81.webp"
          },
          "name_en": "Zhen Zhang",
          "name": "\u5f20\u9707",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1378129\/",
          "id": "1378129"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551089061.43.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551089061.43.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551089061.43.webp"
          },
          "name_en": "Shanshan Li",
          "name": "\u674e\u59d7\u59d7",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1360420\/",
          "id": "1360420"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551082313.51.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551082313.51.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551082313.51.webp"
          },
          "name_en": "",
          "name": "\u5143\u6c14\u7ea3 ",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1411430\/",
          "id": "1411430"
        }
      ],
      "durations": [
        "88\u5206\u949f"
      ],
      "collect_count": 537,
      "mainland_pubdate": "2019-05-01",
      "has_video": false,
      "original_title": "\u609f\u7a7a\u5947\u9047\u8bb0",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551089032.16.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551089032.16.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1551089032.16.webp"
          },
          "name_en": "Yuqi Yin",
          "name": "\u6bb7\u7389\u9e92",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1330909\/",
          "id": "1330909"
        }
      ],
      "pubdates": [
        "2019-05-01(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2019",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552558351.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552558351.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2552558351.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/26878827\/",
      "id": "26878827"
    },
    {
      "rating": {
        "max": 10,
        "average": 7.4,
        "details": {
          "1": 30.0,
          "3": 1765.0,
          "2": 182.0,
          "5": 574.0,
          "4": 2787.0
        },
        "stars": "40",
        "min": 0
      },
      "genres": [
        "\u5267\u60c5"
      ],
      "title": "\u649e\u6b7b\u4e86\u4e00\u53ea\u7f8a",
      "casts": [
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1539580440.17.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1539580440.17.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1539580440.17.webp"
          },
          "name_en": "Jinpa",
          "name": "\u91d1\u5df4",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1362878\/",
          "id": "1362878"
        },
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1478411142.94.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1478411142.94.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1478411142.94.webp"
          },
          "name_en": "Genden Phuntsok",
          "name": "\u66f4\u767b\u5f6d\u63aa",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1363820\/",
          "id": "1363820"
        },
        {
          "avatars": {
            "small": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1535689588.89.webp",
            "large": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1535689588.89.webp",
            "medium": "https://img1.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1535689588.89.webp"
          },
          "name_en": "Sonam Wangmo",
          "name": "\u7d22\u6717\u65fa\u59c6",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1400147\/",
          "id": "1400147"
        }
      ],
      "durations": [
        "87\u5206\u949f"
      ],
      "collect_count": 19830,
      "mainland_pubdate": "2019-04-26",
      "has_video": false,
      "original_title": "\u649e\u6b7b\u4e86\u4e00\u53ea\u7f8a",
      "subtype": "movie",
      "directors": [
        {
          "avatars": {
            "small": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554295519.81.webp",
            "large": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554295519.81.webp",
            "medium": "https://img3.doubanio.com\/view\/celebrity\/s_ratio_celebrity\/public\/p1554295519.81.webp"
          },
          "name_en": "Pema Tseden",
          "name": "\u4e07\u739b\u624d\u65e6",
          "alt": "https:\/\/movie.douban.com\/celebrity\/1316181\/",
          "id": "1316181"
        }
      ],
      "pubdates": [
        "2018-09-04(\u5a01\u5c3c\u65af\u7535\u5f71\u8282)",
        "2019-04-26(\u4e2d\u56fd\u5927\u9646)"
      ],
      "year": "2018",
      "images": {
        "small": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554775210.webp",
        "large": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554775210.webp",
        "medium": "https://img3.doubanio.com\/view\/photo\/s_ratio_poster\/public\/p2554775210.webp"
      },
      "alt": "https:\/\/movie.douban.com\/subject\/30283179\/",
      "id": "30283179"
    }
  ],
  "title": "\u6b63\u5728\u4e0a\u6620\u7684\u7535\u5f71-\u5317\u4eac"
}
```


**数据看着复杂，仔细屡屡结构感觉没啥**
###### 数据的解析类，使用工长模式
```Java
class Movie {

  final int start;
  final int total;
  final int count;
  final String title;
  final List<Subjects> subjects;

  Movie({this.start, this.total, this.count, this.subjects, this.title});


  factory Movie.jsonToBean(Map<String, dynamic> json){
    // 因为是个数组 所以我们要将subjects先转出数组格式
    List subJectsList = json['subjects'] as List;
    //将里面的数据转出List<Subjects> 格式
    List<Subjects> list = subJectsList.map((i) => Subjects.jsonToBean(i))
        .toList();


    return Movie(
        total: json['total'],
        start: json['start'],
        count: json['count'],
        title: json['title'],
        subjects: list
    );
  }
}


class Subjects {

  final Rating rating;
  final List<String> genres;
  final String title;
  final List<Casts> casts;
  final List<String> durations;
  final int collect_count;
  final String mainland_pubdate;
  final bool has_video;
  final String original_title;
  final String subtype;
  final List<Directors> directors;
  final List<String> pubdates;
  final String year;
  final Avatars images;
  final String alt;
  final String id;

  Subjects(
      {this.rating, this.genres, this.title, this.casts, this.durations, this.collect_count, this.mainland_pubdate, this.has_video, this.original_title, this.subtype, this.directors, this.pubdates, this.year, this.images, this.alt, this.id});


  factory Subjects.jsonToBean(Map<String, dynamic> json){
    //转换成List<string>
    List genresStringList = json['genres'].cast<String>();


    List casts = json['casts'] as List;
    List<Casts> castsList = casts.map((i) => Casts.jsonToBean(i)).toList();


    json['durations'].cast<String>() as List<String>;


    return Subjects(
        title: json['title'],
        collect_count: json['collect_count'],
        mainland_pubdate: json['mainland_pubdate'],
        original_title: json['original_title'],
        subtype: json['subtype'],
        year: json['year'],
        alt: json['alt'],
        id: json['id'],
        rating: Rating.jsonToBean(json['rating']),
        genres: genresStringList,
        casts: castsList,
        durations: json['durations'].cast<String>() as List<String>,
        has_video: json['has_video'],
        directors: (json['directors'] as List).map((i) =>
            Directors.jsonToBean(i)).toList(),
        pubdates: json['pubdates'].cast<String>() as List,
        images: Avatars.jsonToBean(json['images'])

    );
  }


}


class Rating {
  final int max;
  final String average;
  final String stars;
  final int min;

  Rating({this.max, this.average, this.stars, this.min});


  factory Rating.jsonToBean(Map<String, dynamic> json){
    String avdr = json['average'].toString();


    return Rating(
      max: json['max'],
      average: avdr,
      stars: json['stars'],
      min: json['min'],
    );
  }


}

class Casts {
  final String id;
  final String alt;
  final String name;
  final String name_en;
  final Avatars avatars;

  Casts({this.id, this.alt, this.name, this.name_en, this.avatars});

  factory Casts.jsonToBean(Map<String, dynamic> json){
    return Casts(
        id: json['id'],
        alt: json['alt'],
        name: json['name'],
        name_en: json['name_en'],
        avatars: Avatars.jsonToBean(json['avatars'])
    );
  }

}

class Avatars {
  final String small;
  final String large;
  final String medium;

  Avatars({this.small, this.large, this.medium});


  factory Avatars.jsonToBean(Map<String, dynamic> json){
    return Avatars(
      small: json['small'],
      large: json['large'],
      medium: json['medium'],
    );
  }


}

class Directors {
  final Avatars avatars;
  final String name_en;
  final String name;
  final String alt;
  final String id;

  Directors({this.avatars, this.name_en, this.name, this.alt, this.id});


  factory Directors.jsonToBean(Map<String, dynamic> json){
    return Directors(
        name: json['name'],
        name_en: json['name_en'],
        alt: json['alt'],
        id: json['id'],
        avatars: Avatars.jsonToBean(json['avatars'])
    );
  }
}
```
###### 使用
```Java
 // content 为json 
 final jsonResponse = json.decode(content);
 Movie movie = Movie.jsonToBean(jsonResponse);
```


---
**这种解析方式是全部解析，比较麻烦。其实可以得到一些想要的信息，进行解析如下**
还是上面的json
```Java
class Movie2 {

  final String title;
  final String img;
  final String directors; //导演
  final String casts; //演员
  final String durations; //时长
  final String type;
  final String createCountry; //制作的国家
  final String average; //评分


  Movie2({this.title, this.img, this.directors, this.casts, this.durations,
    this.type, this.createCountry, this.average});


  // json --> bean
  static List<Movie2> jsonToBean(String jsonData) {
    List<Movie2> movieList = [];


    //解析json
    var jsonstr = json.decode(jsonData);
    //得到 List
    var subjectsList = jsonstr['subjects'];

    //遍历
    for (var sub in subjectsList) {
      var title = sub['title'];
      var img = sub['alt'];

      //导演
      var director = "";
      for (var dire in sub['directors']) {
        director += dire['name'] + "/";
      }

      //演员
      var casts = "";
      for (var cast in sub['casts']) {
        casts += cast['name'] + "/";
      }


      //时长
      var durations = "";
      for (var duration in sub['durations']) {
        durations += duration + "/";
      }


      //type
      var types = "";
      for (var type in sub['genres']) {
        types += type + "/";
      }

      //制作的国家
      var createCountrys = "";
      for (var createCountry in sub['pubdates']) {
        createCountrys += createCountry + '/';
      }


      var average = sub['rating']['average'].toString();


      Movie2 movie = Movie2(
        title: title,
        img: img,
        directors: director,
        casts: casts,
        durations: durations,
        type: types,
        createCountry: createCountrys,
        average: average,

      );
      movieList.add(movie);
    }


    return movieList;
  }
}
```
>用上面的方法进行解析，简单很多。

