##### 在作柱形图时，如果需要给每个柱子设置不同颜色，参考以下说明：

```
var option = {
    xAxis: {
        data: ["苹果","小米","华为","其他"]  
    },
    yAxis: {
        splitLine:{ show:false}  //改设置不显示坐标区域内的y轴分割线
    },
    series: [{
            name: '手机品牌',
            type: 'bar',
            data: [19, 15, 40, 32],
            //设置柱子的宽度
            barWidth : 30,
            //配置样式
            itemStyle: {   
                //通常情况下：
                normal:{  
　　　　　　　　　　　　//每个柱子的颜色即为colorList数组里的每一项，如果柱子数目多于colorList的长度，则柱子颜色循环使用该数组
                    color: function (params){
                        var colorList = ['rgb(164,205,238)','rgb(42,170,227)','rgb(25,46,94)','rgb(195,229,235)'];
                        return colorList[params.dataIndex];
                    }
                },
                //鼠标悬停时：
                emphasis: {
                        shadowBlur: 10,
                        shadowOffsetX: 0,
                        shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
            },
        }],
　　　　　//控制边距　
        grid: {
                left: '0%',
                right:'10%',
                containLabel: true,
        },
    };  
```
