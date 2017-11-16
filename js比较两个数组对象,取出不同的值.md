````
var array1 = [ {"Num": "A "  },{"Num": "B" }];
var array2 = [ {"Num": "A ","Name": "t1 " }, {"Num": "B","Name": "t2"}, {"Num": "C " ,"Name": "t3 "}];
var result = [];
for(var i = 0; i < array2.length; i++){
    var obj = array2[i];
    var num = obj.Num;
    var isExist = false;
    for(var j = 0; j < array1.length; j++){
        var aj = array1[j];
        var n = aj.Num;
        if(n == num){
            isExist = true;
            break;
        }
    }
    if(!isExist){
        result.push(obj);
    }
}
console.log(result);
````
```
                    $scope.datacaseNodeArr = [];
                    for(var i = 0; i < datacaseRes.length; i++){
                        var isExist = false;
                        for(var j = 0; j < flowResData.length; j++){
                            if(flowResData[j].id == datacaseRes[i].id){
                                isExist = true;
                                break;
                            }
                        }
                        if(!isExist){
                            $scope.datacaseNodeArr.push(datacaseRes[i]);
                        }
                    }
```
