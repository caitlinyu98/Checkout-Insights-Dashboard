# Checkout Insights Dashboard

A lightweight **payments analytics** app (Streamlit + Python) that helps product teams visualize **checkout performance**, monitor **payment success**, and spot **revenue leaks** (declines, timeouts, retries).

**Why it exists:** PMs and growth teams need fast visibility into conversion drivers without wrestling with heavy BI stacks.

## ✨ Key Features (v1)
- KPI tiles: **Success Rate**, **Authorization Rate**, **Capture Rate**, **Retry Recovery Rate**, **Revenue at Risk**
- Filters: date range, provider, payment method, country, channel
- Charts: time series of success rate, decline-code distribution, provider mix, latency distribution
- Simple CSV input (`data/transactions.csv`) so anyone can try it immediately

## 🔢 Core KPIs
- **Success Rate** = `Succeeded / (Auth + Capture + Succeeded + Failed + Canceled)`
- **Authorization Rate** = `Authorized / Attempted`
- **Capture Rate** = `Captured / Authorized`
- **Retry Recovery Rate** = `Succeeded_after_retry / Failed_initially`
- **Revenue at Risk (est.)** = `Σ amount for failed or timed-out attempts`
- **P95 Latency** = 95th percentile of `latency_ms`

> Adjust definitions per your org; the app computes and explains formulas inline.

## 🗂️ Data Schema (`data/transactions.csv`)
| column | type | example |
|---|---|---|
| transaction_id | string | tr_0001 |
| timestamp | ISO8601 | 2025-10-20T14:32:05Z |
| merchant_id | string | m_123 |
| amount | float | 49.99 |
| currency | string | USD |
| country | string (ISO2) | US |
| channel | string | web / ios / android / pos |
| device | string | desktop / mobile |
| payment_method | string | card / wallet / bank_transfer |
| provider | string | stripe / adyen / checkout |
| status | string | authorized / captured / succeeded / failed / canceled / timeout |
| error_code | string | do_not_honor / insufficient_funds / n/a |
| retry_number | int | 0,1,2… |
| latency_ms | int | 842 |
| customer_id | string | c_789 |

### Minimal sample rows
```csv
transaction_id,timestamp,merchant_id,amount,currency,country,channel,device,payment_method,provider,status,error_code,retry_number,latency_ms,customer_id
tr_0001,2025-10-20T14:32:05Z,m_001,49.99,USD,US,web,desktop,card,stripe,succeeded,n/a,0,820,c_001
tr_0002,2025-10-20T14:33:10Z,m_001,19.00,USD,US,ios,mobile,card,adyen,failed,do_not_honor,0,950,c_002
tr_0003,2025-10-20T14:35:41Z,m_002,120.00,USD,CA,web,desktop,card,stripe,authorized,n/a,0,640,c_003
tr_0004,2025-10-20T14:36:02Z,m_001,19.00,USD,US,ios,mobile,card,adyen,succeeded,n/a,1,880,c_002
tr_0005,2025-10-20T14:37:55Z,m_003,15.50,USD,PH,android,mobile,wallet,checkout,timeout,network_error,0,3000,c_004

