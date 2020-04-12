# navgridjs
JavaScript version of NavGrid. 

## 1. About NavGrid ##
### (1) What is NavGrid ## 

NavGrid is a tool for coding/decoding geographical grids. It can

+ decode a gridID - generate a grid geometry according to the gridID,
+ encoding a grid - calculate the gridID of one legal grid geometry,
+ calculate the envelope of one grid,
+ calculate the centroid of one grid,
+ calculate the area of one grid,
+ calculate the width and length of one grid,
+ and, generate grids within or touches one specified region. 

Currently there are implementations in pl/pgsql and Java. 

### (2) Why use NavGrid ##

1. A multi-scale gridset for spatial analysis

NavGrid provides a family of world-wide grids of *whole* width/height.

1. To access telecom population data
NavGrid provides access to a world-wide grid system. This grid system is adopt by many large data companies in China, for example, China Telecom, China Unicom,

### (3) Conceptions of NavGrid ###

There are only two important conceptions, *grid* and *gridID*.

1. Grid

One **grid** is a square of *specified* **size** (width/height) in the WGS 1984 coordinate system. 

+ Every grid is unique, so is gridID

Every grid in the NavGrid system has a unique ID, named as *gridID*. A gridID is an `unsigned int64` which conbines the coordinates of the start point and size of the correspoinding grid. For example, gridID 123450034565040 means the grid start at (123.45, 34.565), and the size is 0.005 degree (the rule will be explained later). This means, given one grid, we can get the gridID, vice versa. 

*As seen, the gridID is compact (8 bytes) and human readable (the coordinates are in directly shown in the numbers). It is much friendly to users. Besides, the sizes of grids are carefully choosen, the numbers are comfortable to human.*

+ Grids of them same size make a [partition](https://www.mathwords.com/p/partition_of_a_set.htm) of the longitude-latitude system.

In NavGrid's design, grids of the same size can fully cover the plane `{(x,y)|x in [-180,180], y in [-90,90]}`. 

For one grid of size *s*, its' **start point** is the coordinates of the bottom-left corner. Mathematically it contains points on the left and bottom borders, but do not contains points on the right or the top borders, that is, if its' start point is (x0,y0), the grid can be defined as `{(x,y)|x in [x0,x0+s), y in [y0,y0+s)}`. Then, with a proper size and start point, the grids one by one can cover the whole plane without overlaps.

*Precisely, there should exists a patch of its' mathematical definition. In the plane, the top and right grids should contains the points in the top and right border line of the plane, or these points will be lost. It will be fixed in the next version of implementation.*

+ Grids of different size can be *aligned*

(0,0) is a common start point for grids of different sizes. Predefined grid sizes are:

size (degree) | size code | approximate distance*
--------------| --------- | ------
0.0001        | 0         | 10 meters
0.001         | 1         | 100 meters
0.002         | 2         | 200 meters
reserved      | 3         | reserved
0.005         | 4         | 500 meters
0.01          | 5         | 1 kilometer
0.02          | 6         | 2 kilometers
reserved      | 7         | reserved
0.05          | 8         | 5 kilometers
0.1           | 9         | 10 kilometers

`*1 degree is treated as 100 kilometers here, it is not precise, but easy to understand and memorize, and the error in most countries is not unbearable for people who imaging the size of a grid.`

The largest size is 0.1 degree, and the smallest size is 0.0001 degree. In web map applications, 10-meter-grid is very fine grained for both visualization and analysis, because GPS devices in our phone, tables can hardly get a much better precision. And 10-kilometer-grid is a quite large cell, because the area is about 100 square kilometers, a few grids can cover a town and dozens of grids can cover a city.

These sizes are quite like the face value of cashes, all of them can be aligned at n*0.1 degree, and there are more align ways. Binary divisions are often used in web map datasets, but the precise size of a tile (a grid with map data) might have a lot of significant digits, difficult to store in human memory. So here we use the 1-2-5-10 size series which are very familiar in our everyday life.

The *size code* is a decimal digit represents the size of a grid, it will be used in *gridID*. 2 codes are reserved for future applications. 

 2. gridID
 
 gridID is a bigint number of the form `XXXxxxxYYyyyyZR`, where
 
 + XXXxxxx is the longitude of the start point of a grid, it keeps 4 digits after the decimal point, that is enough for the so called 10-meter-grid.  And it keeps *at most* 3 (0 to 3) digits  before the dicimal point, because the longitude needs at most 3 decimal digits (without the sign).
 
 + YYyyyy is the latitude of the start point of a grid, it keeps 3 digits before the decimal point and 4 after it.
 
 + Z is the size code.
 
 + R is the quadrant code, 0 stands for the first quadrant, 1 for the second, and so on. So we do not have to add sign characters to gridID.
 
 *There are some compatible ways to extend gridID, however not now.*

## 2. Why use navgridjs

With the JavaScript version of NavGrid, it would be much easier to use its functions in browsers and nodejs.

Formerly there are pl/pgsql and Java version of NavGrid. They are both back-end (or server side) tools. To use NavGrid in your web applications, you have to run PostgreSQL and a web wrapper or use a Java web server, that means, you can not use it without a web server. It might be okay for large applications, for example a B/S geo-visualization system, but nonsense for light-weighted web map applications, for example a map of bus stations or ATMs. With navgridjs, there is an elegant way to finish the work.  

## 3. API reference

#### properties of grid

#### coordinates to gridID

#### gridID to geometry

#### virtual grids




