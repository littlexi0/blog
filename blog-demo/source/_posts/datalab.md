---
title: datalab
date: 2022/12/03
updated: 2022/12/18
cover: https://littlexi.oss-cn-shanghai.aliyuncs.com/images/QQ%E5%9B%BE%E7%89%8720221218215423.jpg
top_img: 
description: datalab14个问题的解决方案
swiper_index: 12 #置顶轮播图顺序，非负整数，数字越大越靠前
categories: CSAPP
---



@[TOC](datalab)
# 【LittleXi】CSAPP(datalab)14 problems

## puzzle 1:bitXor
题目要求：求取x，y的异或值

解法：根据x，y的真值表，x ^ y =（~x & y）| (x & ~y)
~~~c
//1
/*
* bitXor - x^y using only ~ and &
*   Example: bitXor(4, 5) = 1
*   Legal ops: ~ &
*   Max ops: 14
*   Rating: 1
*/
int bitXor(int x, int y) {
   	return ~((~(~x & y)) & (~(x & ~y)));
}
~~~

## puzzle 2:tmin
题目要求：返回最小的32位int值

解法：直接返回0x80000000就行啦
~~~c
/*
* tmin - return minimum two's complement integer
*   Legal ops: ! ~ & ^ | + << >>
*   Max ops: 4
*   Rating: 1
*/
int tmin(void) {
	int num = 1 << 31;
	return num;
}
~~~

## puzzle 3:isTmax
题目要求：如果x是32位int的最大值，返回1，否则返回0

解法：可以考虑将x+1，那么int最大值0x7fffffff会变为其取反的值，注意还有这个特性的值是0xffffffff，要特殊考虑
~~~c
/*
 * isTmax - returns 1 if x is the maximum, two's complement number,
 *     and 0 otherwise
 *   Legal ops: ! ~ & ^ | +
 *   Max ops: 10
 *   Rating: 1
 */
int isTmax(int x) {
    int m = x ^ (x + 1);
    return (!(m ^ (~0))) & (!!(~x));
}
~~~

## puzzle 4:allOddBits
题目要求：如果奇数位置上全是1则返回1，否则返回0

解法：先利用掩码思路，仅保留奇数位上的值，然后拿0xAAAAAAAA和x异或就行啦
~~~c
/*
 * allOddBits - return 1 if all odd-numbered bits in word set to 1
 *   where bits are numbered from 0 (least significant) to 31 (most significant)
 *   Examples allOddBits(0xFFFFFFFD) = 0, allOddBits(0xAAAAAAAA) = 1
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 12
 *   Rating: 2
 */
int allOddBits(int x) {
    int temp = 0xAA;
    temp <<= 8;
    temp |= 0xAA;
    temp <<= 8;
    temp |= 0xAA;
    temp <<= 8;
    temp |= 0xAA;
    return !(temp ^ (x & temp));
}
~~~

## puzzle 6:negate
题目要求：返回x的负数

解法：根据补码的规则，将x取反+1就是x的负数的编码
~~~c
/*
 * negate - return -x
 *   Example: negate(1) = -1.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 5
 *   Rating: 2
 */
int negate(int x) {
    return ~x + 1;
}
~~~

## puzzle 7:isAsciiDigit
题目要求：如果x是0x30到0x39范围的数，返回1，否则返回0

解法：先考虑第4、5位是否为11，然后讨论0-3位置的数字
~~~c
/*
 * isAsciiDigit - return 1 if 0x30 <= x <= 0x39 (ASCII codes for characters '0' to '9')
 *   Example: isAsciiDigit(0x35) = 1.
 *            isAsciiDigit(0x3a) = 0.
 *            isAsciiDigit(0x05) = 0.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 15
 *   Rating: 3
 */
int isAsciiDigit(int x) {
    //48 57
    return !(((x >> 4) ^ 3) | (((x >> 3) & 1) & (((x >> 1) & 1) | ((x >> 2) & 1))));
}
~~~

## puzzle 8:conditional
题目要求：模拟三目运算符 x ? y : z

解法：先对x取两次！运算，然后取反+1得到全1或全0，然后再去&y，&x就行啦
~~~c
/*
 * conditional - same as x ? y : z
 *   Example: conditional(2,4,5) = 4
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 16
 *   Rating: 3
 */
int conditional(int x, int y, int z) {
    int n = !!x;
    int m = ~n + 1;
    return (m & y) | (~m & z);
}
~~~

## puzzle 9:isLessOrEqual
题目要求：模拟 x <= y

解法：先判断正负，如果同号，再判断x-y的正负
~~~c
/*
 * isLessOrEqual - if x <= y  then return 1, else return 0
 *   Example: isLessOrEqual(4,5) = 1.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 24
 *   Rating: 3
 */
int isLessOrEqual(int x, int y) {
    int a = x >> 31 & 1;
    int b = y >> 31 & 1;
    int c = x + (~y + 1);
    int d = (!(c ^ 0)) | ((c >> 31) & 1);
    return (a & (!b)) | ((!(a ^ b)) & d);
}
~~~

## puzzle 10:logicalNeg
题目要求：模拟 ！运算

解法：将x与-x异或，观察最高位是否为0
~~~c
/*
 * logicalNeg - implement the ! operator, using all of
 *              the legal operators except !
 *   Examples: logicalNeg(3) = 0, logicalNeg(0) = 1
 *   Legal ops: ~ & ^ | + << >>
 *   Max ops: 12
 *   Rating: 4
 */
int logicalNeg(int x) {
    int a = x & 1;
    x >>= 1;
    return (~a) & (1 ^ (((x ^ (~x + 1)) >> 31) & 1));
}
~~~

## puzzle 11:howManyBits
题目要求：统计32位int数字fabs（x）的里面1的个数然后+1

解法：进行二分操作统计1的数字，注意0，-1要特殊讨论
~~~c
/* howManyBits - return the minimum number of bits required to represent x in
 *             two's complement
 *  Examples: howManyBits(12) = 5
 *            howManyBits(298) = 10
 *            howManyBits(-5) = 4
 *            howManyBits(0)  = 1
 *            howManyBits(-1) = 1
 *            howManyBits(0x80000000) = 32
 *  Legal ops: ! ~ & ^ | + << >>
 *  Max ops: 90
 *  Rating: 4
 */
int howManyBits(int x) {
    int cnt = 1;
    int  s = x >> 31;
    int  t = (s & ~x) | (~s & x);
    int temp1 = t >> 16;
    int t2 = !!temp1;
    int m = 1 << 4;
    int c = 0xff;

    cnt = cnt + (t2 << 4);
    m &= (~t2 + 1);
    t >>= m;
    c <<= 8;
    c |= 0xff;
    t &= c;

    temp1 = t >> 8;
    t2 = !!temp1;
    cnt = cnt + (t2 << 3);
    m = 8;
    m &= (~t2 + 1);
    t >>= m;
    t &= 0xff;

    temp1 = t >> 4;
    t2 = !!temp1;
    cnt = cnt + (t2 << 2);
    m = 4;
    m &= (~t2 + 1);
    t >>= m;
    t &= 0xf;

    temp1 = t >> 2;
    t2 = !!temp1;
    cnt = cnt + (t2 << 1);
    m = 2;
    m &= (~t2 + 1);
    t >>= m;
    t &= 0x3;

    temp1 = t >> 1;
    t2 = !!temp1;
    cnt = cnt + t2;
    m = 1;
    m &= (~t2 + 1);
    t >>= m;
    t &= 0x1;

    return cnt + t;
}
~~~

## puzzle 12:floatScale2
题目要求：对浮点数uf乘2操作

解法：按照IEEE规则，对exp，frac位<<1操作，注意要讨论NaN,正无穷，负无穷，0等特殊情况
~~~c
/*
 * floatScale2 - Return bit-level equivalent of expression 2*f for
 *   floating point argument f.
 *   Both the argument and result are passed as unsigned int's, but
 *   they are to be interpreted as the bit-level representation of
 *   single-precision floating point values.
 *   When argument is NaN, return argument
 *   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
 *   Max ops: 30
 *   Rating: 4
 */
unsigned floatScale2(unsigned uf) {
    int s = (uf >> 31) << 31;
    int exp = (uf >> 23) & (0xff);
    int frac = uf << 9;
    int t = 0x7f;
    if (!(exp ^ 0xFF))
        return uf;
    else
    {
        if (exp == 0)
        {
            if ((frac >> 31) & 1)
            {
                exp = 1;
                frac <<= 1;
            }
            else
            {
                frac <<= 1;
            }
        }
        else
        {
            exp += 1;
            if (!(exp ^ 0xff))
            {
                frac = 0;
            }
        }
    }
    frac >>= 9;
    exp <<= 23;
    t <<= 8;
    t |= 0xff;
    t <<= 8;
    t |= 0xff;
    frac &= t;
    return s | exp | frac;
}
~~~

## puzzle 13:floatFloat2Int
题目要求：将浮点数转为整数

解法：按照IEEE规则，对exp，frac位讨论，返回pow(2,exp)*(1.frac)，越界等特殊情况要讨论
~~~c
/*
 * floatFloat2Int - Return bit-level equivalent of expression (int) f
 *   for floating point argument f.
 *   Argument is passed as unsigned int, but
 *   it is to be interpreted as the bit-level representation of a
 *   single-precision floating point value.
 *   Anything out of range (including NaN and infinity) should return
 *   0x80000000u.
 *   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
 *   Max ops: 30
 *   Rating: 4
 */
int floatFloat2Int(unsigned uf) {
    int s = uf >> 31;
    int exp = (uf >> 23) & 0xff;
    int t = 0x7f;
    int frac = uf & t;
    int ans = 0;

    t <<= 8;
    t &= 0xff;
    t <<= 8;
    t &= 0xff;
    exp -= 127;

    if (exp < 0)
        ans = 0;
    else if (exp == 0)
        ans = 1;
    else if (exp > 31)
        ans = 0x80000000u;
    else if (exp > 23)
        ans = frac << (exp - 23);
    else
        ans = frac >> (exp - 23);
    if (s)
        ans = -ans;
    return ans;
}
~~~

## puzzle 14:floatPower2
题目要求：浮点表示pow(2,x)

解法：按照IEEE规则，对exp，frac位讨论，如果越界则返回max就行了
~~~c
/*
 * floatPower2 - Return bit-level equivalent of the expression 2.0^x
 *   (2.0 raised to the power x) for any 32-bit integer x.
 *
 *   The unsigned value that is returned should have the identical bit
 *   representation as the single-precision floating-point number 2.0^x.
 *   If the result is too small to be represented as a denorm, return
 *   0. If too large, return +INF.
 *
 *   Legal ops: Any integer/unsigned operations incl. ||, &&. Also if, while
 *   Max ops: 30
 *   Rating: 4
 */
unsigned floatPower2(int x) {
    int ans = 0;
    int exp = x;
    exp += 127;
    if (exp >= 255)
    {
        ans = 0x7f;
        ans <<= 8;
        ans |= 0x80;
        ans <<= 16;
    }
    else if (exp <= 0)
    {
        ans = 0;
    }
    else
    {
        ans = exp << 23;
    }
    return ans;
}
~~~

