---
title: APIs
tags: technology, documentation
---

# Developer Notes - FinX SDK

The **FinX SDK** is a python package that has interfaces to the FinX Capital Markets
Analytics Platform (the 'FinX Platform'). The code in the SDK makes calls to REST APIs 
& WebSocket endpoints. A valid ___FinX API Key___ is required for the SDK to return results.

To obtain an API Key, please contact us FinX Capital Markets LLC [via email](mailto:info@finx.io).

Please see LICENSE and SECURITY for terms of use.

***

### Installation FinX SDK

> BETA MODE ANNOUNCEMENT: the TestPyPi package directory is the Beta Version of the FinX SDK.
>
> ___*UNTIL FURTHER NOTICE USE THE FOLLOWING INSTALLATION PATH*___
> 
> ```bash
> pip install -i https://test.pypi.org/simple/ finx-io
> ```

linux
```bash
#! bash
## linux
pip install -i https://test.pypi.org/simple/ finx-io
```
mac-os
```bash
#! bash
## mac-os assuming you use python3
python3 -m pip install -i https://test.pypi.org/simple/ finx-io
```

#### Testing Your Installation

FinX includes unit tests that will confirm that you can connect to the FinX Platform
and establish a secure socket connection.

To run the FinX unit tests:

```bash
#! bash
python3 setup.py test 
```

***

> Now that you have installed the FinX Python SDK, you are ready to use it in your code, in a jupyter notebook, or from a 
> command line.

## FinX SDK - Python

The Python FinX SDK contains classes and methods that connect to various dedicated computing resources on the FinX 
Platform. Depending on your use case, you may use one or more of these Client classes.

##### Clients

- finx.client.FinXClient
- finx.finx_streamer.Streamer
- finx.finx_rest.FinXRest

The functions and use of each client is documented separately.

***

The FinXClient is a websocket client that 
may be run synchronously ('sync' mode) or asynchronously ('async' mode).

Sync mode is a single call to the platform over websocket, including a connect, 
function call, and disconnect. This mode should only be used for one-time calls or 
for testing individual methods. For production purposes, use the Async mode.

Async mode is used in a check_event loop with a connection established, followed 
by streaming data and/or multiple message requests over the same connection. An 
example of how to use Async mode is documented below in the Jump Right In section 
where the run_tests method is included for reference.

#### FinX Websocket Endpoints

The websocket endpoints for FinX TickPlant are:


- TEST ENDPOINT: wss://beta.finx.io/ws/
- PROD ENDPOINT: wss://prod.finx.io/ws/

***

#### Import finx into python

You must set your environment variable 'FINX_API_KEY' to your assigned FinX API Key.

```bash
#! bash
export FINX_API_KEY=my_api_key
```

or, you may set up your environment variable 'FINX_API_KEY' in python directly:

Importing finx into python

```python
#! python
import os
from finx.finx_rest import FinXRest
from finx.client import FinXClient, Hybrid

@Hybrid
async def get_data():
    os.environ['FINX_API_KEY'] = input('---> ENTER FINX API KEY ---> ') 
    rest_client = FinXRest()
    bond_client = FinXClient("sockets", ssl=True)
    ref_data: dict = await rest_client.get_reference_data("BTC")
    bond_data: dict = await bond_client.get_security_reference_data(security_id='125523CJ7')
    pair_quote: dict = await rest_client.pair_quote("BTC:USDT")
    options_timeslice: dict = await rest_client.get_options_timeslice(timeslice_datestamp= "1661670000",timeslice_width_seconds=30,underlying_symbol="BTC")
    
    print(f'**********************\nBTC REFERENCE DATA: {ref_data}\n\n')
    print(f'**********************\nBOND REFERENCE DATA: {bond_data}\n\n')
    print(f'**********************\nBTC:USDT DATA: {pair_quote}\n\n')
    print(f'**********************\nBTC OPTIONS TIMESLICE: {options_timeslice}\n\n')

get_data()    


```

***
