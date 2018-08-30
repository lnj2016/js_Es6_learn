## 简介
首先说说mongoose是做什么的？mongoose是在nodejs中用来操作mongodb数据库，由于其使用方便，深得许多nodejser的热爱。

## 安装
```
yarn add mongoose
```
##使用
```javascript
//引入mogoose模块
const mongoose = require('mongoose')
//连接mongodb
mongoose.connect('mongodb://localhost:27017/mydb')
//连接成功
mongoose.connection('connected',()=>{
    console.log('connect success!');
})
//连接失败
mongoose.connection('error',(err)=>{
    console.log('connect error,'+err);
})
//连接断开
mongoose.connection('disconnected',()=>{
    console.log('connect disconnected');
})
```
## 创建Schema
#### schema 可以定义 mongodb中collection的数据格式
```javascript
//引入mogoose模块
const mongoose = require('mongoose')
const Schema = mongoose.Schema;
//连接mongodb
mongoose.connect('mongodb://localhost:27017/mydb')

//定义User Schema
const UserSchema = new Schema({
    username:String,
    password:String,
    age:Number,
    loginAt:Date
})
```
定义Model
model是由Schema生成的实例模型，可以对mongodb进行操作
```javascript
//定义User Schema
const UserSchema = new Schema({
    username:String,
    password:String,
    age:Number,
    loginAt:Date
})
const User = mongoose.model('User',UserSchema)
```
## 数据库操作
```javascript
//定义User Schema
const UserSchema = new Schema({
    username:String,
    password:String,
    age:Number,
    loginAt:Date
})
const User = mongoose.model('User',UserSchema)
//插入数据
let user = new User({
    username:'Tony'，
    password:'ps',
    age:20,
    loginAt:new Date()
})
//保存
user.save((errr,res)=>{
    if(err){
         return console.track(err)
}
console.log(res)
})
//更新
User.update({username:'tony'},{age:30},(err,doc)=>{
    if(err){
    return console.track(err)
}
console.log('update success!')
})
//删除
User.remove({age:30},(err,doc)=>{
    if(err){
    return console.track(err)
}
console.log('delete success!')
})
//查询
User.find({username:'tony'},(err,doc)=>{
 if(err){
    return console.track(err)
}
console.log(doc);
})
//数量
User.count({age:20},(err,docs)=>{
 if(err){
    return console.track(err)
}
console.log(docs);
})
```
## mongoose文档
http://www.nodeclass.com/api/mongoose.html#quick_start
