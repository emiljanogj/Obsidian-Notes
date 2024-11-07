- The server side visits a URL controlled by the user
- https://blog.appsecco.com/an-ssrf-privileged-aws-keys-and-the-capital-one-breach-4c3c2cded3af
- https://www.hackerone.com/application-security/how-server-side-request-forgery-ssrf


- A mitigation usually implemented against SSRFs is the one 
```
**require 'sinatra'**  
**require 'open-uri'**  
  
**get '/' do**  
  **url = URI.parse params[:url]**  
  
  **halt 403 if url.host =~ /\A10\.0\.0\.\d+\z/**  
  
  **format 'RESPONSE: %s', open(params[:url]).read**  
**end**
```

- We can avoid the following mitigation by using the decimal IP notation `http://167772162/`
instead of `http://10.0.0.2`
- Another way to avoid it is by creating a DNS A record that points to `10.0.0.2` and use `http://subdomain.yourdomain.com`
- Another useful link: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery