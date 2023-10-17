# 1.bitXor - x^y using only ~ and & 

```c
int bitXor(int x, int y) {
  int a = ~x & y;
  int b = x & ~y;
  int result = ~ (~ a &~ b );
  return result;
}
```

^操纵可以写成x&~y | ~x&y,这再用取反来将|转换为&。

# 2.tmin - return minimum two's complement integer 

``` c 
int tmin(void) {

  return 1<<31;//minimum two's complement integer 为最高位为1其余位为0

}
```

# 3.isTmax - returns 1 if x is the maximum, two's complement number

```c
int isTmax(int x) {
    x = ~x;
    return !((~x+1) ^ x) & (!!(x));
    //最大的补码取反为最小的补码，且最小补码的负数等于自己。
    //-x = ~x +1。
    //这里需要排除x=-1的情况，因为化简表达式为x+1 = -x -1.
    //!!将数字转换为逻辑0和1
}
```

# 4.allOddBits - return 1 if all odd-numbered bits in word set to 1

```c
int allOddBits(int x) {
    int mask = 0xAA << 8 | 0xAA;
    mask = (mask << 16) | mask;
    int result = !((x&mask)^ mask);	  
    return result;
    //先获得奇数的掩码 0xA=1010
    //再通过&取出奇数位，然后^比较是否一致
}
```

# 5.negate - return -x 

``` c
int negate(int x) {
  return ~x+1;//-x = ~x+1
}
```

# 6.isAsciiDigit - return 1 if 0x30 <= x <= 0x39 (ASCII codes for characters '0' to '9')

```c
int isAsciiDigit(int x) {
 int y = x&0xf;
 return !((x>>4)^0x3) & !((9+~y+1)>>31); 
 //比较是否为3可以，>>4位然后^,比较是否位0到9,可以用9-a,然后算术右移判断符号位是否为1
}
```

# 7.conditional - same as x ? y : z 

```c
int conditional(int x, int y, int z) {
  int flag = !!x;
  int right_f = ~flag + 1;  
return (right_f & y) | (~right_f &z);
    //这里的right_f是-1，因为-1=0xffffffff，然后&可以保留比特不变
}
```

# 8.isLessOrEqual - if x <= y  then return 1, else return 0 

```c
int isLessOrEqual(int x, int y) {
  int x_h = x>>31;
  int y_h = y>>31;
  int flag = !!(x_h ^ y_h);
  return !(x^y) |  (!flag & ((x-y)>>31)) | (flag & !y_h);
    //这里分情况讨论，先判断符号是否一致，
    //1.若符号不一致，且y为正则返回1
    //2.若符号一致，且x-y小于0返回1
    //3.若x=y，返回1
}
```

# 9.logicalNeg - implement the ! operator, using all of 

``` c
int logicalNeg(int x) {
  int temp =((~x)>>31)  & ((x-1)>>31)&0x1;  

return temp;
    //0的特定在于，0-1小于0，-0-1=~0<0.
}
```

# 10.howManyBits - return the minimum number of bits required to represent x in   two's complement

```c
int howManyBits(int x) {
  //用补码表示正数时，关键在于最高位为0
  //用补码表示负数时，关键在于为1，且对于补码来说111x和11x和1x大小一致
  //所以关键在于表示与最高位不同的数字。
  int temp = x^(x<<1); //取出最高不同位的位置数字标识为1
  int bit_16,bit_8,bit_4,bit_2,bit_1;
  bit_16 = !!(temp>>16)<<4; //判断数字为1的位置是否大于16
  temp = temp >> bit_16;//大于则右移16位，小于则不用
  bit_8 = !!(temp>>8)<<3;
  temp = temp >> bit_8;
  bit_4 = !!(temp>>4)<<2;
  temp = temp >> bit_4;
  bit_2 = !!(temp>>2)<<1;
  temp = temp >> bit_2;
  bit_1 = !!(temp>>1);
  return 1+bit_16+bit_8+bit_4+bit_1+bit_2;
  return 0;
}
```

# 11.floatScale2 - Return bit-level equivalent of expression 2*f for floating point argument f.

```c
 unsigned floatScale2(unsigned uf) {
unsigned exp = (uf>>23)&0xff; //取出指数部分

unsigned s = uf >> 31;//取出符号位
unsigned temp = uf;
if(exp == 0xff) return uf;//指数部分满，*2溢出
if(exp == 0) return uf << 1 | (s<<31);//无指数，左移一位保留符号位
exp ++;
if(exp == 255) return 0x7f800000 | (s<<31); //溢出返回正无穷，小数部分为0
return exp << 23 | (0x807fffff & temp);//正常处理
 
    }

```

# 12.floatFloat2Int - Return bit-level equivalent of expression (int) f for floating point argument f.

```c
int floatFloat2Int(unsigned uf) {
      int exp = (uf >> 23)& 0xff;
      int temp = exp -127;//取指数部分的偏置后的实际值 2^(8-1) - 1=127
     
      if (temp >=32) return 0x80000000u;//大于32，则4个字节无法表示，则溢出
      else if(temp < 0) return 0; //小数则为0
      else {
       int x = (uf | 0x800000 ) &0xffffff;//取小数字段
       if(temp >=23 ) {
       int e = temp -23; //表示要转为int要右移已经在低23位的小数
       x = x << e;
     }
       else {
       int e = 23 - temp;
       x = x >> e;
     }
     int flag = uf >>31 <<31;
     return flag ? -x :x;
}

```

# 13.floatPower2 - Return bit-level equivalent of the expression 2.0^x (2.0 raised to the power x) for any 32-bit integer x.

``` c
unsigned floatPower2(int x) {
             if (x > 127)
             {
                     return 0x7f800000; //指数大于127则溢出，128+偏置127=255
             }
             else if (x < -127)
             {
                     return 0x0;
             }
             else
             {
                     return (x + 127) << 23;
             }
     }

```

