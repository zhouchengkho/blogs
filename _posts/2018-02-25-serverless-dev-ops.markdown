---
layout: post
title:  "Managing production/development env with serverless"
date:   2018-02-25 15:53:24 -0500
categories: AWS
---
Yo What's good:)

I'm sure a lot of dev-ops people have to face the challenge of managing production|development servers. In the old fashion way, just buy two servers and we are done. Now using serverless(which is this toolkit for deploying and operating serverless architectures), it requires a little bit configuration.

Just to show you how I did it and make sure your current project isn't destroyed, let's get started with a new project.
Before we start, if you don’t really know AWS, please get to know it before you play.
Remember to set up your AWS credentials in ~/.aws/credentials

### create empty project
* create serverless.yml, lamba_function.js, config.js, config.development.json, config.production.json in your project
* now your project has 5 files

### your serverless.yml file
```
service: test

provider:
  name: aws
  runtime: nodejs6.10
  stage: ${opt:stage, 'development'}
  region: us-east-1
  environment:
    NODE_ENV: ${self:provider.stage}
  custom:
    user_pool: ${file(./config.${self:provider.stage}.json):user_pool}

functions:
  test:
    handler: lambda_function.test
```

### your lambda_function.js file
```
const config = require('./config');

module.exports.test = (event, context, callback) => {
    let res = {
        statusCode: 200,
        test: config
    };

    callback(null, res);
};
```

### your config.js
```
var stage = process.env.NODE_ENV || 'development';

const config = {
    development: {
        prefix: '',
        db: {
            hostname: 'development host'
        }
    },
    production: {
        prefix: '',
        db: {
            hostname: 'production host'
        }
    }
}

module.exports = config[stage];
```

### your config.development.json and config.production.json
```
{
  "user_pool": ""
}
```

### run serverless deploy —stage development to deploy for development(or production for production, but you know that:) )
### that’s it… code always speaks louder than language, so let’s just leave it there 


