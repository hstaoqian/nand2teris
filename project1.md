# 方法和心得 

按照第一周给出的阅读材料的顺序，在仅有 Nand Gate 的情况下，按以下顺序实现芯片的功能。首先我们来看看 Nand Gate 的真值表。

| a | b | output |
|----|----|----|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

## Not Gate

真值表

| in | output |
|----|----|
| 0 | 1 |
| 1 | 0 |

那么很容易观察到，Nand Gate 里当a，b 值一样时，output 的值正好满足 Not Gate 的功能

```hdl
CHIP Not {
    IN in;
    OUT out;

    PARTS:
    Nand(a=in, b=in, out=out);
}
```

## And Gate

真值表

| a | b | output |
|----|----|----|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

很容易看出来，这和 Nand Gate 的值刚好相反，那么实现起来很简单，直接加个 Not Gate 就行了。

```
CHIP And {
    IN a, b;
    Out output;

    PARTS:
    Nand(a=a, b=b, output=w1)
    Not(in=w1, output=output)
}
```

## Or Gate

真值表

| a | b | output |
|----|----|----|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

实现 Or gate 就不是一眼就好看出来的了，我们可以试着用课程中给出的方法来把真值表转换为 Boolean 方程式。
可以观察到，if a==0 and b == 0时，值为0，其余情况下值为1

那么很容易列出式子

```
not(a and b)
```

由于前面我们已经构造出了 And Gate 和 Not Gate，那么这里用 hdl 语言就很好表达了

```
CHIP Or{
    IN a, b;
    OUT output;

    PARTS:
    And(a=a, b=b, output=w1);
    Not(In=w1, output=output);
}
```

## Xor Gate

真值表

| a | b | output |
|----|----|----|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

同样，单纯的通过观察不容易得出结果。我们用输出结果为1的结果来构造式子

```
(nota and b) or (a and notb)
```

用 HDL 语言构造

```
CHIP Xor{
    IN a, b;
    OUT output;

    PARTS:
    Not(in=a, out=nota);
    Not(in=b, out=notb);
    And(a=nota, b=b, out=w1);
    And(a=a, b=notb, out=w2);
    Or(a=w1, b=w2, out=output);
}
```

## Multiplexor

真值表

|   a   |   b   |  sel  |  out  |
|---|---|---|---|
|   0   |   0   |   0   |   0   |
|   0   |   0   |   1   |   0   |
|   0   |   1   |   0   |   0   |
|   0   |   1   |   1   |   1   |
|   1   |   0   |   0   |   1   |
|   1   |   0   |   1   |   0   |
|   1   |   1   |   0   |   1   |
|   1   |   1   |   1   |   1   |




