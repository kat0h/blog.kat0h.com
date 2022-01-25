---
title: "LT資料"
date: 2021-06-26
tags: ["LT"]
comments: false
---

# 2020-06-26 LT 資料
## Quineで遊んでみる
### 発表スライド
- [スライドのダウンロード](https://github.com/kato-k/LT/blob/main/2021-06-Quine/LT.key?raw=true)
- [ブラウザでスライドを再生(7MBほどのデータをダウンロードします)](./player)
### 発表で紹介したコード
#### 基本のQuine
##### ruby
```ruby
eval$s="puts('eval$s='+$s.inspect)"
```
##### c
```c
#include &ltstdio.h&gt
int main(){char*s="#include <stdio.h>%cint main(){char*s=%c%s%c;printf(s, 10, 34, s, 34);}";printf(s, 10, 34, s, 34);}
```
#### 変種Quine
##### Ruby
```ruby
eval$s=    %w'b="B                                                  AhsK2Z/+    AMAAAA
  AAPA       PPw                                                      84AA       AAA
  AAA8      IDD                                                       AwcA      AAA
  AAAA    8cP                                                         BwAA    AAA
  AAAA   A8H                                                          PA4A   AAA
  AAAD  A4w                       DPAQ                                AADw  AAA
  PAcw  DM                        AAMA                                DAAA  AP
  APwDwA//            AMeAAD    /APwH8Bw       84B8                   AwH8A/wE
  8Dg8MDgDw         H8Dz  AID     DgwM       HADwP8Hw                 A8PBwgPt/
  zwc8HPA/P         Bzg/   t/D    AQ8P      Dg     8PB                zgA8PDAg8
  fBw8  MBDg               A8e    PDAc     /Dw      4IA               BAA8  8PPA
  5vLx  zOADA             A58f    /vwf    fPA        DAP jhDw==";n=Ma rsha  l.loa
  d(b.    unp       ack("m")[0    ]).t    o_s        (2) .reverse.sca n(/.    {1,
  #{86    }}/)     .jo    in("    "<<1    0).        spl              it("    "<<1
  0);e     ="ev   al$     s=%w    "<<3    9<<        ($s              *3);     c=0;
  o=""      ;n.e  ach     {|i|    i.sp     lit       ("               ").e      ach{
  |j|o      +=j==  "1"    ?e[c ]  :""< <32  ;c+    =j=                ="1"      ?1:0;
 };o=o<    <10};o[  -7,6]=""<<3     9<<".     join";                 puts(o    )'.join
```
```ruby
eval$s=%w'a=Array;s=
                  34;b=a.new(s){a.new(s,0)};(0..
              .s).each{|x|(0...s).each{|y|xx=x-s/2.1
            ;yy=y-s/2.1;i=xx**2+yy**2;b[y][x]=(s/2.8)*
          *2<i&&i<(s/2)**2?1:((i<(s/20)**2)?1:0)}};l=Math;
        t=Time.new;h,m=(                  t.hour%12+t.min*
      0.00872664625)                          *0.5235,t.min*
    0.1047;c=->(p,                                z,u,d,e){g=d
    -z;i=(g>=0)?                                    1:-1;o=e-u;j
  =(o>=0)?1:-1                                      ;g=g.abs*2;o
  =o.abs*2;p                                          [u][z]=1;x=z
  ;y=u;if(g>                                            o);f=o-g/2
;until(x==                                              d);k=f>=0;
y+=(k)?j:0                                              ;f-=(k)?g:
0;x+=i;f+=                                                o;p[y][x]=
1;end;else                                                ;f=g-o/2;u
ntil(y==e)                                                ;if(f>=0);
x+=i;f-=o;                                                end;y+=j;f
+=g;p[y][x                                                ]=1;end;en
d;};c.call                                                (b,s/2,s
/2,(l.sin(                                              m)*(s/2.6)
+s/2).to_i,(                                            -l.cos(m)*
  (s/2.5)+s/                                          2).to_i);c.c
  all(b,s/2,s/                                        2,(l.sin(h
    )*(s/5)+s/                                      2).to_i,(-l.
    cos(h)*(s/4)                                  +s/2).to_i);
      e=0;q="";b.eac                            h{|y|y.each{|x
        |q+=(x==1)?("e                      val$s=%w"<<39<<$
        s*3)[e..e+1]:32.chr*           2;e+=(x==1)?2:0;};q
            <<10};q[-33,6]=""<<39<<".join";puts(q)#'.join
              ########################################
                ########Analog Clock Quine########
                    ##########################
                            ##############
```
```ruby
                      eval$s=%w'loop{a=Arr
                  ay;s=34;b=a.new(s){a.new(s,0)}
              ;(0...s).each{|x|(0...s).each{|y|xx=x-
            s/2.1;yy=y-s/2.1;i=xx**2+yy**2;b[y][x]=(s/
          2.8)**2<i&&i<(s/2)**2?1:((i<(s/20)**2)?1:0)}};l=
        Math;t=Time.new;                  h,m,v=(t.hour%12
      +t.min*0.00872      66                  4625)*0.5235,t
    .min*0.1047,t.                                sec*0.1047;c
    =->(p,z,u,d,                                    e){g=d-z;i=(
  g>=0)?1:-1;o                                      =e-u;j=(o>=0
  )?1:-1;g=g                                        .abs*2;o=o.abs
  *2;p[u][z]                                    =1;x    =z;y=u;if(
g>o);f=o-g                                    /2        ;until(x==
d);k=f>=0;                                  y+          =(k)?j:0;f
-=(k)?g:0;                                x+              =i;f+=o;p[
y][x]=1;en                            d;el                se;f=g-o/2
;until(y==            e);if(f>  =0);x+                    =i;f-=o;en
d;y+=j;f+=                    g;p[y]                      [x]=1;end;
end;};b[(-                                                l.cos(v)*(
s/3)+s/2).                                                to_i][(l
.sin(v)*(s                                              /3)+s/2).t
o_i]=1;c.cal                                            l(b,s/2,s/
  2,(l.sin(m                                          )*(s/2.6)+s/
  2).to_i,(-l.                                        cos(m)*(s/
    2.5)+s/2).                                      to_i);c.call
    (b,s/2,s/2,(                                  l.sin(h)*(s/
      5)+s/2).to_i,(                            -l.cos(h)*(s/4
        )+s/2).to_i);e                      =0;q="";b.each{|
        y|y.each{|x|q+=(x==1            )?("eval$s=%w"<<39
            <<$s*3)[e..e+1]:32.chr*2;e+=(x==1)?2:0;};q<<
              10};q[-33,6]=""<<39<<".join#";e=27.chr;p
                uts("##{e}[1;1H#{e}[H#{e}[2J"+10.c
                    hr+q);sleep(1)}#loop{a=Arr
                            ay;s=34;'.join#
```
#### C
```c
main(){char*s="main(){char*s=%c%s%c;printf(s, 34, s, 34);}";printf(s, 34, s, 34);}
```
`#include`文や`main()`の返り値が省略されています
```c
#define ADD(a, b) (a + b)

int main() {
  printf("%d\n", ADD(1, 2));
  return 0;
}
```
マクロ
```c
#define QUOTE(s) #s

int main() {
  printf("%s\n", QUOTE(test test));
  return 0;
}
```
`#`で文字列をクオートできます
```c
#define Q(a) char*s=#a;a
Q(int main(){printf("#define Q(a) char*s=#a;a%cQ(%s)",10,s);})
```
## 参考文献
- https://mickey24.hatenablog.com/entry/20100915/ruby_udonge_quine
- https://docs.ruby-lang.org/ja/latest/doc/symref.html
- https://mametter.hatenablog.com/
- https://github.com/mame/trance-book/blob/master/3-3/macro-quine.c

