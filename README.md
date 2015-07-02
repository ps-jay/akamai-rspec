# akamai-rspec

Use rspec to test your akamai configuration.

## How to use

### Basic configuration
To use, you must configure you domain:
```
RestClient::Request.domain("www.example.com")
```
Where my-domain does not include '.edgesuite.net' and '.edgesuite-staging.net', because the library
adds them automatically.

If you need to set both the staging and prod domains manually, you can use:

```
RestClient::Request.prod_domain("www.example.com.edgesuite.net")
RestClient::Request.stg_domain("www.example.com.edgesuite-staging.net")
```

The default akamai network used is prod, to test in staging you must specify.
```
RestClient::Request.akamai_network("staging")
```

### matchers

#### ``` be_permanently_redirected_to ```
``` expect(old).to be_permanently_redirected_to(new) ```
Requires the response code to be 301, and redirect to new

#### ``` be_temporarily_redirected_to ```
The same as be_permanently_redirected_to, except expecting a 302

#### ``` be_temporarily_redirected_to_with_trailing_slash ```
The same as be_temporarily_redirected_to, but also expect the response location to have a '/' added

#### ``` be_cacheable ```
Requests to the url with the akamai debug headers should have X-Check-Cacheable as yes in the
response headers

#### ``` have_no_cache_set ```
```
response = RestClient.get(url)
expect(response).to have_no_cache_set
```
Check that the header ```Cache-control = no-cache```

#### ``` not_be_cached ```
```expect(url).to not_be_cached```
Requests the resource twice, and check that the 'x-cache' header says miss, and response code is 200

#### ``` be_successful ```
```
response = RestClient.get(url)
expect(url).to be_successful
expect(response).to be_successful
```
expect a response code of 200

#### ``` be_verifiably_secure ```
```expect(url).to be_verifiably_secure```
The SSL cert can be verified

#### ``` be_served_from_origin ```
```expect(url).to be_served_from_origin origin```
Response code is 200 and 'x_cache_key' header to include origin url

#### ``` honour_origin_cache_headers```
```expect(url).to honour_origin_cache_headers origin```
Check that akamai and origin cache headers correspond, and takes in to account expected differences.

#### ``` be_forwarded_to_index ```
```expect(url + “/” channel).to be_forwarded_to_index(channel)```
- Expect response to have x-akamai-session-info
- Expect AKA_PM_FWD_URL header to end in channel, which means it is being passed to origin
- Response code is 200

#### ``` be_tier_distributed ```
```expect(response).to be_tier_distributed```
Forces a cache miss with a query string and checks that 'x_cache_remote' header is set.

#### ``` be_gzipped ```
```expect(url).to be_gzipped```
Expect the response to be gzipped.

#### ``` have_cookie ```
```expect(response).to have_cookie cookie```
Expect the response to contain the specified cookie

#### ``` have_cp_code ```
```expect(url).to have_cp_code(cp_code)```
- Check that cache key contains Content Provider Code
- 200 response

# making requests

## Contributions
We would be very thankful for any contributions, particularly documentation or tests.

## License
Copyright (C) 2015 REA Group Ltd.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
