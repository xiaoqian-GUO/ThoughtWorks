### 训练题目
---

#### 环境

使用 Visual Studio Code工具  
经测试，测试用例和额外用例都正常输出
---
#### 代码以及单元测试的方法

代码放在render.js 里面  
1. 内部有一个render函数，该函数有两个参数，分别对应给定的第一行输入和第二行输入；
2. 使用方法：
```
 // 在render.js里面加上下面语句，执行即可
 var firstLine="3 3";
 var secondLine="0,1 0,2;0,0 1,0;0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1";
 var rsu=render(firstLine,secondLine);
 console.log(rsu);
```
3. render方法：
```
// 定义一个函数render，输入两行的字符串作为参数
function render(firstLine,secondLine){
    var result="";    //存储最终返回的字符串
    var resultArr=[]; //存储最终结果的数组
    var str=firstLine.trim();
    var regFormat=/^[\d]+[\s]+[\d]+$/;
    var rows,cols,arrRows,arrCols;
    var regBool=regFormat.test(str);
    if(!regBool){
        result="Incorrect command format.";
        return result;
    }
    else{
        var arr=str.split(' ');
        var reg=/\D+/;
        var rowBool=reg.test(arr[0]);
        var colBool=reg.test(arr[1]);
        if(!rowBool && !colBool){
            rows=parseInt(arr[0]);
            cols=parseInt(arr[1]);
            if(rows>0 && cols>0){
                arrRows=3*rows-(rows-1);
                arrCols=3*cols-(cols-1);
                var i,j;
                for(i=0;i<arrRows;i++){
                    resultArr[i]=[];
                    for(j=0;j<arrCols;j++){
                        resultArr[i][j]="[W]";
                    }
                }
                var middleArr=secondLine.trim().split(';');
                var len=middleArr.length;
                if(len<1){
                    result="Incorrect command format.";
                    return result;
                }
                else{
                    for(i=0;i<len;i++){
                        var temReg=/^[\d]+\,[\d]+\s[\d]+\,[\d]+$/;
                        if(temReg.test(middleArr[i])){
                            var str1=middleArr[i].split(' ')[0];
                            var str2=middleArr[i].split(' ')[1];
                            var num1=parseInt(str1.split(',')[0]);
                            var num2=parseInt(str1.split(',')[1]);
                            var num3=parseInt(str2.split(',')[0]);
                            var num4=parseInt(str2.split(',')[1]);
                            if((num1>=0 && num1<rows)&&(num2>=0 && num2<cols)&&(num3>=0 && num3<rows)&&(num4>=0 && num4<cols)){
                                num1=(num1+1)*2-1;
                                num2=(num2+1)*2-1;
                                num3=(num3+1)*2-1;
                                num4=(num4+1)*2-1;
                                if(num1==num3){
                                    var reduce=num2-num4;
                                    if(reduce==2 ||reduce==-2){
                                        resultArr[num1][num2]="[R]";
                                        resultArr[num3][num4]="[R]";
                                        var mdl=(num2+num4)/2;
                                        resultArr[num1][mdl]="[R]";
                                    }
                                    else{
                                        result="Maze format error.";
                                        return result;
                                    }
                                }
                                else if(num2==num4){
                                    var reduce=num1-num3;
                                    if(reduce==2 ||reduce==-2){
                                        resultArr[num1][num2]="[R]";
                                        resultArr[num3][num4]="[R]";
                                        var mdl=(num1+num3)/2;
                                        resultArr[mdl][num2]="[R]";
                                    }
                                    else{
                                        result="Maze format error.";
                                        return result;
                                    }
                                }
                                else{
                                    result="Maze format error.";
                                    return result;
                                }
                            }
                            else{
                                result="Number out of range.";
                                return result;
                            }
                        }
                        else{
                            result="Incorrect command format.";
                            return result;
                        }
    
                    }
                }
            }
            else{
                result="Nmuber out of range.";
                return result;
            }
        }
        else{
            result="Invalid number format.";
            return result;
        }
    }

    var endRsu="";
    for(var k=0;k<resultArr.length;k++){
        endRsu+=resultArr[k].join(' ');
        endRsu+="\n";
    }
    return endRsu;
}
```
