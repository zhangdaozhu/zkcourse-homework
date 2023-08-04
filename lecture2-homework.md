# 第2课 课后作业

## 第1题 转换为bit位 Num2Bits
```circum
pragma circom 2.1.4;


template Num2Bits(n) {

    var res = 0;
    var lv = 1;
    
    signal input in;
    signal output out[n];
   
    for (var i =0 ;i< n ;i++) {
        out[i] <-- (in >> i) & 1;
        out[i]*(out[i] - 1) === 0; 
        res += out[i]*lv;
        lv = lv + lv;
    }
    res === in;
}

```

## 第2题 判零 IsZero
```
pragma circom 2.1.4;
template IsZero() {
    signal input in;
    signal output out;

    signal inv;

    inv <-- in!=0 ? 1/in : 0;

    out <== -in*inv +1;
    in*out === 0;
}
```
## 第3题 相等 IsEqual
```
pragma circom 2.1.4;
template IsEqual() {
    signal input in[2];
    signal output out;

    component isz = IsZero();

    in[1] - in[0] ==> isz.in;

    isz.out ==> out;
}
```

## 第4题 选择器 Selector

```
pragma circom 2.1.4;

template IsEqual () {
    signal input in[2];
    signal output out;
    signal inv;

    inv <-- (in[0] - in[1]) == 0 ? 0 : 1 / (in[0] - in[1]);
    out <== - inv * (in[0] - in[1]) + 1;
    out * (in[0] - in[1]) === 0;
}

template Sum (n) {
    signal input in[n];
    signal output out;
    signal sum[n + 1];

    sum[0] <== 0;
    for (var i = 0; i < n; i++) {
        sum[i + 1] <== in[i] + sum[i];
    }
    out <== sum[n];
}

template Selector (nChoices) {
    signal input in[nChoices];
    signal input index;
    signal output out;
    
    component iseq[nChoices];
    component sum = Sum(nChoices);
    for (var i = 0; i < nChoices; i++) {
        iseq[i] = IsEqual();
        iseq[i].in[0] <== i;
        iseq[i].in[1] <== index;
        sum.in[i] <== iseq[i].out * in[i]; 
    }
    out <== sum.out;
}

```


## 第5题 判负 IsNegative
```
template IsNegative() {
    signal input in[254];
    signal output sign;

    component comp = CompConstant(10944121435919637611123202872628637544274182200208017171849102093287904247808);

    var i;

    for (i=0; i<254; i++) {
        comp.in[i] <== in[i];
    }

    sign <== comp.out;
}
```
## 第6题 少于 LessThan
```
template LessThan(n) {
    assert(n <= 252);
    signal input in[2];
    signal output out;

    component n2b = Num2Bits(n+1);

    n2b.in <== in[0]+ (1<<n) - in[1];

    out <== 1-n2b.out[n];
}
```

