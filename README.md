# lazada_api_example

Example of using Lazada Seller Center API on ruby language. 

```
require 'open-uri'
require 'openssl'
require 'cgi'

api_key = 'cc136e374a20a4051a1e0c36c18607729e4b0df29eb2c37b8792bb2eb7e74d4d'

params = {
  'Action': 'GetOrders',
  'CreatedAfter': (Time.zone.now - 1.days).iso8601,
  'Format': 'json',
  'Timestamp': Time.now.iso8601,
  'UserID': 'merchant@gowabi.com',
  'Version': '1.0'  
}.sort.to_h

concatenated = params.map{|k,v| "#{k}=#{CGI::escape(v)}"}.join('&')
params['Signature'] = OpenSSL::HMAC.hexdigest('SHA256', api_key, concatenated)

response = open("https://api.sellercenter.lazada.co.th?#{params.map{|k,v| "#{k}=#{CGI::escape(v)}"}.join('&')}").read
JSON.parse(response)
```
