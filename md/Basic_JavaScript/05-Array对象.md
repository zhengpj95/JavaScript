# Array 对象

## 1. 构造函数

Array构造函数有一个很大的缺陷，就是不同的参数，会导致它的行为不一致。
因此，不建议使用它生成新数组，直接使用数组字面量是更好的做法。

> var arr = [1, 2, ...]

## 2. 静态方法

`Array.isArray()` 方法返回一个布尔值，表示参数是否为数组。
`typeof`运算符只能显示数组的类型是`Object`，而`Array.isArray`方法可以识别数组。

## 3. 实例方法

### 3.1 valueOf(), toString()

valueOf方法是一个所有对象都拥有的方法，表示对该对象求值。
不同对象的valueOf方法不尽一致，数组的valueOf方法返回数组本身。
toString方法也是对象的通用方法，数组的toString方法返回数组的字符串形式。

### 3.2 push(), pop()

push方法用于在数组的末端添加一个或多个元素，并返回添加新元素后的**数组长度**。注意，该方法会改变原数组。
pop方法用于删除数组的最后一个元素，并返回**该元素**。注意，该方法会改变原数组。
push和pop结合使用，就构成了“后进先出”的栈结构（stack）。

### 3.3 shift(), unshift()

shift()方法用于删除数组的第一个元素，并返回**该元素**。注意，该方法会改变原数组。
push()和shift()结合使用，就构成了“先进先出”的队列结构（queue）。
unshift()方法用于在数组的第一个位置添加元素，并返回添加新元素后的**数组长度**。注意，该方法会改变原数组。

- push() 和 unshift()   ： 添加一个或多个新元素，并返回新数组的数组长度
- pop() 和 shift() ： 删除元素，并返回删除的元素

### 3.4 join()

join()方法以指定参数作为分隔符，将所有数组成员连接为一个字符串返回。如果不提供参数，默认用逗号分隔。
如果数组成员是undefined或null或空位，会被转成空字符串。
通过call方法，这个方法也可以用于字符串或类似数组的对象。

```javascript
Array.prototype.join.call('hello', '-')
// "h-e-l-l-o"

var obj = { 0: 'a', 1: 'b', length: 2 };
Array.prototype.join.call(obj, '-')
// 'a-b'
```

### 3.5 concat()

concat方法用于多个数组的合并。返回新的数组，原数组不变。
除了数组作为参数，concat也接受其他类型的值作为参数，添加到目标数组尾部。

### 3.6 reverse()

reverse方法用于颠倒排列数组元素，返回改变后的数组。注意，该方法将改变原数组。

### 3.7 slice()

slice方法用于提取目标数组的一部分，返回一个新数组，原数组不变。
它的第一个参数为起始位置（从0开始），第二个参数为终止位置（但该位置的元素本身不包括在内）。如果省略第二个参数，则一直返回到原数组的最后一个成员。

如果slice方法的参数是负数，则表示倒数计算的位置。
`-1`表示倒数计算的第一个位置。

如果第一个参数大于等于数组长度，或者第二个参数小于第一个参数，则返回空数组。

slice方法的一个重要应用，是将类似数组的对象转为真正的数组。

```javascript
Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 })
// ['a', 'b']

Array.prototype.slice.call(document.querySelectorAll("div"));
Array.prototype.slice.call(arguments);
```

### 3.8 splice()

splice方法用于删除原数组的一部分成员，并可以在删除的位置添加新的数组成员，返回值是被删除的元素。注意，该方法会改变原数组。
splice的第一个参数是删除的起始位置（从0开始），第二个参数是被删除的元素个数。如果后面还有更多的参数，则表示这些就是要被插入数组的新元素。
起始位置如果是负数，就表示从倒数位置开始删除。
如果只是单纯地插入元素，splice方法的第二个参数可以设为0
如果只提供第一个参数，等同于将原数组在指定位置拆分成两个数组。

### 3.9 sort()

sort方法对数组成员进行排序，默认是按照**字典顺序**排序。排序后，原数组将被改变。
sort方法不是按照大小排序，而是按照字典顺序。也就是说，数值会被先转成字符串，再按照字典顺序进行比较。
如果想让sort方法按照自定义方式排序，可以传入一个函数作为参数。
sort的参数函数本身接受两个参数，表示进行比较的两个数组成员。如果该函数的返回值大于0，表示第一个成员排在第二个成员后面；其他情况下，都是第一个元素排在第二个元素前面。

```javascript
[].sort(function (a, b) {
  return a - b;// 从小到大排序
})
```

### 3.10 map()

map方法将数组的所有成员依次传入参数函数，然后把每一次的执行结果组成一个新数组返回。
map方法接受一个函数作为参数。该函数调用时，map方法向它传入三个参数：当前成员、当前位置和数组本身。
map方法还可以接受第二个参数，用来绑定回调函数内部的this变量，this指向第二个参数arr。

```javascript
var arr = ['a', 'b', 'c'];
[1, 2].map(function (e) {
  return this[e];  // this指向arr
}, arr)
// ['b', 'c']
```

如果数组有空位，map方法的回调函数在这个位置不会执行，会跳过数组的空位。
map方法不会跳过undefined和null，但是会跳过空位。

### 3.11 forEach()

forEach方法与map方法很相似，也是对数组的所有成员依次执行参数函数。但是，forEach方法不返回值，只用来操作数据。
forEach的用法与map方法一致，参数是一个函数，该函数同样接受三个参数：当前值、当前位置、整个数组。
forEach方法也可以接受第二个参数，绑定参数函数的this变量。
注意，forEach方法无法中断执行，总是会将所有成员遍历完。如果希望符合某种条件时，就中断遍历，要使用for循环。
forEach方法不会跳过undefined和null，会跳过数组的空位。

### 3.12 filter()

filter方法用于过滤数组成员，满足条件的成员组成一个新数组返回。
它的参数是一个函数，所有数组成员依次执行该函数，返回结果为true的成员组成一个新数组返回。该方法不会改变原数组。
filter方法的参数函数可以接受三个参数：当前成员，当前位置和整个数组。
filter方法还可以接受第二个参数，用来绑定参数函数内部的this变量。

### 3.13 some(), every()

返回一个布尔值，表示判断数组成员是否符合某种条件。
它们接受一个函数作为参数，所有数组成员依次执行该函数。该函数接受三个参数：当前成员，当前位置和整个数组，然后返回一个布尔值。
some方法是只要一个成员的返回值为true，则整个some方法的返回值就是true。
every方法是所有成员的返回值都是true，整个every方法才返回true。
对于空数组，some方法返回false，every方法返回true，回调函数都不会执行。
some和every方法还可以接受第二个参数，用来绑定参数函数内部的this变量。

### 3.14 reduce(), reduceRight()

reduce方法和reduceRight方法依次处理数组的每个成员，最终累计为一个值。它们的差别是，reduce是从左到右处理（从第一个成员到最后一个成员），reduceRight则是从右到左（从最后一个成员到第一个成员），其他完全一样。

```javascript
[1, 2, 3, 4, 5].reduce(function (a, b) {
  console.log(a, b);
  return a + b;
})
// 1 2
// 3 3
// 6 4
// 10 5
//最后结果：15
```

reduce方法和reduceRight方法的第一个参数都是一个函数。该函数接受以下四个参数:

- 累积变量，默认为数组的第一个成员
- 当前变量，默认为数组的第二个成员
- 当前位置（从0开始）
- 原数组

这四个参数之中，只有前两个是必须的，后两个则是可选的。

如果要对累积变量指定初值，可以把它放在reduce方法和reduceRight方法的第二个参数。

### 3.15 indexOf(), lastIndexOf()

indexOf方法返回给定元素在数组中第一次出现的位置，如果没有出现则返回-1。
indexOf方法还可以接受第二个参数，表示搜索的开始位置。

lastIndexOf方法返回给定元素在数组中最后一次出现的位置，如果没有出现则返回-1。

注意，这两个方法不能用来搜索NaN的位置，即它们无法确定数组成员是否包含NaN。
这是因为这两个方法内部，使用严格相等运算符（===）进行比较，而NaN是**唯一一个**不等于自身的值。
