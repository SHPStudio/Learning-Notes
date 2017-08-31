# ES6的特殊语法
## [`${variable}`]
 从变量中获取值来设置obj对象中的某个属性。
 
 也就是说在设置obj对象的属性值时，在获取obj对象的属性时，可以通过一个变量里储存的值来作为key在对象中查找
 相应的键值对然后赋值。
 
    			var name = "node.js";
    			var field = "name";
    			var obj = {
    				"name": ""
    			};
    			$().ready(function(){
    				obj[`${field}`] = name;
    				console.log(obj.name);
    			})