# Overview
Weibo is a Chinese microblogging website. It is one of the most popular sites in China, with a market penetration similar to the Twitter. “Weibo” is a Chinese word for “microblog”. User behaviors such as forwarding, commenting and liking are important factors that can be used to estimate the quality of a certain weibo and implement the recommendation and feed controlling strategy. In this competition, participants are required to predict the forwarding, commenting and liking amount of a weibo based on the historical interaction data.

The difference of each behavior to a certain weibo is calculated by:

<a href="https://www.codecogs.com/eqnedit.php?latex=deviation_f&space;=&space;\frac{count_{fp}&space;-&space;count_{fr}}{count_{fr}&space;&plus;&space;5}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?deviation_f&space;=&space;\frac{count_{fp}&space;-&space;count_{fr}}{count_{fr}&space;&plus;&space;5}" title="deviation_f = \frac{count_{fp} - count_{fr}}{count_{fr} + 5}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=deviation_c&space;=&space;\frac{count_{cp}&space;-&space;count_{cr}}{count_{cr}&space;&plus;&space;3}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?deviation_c&space;=&space;\frac{count_{cp}&space;-&space;count_{cr}}{count_{cr}&space;&plus;&space;3}" title="deviation_c = \frac{count_{cp} - count_{cr}}{count_{cr} + 3}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=deviation_l&space;=&space;\frac{count_{lp}&space;-&space;count_{lr}}{count_{lr}&space;&plus;&space;3}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?deviation_l&space;=&space;\frac{count_{lp}&space;-&space;count_{lr}}{count_{lr}&space;&plus;&space;3}" title="deviation_l = \frac{count_{lp} - count_{lr}}{count_{lr} + 3}" /></a>

where count_{fp}
