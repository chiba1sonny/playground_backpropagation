# playground_backpropagation

## 1.はじめに

今日は、ニューラルネットワークの基本アルゴリズムである誤差逆伝播法（Backpropagation)をわかりやすく紹介したいと思います。疲れた時、色々な数式を見ると頭は痛くなっちゃいます。なので、具体的な数字を使って誤差逆伝播法を紹介します。

## 2.ニューラルネットワーク

![1627030564(1).png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1668082/edcc9c0c-98f5-0ff9-5107-06e526f1418d.png)
```i1```と```i2```は入力、```o1```と```o2```は出力、```w```はパラメータです(Biasはなし、活性化関数はidentity)。

```
Hidden層：h1=i1*w1+i2*w3,  h2=i1*w2+i2*w4
```

```
Output層：o1=h1*w5+h2*w7,  o2=h1*w6+h2*w8
```
誤差逆伝播法の目標：入力値を与え、```w```を学習させることより、出力値を```o1，o2```に近くする。

## 3.誤差逆伝播法（Backpropagation)

具体的な数字を与える：

```
i1＝1, i2=1.5, o1target=2.5, o2target=3
```
パラメータ```w```の初期値を設定する（初期値の設定は基準がある、こちらで適当にしました）：

```
w1＝1.5, w2=2, w3=2, w4=2.5, w5=1.2, w6=2.5, w7=0.5, w8=1
```

### ステップ1　順伝播

```
h1=i1*w1+i2*w3=1*1.5+1.5*2=4.5
h2=i1*w2+i2*w4=1*2+1.5*2.5=5.75
o1=h1*w5+h2*w7=4.5*1.2+5.75*0.5=8.275
o2=h1*w6+h2*w8=4.5*2.5+5.75*1=17
```

上記通り、順伝播が終わりです。```w```の初期値を使って入力```i1, i2```における出力```o1(8.275), o2(17)```を算出しました。しかし、正解の```o1target(2.5), o2target(3)```と全然違います。これから、出力の誤差に逆伝播を行い、パラメータ```w```を更新し、改めて出力を算出します。

### ステップ2　逆伝播

誤差設定

```
E=E1+E2=1/2*(o1target-o1)^2+1/2*(o2target-o2)^2
```

誤差計算

```
E=E1+E2=1/2*(o1target-o1)^2+1/2(o2target-o2)^2=1/2*(2.5-8.275)^2+1/2*(3-17)^2=114.68
```

パラメータ```w```の更新

パラメータ```w```の更新は、Output層からHidden層までは```w5, w6, w7, w8```、Hidden層からInput層までは```w1, w2, w3, w4```の更新です。こちらで```w5```の更新を例として解説します。
考え方は：パラメータ```w5```は誤差```E```にどのような影響を与えたかを知りたい、そして```w5```を更新します。

#### 1)```w5```は誤差```E```にどのような影響を与えたかーーー編微分

``` w5 ```で編微分： 

```
∂E/∂w5=∂E/∂E1*∂E1/∂w5
∂E/∂E1=2*1/2*(o1target-o1)*(-1)+0=5.775
∂E1/∂w5=h1+0=4.5
∂E/∂w5=∂E/∂E1*∂E1/∂w5=5.775*4.5=25.9875
```

#### 2)```w5```を更新する

学習率```η```を0.1と設定し、```w5```を更新する：

```
w5+=w5-η*∂E/∂w5=1.2-0.1*25.9875=-1.3986
```

同じように、他の```w```を更新します。

これで、一回の誤差逆伝播は終わりです。更新された```w```を改めてネットワークに入れて計算します。多く反復した後、誤差が小さくなり、出力はターゲット値に近くなります。

## 4.まとめ

具体的な数字を使って誤差逆伝播法を紹介してみました。

参考

Understanding and coding Neural networks From Scratch in Python and R
https://www.analyticsvidhya.com/blog/2020/07/neural-networks-from-scratch-in-python-and-r/

Ps
Multilayer Perceptron Training VisualizationというMLPの可視化できるツールをお勧めします。

サイト：https://borgelt.net/doc/mlpd/mlpd.html#Introduction
