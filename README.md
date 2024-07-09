# Description

PoC to autenticate a mock service using OAuth 2

- **HAProxy**: Reverse proxy service version 3.0.2
- **Springboot**: Mock Springboot service version 3.3.1
- **Keycloak**: IAM service version 24.0.4

## Compile springboot mock service
```
./mvnw clean install
```

## Create a network called consum for our stack
```
docker network create consum
```

## Compile services and start compose stack
```
docker compose up -d --no-deps --build
```

## Create two users to test
We must create manually two users accounts after keycloak start from Admin Portal UI. Access to Web Portal UI from:

```
http://localhost:8080
```

with credentials: admin/password

1. **Admin Account**
- Credentials: admin/password
- Email: admin@oferto.io
- Firstname: Admin
- Lastname: Actor
- Role: admin

2. **User Account**:
- Credentials: user/password
- Email: user@oferto.io
- Firstname: User
- Lastname: Actor
- Role: user

## Get a valid token

Get a valid token from Admin account:
```
curl --location 'http://localhost:8080/realms/mock/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'client_id=mock' \
--data-urlencode 'username=admin' \
--data-urlencode 'password=password'
```

## Test authenticated mock service

Test mock service authenticated from HAProxy using the previous issued token 
```
curl --location 'http://localhost' \
--header "Authorization: Bearer <YOUR_ACCESS_TOKEN>"
```

If we access directly to the mock service we don't need any credentials
```
curl --location 'http://localhost:8088'
```

Possible results:

- **Try mock request without any token**: Missing Authorization HTTP header
- **Try mock request with expired token**: JWT has expired
- **Try mock request with expired token not well formed**: Unsupported JWT signing algorithm
- **Try mock request with a valid token**: Hello Mock

## Stop and remove stack from compose
```
docker compose down
```
