# koa-cms
easy to use cms platform using koa framework.


```javascript
var cms = require('koa-cms');
var app = new cms({
	mongo: 'mongodb://localhost/test',
	passport: {
	  'FACEBOOK_APP_ID': '--insert-facebook-app-id-here--',
    'FACEBOOK_APP_SECRET': '--insert-facebook-app-secret-here--'
	}
})
app.start()
```
