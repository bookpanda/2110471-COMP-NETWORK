# 2110471-COMP-NETWORK

```bash
# netcat
# -c: Close the connection when EOF (ctrl+z) is received
# -v verbose
nc -c -v gaia.cs.umass.edu 80

# can type in request
GET /kurose_ross/interactive/index.php HTTP/1.1
Host: gaia.cs.umass.edu

# find local dns server
scutil --dns


# dig: The DNS lookup utility
# @8.8.8.8: Specifies Google's public DNS server as the resolver
# SOA: Requests the Start of Authority record
# +noall: Disables all output sections by default
# +answer: Re-enables just the answer section in the output
# www.wongnai.com: The domain name you're querying
dig @8.8.8.8 SOA +noall +answer www.wongnai.com
```