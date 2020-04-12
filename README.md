# navgridjs
JavaScript version of NavGrid. 

## About NavGrid ##
### What is NavGrid ## 

NavGrid is a tool for coding/decoding geographical grids. It can

+ decode a gridID - generate a grid geometry according to the gridID,
+ encoding a grid - calculate the gridID of one legal grid geometry,
+ calculate the envelope of one grid,
+ calculate the centroid of one grid,
+ calculate the area of one grid,
+ calculate the width and length of one grid,
+ and, generate grids within or touches one specified region. 

Currently there are implementations in pl/pgsql and Java. 

### Why use NavGrid ##

1. A multi-scale gridset for spatial analysis

NavGrid provides a family of world-wide grids of *whole* width/height.

1. To access telecom population data
NavGrid provides access to a world-wide grid system. This grid system is adopt by many large data companies in China, for example, China Telecom, China Unicom,

### Definitions of NavGrid ###

1. Grid

One **grid** is a square of *specified* **size** (width/height) in the WGS 1984 coordinate system. 

+ Grids of them same size make a [partition](https://www.mathwords.com/p/partition_of_a_set.htm) of the longitude-latitude system.

In NavGrid's design, grids of the same size can fully cover the plane `{(x,y)|x in [-180,180], y in [-90,90]}`. 

For one grid of size *s*, its' **start point** is the coordinates of the bottom-left corner. Mathematically it contains points on the left and bottom borders, but do not contains points on the right or the top borders, that is, if its' start point is (x0,y0), the grid can be defined as {(x,y)|x in [x0,x0+s), y in [y0,y0+s)}.



 

## Why use navgridjs

With the JavaScript version of NavGrid, it would be much easier to use its functions in browsers and nodejs.

Formerly there are pl/pgsql and Java version of NavGrid. They are both back-end (or server side) tools. To use NavGrid in your web applications, you have to run PostgreSQL and a web wrapper or use a Java web server, that means, you can not use it without a web server. It might be okay for large applications, for example a B/S geo-visualization system, but nonsense for light-weighted web map applications, for example a map of bus stations or ATMs. With navgridjs, there is an elegant way to finish the work.







