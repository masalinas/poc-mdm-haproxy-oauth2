# Description

PoC to autenticate a mock service using OAuth 2

- **HAProxy**: Reverse proxy service version 3.0.2
- **Springboot**: Mock Springboot service version 3.3.1
- **Keycloak**: IAM service version 24.0.4

## Compile service and start compose stack
```
docker compose up -d --no-deps --build
```

## Stop compose stack services
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

## Notes

I had to start HAProxy as root inside the container if not errors like this happend after 2.4 version.

```
[NOTICE]   (1) : haproxy version is 3.0.2-a45a8e6
[NOTICE]   (1) : path to executable is /usr/local/sbin/haproxy
[WARNING]  (1) : config : unexpected character '�' after the timer value '10sº', only (us=microseconds,ms=milliseconds,s=seconds,m=minutes,h=hours,d=days) are supported. This will be reported as an error in next versions.
[ALERT]    (1) : Binding [/usr/local/etc/haproxy/haproxy.cfg:4] for frontend GLOBAL: protocol unix_stream: cannot bind UNIX socket (Permission denied) [/var/run/api.sock].
[ALERT]    (1) : [haproxy.main()] Some protocols failed to start their listeners! Exiting.
```
