# dva-mock

1. 安装dva-cli
`npm install dva-cli -g`

2. 安装mockjs
`npm install mockjs --save`

3. 配置mockjs
打开 `.roadhogrc.mock.js`， 设置如下
```javasc
const fs = require('fs');
const path = require('path');
const mockPath = path.join(__dirname+'/mock');
const mock={};
fs.readdirSync(mockPath).forEach(file => { Object.assign(mock, require('./mock/'+file)); });

module.exports = mock;
```

4. 模拟数据
`mock` 文件夹下新建  `user.js`, 模拟2个 API 请求 **GET /api/users**, **POST /api/users**
```javascript
const Mock = require('mockjs');
let db = Mock.mock({
  'data|3-6': [{
    id: '@id',
    name: '@name',
    'age|18-32': 1
  }]
});

module.exports = {
  [`GET /api/users`](req, res) {
    res.status(200).json(db);    
  },

  [`POST /api/users`](req, res) {
    let user = req.body;
    console.assert(req);
    user.id = Mock.mock('@id');
    db.data.push(user);

    res.status(200).json(user);
  }
}
```

5. 运行项目
`npm start`
查看模拟结果，　｀http://loclahost:8000/api/users`


