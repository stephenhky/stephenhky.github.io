# fininfoestimate

Aim: estimate various performance metrics for a given symbol
Input parameters: symbol, start date, end date

Example input:

```
GET /default/fininfoestimate HTTP/1.1
Host: 1phrvfsc16.execute-api.us-east-1.amazonaws.com
Content-Type: application/json
Content-Length: 70

{"symbol": "TSLA", "startdate": "2019-01-01", "enddate": "2021-03-03"}
```

Corresponding output:

```
{
    "symbol": "TSLA",
    "r": 0.8768608315711468,
    "vol": 0.7516887431835725,
    "downside_risk": 0.5216635625645077,
    "upside_risk": 0.5444380568397881,
    "beta": 1.1613301756015018,
    "data_startdate": "2019-01-02",
    "data_enddate": "2021-03-03",
    "nbrecs": 546
}
```

# Portfolio optimization

Aim: optimize a portfolio from a given list of symbols:
Input: 
- start date
- end date
- maximum value of the portfolio
- list of symbols
- number of steps (suggested value: from 1000 to 10000)
- whether to include dividends in estimation
- lambda1 (how much risk to suppress; suggested value: 0.3)
- lambda2 (diversification parameter; suggested value: 0.01)
- user e-mail

Computation result will not be returned in the API, but through the
e-mail address that the user provided. Computation takes 30 seconds to 15 minutes.

Example input:

```
POST /default/portfolio-simulated-annealing-starter HTTP/1.1
Host: g28005k4u1.execute-api.us-east-1.amazonaws.com
Content-Type: application/json
Content-Length: 261

{
    "startdate": "2019-01-01",
    "enddate": "2021-03-19",
    "maxval": 10000,
    "symbols": ["NVDA", "LMT", "COST", "AVGO"],
    "nbsteps": 7500,
    "with_dividents": true,
    "lambda1": 0.3,
    "lambda2": 0.01,
    "email": "stephenhky@yahoo.com.hk"
}
```

Corresponding output:

```
{
    "startdate": "2019-01-01",
    "enddate": "2021-03-19",
    "maxval": 10000,
    "symbols": [
        "NVDA",
        "LMT",
        "COST",
        "AVGO"
    ],
    "nbsteps": 7500,
    "with_dividents": true,
    "lambda1": 0.3,
    "lambda2": 0.01,
    "email": "stephenhky@yahoo.com.hk",
    "init_temperature": 1000.0,
    "decfactor": 0.75,
    "temperaturechange_step": 100,
    "with_dividends": true,
    "index": "DJI",
    "filebasename": "20210323040251UTC_aouzncezdxnnfgqxflbd",
    "sender_email": "finportlag@gmail.com",
    "email_body": "<html>\n    <head></head>\n    <body>\n      <h1>Portfolio Optimization Computation Started</h1>\n          <p>\n              The optimization for the financial portfolio with the following symbols\n          have been started: NVDA, LMT, COST, AVGO.\n          </p>\n          <p>\n              The optimization is based on the prices between 2019-01-01 and 2021-03-19,\n          with maximum total value of the portfolio being USD$10000.\n          </p>\n          <p>\n              Other parameters for the computation using simulated annealing:\n              <ul>\n                <li>Number of steps: 7500</li>\n                <li>Initial temperature: 1000.0 (temperature decrease every 100 by a factor 0.75)</li>\n                <li>Income from dividends considered: Yes</li>\n                <li>lambda1 (regularization with volatility): 0.3</li>\n                <li>lambda2 (regularization with uniformity): 0.01</li>\n                <li>beta computed with respect to DJI</li>\n              </ul>\n          </p>\n          <p>\n              An e-mail will be sent to this e-mail address once the computation is completed.\n          </p>\n          <p>\n              Control: 20210323040251UTC_aouzncezdxnnfgqxflbd\n          </p>\n    </body>\n    </html>"
}
```