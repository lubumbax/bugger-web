
# Bugger Web

Testing inclusion of `traceId/spanId` in logs by Micrometer Tracing, for servlet based applications. 

Micrometer Tracing automatically includes `traceId/spanId` in logs created from our controllers' endpoints (provided 
that we have configured `logging.pattern.level` for that - see `application.yaml`).  

In case that we want `traceId/spanId` also included in errors resulting from unhandled exceptions thrown while 
servicing our endpoints, we can implement `@ExceptionHandler(RuntimeException.class)` in a `@ControllerAdvice` and 
explicitly log exception from there.  
Micrometer Tracing seems to be including `traceId/spanId` for these logs too.

That's not the case though for _reactive/webflux_ based applications.  
See [Bugger Webflux](https://www.github.com/lubumbax/bugger-webflux) for more details.

### Test

Hit the bug:

```shell
http :8080/bug/0
```

That results in the following log:
```
2022-12-15T17:30:17.308+01:00  WARN [bugger-web,af4a3f717c342916ada49a1cc0cd4a8d,da6dcb44c928428e] 7501 --- [nio-8080-exec-4] com.example.bugger.BuggerApplication     : Possible bug in span af4a3f717c342916ada49a1cc0cd4a8d-da6dcb44c928428e 
2022-12-15T17:30:17.310+01:00 ERROR [bugger-web,af4a3f717c342916ada49a1cc0cd4a8d,da6dcb44c928428e] 7501 --- [nio-8080-exec-4] com.example.bugger.BuggerApplication     : / by zero
```
