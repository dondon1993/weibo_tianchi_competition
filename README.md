# Overview
Weibo is a Chinese microblogging website. It is one of the most popular sites in China, with a market penetration similar to the Twitter. “Weibo” is a Chinese word for “microblog”. User behaviors such as forwarding, commenting and liking are important factors that can be used to estimate the quality of a certain weibo and implement the recommendation and feed controlling strategy. In this competition, participants are required to predict the forwarding, commenting and liking amount of a weibo based on the historical interaction data.

The difference of each behavior to a certain weibo is calculated by:

<a href="https://www.codecogs.com/eqnedit.php?latex=deviation_f&space;=&space;\frac{count_{fp}&space;-&space;count_{fr}}{count_{fr}&space;&plus;&space;5}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?deviation_f&space;=&space;\frac{count_{fp}&space;-&space;count_{fr}}{count_{fr}&space;&plus;&space;5}" title="deviation_f = \frac{count_{fp} - count_{fr}}{count_{fr} + 5}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=deviation_c&space;=&space;\frac{count_{cp}&space;-&space;count_{cr}}{count_{cr}&space;&plus;&space;3}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?deviation_c&space;=&space;\frac{count_{cp}&space;-&space;count_{cr}}{count_{cr}&space;&plus;&space;3}" title="deviation_c = \frac{count_{cp} - count_{cr}}{count_{cr} + 3}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=deviation_l&space;=&space;\frac{count_{lp}&space;-&space;count_{lr}}{count_{lr}&space;&plus;&space;3}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?deviation_l&space;=&space;\frac{count_{lp}&space;-&space;count_{lr}}{count_{lr}&space;&plus;&space;3}" title="deviation_l = \frac{count_{lp} - count_{lr}}{count_{lr} + 3}" /></a>

where count<sub>*p</sub> is the prediction value and count<sub>*r</sub> is the target value with * being f(forward), c(comment) or l(like) respectively.

The preciison for each weibo is:

<a href="https://www.codecogs.com/eqnedit.php?latex=precision_i&space;=&space;1&space;-&space;0.5&space;\times&space;deviation_f&space;-&space;0.25&space;\times&space;deviation_c&space;-&space;0.25&space;\times&space;deviation_l" target="_blank"><img src="https://latex.codecogs.com/gif.latex?precision_i&space;=&space;1&space;-&space;0.5&space;\times&space;deviation_f&space;-&space;0.25&space;\times&space;deviation_c&space;-&space;0.25&space;\times&space;deviation_l" title="precision_i = 1 - 0.5 \times deviation_f - 0.25 \times deviation_c - 0.25 \times deviation_l" /></a>

The global precision:

<a href="https://www.codecogs.com/eqnedit.php?latex=precision&space;=&space;\frac{\sum_{1}^{n}(count_i&space;&plus;&space;1)\times&space;sgn(precision_i&space;-&space;0.8)}{\sum_{1}^{n}(count_i&space;&plus;&space;1)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?precision&space;=&space;\frac{\sum_{1}^{n}(count_i&space;&plus;&space;1)\times&space;sgn(precision_i&space;-&space;0.8)}{\sum_{1}^{n}(count_i&space;&plus;&space;1)}" title="precision = \frac{\sum_{1}^{n}(count_i + 1)\times sgn(precision_i - 0.8)}{\sum_{1}^{n}(count_i + 1)}" /></a>

where sgn(x) is a modified signal function defined as sgn(x)=1 if x > 0 and sgn(x) = 0 if x<=0.
count<sub>i</sub> is the sum amount of forwarding, commenting and liking of ith weibo. count<sub>i</sub> take the value of 100 if count<sub>i</sub> is larger than 100.
