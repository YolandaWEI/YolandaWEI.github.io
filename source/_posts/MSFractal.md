---
title: TP1 Mandelbrot Set Fractal
tags:
  - Node
  - JS
  - TP
categories:
  - Event-drive Asynochronous Programming
images: header_images/mandelbort(1).png
mathjax: true
comments: true
abbrlink: a353
date: 2018-10-08 18:28:58
---
<p></p><!--more-->



# Prepare your project Node.js
Install node from the official website: https://nodejs.org.
Create a new folder and add an app.js script, whose contents are as follows:
```JavaScript
console.log("Hello World")
```
Open a new terminal and locate it in your project folder.
Launch your application with the command:

node app.js

We will configure the project in npm, thanks to the command `npm init`. You will answer the different questions.

# Complex numbers
We will create a small class to encapsulate and manipulate complex numbers, which will meet the needs of the TP suite.

Resume and complete the following Complex class. None of the methods of the Complex class should alter the fields `this.re` and `this.im`. Complex objects that they return are therefore always new instances.

```JavaScript
class Complex {

constructor (re, im) {
this.re = re
this.im = im
}

/*
* @return Complex le carré du complexe this
*/
square () { /*...*/ }

/*
* @param {Complex} second à sommer à this
* @return Complex la somme this + second
*/
sum (second) { /*...*/ }

/*
* @return Number le module du complexe this
*/
module () { /*...*/ }
}
```
To test that the library works:
```JavaScript
var c1 = new Complex(0, 1)
console.log("|c1| = " + c1.module())
console.log("c1 * c1 = " + JSON.stringify(c1.square()))

var c2 = new Complex(1, 0)
console.log("c1 + c2 = " + JSON.stringify(c1.sum(c2)))
```
What should display at runtime:

|c1| = 1
c1 * c1 = {"re":-1,"im":0}
c1 + c2 = {"re":1,"im":1}


## Notes: Complex operation
>A complex number is a number that can be expressed in the form $ z = x + iy $, where $ x $ and $ y $ are real numbers, and $ i $ is a solution of the equation $ x^2 = −1 $. It can also be expressed in polar form as $ z=re^{i\varphi} $.

- The sum
$ z_1+z_2=(x_1+x_2)+i(y_1+y_2) $


{% asset_img coord.jpg $ z = x + iy $ and $ z=re^{i\varphi} $ %}

- The module
In this way, $ r=|z| $, and $ x=rcos\varphi $, $ y=rsin\varphi $. 
So we can get $ |z|=\sqrt{x^2+y^2} $
- The square
$ z^2=(x+iy)^2=x^2+i2xy+(iy)^2=(x^2-y^2)+i2xy $

```JavaScript
//Complex numbers
class Complex {

constructor(re, im) {
this.re = re
this.im = im
return this
}

/*get the square of this complex number
* @method square()
* @return {Complex} the square of this complex
*/
square() {
var squ=new Complex()
squ.re= this.re * this.re - this.im * this.im
squ.im = 2 * this.re * this.im
return squ
}

/*get the sum of this complex number and the second one
* @method sum(second)
* @param second{Complex}  to summon this
* @return {Complex} the sum this + second
*/
sum(second) {
var summ=new Complex()
summ.re = this.re + second.re
summ.im = this.im + second.im
return summ
}

/*get the module of this complex
* @method module()
* @return {Number} the module of this complex
*/
module() {
var Number = Math.sqrt(this.im * this.im + this.re * this.re)
return Number
}
}
```

# Read a number from the console
The [readline module][6] (integrated with node) is used to manipulate flows, read one line at a time. Among these flows, one can access those of the console (process.stdin, process.stdout). Here is what the official documentation gives us:
```JS
const readline = require('readline')

const rl = readline.createInterface({
input: process.stdin,
output: process.stdout
})

rl.question('What do you think of Node.js? ', (answer) => {
// TODO: Log the answer in a database
console.log('Thank you for your valuable feedback:', answer)

rl.close()
})
//https://nodejs.org/api/readline.html#readline_readline
```

To convert a string to a number: `parseInt (string) or Number (string)`
Design a program with the following behavior (question + confirmation):

Size of the image to generate? 128
Choice of dimensions: 128x128px



## Notes: JS readline.Interface module
- When the code is called, the Node.js program does not terminate until `readline.Interface` is closed because the interface is waiting for data to be received in the `input` stream.

```JS
const readline = require('readline')

const rl = readline.createInterface({
input: process.stdin,
output: process.stdout
})

rl.question('Size of the image to generate? ', (size) => {
// TODO: Log the answer in a database
var imsize = size + " x " + size + " px"
console.log('Choice of dimensions:', imsize)
rl.close()
})
```
Compare [require/import/export in JS][7] 
> CommonJS & AMD use require to use module

## Notes: Asyn event
- the second question on console 
```JS
'use strict'
const readline = require('readline')

const rl = readline.createInterface({
input: process.stdin,
output: process.stdout
})



const question1 = () => {
return new Promise((resolve, reject) => {
rl.question('Size of the image to generate? ', (size) => {

//input control
if ((/^\d+$/.test(size)) && (size > 0) && (size <= 1080)) {
console.log('Choice of dimensions:' + size + " x " + size + " px")
//size{String} are all numbers and size is in [1,1080] integer
return resolve(size)
} else {
resolve(question1())
}
})
})
}

const question2 = () => {
return new Promise((resolve, reject) => {
rl.question('Image Name? ', (name) => {
console.log(name+".png on DESKTOP")
return resolve(name)
})
})
}


const main = async () => {
var size = await question1()
var name = await question2()
console.log(name+".png: "+size+" x "+size+" px")

//rl.close()
}

main()
```

# Generate a random image
Thanks to the jimp library, we will be able to create and save images. The dimension will be requested thanks to the previous question.

To install jimp: npm install jimp --save. The dependency is installed and referenced in package.json.

Here are some excerpts from the documentation to manipulate the library:
```JS
const Jimp = require("jimp")

// open a file called "lenna.png"
Jimp.read("lenna.png", function (err, image) {
if (err) throw err
image.write("lena-small-bw.jpg") // save
})

var image = new Jimp(256, 256)

image.getPixelColor(x, y) // colour of that pixel e.g. 0xFFFFFFFF
image.setPixelColor(hex, x, y) // sets the colour of that pixel

// e.g. converts 255, 255, 255, 255 to 0xFFFFFFFF 
Jimp.rgbaToInt(r, g, b, a)

// e.g. converts 0xFFFFFFFF to {r: 255, g: 255, b: 255, a:255} 
Jimp.intToRGBA(hex)

//https://www.npmjs.com/package/jimp
```

- Pixels have 4 components (red, green, blue and alpha). Alpha at 255 equals a fully opaque color, 0 at a transparent color.
- Alpha is of low weight in hexadecimal writing.
- The Math.random() function returns a pseudo random number between 0 and 1 excluded.

Design a `getRandomPixel()` function that returns a hexadecimal color at random, but with an alpha component = 255.

Design the `buildRandomImage (size, name)` function that takes parameters in pixels and the name to save the image to disk. You will call this function inside the callback passed to `readline.question(cb)`.

{% asset_img random.jpg random color image %}

After running your program, you should get an image similar to the example shown here (size: 128px).

## Notes: 
```JS
'use strict'
/* get a random int number in [minNum,maxNum] in hexadecimal String
* @method randomNum()
* @param minNum{Integer}
* @param maxNum{Integer}
* @return {String} (hexadecimal)
*/
function randomNum(minNum, maxNum) {
var num = parseInt(Math.random() * (maxNum - minNum + 1) + minNum, 10)
if (num < 16) {
//if the number is just in one bit, add a 0 in high-order position ("e"->"0e")
num = '0' + num.toString(16) 
} else {
num=num.toString(16)
}
return num; 
}
/* get a random rgb pixel color with 255 alpha 
* @method getRandomPixel()
* @return {String} (hexadecimal and start with '0x')
*/
function getRandomPixel() {
var R = randomNum(0, 255)
var G = randomNum(0, 255)
var B = randomNum(0, 255)
return '0x'+R+G+B+'ff' //with 255 alpha
}

/* get a random picture in size x size px with 255 alpha RGBA PNG on DeskTop
* @method buildRandomPixel()
* @param size
* @param name image name(without)
*/

function buildRandomImage(size, name) {
const DESKTOP_URL = "C:\\Users\\YolandaW\\Desktop\\"
//Generate a random image
const Jimp = require("jimp")
var image = new Jimp(size, size) 
for ( i = 0; i < size ; i++) {
for ( j = 0; j < size; j++) {
image.setPixelColor(parseInt(getRandomPixel()), i, j) // sets the colour of that pixel
}
}
name = name + ".png" //save on Desktop
image.write(DESKTOP_URL+name) //save image  
}

buildRandomImage(256, "image")
```

After combine two question together

```JS
'use strict'
const readline = require('readline')

const rl = readline.createInterface({
input: process.stdin,
output: process.stdout
})

const askImageSize = () => {
return new Promise((resolve, reject) => {
rl.question('Size of the image to generate? ', (size) => {
//input control
if ((/^\d+$/.test(size)) && (size > 0) && (size <= 1080)) {
console.log('Choice of dimensions:' + size + " x " + size + " px")
//size{String} are all numbers and size is in [1,1080] integer
return resolve(size)
} else {
resolve(askImageSize())
}
})
})
}

const askImageName = () => {
return new Promise((resolve, reject) => {
rl.question('Image Name? ', (name) => {
console.log(name+".png on DESKTOP")
return resolve(name)
})
})
}

/* get a random int number in [minNum,maxNum] in hexadecimal String
* @method randomNum()
* @param minNum{Integer}
* @param maxNum{Integer}
* @return {String} (hexadecimal)
*/
function randomNum(minNum, maxNum) {
var num = parseInt(Math.random() * (maxNum - minNum + 1) + minNum, 10)
if (num < 16) {
//if the number is just in one bit, add a 0 in high-order position ("e"->"0e")
num = '0' + num.toString(16)
} else {
num = num.toString(16)
}
return num;
}

/* get a random rgb pixel color with 255 alpha 
* @method getRandomPixel()
* @return {String} (hexadecimal and start with '0x')
*/
function getRandomPixel() {
var R = randomNum(0, 255)
var G = randomNum(0, 255)
var B = randomNum(0, 255)
return '0x' + R + G + B + 'ff' //with 255 alpha
}

/* get a random picture in size x size px with 255 alpha RGBA PNG on DeskTop
* @method buildRandomPixel()
* @param size
* @param name image name(without)
*/

function buildRandomImage(size, name) {
const DESKTOP_URL = "C:\\Users\\YolandaW\\Desktop\\"
//Generate a random image
const Jimp = require("jimp")
var image = new Jimp(size, size)
for (var i = 0; i < size; i++) {
for (var j = 0; j < size; j++) {
image.setPixelColor(parseInt(getRandomPixel()), i, j) // sets the colour of that pixel
}
}
name = name + ".png" //save on Desktop
image.write(DESKTOP_URL + name) //save image  
}


const main = async () => {
var size = await askImageSize()
var name = await askImageName()
buildRandomImage(size, name)
console.log(name + ".png: " + size + " x " + size + " px has been created on DESKTOP")

//rl.close()
}

main()

```

## Notes: different image form PNG、JP
## Notes: read/write image
## Notes: JS comments and How to name method


# A fractal in shades of gray
The sought-after fractal is Mandelbrot's fractal. The color of each pixel is calculated as a function of the divergence rate of a complex recursive sequence.

For each pixel, we set a number $ C = x + iy $, with x and y the coordinates of the standardized pixel between 0 and 1. To normalize, divide the usual coordinates by the size of each dimension.

The recurrence formula is: $ Z_n = (Z_{n-1})^2 + C $

- $ Z_n $ is a rank n term of a complex suite
- $ Z_0  = 0 + i0 = 0 $
- $ C  $ is a complex constant, added to each recursion and linked to the pixel

We consider that the series diverges as soon as the modulus of a term exceeds 2. When a divergence is detected, we return n, the rank from which $ | Z_n | > 2 $. We calculate at most 20 terms: beyond, we suppose that the sequence converges and we return 20.

We give the algorithm in algorithmic language. Translate it as a function `getMandelbrotDivergenceSpeed (Complex): Number`.

<table>
<tr>
<th colspan="2">SpeedDevergence (c: Complex): integer</th>
</tr>
<tr>
<th>Data:</th>
<td>c: Complex, the coordinates of a pixel such as $(c.re, c.im)$ $E [0,1] ^2$</td>
</tr>
<tr>
<th>Local variable:</th>
<td>z: Complex, the term of the sequence $Z$ at rank $n$</td>
</tr>
<tr>
<th>Local variable:</th>
<td>n: integer the currently calculated rank </td>
</tr>
<tr>
<th>Result:</th>
<td>Returns the rank $[0, 19]$ from which the divergence is found or $20$ otherwise</td>
</tr>
</table>

beginning

- $z ← Complex (0, 0)$
- $n ← 0$
- as long as $n <20 $ repeat 
- $z ← z * z + c$
-  if $ z.module ()> 2 $ then return $ n $
- $n ← n + 1$
- end as long as
- return $20$

end

Design a function `coordToComplex (x, y, size): Complex` Very simple that accepts two coordinates (x, y) as well as the size of the image and returns a complex c such that:

- c.re be x normalized between 0 and 1
- c.im is normalized between 0 and 1


Design a `speedToGrey (n): Number` function that takes a divergence rate parameter and returns a fully opaque gray hue in hexadecimal such as:

- n = 0 returns white.
- n = 20 returns black.

Design a `buildGreyFractal (size, name)` function that takes parameters in pixels and the name to save the image to disk.



# See the entire fractal
Currently, you only see a quarter of the fractal. Why? Because we associate with our pixel coordinates strictly positive values.

For example, on an image of 128x128px:

- currently
- the pixel (0,0) is transformed into Complex (0, 0)
- the pixel (128, 128) is transformed into Complex (1, 1)
- in the version that you will improve
- the pixel (0, 0) is transformed into Complex (-1, -1)
- the pixel (64, 64) is transformed into Complex (0, 0)
- the pixel (128, 128) is transformed into Complex (1, 1)
- 
Get such behavior only changing the function coordToComplex (x, y, size).

# Go further on the manipulation of coordinates
If you still have time at the end of the session, you can tackle the following issues:

- choose the zoom to apply
- choose a translation to apply
- choose a rotation to apply
- choose a stretch on each of the dimensions

# A fractal in color
To enjoy your fractal in color, you can use the following function
```JS
function speedToColor (n) {
n = Math.min(n / 50, 1)
let s = 1
let l = 0.6, r, g, b

if (s === 0) {
r = g = b = l // achromatic
} else {
function hue2rgb(p, q, t) {
if(t < 0) t += 1
if(t > 1) t -= 1
if(t < 1/6) return p + (q - p) * 6 * t
if(t < 1/2) return q
if(t < 2/3) return p + (q - p) * (2/3 - t) * 6
return p
}

var q = l < 0.5 ? l * (1 + s) : l + s - l * s
var p = 2 * l - q
r = hue2rgb(p, q, n + 1/3)
g = hue2rgb(p, q, n)
b = hue2rgb(p, q, n - 1/3)
}

return Jimp.rgbaToInt(
Math.round(r * 255),
Math.round(g * 255),
Math.round(b * 255),
255
)
}
```

Of course, this is just one example of functions that can turn speed into color. You can design more original ones if you wish.

## Smooth iter
http://blog.miskcoo.com/2016/10/mandelbrot-set


[6]: https://blog.csdn.net/chy555chy/article/details/52626404
[7]: https://www.cnblogs.com/libin-1/p/7127481.html
