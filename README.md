# koa-cms
easy to use cms platform using koa framework.


```javascript
var cms = require('koa-cms')
var view = require('koa-cms-view')
var cmsBody = require('koa-cms-body')
var cmsS3 = require('koa-cms-S3')
var cmsMonk = require('koa-cms-monk')
var myproject = cmsMonk('project')
var cmsAuth = require('koa-cms-auth')
var config = require('./config')

var app = new cms(config);

app.get('/', function*(){
	view.render('home/index')
})

//post new
app.post('/projects', cmsBody, function*(){
	var files = this.request.files;
	var fields = this.request.fields;
  var paths = yield cmsS3.upload(files);
  fields.paths = paths;

  myproject.save(fields)
  this.status=200;
})

//update
app.put('/projects/:id', cmsBody, function*(){
	var id = this.params.id;
	var fields = this.request.fields;
	
	if(this.request.files){
		var p = myproject.get(id)
		cmsS3.remove(p.paths)
		myproject.remove(id,'paths')
	}
	myproject.update(id, fields)
	this.status=200;
})

//delete
app.del('/projects/:id', function*(){
	var id = this.params.id;
	var p = myproject.get(id);
	cmsS3.remove(p.paths);
	myproject.remove(id)
})

//query item
app.get('/projects/:q', function*(){
	var q = this.params.q;
	var p = myproject.get(q)
	this.body=p;
})

app.start(3000)
```
