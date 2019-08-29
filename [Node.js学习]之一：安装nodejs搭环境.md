[Node.js学习]之一：安装nodejs搭环境
------------------------

深入学习：
 1. [\[Node.js 学习\]之一：安装node](https://github.com/ToSaySomething/Node.jsStudying/blob/master/%5BNode.js学习%5D之一：安装nodejs搭环境.md)
 2. [\[Node.js学习\]之二：为什么要使用Node.js](https://github.com/ToSaySomething/Node.jsStudying/blob/master/%5BNode.js学习%5D之二：为什么要使用Node.js.md)
 3. [\[[Node.js学习]之三：使用数据库](https://github.com/ToSaySomething/Node.jsStudying/blob/master/%5BNode.js学习%5D之三：使用数据库.md)

## 1、安装nodejs（选对版本）
## 2、vs code 运行.ts
##### 1) 问题：TypeScript用let写了一行变量声明就不能编译通过，是什么问题？
	解决：版本问题，tsc命令行引用了默认的版本，删除：C:\Program Files (x86)\Microsoft SDKs\Typescript\1.0自带的这个版本即可。
	输入命令 安装npm，g代表全局安装
```
	npm install npm -g
	npm --v
	npm -g install typescript@x.x.x
	tsc --v
```
##### 2）问题：服务端编译版本问题
> Executing task: tsc -p e:\bigBangHero\server\tsconfig.json --watch <
> error TS5023: Unknown compiler option 'strict'.
> error TS5023: Unknown compiler option 'esModuleInterop'.
> The terminal process terminated with exit code: 1
> Terminal will be reused by tasks, press any key to close it.
> 版本不对 ：npm -g install typescript@3.1.1
> ![](https://img-blog.csdnimg.cn/20190731151957217.png)

	原因：ts的版本问题改成了2.1.5 ，最新的版本格式要求比较严格
	npm -g install typescript@2.1.5
参考：https://www.jianshu.com/p/7da75c10b821
##### 3）问题：版本修改不了总显示1.0.3.0 服务端编译问题
	这个问题主要是全局的tsc的版本和当前项目的tsc版本不一致导致的，全局的tsc版本太旧了到环境变量里将tsc 1.0 的路径删了
	Open your environment settings and remove the old Typescript from your system PATH variable. Mine was C:\Program Files (x86)\Microsoft SDKs\TypeScript\1.0\.
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019073115204729.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25vX2FsdGVybmFudGl2ZQ==,size_16,color_FFFFFF,t_70)
	其实解决主要方法：
		用vs code 继承的 MSI文件安装的typescipt是**1.0**版本，去到
		**C:\Program Files (x86)\Microsoft SDKs\TypeScript 对应的1.0文件夹删掉**
	
结果才显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190731152111397.png)
