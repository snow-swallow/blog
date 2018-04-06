## JavaScript 实现深度拷贝

1. 真深拷贝
```
function deepCopy(source) {
	var dest = {};
	var keys = Object.keys(source);
	for(var idx in keys) {
		var key = keys[idx];
		if (typeof source[key] == "object") {
			dest[key] = deepCopy(source[key]);
        } else {
			dest[key] = source[key];
        }
    }
	return dest;
}
```

2. 一层深拷贝，实则浅拷贝（其实效果就是 `Object.assign(target, ...source)`）
```
function fakeDeepCopy(source) {
	var dest = {};
	var keys = Object.keys(source);
	for(var idx in keys) {
		var key = keys[idx];
		dest[key] = source[key];
    }
	return dest;
}
```

3. 验证方法

```
let foo = {
	a: 1,
	b: {
		c: 1
	}
};
let deepFollower = deepCopy(foo);
let faker = fakeDeepCopy(foo);
let sir = {self: 1};
Object.assign(sir, foo);

foo.a ++;
foo.b.c ++;

foo.a; // 2
foo.b.c; // 2

deepFollower.a; // 1
deepFollower.b.c; //1

faker.a; // 1
faker.b.c; // 2

sir.a; // 1
sir.b.c; // 2
```

> Reference:
- [Object.assign()与深拷贝](https://segmentfault.com/a/1190000010661297)
- [MDN Object.assign()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)