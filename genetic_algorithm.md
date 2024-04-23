# 遺伝的アルゴリズムとは? 仕組みや応用例を理解しよう!


## はじめに
遺伝的アルゴリズムという言葉を聞いたことがあるでしょうか? 

遺伝的アルゴリズムとは、生物の進化からヒントを得た最適化のためのアルゴリズムの一種です。

株の自動取引や工場での製品生産といった、経済学や工学での様々な問題に応用されています。


この記事では、まず、遺伝的アルゴリズムの仕組みや特徴、応用例について説明します。

その後に、社会で生活していくうえで、遺伝的アルゴリズムの考え方をどのように生かすことができるのかということに関して、私の考えを説明します。

- [遺伝的アルゴリズムとは? 仕組みや応用例を理解しよう!](#遺伝的アルゴリズムとは-仕組みや応用例を理解しよう)
  - [はじめに](#はじめに)
  - [遺伝的アルゴリズムの説明](#遺伝的アルゴリズムの説明)
    - [遺伝的アルゴリズムの手順](#遺伝的アルゴリズムの手順)
      - [1 初期世代の生成](#1-初期世代の生成)
      - [2 適合度の計算](#2-適合度の計算)
      - [3 選択](#3-選択)
      - [4 交叉、突然変異](#4-交叉突然変異)
  - [遺伝的アルゴリズムの応用例](#遺伝的アルゴリズムの応用例)
    - [応用例1 オプション取引](#応用例1-オプション取引)
    - [応用例2 工場の生産計画](#応用例2-工場の生産計画)
  - [遺伝的アルゴリズムのメリット、デメリット](#遺伝的アルゴリズムのメリットデメリット)
    - [メリット](#メリット)
      - [様々な問題に対して適用できる](#様々な問題に対して適用できる)
      - [ランダム性を持たせることができる](#ランダム性を持たせることができる)
    - [デメリット](#デメリット)
      - [どのようにパラメータや評価関数を定めるかがはっきりと確立されていない](#どのようにパラメータや評価関数を定めるかがはっきりと確立されていない)
      - [最適解であることが保証されない](#最適解であることが保証されない)
  - [遺伝的アルゴリズムの考え方から得られる教訓](#遺伝的アルゴリズムの考え方から得られる教訓)
      - [未知の事柄に積極的にチャレンジすることの重要性](#未知の事柄に積極的にチャレンジすることの重要性)
      - [異なるバックグラウンドを持つ人との交流の重要性](#異なるバックグラウンドを持つ人との交流の重要性)
      - [状況の変化に対応することの重要性](#状況の変化に対応することの重要性)
  - [まとめ](#まとめ)
  - [参考　ボラティリティのキャリブレーションへの適用例](#参考ボラティリティのキャリブレーションへの適用例)
    - [実装例](#実装例)


## 遺伝的アルゴリズムの説明


### 遺伝的アルゴリズムの手順
遺伝的アルゴリズムがどのような手順で解を求めるのか簡単に説明します。


遺伝的アルゴリズムは次の図のような手順により行われます。


```mermaid
graph TD;
    1.初期世代の生成-->2.適合度の計算-->3.個体の選択-->4.交叉,突然変異-->|終了条件が満たされた場合|5.終了;
    4.交叉,突然変異-->|"終了条件(試行回数など)</br>が満たされていない場合"|2.適合度の計算
```

それぞれの世代の個体数をNとします。

#### 1 初期世代の生成
まずはN個の個体をランダムに生成し、N個の個体全体からなる初期世代を作成します。(最初の時点ではこの初期世代を現世代と呼びます。)
それぞれの個体は、数字やアルファベットで表現された遺伝子が複数個並んだ染色体をもちます。

<!-- - 染色体の例(0または1で表されている各数値が遺伝子を表現しています)
<img src="image.png" width="30%"> -->
<figure>
  <img src="image.png" width="40%" alt="染色体の例" />
  <figcaption>染色体の例(0または1からなる数値が遺伝子を表しています)</figcaption>
</figure>


#### 2 適合度の計算
評価関数によって、適応度の計算を各個体の染色体に対して行います。適応度はそれぞれの個体が環境に適応している度合いを表しています。

#### 3 選択
環境に適応した個体は次の世代で個体数を増やし、適応しない個体は減らすように、現世代での個体から適合度の高い個体を残す操作を行います。
選択の方法には、適合度に比例した確率で個体を選択する「ルーレット方式」、適応度の順位に基づいてあらかじめ各個体を選択する確率を決めておく「ランキング方式」などがあります。



- 選択の例(選択の方法にルーレット選択を用いた場合)

| 個体の番号 | 適合度 | 選択確率 |
| :----: | :----: | :----: |
| 1 | 0.1 | 10%  |
| 2 | 0.2 | 20%  |
| 3 | 0.1 | 10%  |
| 4 | 0.3 | 30%  |
| 5 | 0.3 | 30%  |



#### 4 交叉、突然変異
選択した個体に対して、2つの個体を選んで新しい個体を生成する「交叉」、一定の割合の個体に対して染色体の一部の遺伝子を変化させる「突然変異」といった操作を適用することで次の世代を作成します。

新たに作成した世代を「現世代」として古い世代と置き換えて2の操作に戻ります。



このように、2\~4の「適応度の評価」、「選択」、「交叉、突然変異」といった手順を現世代に対して適用することで次の世代を生成し、生成された世代が次の現世代となり、今度はこの新たな世代に対して、2\~4の操作を再び適用して新たな世代を生成する...という操作を繰り返します。

その結果、適合度の高い個体がより多くの子孫を残すことで徐々に集団全体が良くなっていくというのが、遺伝的アルゴリズムの考え方です。

## 遺伝的アルゴリズムの応用例

パラメータを遺伝子とした「個体」として解の候補を表して、複数の解の候補の中から最適な解を見つけるという形で、遺伝的アルゴリズムは様々な分野の問題に対して応用されています。

### 応用例1 オプション取引

オプションとは「将来のある時点で、特定の価格で原資産を取引する金融商品」を表しています。ここで、原資産とは株や債券などのオプションのもととなる金融商品を表しています。
国内では、日経平均株価を原資産とした日経225、TOPIX（東証株価指数）を対象としたTOPIXオプション取引が代表的です。

オプションの価格を計算するために、まず原資産が従うモデルを考えます。

様々なモデルが存在しますが、ここでは次のようなHestonモデルというモデルを考えます。

- Hestonモデル
  
![alt text](heston_1.png)

<!-- $$
\begin{cases}
dS_t = rS_t dt + \sqrt{\nu_t}S_t dW^{(1)}_t \\
d\nu_t = \kappa(\theta-\nu_t)dt + \sigma \sqrt{\nu_t} dW^{(2)}_t \\
E[dW^{(1)}_t dW^{(2)}_t] = \rho dt
\end{cases}
$$


($S_t$: 時刻$t$での原資産の価格)
ここで、$S_t$は時点$t$での原資産の価格、$\nu_t$は価格の変動の大きさを表しています。  -->

上の式で出てくるパラメータσ, κ, θ, ρと時点0でのν_tの値を市場のデータを使って適切に定める必要があります。

このように、市場のデータからモデルのパラメータを決定することをキャリブレーションとよびます。


キャリブレーションでは、遺伝的アルゴリズムを以下のように適用することができます。

最小化をする目的関数を

![alt text](heston_2.png)
<!-- $$\sum_{i=1}^N \frac{|\text{MarketPrice$^{(i)}$} - \text{ModelPrice$^{(i)}$}|}{\text{MarketPrice$^{(i)}$}}$$
と定めます。

ここで、$\text{MarketPrice}^{(i)}$,$\text{ModelPrice}^{(i)}$はそれぞれ$i$番目の市場価格、モデルによって算出された価格を表しています。  -->

すなわち、市場価格とモデルによって算出された価格の(相対)誤差を最小化するパラメータを遺伝的アルゴリズムによって探索します。
(参考: https://uu.diva-portal.org/smash/get/diva2:729886/FULLTEXT01.pdf)

[この記事の最後](#参考ボラティリティのキャリブレーションへの適用例)で、より基本的な設定での遺伝的アルゴリズムの応用について実装例を載せました。

<!-- 株価の予測を行う上で、過去の株価の値動きを分析して、将来の値動きを予測するための指標として「テクニカル指標」と呼ばれるパラメータが用いられています。

株式の自動売買において、テクニカル指標のパラメータを遺伝子としてとらえて、遺伝的アルゴリズムを適用するという研究が存在します。
(参考: 新井佑弥「遺伝的アルゴリズムを用いたシステムトレードに向けた考察」
https://uec.repo.nii.ac.jp/record/2158/files/1151002.pdf) -->

### 応用例2 工場の生産計画

工場で製品の製造を行う場合を考えます。

製造する製品を切り替える際には、セットアップ時間とよばれる機械の設定を変更する時間が必要です。

<!--  複数の製品と機械との間の依存関係を考慮したうえで、--> セットアップ時間を最小化しつつ製品在庫を安定させるという問題を解くために、遺伝的アルゴリズムを用いて各製品の生産スケジュールを自動生成するという研究が存在します。

このように、パラメータが大量に存在する場合や、最適化の目的となる関数が複数個ある場合であっても、遺伝的アルゴリズムを使用することができます。

## 遺伝的アルゴリズムのメリット、デメリット
数学的な手法や他の探索的アルゴリズムと比較した際に、遺伝的アルゴリズムには次のようなメリット、デメリットが存在します。
### メリット 
#### 様々な問題に対して適用できる

　　複数の解の候補を並行的に用いて解を探索するという点が、遺伝的アルゴリズムの特徴として挙げられます。

このため、次のような数学的な手法や従来の探索方法では解を探すのが困難な場合でも、遺伝的アルゴリズムであれば適用できる場合があります。

- 解を求めるために必要なパラメータの数が多すぎる。
  
- 一つ一つの解の候補を順番に探索していると時間が掛かりすぎてしまう。
  

#### ランダム性を持たせることができる
問題に対して最適解を探索する場合に、局所解(狭い範囲でしか最適でない解)にたどり着いた後にそこから抜け出すことが
できなくなってしまうという問題がしばしば発生します。

- 局所解と最適解の例(最小値を求める場合にx=3で局所解、x=-2で最適解をとる)
<figure>
  <img src="min_local.png" width="70%" alt="graph" />
  <figcaption></figcaption>
</figure>


遺伝的アルゴリズムでは、個体の遺伝子の一部を変化させる「突然変異」を行うことでランダム性を持たせ、局所解を抜け出す効果があります。

一方で、「突然変異」を起こす割合である突然変異率をあまり高くしてしまうと、単にランダムに探索している場合とあまり変わらなくなってしまいます。

そのため、通常の場合では突然変異率は低く設定されるようです。




### デメリット
#### どのようにパラメータや評価関数を定めるかがはっきりと確立されていない

突然変異率などのパラメータや適合度の計算に必要な評価関数は、進化的アルゴリズムがうまくいくためには非常に重要な要素です。
しかしながら、これらの決め方ははっきりとは確立されていません。

そのため、試行錯誤により職人技的に定めているのが現状のようです。


#### 最適解であることが保証されない

遺伝的アルゴリズムによって求めた解は、常に最適な解であるとは限りません。
例えば、初めのころの世代に非常に適合値が高い個体が存在していた場合には、その個体の遺伝子が急速に全体に広がってしまうことで多様性が失われてしまいます。

結果として、本来の最適な解を得ることができなくなります。






## 遺伝的アルゴリズムの考え方から得られる教訓

最後に、遺伝的アルゴリズムの仕組みから、私たちが人生を生きる上で次のような教訓を得ることができると考えました。

#### 未知の事柄に積極的にチャレンジすることの重要性

遺伝的アルゴリズムでは局所解の脱出を目標として、「突然変異」によって遺伝子をランダムに変化させます。

人生を生きる上でも、安心して生活することのできる「コンフォートゾーン」に留まるのではなく、未知の事柄に積極的にチャレンジすることが充実した人生を生きる上で重要であると考えられます。


#### 異なるバックグラウンドを持つ人との交流の重要性
遺伝的アルゴリズムにおいては、それぞれの世代の母集団内での多様性を持たせることが最適解を得るために不可欠です。このことから、人生においても異なるバックグラウンドを持つ人との積極的な交流が重要であると考えられます。


#### 状況の変化に対応することの重要性
遺伝的アルゴリズムでは、それぞれの個体がどれだけ現在の環境に適応しているのか表す適応度が重要な指標です。

私たちの生活でも、状況が刻々と変化していく中でどのような行動をとれば周囲の状況に適応することができるか考えることが重要であると考えられます。

## まとめ

遺伝的アルゴリズムには次のような特徴があります。

- 生物の進化の過程を模倣したアルゴリズム

- 幅広い問題に活用できる一方、パラメータの決め方が確立されていないなどの課題もある

また、遺伝的アルゴリズムは次のような問題に対して応用されています。

- 株式のシステムトレードでの売買ルールの作成

- 工場の最適な生産計画の作成

最後に遺伝的アルゴリズムの概念から、私たちが社会を生きる上で次のようなことが重要であると考えました。

- 未知の事柄に挑戦すること

- 異なるバックグラウンドを持つ人との交流

- 現在の状況への適応


## 参考　ボラティリティのキャリブレーションへの適用例
最後に、BSモデルでのボラティリティのキャリブレーションへの遺伝的アルゴズムの適用例と実装を説明します。
(参考: [ON THE CALCULATION OF IMPLIED VOLATILITY USING A GENETIC
ALGORITHM](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://journal.uii.ac.id/Snati/article/download/1511/1292/1351&ved=2ahUKEwi03O3ZnNeFAxW-Z_UHHbO0AMwQFnoECDsQAQ&usg=AOvVaw0lky-5F9RffBQ9jufIcYRq))

まず、BSモデルについて説明します。
時点tでの株価S_tは次のような方程式に従うとします:

![alt text](bs_1.png)

<!-- $$d S_t = r S_t dt + \sigma S_t dW_t \\
(r: \text{(リスクフリー)金利}, \sigma: \text{株価のボラティリティ})
$$ -->

<!-- ここで、$r$, $\sigma$はそれぞれ定数とします。

このとき、満期$T$でのオプション価格は次の式で表されます。(以下、この式をBS式とよびます) -->
ただし、


![alt text](bs_2.png)


<!-- $$C^{BS} = S_0 N(d_1) - K e^{-rT} N(d_2)$$  -->
<!-- ただし、
$$K: \text{満期$T$での行使価格}$$
$$N(x) = \int_{-\infty}^x \frac{1}{\sqrt{2\pi}} \exp{(-\frac{1}{2}x^2)}dx$$
$$d_1 = \frac{\log{\frac{S_0}{K}} + (r + \frac{\sigma^2}{2}T)}{\sigma \sqrt{T}}$$

$$d_2 = d_1 - \sigma \sqrt{T}$$  -->

最小化する目的関数を次のように設定します。
![alt text](f_sigma.png)
<!-- $$ f(\sigma) = |C_i^{\text{BS}}(\sigma) - C_i^{(*)}|\\
(C_i^{\text{BS}}(\sigma): \text{ボラティリティが$\sigma$の場合のBS式によるオプション価格}) \\
(C_i^{(*)}: \text{オプションの市場価格})$$ -->

次のアルゴリズムにより、モデルに使用するボラティリティを求めます

![](alg_introduction.png)

<!-- 世代数を$g$, 各世代での個体数を$n$とする
1. 初期母集団$\sigma^{0} = [\sigma_1^{0}, \sigma_2^{0}, \cdots, \sigma_n^{0}]$をランダムに選ぶ
   
以下の2\~3の操作により、母集団$\sigma^{i-1}$から母集団$\sigma^i$を作り出す操作を$i = 1, 2 \cdots, g$に渡って繰り返す:

2. 母集団$\sigma^{i-1}$の各要素$\sigma_l^{i-1}\,(l = 1,2,\cdots, n)$について誤差関数$f(\sigma_l^{i-1})$を計算して、誤差の小さいものから順に一定数を選択する。

3. 選択した個体の中から一定の割合で、交叉、突然変異を行う。選択、交叉、突然変異をした個体を次の世代$\sigma^i$として、2に戻る。

4. 2\~3の操作を繰り返して、最終的に母集団$\sigma^g$を得ることができた。$\sigma^g$の各要素について目的関数$f(\sigma_l^g)\,(l=1,2,\cdots, n)$を計算して、もっとも誤差が小さい個体をモデルに使用するボラティリティとする。  -->


### 実装例
```
import numpy as np
from scipy.stats import norm

#BS式
def european_option_price(S, K, r, sigma, T):
    d1 = (np.log(S / K) + (r + 0.5 * sigma ** 2) * T) / (sigma * np.sqrt(T))
    d2 = d1 - sigma * np.sqrt(T)
    call_price = S * norm.cdf(d1) - K * np.exp(-r * T) * norm.cdf(d2)
    return call_price

# 最小化する対象の目的関数：
#予測価格と実際の価格のabsolute error
def absolute_error_function(predicted_price, actual_price):
    return np.abs(predicted_price - actual_price)

# 遺伝的アルゴリズムで最適な価格を見つける関数
def genetic_algorithm(S, K, r, T, actual_price, population_size=1000, generations=1000, crossover_rate = 0.1, mutation_rate = 0.05):
    # 初期個体群の生成
    population = np.random.uniform(0, 1, size=(population_size,))
    
    for generation in range(generations):
        # 市場価格との絶対誤差が小さい⇔適応度が大きい　と定める
        predicted_prices = european_option_price(S, K, r, population, T)
        absolute_error_list = absolute_error_function(predicted_prices, actual_price)
        
        # 選択、交叉、突然変異に用いる個体数
        crossover_num = int(population_size*crossover_rate)
        mutation_num = int(population_size*mutation_rate)
        select_num = population_size - crossover_num - mutation_num 

        # 選択：適応度が高い(誤差が小さい)個体を残す
        selected_indices = np.argsort(absolute_error_list)[:select_num]
        selected_population = population[selected_indices]
        
        # 交叉：選択された個体同士を組み合わせて新しい個体を生成  
        offspring = np.array([np.random.choice(selected_population, 2) for _ in range(crossover_num)])
        offspring = np.mean(offspring, axis=1) #2つの親の平均をとって子を作成
        # 突然変異：ランダムに個体を変異させる
        mutation_list = np.random.uniform(0, 1, size=(mutation_num,))
        # 次世代の生成
        population = np.concatenate((selected_population, offspring, mutation_list))
        
        
    last_predicted_price = european_option_price(S, K, r, population, T)
    last_error = absolute_error_function(last_predicted_price, actual_price)
    
    # 最適解の選択
    last_best_index = np.argmin(last_error)
    last_best_volatility = population[last_best_index]
    return last_best_volatility

# パラメータの設定
S = 100  # 現在の株価
K = 105  # 行使価格
r = 0.05  # 利子率
true_sigma = 0.3 # ボラティリティ
T = 1  # オプションの満期

# 実際の価格(ここではsigma=0.3のもとでのBS価格を市場価格とする)
actual_price = european_option_price(S, K, r, true_sigma, T)

# 遺伝的アルゴリズムで求められたボラティリティの値
genetic_volatility = genetic_algorithm(S, K, r, T, actual_price)
print("GAで求めたボラティリティ:", genetic_volatility)
```


<!-- ### メモ
- 種という言葉を使うのかが文献によって異なる。種という言葉を使わない場合は
種は1つでその中に様々な遺伝子を持った個体が存在するということを仮定していそう
種という言葉を使わないほうが説明しやすそうなので後者に合わせる
(後者の説明の例 https://www.robot.t.u-tokyo.ac.jp/dcm/lec/lec05.pdf)

- 選択、交叉、突然変異の手順に関して、
世代全体が$N$個の個体からなるとするとき、選択によって、適合率が高い個体が増えるように
重複込みで$N$個選んで、その中の個体に対して交叉、突然変異といった操作を適用することで$N$個の次世代を作成するということでよさそう?
参考:https://www.robot.t.u-tokyo.ac.jp/dcm/lec/lec05.pdf
https://sys.ci.ritsumei.ac.jp/~sin/Paper/file/doctor_watanabe20021206.pdf

todo: 社会にどのように生かすことができるか -->


<!-- 


### Todo
- 一文の長さを短くする

フックとまとめを入れる



## 参考
- 橋本文彦「遺伝的アルゴリズムの基礎」
https://www.econ.osaka-cu.ac.jp/~hashimo/Genetic.pdf


- 渡邉真也「遺伝的アルゴリズムによる多目的最適化に関する研究」 <!--3章の説明がわかりやすい -->
<!-- https://sys.ci.ritsumei.ac.jp/~sin/Paper/file/doctor_watanabe20021206.pdf

- 多点探索型アルゴリズムの基礎と最前線　永田　裕一   <!-- 遺伝的アルゴリズムの手順の解説もわかりやすい -->
  <!-- https://orsj.org/wp-content/corsj/or58-12/or58_12_708.pdf



- 
https://www.cis.twcu.ac.jp/~asakawa/MathBio2010/lesson11/ -->

<!-- -
https://www.robot.t.u-tokyo.ac.jp/dcm/lec/lec05.pdf

- 
http://www.iba.t.u-tokyo.ac.jp/rs/ga.html

- 
https://www.jstage.jst.go.jp/article/jjsai/7/1/7_26/_pdf

- introduction to genetic algotithm
https://www.obitko.com/tutorials/genetic-algorithms/japanese/index.php -->

<!-- - 
https://www.kansai-u.ac.jp/reed_rfl/archive/21_2.php

- 応用例(ダイクストラ法)
https://www.kochi-tech.ac.jp/library/ron/2000/info/1010369.pdf

- 巡回セールスマン問題への応用の紹介
https://core.ac.uk/download/pdf/59264983.pdf

- システムトレードへの応用例
  file:///C:/Users/%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0%E3%82%BA%E6%AB%9B%E7%94%B0%E5%8B%87%E4%BB%8B/Downloads/1151002%20(2).pdf
 

- オプション価格のプライシングへの適用
  https://pdf.sciencedirectassets.com/280203/1-s2.0-S1877050922X00021/1-s2.0-S1877050922001715/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJGMEQCICUjahdQt6sL8SL2W%2B23mnk0gMBaQfZzTrWnnNehnopOAiBwXJA9ykp21GaFTEEdr0OACf80HVXmGLRPmsgQ8Vj%2FWiqzBQgQEAUaDDA1OTAwMzU0Njg2NSIMg13%2FTXuLeknAf6p4KpAFzgr2oQA7kQJK%2FDv3fMTUwg0KHWgZ42sQh2ffaaBps0qm2D1pxUS%2BUWCjOs0xtJMTGwF88JjHN5FE3xL1sPGsiemkw6b1i3p2CggIT7YmUMMVjbRjNglKU1s68ImgoquzrYTHQZn9pBhafjH5ZB4lcVR2f5%2FJtSdTVMvjsDDGoG7CiAnZmf4Zb754OuoxBpqvRz7wfyCScTcSRBrHbVRLntneq8olhy2dtSczSFD1LuerYJJpqym%2B3RdL3ZMeIkKcN8d9%2B0dXIO%2BkemtTWyqohlXFTneAc8cJLO4ELtvcJLyfVKLC66cMaptT5IamCjXal0W157xUCYjSBEtsgGmvO8q3KN6npyF2qNgS%2BIOgxpmejFDCl3rQYpuzsZ4tb6yhKwbqFPZVXp1YjY%2Babmo86wcqO8C4K027OtEM3DoQ4x6q7YGNfPP7fO7UWh%2Ff61U5Yr41tcUORXnFEPf6ApO%2BR%2Bot6Z0eY5fmpHQ6CCB0g6KEQlkUdkiUSb7HvQbIIYOkChxO7Yf%2BNoecubJkr8KZeY6YWzW5eHL5%2F%2Bj5BhZv6ya%2Bm8qZIyw0DTm8zRMu6kKJqefuehup42P8giAwBsyYA54BWkIKQeEm%2B6FNYdDbfRHC2OcnaiTCeD3ha1NjjemLkEIsrNkm7XCI3kAijS4SH6hL29vrW%2FmhaIZS6PxRKT%2Bohh%2Fe3HPkCp1PorAUfpBqBeVI0ci0Itj8U8fohDBdQNyrKajnN8Qqbpc4idBIy5n4vGBS%2BAJ%2FLkYirHWbTSGh%2F%2FMSITCBOuYP5a%2B3T5IQNqWv95n33XJbp5ChyP79Q8K2ERzyEDH1olic%2BqPES5pywKlueWtpuQ7%2Fujv%2FtLbSkrP8iLFzc4%2F4Q93MCuU6inAwlqaIsQY6sgGol8ErBpP46hb%2FmM7ONDxZ%2BlcSNqx7bVdgk%2FvlSNQavKuBP9M2FrAnY1CnttYqPYskPlB7O6KH1AHo66gNVsae5Kd9se%2FzgAAWTzgMUMPplmQ4YgwiSjVnNTgm0bChneCwlIovrZo80npV5K6QhwlPdfZuzZIYKOXkMHApUu9mbd8kc4P3sED%2BGsqVkkB2exyEk5o8zeHoKS76ZDJUgTUIV8WuXkCAJmALYR%2FmpjBoJLUq&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240419T072539Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYRW7VCUNN%2F20240419%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=b582b35fb5421d2d1d582bfd2fb8983fb7f7f5481e7be96dd05ff3c26c451fdc&hash=9bcc1751d49377e20fdad2b65c4024023bca68e78ea2839fced296374f23af9b&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S1877050922001715&tid=spdf-55877353-3718-4cc1-b4cd-de0fd5663ff5&sid=62a178cc80b016463449b0e6ff8380d3bb74gxrqa&type=client&tsoh=d3d3LnNjaWVuY2VkaXJlY3QuY29t&ua=0c125851590c535e585c&rr=876b297298fd7821&cc=jp

- 実装付き
  https://www.m-hikari.com/ams/ams-2014/ams-65-68-2014/ackoraprahAMS65-68-2014-1.pdf

  https://www.m-hikari.com/ams/ams-2014/ams-141-144-2014/ackoraprahAMS141-144-2014-1.pdf

- キャリブレーション　実装付き
- https://uu.diva-portal.org/smash/get/diva2:729886/FULLTEXT01.pdf

- GAを使ったキャリブレーションについて詳しく書いてある
  https://core.ac.uk/download/pdf/39670812.pdf
 --> 


