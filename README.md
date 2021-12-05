# Mock auth UI

Mock auth UI provides generic authentication simulation interface controlled through the GET parameter
`login_hint`. The user input is returned through callback redirect inside the GET parameter `code` as
base64 encoded JSON.

![Mock auth UI](mock-auth-ui.png)

[MockASPSP](https://enablebanking.com/docs/sdk/latest/#mockaspspconnectorsettings-type) and
[MockASPSPBusiness](https://enablebanking.com/docs/sdk/latest/#mockaspspbusinessconnectorsettings-type)
connectors are using this UI for authorization methods using REDIRECT approach.

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/enablebanking/mock-auth-ui)

## Data format

Desired credentials to be displayed are to be passed into the GET parameter `login_hint`. The `code`
parameter in the callback redirect URI (set via `redirect_uri`) will contain base64 encoded JSON object
with the data input into the generate form (in the field `credentials`) and some additional data.

### Example 1

Login hint:

```javascript
base64({"credentials":[{"name":"userId","title":"User ID","description":"User ID description"}]})
```

Mock auth URL:

https://mock-auth-ui.netlify.app/?login_hint=eyJjcmVkZW50aWFscyI6W3sibmFtZSI6InVzZXJJZCIsInRpdGxlIjoiVXNlciBJRCIsImRlc2NyaXB0aW9uIjoiVXNlciBJRCBkZXNjcmlwdGlvbiJ9XX0&redirect_uri=https%3A%2F%2Fexample.com%2F&state=random

Callback URL:

https://example.com/?code=eyJjcmVhdGVkQXQiOjE2Mzg3MzkxMTksImNyZWRlbnRpYWxzIjp7InVzZXJJZCI6IjEyMyJ9fQ%3D%3D&state=random

```javascript
code == base64({"createdAt":1638739119,"credentials":{"userId":"123"}})
```

### Example 2

Login hint:

```javascript
base64({"credentials":[{"name":"email","title":"E-mail"},{"name":"password","title":"Password"}]})
```

Mock auth URL:

https://mock-auth-ui.netlify.app/?login_hint=eyJjcmVkZW50aWFscyI6W3sibmFtZSI6ImVtYWlsIiwidGl0bGUiOiJFLW1haWwifSx7Im5hbWUiOiJwYXNzd29yZCIsInRpdGxlIjoiUGFzc3dvcmQifV19&redirect_uri=https%3A%2F%2Fexample.com%2F&state=random

Callback URL:

https://example.com/?code=eyJjcmVhdGVkQXQiOjE2Mzg3MzkwMDksImNyZWRlbnRpYWxzIjp7ImVtYWlsIjoiYWJjQGV4YW1wbGUuY29tIiwicGFzc3dvcmQiOiJhYmMifX0%3D&state=random

```javascript
code == base64({"createdAt":1638739009,"credentials":{"email":"abc@example.com","password":"abc"}})
```

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```
