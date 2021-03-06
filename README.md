# currencycloud-python

[![Build Status](https://travis-ci.org/CurrencyCloud/currencycloud-python.png?branch=master)](https://travis-ci.org/CurrencyCloud/currencycloud-python)
[![PyPi Status](https://img.shields.io/pypi/v/currencycloud.svg)](https://pypi.python.org/pypi/currencycloud)

# Currencycloud

This is the official Python SDK for v2 of Currencycloud's API.
Additional documentation for each API endpoint can be found at
[developer.currencycloud.com](https://developer.currencycloud.com/documentation/getting-started/introduction/).
If you have any queries or you require support, please contact our
sales team at sales@currencycloud.com.

The full source code, tests and examples can be always found on
[github](https://github.com/CurrencyCloud/currencycloud-python).

## Installation

We supports installation using standard Python "distutils" or
"setuptools" methodologies. An overview of potential setups is as
follows:

-  **pip** - [pip](http://pypi.python.org/pypi/pip/) is an installer
   that rides on top of setuptools or distribute, replacing the usage of
   easy\_install. It is often preferred for its simpler mode of usage.
-  **Plain Python Distutis** - The library can be installed with a clean
   Python install using the services provided via Python Distutils,
   using the setup.py script.
-  **Setuptools or Distribute** - When using
   [setuptools](https://pypi.python.org/pypi/setuptools/), the
   library can be installed via setup.py or easy_install.

### Install via pip

When pip is available, the distribution can be downloaded from PyPi and
installed in one step:

    pip install currency_cloud

This command will download the latest released version of the library
from the Python Cheese Shop and install it to your system.

### Install using setup.py

Otherwise, you can install from the distribution using the setup.py
script:

    python setup.py install

## Supported Python versions

This library aims to support and is [tested](https://travis-ci.org/CurrencyCloud/currencycloud-python)
the following Python implementations:

-  CPython 2.6
-  CPython 2.7
-  CPython 3.3
-  CPython 3.4
-  CPython 3.5
-  CPython 3.6

## Usage

```python
>>> import currencycloud

## Configure and instantiate the Client ##
>>> login_id = '<your login id>'
>>> api_key = '<your api key>'
>>> environment = currencycloud.Config.ENV_DEMONSTRATION # use currencycloud.ENV_PRODUCTION when ready
>>> client = currencycloud.Client(login_id, api_key, environment)

## Make API calls ##
>>> currencies = client.reference.currencies()
>>> currencies
[<currencycloud.resources.reference.Currency object at 0x10e6fd190>,
<currencycloud.resources.reference.Currency object at 0x10e6fd1d0>,
<currencycloud.resources.reference.Currency object at 0x10e6fd2d0>,
…
<currencycloud.resources.reference.Currency object at 0x10e6fd9d0>]

>>> balances = client.balances.find()
>>> balances
[<currencycloud.resources.balance.Balance object at 0x10e6fd7d0>]

>>> balances.pagination
<currencycloud.resources.pagination.Pagination object at 0x10b15d6a0>

>>> balances[0].currency
'GBP'

>>> currency_usd = client.balances.for_currency("USD")
>>> currency_usd
<currencycloud.resources.balance.Balance object at 0x10cddcc50>

## Access attributes ##
>>> currency_usd.currency
'USD'

>>> currency_usd['currency']
'USD'
```

### On Behalf Of


If you want to make calls on behalf of another user (e.g. someone who has a sub-account with you), you can execute certain commands 'on behalf of' the user's contact_id. Here is an example:

```python
with client.on_behalf_of('c6ece846-6df1-461d-acaa-b42a6aa74045') as new_client:
	beneficiary = new_client.beneficiaries.create(<params>)
	conversion = new_client.conversions.create(<params>)
	payment = new_client.payments.create(<params>)
```

Alternatively, you can just add ``on_behalf_of`` to the call parameters,
for example:

```python
    client.accounts.create(account_name='My Test User', on_behalf_of='c6ece846-6df1-461d-acaa-b42a6aa74045')
```

Each of the above transactions will be executed in scope of the permissions
for that contact and linked to that contact. Note that the real user who
executed the transaction will also be stored.

### Errors

When an error occurs in the API, the library aims to give us much
information as possible. Here is an example:

```yaml
BadRequestError
---
errors:
- code: term_agreement_is_required
  field: term_agreement
  message: term_agreement is required
  params: {}
- code: term_agreement_type_is_wrong
  field: term_agreement
  message: term_agreement should be of boolean type
  params:
    type: boolean
platform: python - 2.7.6 (default, Sep  9 2014, 15:04:36) - CPython
request:
  parameters:
    amount:
      - '1000'
    buy_currency:
      - GBP
    fixed_side:
      - buy
    reason:
      - mortage
    sell_currency:
      - USD
  url: https://devapi.currencycloud.com/v2/conversions/create
  verb: post
response:
  date: Thu, 25 Jun 2015 16:46:42 GMT
  request_id: 2816384323363505615
  status_code: 400
```

This is split into 5 sections:

1. Error Type: In this case `BadRequestError` represents an HTTP 400 error
2. Platform: The Python implementation that was used e.g. 'python - 2.7.6'
3. Request: Details about the HTTP request that was made e.g. the POST parameters
4. Response: Details about the HTTP response that was returned e.g. HTTP status code
5. Errors: A list of errors that provide additional information

The errors section contains valuable information:

-  Field: The parameter that the error is linked to
-  Code: A code representing this error
-  Message: A human readable message that explains the error
-  Params: A hash that contains dynamic parts of the error message for building custom error messages

When troubleshooting API calls with Currencycloud support, including
the full error in any correspondence can be very helpful.

## Development

To manage Python environments and dependencies we use [pipenv](https://pipenv.readthedocs.org/en/latest/) and [tox](https://tox.readthedocs.org/en/latest/) to run the tests. Both can be easily installed with pip.

    pip install pipenv
    pip install tox

To run the tests:

    tox

## Dependencies

-  [requests](http://docs.python-requests.org/en/latest/)
-  [pyYAML](http://pyyaml.org/)
-  [attrdict](https://pypi.python.org/pypi/attrdict/2.0.0/)

## Versioning

This project uses [semantic versioning](http://semver.org/). You can
safely express a dependency on a major version and expect all minor and
patch versions to be backwards compatible.

## Copyright

Copyright (c) 2017 Currencycloud. See [LICENSE](LICENSE) for
details.
