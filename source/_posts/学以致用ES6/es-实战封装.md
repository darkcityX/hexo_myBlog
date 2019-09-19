---
title:  ES6的学以致用之路——封装模块化
date: 2019-09-20 02:01:46
cover: /images/es6/es6_fengmian1.jpg
tags: ES6
categories: ES6
---

### 一、封装
```javascript
/* 封装常用类方法 */

/**
 * @method 判断是安卓机还是苹果机
 * @param 返回值：ios返回true，an返回false
 */
class mobileModel {
    constructor(){
        // 1、获取UA
        this.tarModel = navigator.userAgent;
    }

    showModel(){
        // 2、安卓机判断
        let isAndroid = this.tarModel.indexOf('Android') > -1 || this.tarModel.indexOf('Adr') > -1; //android终端

        // 3、苹果机判断
        let isiOS = !!this.tarModel.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
        
        return  isAndroid ? "android" : isiOS ? "ios" : "其他" ;
    }
}

/**
 * 身份证号码验证
 * @param code 身份证号
 * @param 
 */
class VerifyCode{
    constructor(userCode){
        this.userCode = userCode;
    }
    
    showResult(){
        let city = {
            11 : "北京",
            12 : "天津",
            13 : "河北",
            14 : "山西",
            15 : "内蒙古",
            21 : "辽宁",
            22 : "吉林",
            23 : "黑龙江 ",
            31 : "上海",
            32 : "江苏",
            33 : "浙江",
            34 : "安徽",
            35 : "福建",
            36 : "江西",
            37 : "山东",
            41 : "河南",
            42 : "湖北 ",
            43 : "湖南",
            44 : "广东",
            45 : "广西",
            46 : "海南",
            50 : "重庆",
            51 : "四川",
            52 : "贵州",
            53 : "云南",
            54 : "西藏 ",
            61 : "陕西",
            62 : "甘肃",
            63 : "青海",
            64 : "宁夏",
            65 : "新疆",
            71 : "台湾",
            81 : "香港",
            82 : "澳门",
            91 : "国外 "
        };

        let code;
        let msg = "";
        let pass = true;

        console.log( this.userCode );

        if (!this.userCode || !/^\d{6}(18|19|20)?\d{2}(0[1-9]|1[12])(0[1-9]|[12]\d|3[01])\d{3}(\d|X)$/i.test(this.userCode)) {
            code = 301;
            msg = "身份证号格式错误";
            pass = false;
        }else if (!city[this.userCode.substr(0, 2)]) {
            code = 302;
            msg = "地址编码错误";
            pass = false;
        } else {
            //18位身份证需要验证最后一位校验位
            if (this.userCode.length == 18) {
                this.userCode = this.userCode.split('');
                //∑(ai×Wi)(mod 11)
                //加权因子
                var factor = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2];
                //校验位
                var parity = [1, 0, 'X', 9, 8, 7, 6, 5, 4, 3, 2];
                var sum = 0;
                var ai = 0;
                var wi = 0;
                for (var i = 0; i < 17; i++) {
                    ai = this.userCode[i];
                    wi = factor[i];
                    sum += ai * wi;
                }
                var last = parity[sum % 11];
                if (parity[sum % 11] != this.userCode[17]) {
                    code = 303
                    msg = "校验位错误";
                    pass = false;
                }
            }
        }
        return {
            code: code == undefined ? 200 : code,
            msg: msg == "" ? "验证通过!" : msg,
            pass,
        };

    }
}

export {mobileModel,VerifyCode};
```
