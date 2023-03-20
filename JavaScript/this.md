#this란 무엇인가
- this는 함수 내에서 함수 호출 맥락(context)를 의미한다.
- 함수안에서 사용할 수 있는 변수, 약속된 변수이다. 

```javascript
function func() {
    if(window == this) {
        console.log("window ===this");
    }
}
func() //window.func();
```

```javascript
var o = {
    func : function() {
        if(o === this) {
            console.log("o === this");
        }
    }
}
o.func() //o === this
```

어떤 객체에 속해있지 않은 전역변수 / 전역객체는 window라는 것이 생략되어 있다. 
<code>window.func();<code/>

####생성자 객체

``` javascript
function Func() {
    funcThis = this;
}
```

``` javascript
function sum(x,y) {return x+y;}
```

``` javascript
var sum2 = new Function('x','y','return x+y;');
```

``` javascript
var  o = { ... }
var a = [1,2,3];
new Array(1,2,3);
```

<strong>객체(master) 안에 메소드(slave)가 있다.</strong>

###this는 변화무쌍하다. <br>
누구의 소속이냐에 따라 this객체는 달라진다. 



















