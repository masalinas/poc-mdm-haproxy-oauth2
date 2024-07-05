# Description

PoC to autenticate a mock service using OAuth 2

- **HAProxy**: Reverse proxy service.
- **Springboot**: Mock service.
- **Keycloak**: IAM service.

## compile service and start compose stack
```
docker compose up -d --no-deps --build
```

## stop compose stack
```
docker compose down
```

## create two users
We must create manually two users in the real imported

- Account Admin:
  Credentials: admin/password
  Email: admin@oferto.io
  Firstname: Admin
  Lastname: Actor
  Role Admin

- Account User:
  Credentials: user/password
  Email: user@oferto.io
  Firstname: User
  Lastname: Actor
  Role: user

## get a valid token
```
curl --location 'http://localhost:8080/realms/mock/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'client_id=mock' \
--data-urlencode 'username=admin' \
--data-urlencode 'password=password'
```
