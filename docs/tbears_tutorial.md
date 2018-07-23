

# ICON SCORE development suite (tbears) TUTORIAL

## Change History

| Date       | Version  | Author | Description                                                 |
| :--------- | :---- | :----: | :--------------------------------------------------- |
| 2018.07.16 | 0.9.3 | Soobok Jin | tbears cli commands updated. Document structure revised, and translated to English. |
| 2018.07.10 | 0.9.3 | Chiwon Cho | earlgrey package description added. |
| 2018.07.06 | 0.9.3 | Inwon Kim | Configuration file updated.       |
| 2018.06.12 | 0.9.2 | Chiwon Cho | Tutorial moved from doc to markdown. |
| 2018.06.11 | 0.9.1 | Chiwon Cho | Error codes added. icx_getTransactionResult description updated. |
| 2018.06.01 | 0.9.0 | Chiwon Cho | JSON-RPC API v3 ChangeLog added. |

## tbears Overview
tbears is a suite of development tools for SCORE. You can code and test your smart contract locally, and when ready, deploy SCORE onto the ICON network from command-line interface. tbears provides a project template for SCORE to help you start right away.

## Installation

This guide will explain how to install tbears on your system. 

### Requirements

ICON SCORE development and execution requires following environments.

* OS: MacOS, Linux
    * Windows are not supported yet.
* Python
    * Version: python 3.6+
    * IDE: Pycharm is recommended.

**Libraries**

| name        | description                                                  | github                                                       |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| LevelDB     | ICON SCORE uses levelDB to store its states.                 | [LevelDB GitHub](https://github.com/google/leveldb)          |
| libsecp256k | ICON SCORE uses secp256k to sign and validate a digital signature. | [secp256k GitHub](https://github.com/bitcoin-core/secp256k1) |

### Setup on MacOS

```bash
#install levelDB
$ brew install leveldb

# Create a working directory
$ mkdir work
$ cd work

# setup the python virtualenv development environment
$ virtualenv -p python3 .
$ source bin/activate

# Install the ICON SCORE dev tools
(work) $ pip install earlgrey-x.x.x-py3-none-any.whl
(work) $ pip install iconservice-x.x.x-py3-none-any.whl
(work) $ pip install tbears-x.x.x-py3-none-any.whl
```

### Setup on Linux

```bash
# Install levelDB
$ sudo apt-get install libleveldb1 libleveldb-dev
# Install libSecp256k
$ sudo apt-get install libsecp256k1-dev

# Create a working directory
$ mkdir work
$ cd work

# Setup the python virtualenv development environment
$ virtualenv -p python3 .
$ source bin/activate

# Install the ICON SCORE dev tools
(work) $ pip install earlgrey-x.x.x-py3-none-any.whl
(work) $ pip install iconservice-x.x.x-py3-none-any.whl
(work) $ pip install tbears-x.x.x-py3-none-any.whl
```

## How to use tbears

### Command-line Interfaces(CLIs)

#### Overview

tbears has 7 commands, `init`, `start`, `stop`, `deploy`, `clear`, `samples`, and `keystore`.

**Usage**

```bash
usage: tbears [-h] [-d] command ...

tbears v0.9.3 arguments

optional arguments:
  -h, --help            show this help message and exit
  -d, --debug           Debug mode

Available commands:
  If you want to see help message of commands, use "tbears command -h"

  command
    start      Start tbears serivce
    stop       Stop tbears service
    deploy     Deploy the SCORE
    clear      Clear all SCORE deployed on tbears service
    init       Initialize tbears project
    samples    Create two SCORE samples (standard_crowd_sale, standard_token)
    keystore   Create keystore file in passed path
```

**Options**

| shorthand, Name | default | Description                     |
| --------------- | :------ | ------------------------------- |
| -h, --help      |         | show this help message and exit |
| -d, --debug     |         | Debug mode                      |

#### tbears init

**Description**

Initialize SCORE development environment. Generate <project\>.py and package.json in <project\> directory. The name of the score class is \<score_class\>.  Configuration files, "tbears.json" used when starting tbears and "deploy.json" used when deploying SCORE, are also generated.

**Usage**

```bash
usage: tbears init [-h] project score_class
Initialize SCORE development environment. Generate <project>.py and
package.json in <project> directory. The name of the score class is
<score_class>.

positional arguments:
  project      Project name
  score_class  SCORE class name

optional arguments:
  -h, --help   show this help message and exit
```

**Options**

| shorthand, Name | default | Description                     |
| --------------- | :------ | ------------------------------- |
| project         |         | Project name                    |
| score_class     |         | SCORE class name                |
| -h, --help      |         | show this help message and exit |

**Examples**

```bash
(work) $ tbears init abc ABCToken
(work) $ ls abc
abc.py  __init__.py package.json tests
```

**File description**

| **Item**                   | **Description**                                              |
| :------------------------- | :----------------------------------------------------------- |
| \<project>                 | SCORE project name. Project directory is create with the same name. |
| tbears.json                | tbears default configuration file will be created on the working directory. |
| deploy.json                | Configuration file for deploy command will be created on the working directory. |
| \<project>/\_\_init\_\_.py | \_\_init\_\_.py file to make the project folder recognized as a python package. |
| \<project>/package.json    | SCORE metadata.                                              |
| \<project>/\<project>.py   | SCORE main file. ABCToken class is defined.                  |
| tests                      | Directory for SCORE unittest                                 |

#### tbears start

**Description**

Start tbears service. Whenever tbears service starts, it loads the configutaion from "tbears.json" file. If you want to use other configuration file, you can specify the file location with the '-c' option.

**Usage**

```bash
usage: tbears start [-h] [-a ADDRESS] [-p PORT] [-c CONFIG]

Start tbears service

optional arguments:
  -h, --help                       show this help message and exit
  -a ADDRESS, --address ADDRESS    Address to host on (default: 0.0.0.0)
  -p PORT, --port PORT             Listen port (default: 9000)
  -c CONFIG, --config CONFIG       tbears configuration file path(default:./tbears.json)
```

**Options**

| shorthand, Name | default       | Description                     |
| --------------- | :------------ | ------------------------------- |
| -h, --help      |               | show this help message and exit |
| -a, --address   | 0.0.0.0       | Address to host on              |
| -p, --port      | 9000          | Listen port                     |
| -c, --config    | ./tbears.json | tbears configuration file path  |

#### tbears stop

**Description**

Stop all running SCOREs and tbears service.

**Usage**

```bash
usage: tbears stop [-h]

Stop all running SCORE and tbears service

optional arguments:
  -h, --help  show this help message and exit
```

**Options**

| shorthand, Name | default | Description                     |
| --------------- | :------ | ------------------------------- |
| -h, --help      |         | show this help message and exit |

#### tbears deploy

**Description**

Deploy the SCORE. You can deploy it on local or icon service.

"deploy.json" file contains deploymenet configuration properties. (See below 'Configuration Files' chapter). If you want to use other configuration file, you can specify the file location with the '-c' option.

**Usage**

```bash
usage: tbears deploy [-h] [-u URI] [-t {tbears,icon}] [-m {install,update}]
                     [-f FROM] [-o TO] [-k KEYSTORE] [-c CONFIG]
                     project

Deploy the SCORE in project

positional arguments:
  project               Project name

optional arguments:
  -h, --help                                   show this help message and exit
  -u URI, --node-uri URI                       URI of node
                                               (default: http://127.0.0.1:9000/api/v3)
  -t {tbears,icon}, --type {tbears,icon}       Deploy SCORE type
                                               (default: tbears)
  -m {install,update}, --mode {install,update} Deploy mode (default: install)
  -f FROM, --from FROM                         From address. i.e. SCORE owner address
  -o TO, --to TO                               To address. i.e. SCORE address
  -k KEYSTORE, --key-store KEYSTORE            Key store file for SCORE owner
  -n NID, --nid NID                            Network ID of node
  -c CONFIG, --config CONFIG                   deploy config path (default: ./deploy.json)
```

**Options**

| shorthand, Name                                 | default                      | Description                                                  |
| ----------------------------------------------- | :--------------------------- | ------------------------------------------------------------ |
| project                                         |                              | Project directory which contains the SCORE package.          |
| -h, --help                                      |                              | show this help message and exit                              |
| -u, --node-uri                                  | http://127.0.0.1:9000/api/v3 | URI of node                                                  |
| -t {tbears,icon},<br> --type {tbears,icon}      | tbears                       | Deploy SCORE type ("tbears" or "icon" ).                     |
| -m {install,update},<br>--mode {install,update} | install                      | Deploy mode ("install" or "update").                         |
| -f, --from                                      |                              | From address. i.e. SCORE owner address                       |
| -o, --to                                        |                              | To address. i.e. SCORE address <br>**(needed only when updating score)** |
| -k, --key-store                                 |                              | Key store file for SCORE owner                               |
| -n, --nid                                       |                              | Network ID of node                                           |
| -c, --config                                    | ./deploy.json                | deploy config path                                           |

**Examples**

```bash
(Work)$ tbears deploy -t tbears abc

(work) $ cat ./deploy.json
{
    "uri": "http://127.0.0.1:9000/api/v3",
    "scoreType": "tbears",
    "mode": "install",
    "keyStore": null,
    "from": "hxaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
    "to": "cx0000000000000000000000000000000000000000",
    "nid": "0x1234",
    "stepLimit": "0x1234",
    "txType": "dummy",
    "scoreParams": {}
}
(work) $ tbears deploy abc -c ./deploy.json

#when you deploy SCORE to icon, input keystore password
(Work)$ tbears deploy -t icon -f hxaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa -k keystore abc
input your key store password:

Send deploy request successfully.
transaction hash: 0x9c294b9608d9575f735ec2e2bf52dc891d7cca6a2fa7e97aee4818325c8a9d41

#update abc SCORE
#append prefix 'cx' in front of score address
(Work)$ tbears deploy abc -m update -o cx6bd390bd855f086e3e9d525b46bfe24511431532
Send deploy request successfully.
transaction hash: 0xad292b9608d9575f735ec2ebbf52dc891d7cca6a2fa7e97aee4818325c80934d
```

#### tbears clear

**Description**

Clear tbears service.

**Usage**

```bas
usage: tbears clear [-h]

Clear all SCORE deployed on local tbears service

optional arguments:
  -h, --help  show this help message and exit
```

**Options**

| shorthand, Name | default | Description                     |
| --------------- | :------ | ------------------------------- |
| -h, --help      |         | show this help message and exit |

#### tbears samples

**Description**

Create two SCORE samples ("standard_crowd_sale", and "standard_token").

**usage**

```bash
usage: tbears samples [-h]

Create two SCORE samples (standard_crowd_sale, standard_token)

optional arguments:
  -h, --help  show this help message and exit
```

**Options**

| shorthand, Name | default | Description                     |
| --------------- | :------ | ------------------------------- |
| -h, --help      |         | show this help message and exit |

**Examples**

```bash
(work) $ tbears samples

(work) $ ls standard*
standard_crowd_sale:
__init__.py  package.json  standard_crowd_sale.py

standard_token:
__init__.py  package.json  standard_token.py
```

#### tbears transfer

**Description**

Transfer designated amount of ICX coins.

**Usage**

```bash
usage: tbears transfer [-h] [-f FROM] [-t {dummy,real}] [-k KEYSTORE] [-u URI]
                       [-c CONFIG]
                       to value

Transfer ICX coin.

positional arguments:
  to                    Recipient
  value                 Amount of ICX coin to transfer in loop(1 icx = 1e18
                        loop)

optional arguments:
  -h, --help            show this help message and exit
  -f FROM, --from FROM  From address. Must use with dummy type.
  -t {dummy,real}, --type {dummy,real}
                        Type of transfer request. Dummy type is valid in
                        tbears node only(default: dummy)  
  -k KEYSTORE, --key-store KEYSTORE
                        Sender's key store file
  -u URI, --node-uri URI
                        URI of node (default: http://127.0.0.1:9000/api/v3)
  -c CONFIG, --config CONFIG
                        deploy config path (default: ./deploy.json)
```

**Options**

| shorthand, Name | default | Description                     |
| --------------- | :------ | ------------------------------- |
| to              |         | Recipient address.          |
| value           |         | Amount of ICX coin in loop to transfer to "to" address. (1 icx = 1e18 loop) |
| -h, --help      |         | show this help message and exit |
| -f, --from      |         | From address. Must be used with "-t dummy" option. |
| -t, --type      | dummy   | Type of transfer request. ("dummy" or "real") |
| -k, --key-store |         | Keystore file path. Used to generate "from" address and transaction signature. |
| -u, --node-uri  | http://127.0.0.1:9000/api/v3 | URI of node                                                  |
| -c, --config    | ./deploy.json | Configuration file path. This file defines the default values for the properties "keyStore", "uri", "txType" and "from". |

**Examples**

```bash
(work) $ transfer -f hxaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa hxaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaab 100
Got an error response
{'jsonrpc': '2.0', 'error': {'code': -32600, 'message': 'Out of balance'}, 'id': 1}

(work) $ tbears transfer -k tests/test_util/test_keystore -t real hxaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaab 1e18
Send transfer request successfully.
transaction hash: 0xc1b92b9a08d8575f735ec2ebbf52dc831d7c2a6a2fa7e97aee4818325cad919e
```

#### tbears txresult

**Description**

Get transaction result.

**Usage**

```bash
usage: tbears txresult [-h] [-u URI] [-c CONFIG] hash

Get transaction result by transaction hash

positional arguments:
  hash                  Transaction hash of the transaction to be queried.

optional arguments:
  -h, --help            show this help message and exit
  -u URI, --node-uri URI
                        URI of node (default: http://127.0.0.1:9000/api/v3)
  -c CONFIG, --config CONFIG
                        config path. Use "uri" value (default: ./deploy.json)
```

**Options**

| shorthand, Name | default | Description                     |
| --------------- | :------ | ------------------------------- |
| hash  |         | Hash of the transaction to be queried |
| -h, --help      |         | show this help message and exit |
| -u, --node-uri  | http://127.0.0.1:9000/api/v3 | URI of node   |
| -c, --config    | ./deploy.json | Configuration file path. This file defines the default value for the "uri". |

**Examples**

```bash
(work) $ tbears txresult 0x2adc1c73da604c7c4247336d98bf5902eee9977ed017ffa3b2b5fd266ab3cd8d
Transaction result: {
    "jsonrpc": "2.0",
    "result": {
        "txHash": "0x2adc1c73da604c7c4247336d98bf5902eee9977ed017ffa3b2b5fd266ab3cd8d",
        "blockHeight": "0x1",
        "blockHash": "86fb191dada862c0996d883d8112baea212c4b2705676bf15f5eadb60a29de72",
        "txIndex": "0x0",
        "to": "cx0000000000000000000000000000000000000000",
        "scoreAddress": "cxeee6a1a4e8ab4d4e3d39c520ceb722217f9a9ef1",
        "stepUsed": "0x11670",
        "stepPrice": "0x0",
        "cumulativeStepUsed": "0x11670",
        "eventLogs": [],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1",
        "from": "hxaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
    },
    "id": 1
}
```

### Configuration Files

#### tbears.json

When starting tbears (`tbears start`), "tbears.json" is used to configure the parameters and initial settings.

```json
{
    "hostAddress": "0.0.0.0",
    "port": 9000,
    "scoreRoot": "./.score",
    "dbRoot": "./.db",
    "enableFee": false,
    "enableAudit": false,
    "log": {
        "colorLog": true,
        "level": "debug",
        "filePath": "./tbears.log",
        "outputType": "console|file"
        "rotateType": "D",
        "rotateInterval": 1
    },
    "accounts": [
        {
            "name": "genesis",
            "address": "hx0000000000000000000000000000000000000000",
            "balance": "0x2961fff8ca4a62327800000"
        },
        {
            "name": "fee_treasury",
            "address": "hx1000000000000000000000000000000000000000",
            "balance": "0x0"
        }
    ]
}
```

| Field          | Data type | Description                                                  |
| :------------- | :-------- | :----------------------------------------------------------- |
| hostAddress    | string    | Address to host on                                           |
| port           | int       | JSON-RPC server port number.                                 |
| scoreRoot      | string    | Root directory that SCORE will be installed.                 |
| dbRoot         | string    | Root directory that state DB file will be created.           |
| enableFee      | boolean   | not implemented                                              |
| enableAudit    | boolean   | not implemented                                              |
| log            | dict      | tbears log setting                                       |
| log.colorLog   | boolean   | Log display option (color or black)                         |
| log.level      | string    | log level. <br/>"debug", "info", "warning", "error"          |
| log.filePath   | string    | log file path.                                               |
| log.outputType | string    | “console”: log outputs to the console that tbears is running.<br/>“file”: log outputs to the file path.<br/>“console\|file”: log outputs to both console and file. |
| log.rotateType | string    | log file rotate type. 'S' second, 'M' minute, 'H' hour, 'D' day, 'W' week |
| log.rotateInterval | int      | log file rotate interval.                                |
| accounts       | list      | List of accounts that holds initial coins. <br>(index 0) genesis: account that holds initial coins.<br>(index 1) fee_treasury: account that collects transaction fees.<br>(index 2~): accounts. |

#### deploy.json

When deploying a SCORE (`tbears deploy`), this file is used to configure the parameters and initial settings .

SCORE's  `on_install()` or  `on_update()`  method is called on deployment.  For example, if you deploy a new SCORE (mode: install), `on_install()` method is called to initialize the SCORE. In this config file, you can set the deploy "mode" and the parameters ("scoreParams") of `on_install()` or `on_update()`.

```json
{
    "uri": "http://127.0.0.1:9000/api/v3",
    "scoreType": "tbears",
    "mode": "install",
    "keyStore": null,
    "from": "hxaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
    "to": "cx0000000000000000000000000000000000000000",
    "nid": "0x1234",
    "stepLimit": "0x1234",
    "txType": "dummy",
    "scoreParams": {}
}
```

| Field       | Data  type | Description                                                  |
| ----------- | :--------- | :----------------------------------------------------------- |
| uri         | string     | uri to send the deploy request.                              |
| scoreType   | string     | SCORE type of the deployment (tbears or icon)                |
| mode        | string     | Deploy mode.<br>install: new SCORE deployment.<br>update: update the SCORE that was previously deployed. |
| from        | string     | Address of the SCORE deployer                                |
| to          | string     | (optional) Used when update SCORE (The address of the SCORE being updated).<br/>(in the case of "install" mode, the address should be 'cx0000~') |
| nid         | string     | Network ID                                                   |
| stepLimit   | string     | (optional) stepLimit value                                   |
| keyStore    | string     | (optional) keystore file path                                |
| txType      | string     | Transaction type for send command.<br/>dummy: send transaction with an arbitrary signature<br/>real: send transaction with valid transaction |
| scoreParams | dict       | Parameters to be passed to on_install() or on_update()       |

## Utilities

Utilities for SCORE development.

### Logger

Logger is a package writing outputs to log file or console.

#### Method

```python
@staticmethod
def debug(msg: str, tag: str)
```

#### Usage

```python
from iconservice.logger import Logger

TAG = 'ABCToken'

Logger.debug('debug log', TAG)
Logger.info('info log', TAG)
Logger.warning('warning log', TAG)
Logger.error('error log', TAG)
```

## Notes

* tbears currently does not have loopchain engine, so some JSON-RPC APIs which are not related to SCORE development may not function.
    * Below JSON-RPC APIs are supported in tbears:
        * `icx_getBalance`, `icx_getTotalSupply`, `icx_getBalance`, `icx_call`, `icx_sendTransaction`, `icx_getTransactionResult`
* When unavoidable, tbears commands or its behavior may change for the sake of improvement.
* For the development convenience, JSON-RPC server in tbears does not verify the transaction signature.

## Reference
[tbears JSON-RPC API v3](https://repo.theloop.co.kr/icon/tbears/blob/47-tbears_tutorial/docs/tbears_jsonrpc_api_v3.md)