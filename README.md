# Description

PoC to autenticate a mock service using OAuth 2

- **HAProxy**: Reverse proxy service version 3.0.2
- **Springboot**: Mock service. (Springboot 3.3.1)
- **Keycloak**: IAM service (24.0.4)

## Compile service and start compose stack
```
docker compose up -d --no-deps --build
```

## stop compose stack
```
docker compose down
```

## Create two users to test
We must create manually two users after keycloak start from Admin Portal UI. These are the credentials:

1. **Admin Credential**
- Credentials: admin/password
- Email: admin@oferto.io
- Firstname: Admin
- Lastname: Actor
- Role Admin

2. **User Credential**:
- Credentials: user/password
- Email: user@oferto.io
- Firstname: User
- Lastname: Actor
- Role: user

## Get a valid token

Get a valid token from Admin Credentials:
```
curl --location 'http://localhost:8080/realms/mock/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'client_id=mock' \
--data-urlencode 'username=admin' \
--data-urlencode 'password=password'
```

## test authenticated mock service

Test mock service authenticated from H2Proxy using the previous issued token 
```
curl --location 'http://localhost' \
--header "Authorization: Bearer <YOUR_ACCESS_TOKEN>"
```

If we access directly to the mock service we don't need any credentials
```
curl --location 'http://localhost:8088'
```

Possible results:

- **Try requuest without any token**: Missing Authorization HTTP header
- **Try to request with expired token**: JWT has expired
- **Try to request with a valid token**: Hello Mock
