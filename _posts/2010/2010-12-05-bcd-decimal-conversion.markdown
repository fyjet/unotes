---
layout: "post"
title: "BCD/decimal conversion"
date: "2010-12-05 15:15"
tags: [fsx,fsuipc]
categories:
- programming
---
As Flight Simulator published radio freqencies in BCD, it is necessary to convert them before computing frequency change.

Note: frequencies are not fully published in BCD. For example, for COM frequencies (VHF freq), the initial “1″ is not published, the central dot is not, and the final “5″ or “0″ is also not. FS assumes that 118.075 BCD equivalent is 1807.

Here is an extract from convlib.cpp (OnePanel software):

```c++
long bcd2dec(DWORD unsigned long  bcd)
{
  unsigned long dec=0;
  int size,f,g;

  size = (bcd &gt; 32768) ? 16 : 4 ;
  int p=1;

  for (f=0;f&lt;size;f++)
  {
    dec+=(bcd%10)*p;
    bcd=bcd/10;
    p*=16;
  }
  return dec;
}
```

```c++
long dec2bcd(DWORD unsigned long dec)
{
unsigned long bcd=0;
int size,f;
int p=1;

size = (dec &gt; 65536) ? 16 : 4 ;

for (f=0;f&lt;size;f++)
{
bcd+=((dec&gt;&gt;(f*4)) &amp; 15)*p;
p*=10;
}
return bcd;
}
```
