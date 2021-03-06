# serverless rust [![Build Status](https://travis-ci.org/softprops/serverless-rust.svg?branch=master)](https://travis-ci.org/softprops/serverless-rust) [![npm](https://img.shields.io/npm/v/serverless-rust.svg)](https://www.npmjs.com/package/serverless-rust)


> A ⚡ [Serverless framework](https://serverless.com/framework/) ⚡ plugin for [Rustlang](https://www.rust-lang.org/en-US/) applications 🦀

## 📦 Install

Install the plugin with npm

```bash
$ npm install serverless-rust@0.1.5
```

💡 This serverless plugin assumes you are building Rustlang lambdas using the [lando](https://github.com/softprops/lando) or [crowbar](https://github.com/ilianaw/rust-crowbar) rustlang crates.

Add the following to your serverless project's `serverless.yml` file

```yaml
service: demo
provider:
  name: aws
  # crowbar and lando integrate with aws lambda's python3.6 runtime
  runtime: python3.6
plugins:
  # this adds informs servleress to use
  # the serverless-rust plugin
  - serverless-rust
# the follow is recommended for small deployment sizes
# (faster uploads)
package:
  individually: true
  exclude:
    - ./**
functions:
  test:
    # liblambda.handler is the default function name when
    # you follow lando/crowbar conventions
    handler: liblambda.handler
    # the following limits the function packaging
    # to just the resulting binary
    package:
      include:
        - liblambda.so
    events:
      - http:
          path: /test
          method: GET
```


## 🖍️ customize

You can optionally adjust the default settings of the dockerized build env using
a custom section of your serverless.yaml configuration


```yaml
custom:
  # this section customizes the default
  # serverless-rust plugin settings
  rust:
    # flags passed to cargo
    cargoFlags: '--features lando/python3-sys'
    # custom docker tag
    dockerTag: 'some-custom-tag'
```

### 🎨 Per function customization

If your serverless project contains multiple functions, you may sometimes
need to customize the options above at the function level. You can do this
by defining a `rust` key with the same options inline in your function
specficiation.

```yaml
functions:
  test:
    rust:
      # function specific flags passed to cargo
      cargoFlags: '--features ...'
    # liblambda.handler is the default function name when
    # you follow lando/crowbar conventions
    handler: liblambda.handler
    # the following limits the function packaging
    # to just the resulting binary
    package:
      include:
        - liblambda.so
    events:
      - http:
          path: /test
          method: GET
```


## 🏗️ serverless templates

* lando api gateway application - https://github.com/softprops/serverless-lando
* multi function lando api gateway application - https://github.com/softprops/serverless-multi-lando
* crowbar cloudwatch scheduled lambda application - https://github.com/softprops/serverless-crowbar
