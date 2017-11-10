# Get Token

At first you need to get a session token for the further request.

## Request

```
POST /oauth/token HTTP/1.1
Host: connect-testing.secupay-ag.de
Content-Type: application/x-www-form-urlencoded
Cache-Control: no-cache

grant_type=client_credentials&client_id=61c4e67fca435d864436fc174a0c309d&client_secret=0ecfb081e6af8c5bae90923ec8fa384b83bb7097c21ee1457fad0ab994a81617
```

## Response

```js
{
    "access_token": "biu3647lorggqhhoh46qsou202",
    "expires_in": 1200,
    "token_type": "bearer",
    "scope": "https://scope.secucard.com/e/api"
}
```
