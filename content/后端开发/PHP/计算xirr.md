```json
{
  "date": "2022.06.06 11:00",
  "tags": ["PHP","代码片段"],
  "description": "XIRR函数可以计算年化收益率，用PHP怎么实现呢？"
}
```

```php

//计算xirr方法
function getXirr($cashflows){
    if(empty($cashflows)) return 0;
    $years=[];
    $first_time=strtotime($cashflows[0]['stringTime']);

    foreach ($cashflows as $v){
        $years[]=(strtotime($v['stringTime'])-$first_time)/86400/365;
    }
    $residual = 1.0;
    $step = 0.05;
    $guess = 0.05;
    $epsilon = 0.0001;
    $limit = 10000;
    $count=0;
    while (abs($residual)>$epsilon&&$limit > 0){
        $limit--;
        $residual=0.0;
        foreach ($cashflows as $i=>$trans){
            ((float)pow($guess, $years[$i])==0)?:$residual += $trans['payment'] / pow($guess, $years[$i]);
        }
        if(abs($residual)>$epsilon){
            if($residual>0){
                $guess+=$step;
            }else{
                $guess-=$step;
                $step/=2.0;
            }
        }
        $count++;
        if($count>1000)break;
    }
    return formateNumber(100*($guess-1),2);

}
 


```

 

```php


//调用计算xirr
public function testXirr()
{
    $list=array(
        ['stringTime'=>'2019-01-01','payment'=>-100.00],
        ['stringTime'=>'2020-01-01','payment'=>120.00]
    );
    $xirr = getXirr($list);
    dump($xirr);

}

```
 