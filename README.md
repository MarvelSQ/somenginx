# somenginx

about nginx

## find nginx conf

default config file position `/usr/local/etc/nginx/nginx.conf`

## set local https

1. generate certification
   - [ ] cert.key  
          in MacOS
   ```shell
   # generate cert.key
   openssl genrsa > cert.key
   ```
   - [ ] cert.pem
   ```shell
   # generate cert.pem
   openssl req -new -x509 -key cert.key > cert.pem
   ```
   > nginx will try need `-x509` config
2. config nginx.conf

   ```nginx
   server {
           listen       443 ssl;
           server_name  <you domain>;

           ssl_certificate      /Users/sq/nginx/cert.pem;
           ssl_certificate_key  /Users/sq/nginx/cert.key;

           ssl_session_cache    shared:SSL:1m;
           ssl_session_timeout  5m;

           location / {
               proxy_pass http://localhost:8000;
           }
   }
   ```

   try test the config

   ```shell
   nginx -t
   ```

3. change host

   - macOS  
     edit file in `/private/etc/hosts` 
       
     ```vi
     # proxy host
     127.0.0.1 google.com
     ```
     or you can use [SwicthHosts](https://swh.app/)

4. bypass your chrome.  
   while you try to proxy some https content, chrome will block you.
   you can type `thisisunsafe` ([twitter](https://twitter.com/zairwolf/status/1196878125734486021)) to proceed
5. need clean dns cache  
   cause of browser keep dns in cache, you may need to flush cache
   - [chrome://net-internals/#dns](chrome://net-internals/#dns) clean dns cache
   - [chrome://net-internals/#sockets](chrome://net-internals/#sockets) break connnection
