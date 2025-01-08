---
date:
  - 14/05/2024 14:00
tags:
  - Yandex
  - CI
  - issue
  - error
  - vvdev/betting
cssclasses:
  - image-borders
---
# Pipeline failure

## Front
```shell
error parsing HTTP 408 response body: invalid character 's' looking for beginning of value: "stream timeout\nrequest-id: 02becc19-2cd7-49e9-be3d-2f65683967d4\ntrace-id: 8251c0b667788715:93a108c2097758d9:8251c0b667788715:1\n"
```
## Back
```shell
fatal: [pepper]: FAILED! => {"changed": false, "errors": [], "module_stderr": "", "module_stdout": "", "msg": "Error starting project 500 Server Error for http+docker://localhost/v1.43/images/create?tag=develop-81&fromImage=cr.yandex%2Fcrpqu4ks5tbc7je2mnqu%2Fvolga-volga-pepper-ipepper-news-multi-news-bettings-news-betts: Internal Server Error (\"Head \"[https://cr.yandex/v2/crpqu4ks5tbc7je2mnqu/volga-volga-pepper-ipepper-news-multi-news-bettings-news-betts/manifests/develop-81](https://cr.yandex/v2/crpqu4ks5tbc7je2mnqu/volga-volga-pepper-ipepper-news-multi-news-bettings-news-betts/manifests/develop-81)\": Get \"[https://cr.yandex/v2/token/?account=json_key&scope=repository%3Acrpqu4ks5tbc7je2mnqu%2Fvolga-volga-pepper-ipepper-news-multi-news-bettings-news-betts%3Apull&service=cr.yandex](https://cr.yandex/v2/token/?account=json_key&scope=repository%3Acrpqu4ks5tbc7je2mnqu%2Fvolga-volga-pepper-ipepper-news-multi-news-bettings-news-betts%3Apull&service=cr.yandex)\": context deadline exceeded (Client.Timeout exceeded while awaiting headers)\")"}
```

| <mark style="background: #FF5582A6;">Problem</mark>                   | <mark style="background: #BBFABBA6;">Solution</mark> |
| --------------------------------------------------------------------- | ---------------------------------------------------- |
| The problem appeared due to the ==Yandex Registry== technical issues. | Just wait and try again 👍                           |
