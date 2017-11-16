在设置中设置去除
```

geo: {
        map: 'china',
        label: {
            emphasis: {
                show: false
            }
        },
        regions: [
            {
                name: '南海诸岛', 
                value: 0, 
                itemStyle: 
                    {normal: 
                        {opacity: 0,
                        label: {
                            show: false
                            
                        }
                    }
                }
            }],
        roam: true,
        itemStyle: {
            normal: {
                areaColor: '#323c48',
                borderColor: '#404a59'
            },
            emphasis: {
                areaColor: '#2a333d'
            }
        }
    },


````

或者在数据中把南海地图隐藏
```

{"name": "南海诸岛","value": 0, "itemStyle":{ "normal":{"opacity":0}} }

```
