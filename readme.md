To reproduce http_server_active_requests issue with TLS Terminating Load Balancer

Run this project with `dotnet run`

View metrics at `http://localhost:81/metrics`

Make a test request similar to how a TLS-terminating load balancer would:

```
invoke-webrequest -Uri http://localhost:81/ -Headers @{'X-Forwarded-For' = '127.0.0.1'; 'X-Forwarded-Proto' = 'https';}
```

view metrics, `http.server.active_requests` will look like this:

```
http_server_active_requests{otel_scope_name="Microsoft.AspNetCore.Hosting",http_request_method="GET",url_scheme="http"} 4 1713790866344
http_server_active_requests{otel_scope_name="Microsoft.AspNetCore.Hosting",http_request_method="GET",url_scheme="https"} -3 1713790866344
```

I.e. postitive number for `http` requests, negative number for `https` requests