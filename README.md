# @ismaelalves Logger
This is a package to add logging to request/response packages inside backend services and web applications.

## Installing
Using NPM:

```sh
npm install @ismaelalves/logger
```

## Usage
There are 4 ways to use this package, for now. This package can be:

1. A simple logger that uses [bunyan](https://github.com/trentm/node-bunyan) as logging package.
2. A middleware for [Express](https://github.com/expressjs/express) services, used to log all requests received by the service
3. An interceptor for [axios](https://github.com/axios/axios) request package, logging all requests that axios executes
4. An interceptor for [request](https://github.com/request/request) package, logging all requests that request executes
5. A redact package to remove secret information from the logs

### Configuration

The logger configuration (`type LoggerConfig`) has these properties:

|Name|Description|Type|Required|
|----|-----------|----|--------|
|PROJECT_NAME|The name of the project using the logger|String|true|
|LOG_LEVEL|The level to start logging the messages|[Log Level](https://github.com/trentm/node-bunyan#levels)|false|
|OMIT_ROUTES|Routes that the express middleware will not log|String[]|false|
|STREAMS|Configuration for the location of the output of the log level|[Stream](https://github.com/trentm/node-bunyan#streams)|false|

### Using require

#### Simple Logger

```javascript
const { Logger } = require('@ismaelalves/logger').init({
  PROJECT_NAME: 'project-name',
});

Logger.info(JSON.stringify({
  message: 'This is a cool message',
  origin: 'Some Place',
  status: 200,
})); 
/*
  This will create the following log: 
 {"name":"project-name","hostname":"hostname.local","pid":245,"level":30,"msg":"{\"message\":\"This is a cool message\",\"origin\":\"Some Place\",\"status\":200}","time":"2019-09-10T00:42:46.361Z","v":0}
*/
```

#### Express Middleware
```javascript
const { ExpressLogger } = require('@ismaelalves/logger').init({
  PROJECT_NAME: 'project-name',
});

/** Request/Response Logger */
app.use(ExpressLogger.onSuccess.bind(ExpressLogger));
app.use(ExpressLogger.onError.bind(ExpressLogger));

/*
  This will create the following log:
{"name":"project-name","hostname":"hostname.local","pid":298,"level":30,"msg":"{\"origin\":\"Express\",\"requestId\":\"77215bf8-f821-4faf-bcc1-2c0260eafc66\",\"type\":\"Request or Response\",\"headers\":{\"data\":\"all headers\"},\"body\":{\"data\":\"all body\"}}","time":"2019-09-10T00:49:04.394Z","v":0}
*/
```

#### Axios Interceptor
```javascript
const { AxiosLogger } = require('@ismaelalves/logger').init({
  PROJECT_NAME: 'project-name',
});

const axiosInstance = require('axios').default.create({ ...config });

AxiosLogger.attachInterceptor.bind(AxiosLogger)(axiosInstance);

/*
  This will create the following log:
{"name":"project-name","hostname":"hostname.local","pid":24654,"level":30,"msg":"{\"origin\":\"Axios\",\"requestId\":\"77215bf8-f821-4faf-bcc1-2c0260eafc66\",\"type\":\"Request or Response\",\"headers\":{\"data\":\"all headers\"},\"body\":{\"data\":\"all body\"},\"method\":\"HTTP Method\",\"url\":\"https://somosphi.com\",\"data\":{\"data\":\"all data from axios\"},\"params\":{\"data\":\"params used\"},\"status\":200,\"statusText\":\"OK\"}","time":"2019-09-10T00:53:40.767Z","v":0}
*/
```

#### Request Debug
```javascript
const { RequestLogger } = require('@ismaelalves/logger').init({
  PROJECT_NAME: 'project-name',
});
const request = require('request');

RequestLogger.attachDebug.bind(RequestLogger)(request);

/*
  This will create the following log:
{"name":"project-name","hostname":"hostname.local","pid":24654,"level":30,"msg":"{\"origin\":\"Request\",\"requestId\":\"77215bf8-f821-4faf-bcc1-2c0260eafc66\",\"type\":\"Request or Response\",\"headers\":{\"data\":\"all headers\"},\"body\":{\"data\":\"all body\"},\"method\":\"HTTP Method\",\"url\":\"https://somosphi.com\",\"data\":{\"data\":\"all data from axios\"},\"params\":{\"data\":\"params used\"},\"status\":200,\"statusText\":\"OK\"}","time":"2019-09-10T00:53:40.767Z","v":0}
*/
```

### Using import

#### Simple Logger

```javascript
import { init } from '@ismaelalves/logger';

const {
  Logger,
} = init({
  PROJECT_NAME: 'project-name',
});

Logger.info(JSON.stringify({
  message: 'This is a cool message',
  origin: 'Some Place',
  status: 200,
})); 
/*
  This will create the following log: 
 {"name":"project-name","hostname":"hostname.local","pid":245,"level":30,"msg":"{\"message\":\"This is a cool message\",\"origin\":\"Some Place\",\"status\":200}","time":"2019-09-10T00:42:46.361Z","v":0}
*/
```

#### Express Middleware
```javascript
import { init } from '@ismaelalves/logger';

const {
  ExpressLogger,
} = init({
  PROJECT_NAME: 'project-name',
});

/** Request/Response Logger */
app.use(ExpressLogger.onSuccess.bind(ExpressLogger));
app.use(ExpressLogger.onError.bind(ExpressLogger));

/*
  This will create the following log:
{"name":"project-name","hostname":"hostname.local","pid":298,"level":30,"msg":"{\"origin\":\"Express\",\"requestId\":\"77215bf8-f821-4faf-bcc1-2c0260eafc66\",\"type\":\"Request or Response\",\"headers\":{\"data\":\"all headers\"},\"body\":{\"data\":\"all body\"}}","time":"2019-09-10T00:49:04.394Z","v":0}
*/
```

#### Axios Interceptor
```javascript
import { init } from '@ismaelalves/logger';
import axios from 'axios';

const {
  AxiosLogger,
} = init({
  PROJECT_NAME: 'project-name',
});

const axiosInstance = axios.default.create({ ...config });

AxiosLogger.attachInterceptor.bind(AxiosLogger)(axiosInstance);

/*
  This will create the following log:
{"name":"project-name","hostname":"hostname.local","pid":24654,"level":30,"msg":"{\"origin\":\"Axios\",\"requestId\":\"77215bf8-f821-4faf-bcc1-2c0260eafc66\",\"type\":\"Request or Response\",\"headers\":{\"data\":\"all headers\"},\"body\":{\"data\":\"all body\"},\"method\":\"HTTP Method\",\"url\":\"https://somosphi.com\",\"data\":{\"data\":\"all data from axios\"},\"params\":{\"data\":\"params used\"},\"status\":200,\"statusText\":\"OK\"}","time":"2019-09-10T00:53:40.767Z","v":0}
*/
```

#### Request Debug
```javascript
import { init } from '@ismaelalves/logger';
import request from 'request';

const {
  RequestLogger,
} = init({
  PROJECT_NAME: 'project-name',
});

RequestLogger.attachDebug.bind(RequestLogger)(request);

/*
  This will create the following log:
{"name":"project-name","hostname":"hostname.local","pid":24654,"level":30,"msg":"{\"origin\":\"Request\",\"requestId\":\"77215bf8-f821-4faf-bcc1-2c0260eafc66\",\"type\":\"Request or Response\",\"headers\":{\"data\":\"all headers\"},\"body\":{\"data\":\"all body\"},\"method\":\"HTTP Method\",\"url\":\"https://somosphi.com\",\"data\":{\"data\":\"all data from axios\"},\"params\":{\"data\":\"params used\"},\"status\":200,\"statusText\":\"OK\"}","time":"2019-09-10T00:53:40.767Z","v":0}
*/
```

#### Redact Logger

```javascript
import { init } from '@ismaelalves/logger';

const {
  RequestLogger,
  Redact,
} = init({
  PROJECT_NAME: 'project-name',
});

RequestLogger.info(Redact.map({
  'password': 'secret',
}));

RequestLogger.info(Redact.map({
  'code': 'secret',
}));

Redact.addKey(/code/i);

RequestLogger.info(Redact.map({
  'code': 'secret',
}));
```

#### Change requestId
```javascript
import { init } from '@ismaelalves/logger';

const {
  ExpressLogger,
} = init({
  PROJECT_NAME: 'project-name',
});

/** Request/Response Logger */
app.use(ExpressLogger.onSuccess.bind(ExpressLogger));
app.use(ExpressLogger.onError.bind(ExpressLogger));

/** Change requestId */
app.use((req, res, next)=>{
  req.headers["requestId"] = "new value";
  next();
});

/*
  This will create the following log:
{"name":"project-name","hostname":"hostname.local","pid":298,"level":30,"msg":"{\"origin\":\"Express\",\"requestId\":\"new value\",\"type\":\"Request or Response\",\"headers\":{\"data\":\"all headers\"},\"body\":{\"data\":\"all body\"}}","time":"2019-09-10T00:49:04.394Z","v":0}
*/
```
