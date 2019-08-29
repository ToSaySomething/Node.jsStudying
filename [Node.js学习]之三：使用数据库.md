# [Node.js 学习]之二  数据库

## **前言**

​		首先是结构化数据，根据定义结构化数据指的是由表结构来表现的数据，相同的行或者列的属性是相同的。因此我们大学的学习的关系型数据库很适用于这类。

​		非结构化数据，指的是数据结构不规则或不完整，没有任何预定义的数据模型，不方便用二维逻辑表来表现的数据，例如办公文档（Word）、文本、图片、HTML、各类报表、视频音频等。

​		介于两者中间的比较常见的表现：xml和json

​		为了解决关系数据库的弱点，就出现了NoSQL数据库（具体内容参考，参考文章：https://mp.weixin.qq.com/s/1RjH8MBttLlH39-cWUT0pQ）

## **MongoDB** 

　　是一种开源，高性能的NoSQL数据库，主要是基于分布式文件存储；支持索引、集群、复制和故障转移、各种语言的驱动程序，官网：http://www.mongodb.org/ 和 API Docs：http://docs.mongodb.org/manual/

​		语言是Node.js，使用了Mongoose对MongoDB操作。Mongoose 是在nodejs环境下，对mongodb进行便捷操作的对象模型工具。官网：

1、安装：

​		node、mongoDB

​		npm install mongoose

2、初使用

​		连接：connect

```ts
connectDB() {
        let db_url = "mongodb://user:pass@localhost:port/database"
        mongoose.connect(db_url);
        mongoose.connection.on('connected', function () {
            console.log('connection open to ' + db_url);
        });
        mongoose.connection.on('error', function (err) {
            console.log('connection error: ' + err);
        });
        mongoose.connection.on('disconnected', function () {
            console.log('connection disconnected');
        });

    }
```

2、Schema

其实Schema映射的是 mongoDB 的 collection  (相当于 SQL table)，并且定义其结构。

```ts
let manSchema = new Schema({
     name: first: String,
     sex: Number,
});
```

model 是从 Schema 中定义的一种多样化的构造函数，model 的实例可以使用很多操作，所有文档的creat 和 index 都是由 model 来处理

```ts
let manModel = dbHelper.model('Man', manSchema);
```

然后再实例化当前model，对某data操作

```ts
var man = new Man({
	name: { first: 'Axl', last: 'Rose' },
    id: 1
});
```

2.1 保存数据

```ts
let conditions: any = {name: name};
manModel.save(function (err, args) {
	if (err) return handleError(err);
	console.log(args);
});
//or
manModel.create (conditions, function(err, args) { 
    if (err) return console.log(err);
    console.log(args);
});
//或者可以使用 insertMany / insertOne
```

2.2 修改

```ts
let conditions: any = {{id: 2}, { name: { first: 'Axl', last: 'Tom' }};
manModel.updateOne(conditions, unction(err, args) {
	if(err) return handleError(err);
	console.log(args);
});
// update / updateMany
// findOneAndUpdate 查找出相应的数据，修改，并返还给程序
```

2.3 查找

```ts
let conditions: any = {id: 2};
manModel.findOne(conditions, function(err, args){ 
    if(err) return handleError(err);
    console.log(args);
});
// find
```

2.4 删除

```ts
// deleteOne只删掉符合项的第一条 或者 deleteMany 删所有符合条件
manModel.deleteOne({ id: 1 }, function (err, args) {
	if (err) return handleError(err);
	console.log(args);
});
```

题外：删除某Array数组的某下标指向的数据

```ts
/** 根据下标删除某个多元素（object）的数组
 * @author deng
 * @param arr 要删除的数组
 * @param dx 要删除的下标
 * @returns arr 删除后的数组
 */
export function removeArr(arr:Array<any>, dx:number) {
    if (isNaN(dx) || dx > arr.length)
        return arr;
    for (var i = 0, n = 0; i < arr.length; i++) {
        if (arr[i] != arr[dx]) {
                arr[n++] = arr[i];
        }
    }
    arr[arr.length - 1] = undefined;
    arr.length -= 1;
    return arr;
}
```