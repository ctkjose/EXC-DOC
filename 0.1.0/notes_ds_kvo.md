

### Datasource Definition ###

```javascript

definition = {
	name: "dsname",
	url : "",
}

definition = {
	name: "dsname",
	source : [

	]
}
```

### Walking a Datasource ###

```javascript
	var ops = {
		source : [
			{n:'Jose',ln:'Cuevas'},
			{n:'Joe',ln:'Cuevas'},
			{n:'Nate',ln:'Cuevas'},
			{n:'Carlos',ln:'Martinez'}
		],
	};

	var ds = exc.data.datasource.create(ops);

	ds.load().then(function(ds){
		var c = function(ds,item){
			ds.next().then(processRecord);
		};
		ds.next().then(processRecord);
	});
	console.log(ds);
```




### Dataset Object ###


```javascript
ds.value(key)
ds.value(key, value)
ds.hasValue(key)
ds.setValue(key, value)
ds.getValue(key)
ds.observe(key, callback)

ds.each(callback [,this] )
```

```javascript
	var ops = {
		source : [
			{n:'Jose',ln:'Cuevas'},
			{n:'Joe',ln:'Cuevas'},
			{n:'Nate',ln:'Cuevas'},
			{n:'Carlos',ln:'Martinez'}
		],
	};

	var ds = exc.data.datasource.create(ops);

	ds.load().then(function(ds){
		var c = function(ds,item){
			ds.next().then(processRecord);
		};
		ds.next().then(processRecord);
	});
	console.log(ds);
```

we can add datasets on module
```javascript
if(module.hasOwnProperty("datasets") && (typeof(module.datasets) =="object") ){
		console.log("[EXC][BOOTSTRAP][LOADING MODULE DATASETS][" + module.moduleName + "]");
		exc.data.loadDatasetsFromDefinitions(module.datasets);
}
```

## References ##
[AppMaker Datasource](https://developers.google.com/appmaker/scripting/api/client#DataSource)<br>
(https://bocoup.com/blog/JavaScript-object-observe)<br>

add a property to an existing object:

```javascript
Object.defineProperty(o, "b", { get: function () { return this.a + 1; } });


var log = ['test'];
var obj = {
  get latest () {
    if (log.length == 0) return undefined;
    return log[log.length - 1]
  }
}
console.log (obj.latest); // Will return "test".
```
```javascript
var o = {
  set current (str) {
    this.log[this.log.length] = str;
  },
  log: []
}
```

```javascript
var o = { a:0 };
Object.defineProperty(o, "b", { set: function (x) { this.a = x / 2; } });
```

delete the property
```javascript
delete obj.latest;
```
