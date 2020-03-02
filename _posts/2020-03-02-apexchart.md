---
layout: default
title: apexchart essential 
categories: [programming]
tags: [javascript, chart, apexchart]
---

>xaxis

1. x축 레이블 지정과 y축 레이블 지정

```js
    xaxis: {
        type: "category",
        categories: [
            "ROV",
            "RUV",
            "COV",
            "CUV",
            "CHG",
            "DHG",
            "SOV",
            "SUV",
            "TOV",
            "TUV",
            "COM",
            "REL",
            "FAN",
            "ISO",
            "FSE",
            "CEL"
        ],
        labels: {
            show: true,
            rotate: -90,
            style: {
                fontSize: "11px",
                fontFamily: "Verdana, Geneva, sans-serif"
            }
        }
    },
    yaxis: {
        labels: {
            style: {
                fontSize: "11px",
                fontFamily: "Verdana, Geneva, sans-serif"
            }
        }
    }
```
