# Axios

token 

```
https://axios-http.com/kr/docs/config_defaults
```

[Config 기본값 | Axios Docs](https://axios-http.com/kr/docs/config_defaults)

유저 인증을 위한 토큰을 front에서 저장했다가 보내후 back 작업이 필요하다면 

```typescript
const accessToken = localStorage.getItem('accessToken')
axios.defaults.headers.common['Authorization'] = "Bearer " + accessToken
```

그후 jwt토큰을 위한 작업을 nestjs 쪽에서 해줘야함 

## interceptors (재요청)

```
https://medium.com/zigbang/%EC%9A%B0%EB%A6%AC-axios%EC%97%90%EA%B2%8C-%EB%8B%A4%EC%8B%9C-%ED%95%9C-%EB%B2%88-%EA%B8%B0%ED%9A%8C%EB%A5%BC-%EC%A3%BC%EC%84%B8%EC%9A%94-a7b32f4f2db2
```
