# 액츄에이터


프로덕션 준비 기능이란?

`전투에서 실패한 지휘관은 용서할 수 있지만 경계에서 실패하는 지휘관은 용서할 수 없다` 라는말이있다.

이말을 서비스를 운영하는 개발자에게 맞추어 보면 장애는 언제든지 발생할 수 있다. 

하지만 모니터링(경계)은 잘 대응하는 것이 중요하다.

개발자가 애플리케이션을 개발할 때 기능 요구사항만 개발하는 것은 아니다. 

서비스를 실제 운영 단계에 올리게 되면 개 발자들이 해야하는 또 다른 중요한 업무가 있다. 

바로 서비스에 문제가 없는지 모니터링하고 지표들을 심어서 감시하는 활동들이다.

운영 환경에서 서비스할 때 필요한 이런 기능들을 프로덕션 준비 기능이라 한다. 

쉽게 이야기해서 프로덕션을 운영에 배 포할 때 준비해야 하는 비 기능적 요소들을 뜻한다.

지표(metric), 추적(trace), 감사(auditing) 모니터링 좀 더 구제적으로 설명하자면, 

애플리케이션이 현재 살아있는지, 로그 정보는 정상 설정 되었는지, 커넥션 풀은 얼마나 사용되고 있는지 등을 확인할 수 있어야 한다.

스프링 부트가 제공하는 액추에이터는 이런 프로덕션 준비 기능을 매우 편리하게 사용할 수 있는 다양한 편의 기능들을 제공한다. 

더 나아가서 마이크로미터, 프로메테우스, 그라파나 같은 최근 유행하는 모니터링 시스템과 매우 쉽게 연동할 수 있는 기능도 제공한다.

참고로 액추에이터는 시스템을 움직이거나 제어하는 데 쓰이는 기계 장치라는 뜻이다. 

여러 설명보다 한번 만들어서 실행해보는 것이 더 빨리 이해가 될 것이다.

```shell
http://localhost:8080/actuator 실행
```

* 실행 결과

```json
{
  "_links": {
     "self": {
       "href": "http://localhost:8080/actuator",
       "templated": false
     },
     "health-path": {
       "href": "http://localhost:8080/actuator/health/{*path}",
       "templated": true
     },
     "health": {
       "href": "http://localhost:8080/actuator/health",
       "templated": false
     }
  }
}
```


액츄에이터는 `/actuator` 경로를 통해서 기능을 제공한다. 

화면에 보이는 `health` 결과를 제공하는 다음 URL도 실행해보자.

```shell
http://localhost:8080/actuator/health
```

```json
{
  "status": "UP"
} 
```

이 기능은 현재 서버가 잘 동작하고 있는지 애플리케이션의 헬스 상태를 나타낸다.

지금 눈에 보이는 기능은 헬스 상태를 확인할 수 있는 기능 뿐이다. 

액츄에이터는 헬스 상태 뿐만 아니라 수 많은 기능을 제공하는데, 이런 기능이 웹 환경에서 보이도록 노출해야 한다.


application.yml 파일에 추가하자.

```yaml
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

**동작 확인**

기본 메인 클래스 실행( `ActuatorApplication.main()` ) 

```shell
http://localhost:8080/actuator
```


```json
{
  "_links": {
    "self": {
      "href": "http://localhost:8080/actuator",
      "templated": false
    },
    "beans": {
      "href": "http://localhost:8080/actuator/beans",
      "templated": false
    },
    "caches-cache": {
      "href": "http://localhost:8080/actuator/caches/{cache}",
      "templated": true
    },
    "caches": {
      "href": "http://localhost:8080/actuator/caches",
      "templated": false
    },
    "health-path": {
      "href": "http://localhost:8080/actuator/health/{*path}",
      "templated": true
    },
    "health": {
      "href": "http://localhost:8080/actuator/health",
      "templated": false
    },
    "info": {
      "href": "http://localhost:8080/actuator/info",
      "templated": false
    },
    "conditions": {
      "href": "http://localhost:8080/actuator/conditions",
      "templated": false
    },
    "configprops": {
      "href": "http://localhost:8080/actuator/configprops",
      "templated": false
    },
    "configprops-prefix": {
      "href": "http://localhost:8080/actuator/configprops/{prefix}",
      "templated": true
    },
    "env": {
      "href": "http://localhost:8080/actuator/env",
      "templated": false
    },
    "env-toMatch": {
      "href": "http://localhost:8080/actuator/env/{toMatch}",
      "templated": true
    },
    "loggers": {
      "href": "http://localhost:8080/actuator/loggers",
      "templated": false
    },
    "loggers-name": {
      "href": "http://localhost:8080/actuator/loggers/{name}",
      "templated": true
    },
    "heapdump": {
      "href": "http://localhost:8080/actuator/heapdump",
      "templated": false
    },
    "threaddump": {
      "href": "http://localhost:8080/actuator/threaddump",
      "templated": false
    },
    "metrics": {
      "href": "http://localhost:8080/actuator/metrics",
      "templated": false
    },
    "metrics-requiredMetricName": {
      "href": "http://localhost:8080/actuator/metrics/{requiredMetricName}",
      "templated": true
    },
    "scheduledtasks": {
      "href": "http://localhost:8080/actuator/scheduledtasks",
      "templated": false
    },
    "mappings": {
      "href": "http://localhost:8080/actuator/mappings",
      "templated": false
    }
  }
}
```

액츄에이터가 제공하는 수 많은 기능을 확인할 수 있다.

액츄에이터가 제공하는 기능 하나하나를 엔드포인트라 한다. 

`health` 는 헬스 정보를, `beans` 는 스프링 컨테이너에 등록된 빈을 보여준다.

각각의 엔드포인트는 `/actuator/{엔드포인트명}` 과 같은 형식으로 접근할 수 있다. 

`http://localhost:8080/actuator/health` : 애플리케이션 헬스 정보를 보여준다. 

`http://localhost:8080/actuator/beans` : 스프링 컨테이너에 등록된 빈을 보여준다.


## 앤드포인트 설정

엔드포인트를 사용하려면 다음 2가지 과정이 모두 필요하다.

1. 엔드포인트 활성화
2. 엔드포인트 노출

엔드포인트를 활성화 한다는 것은 해당 기능 자체를 사용할지 말지 `on` , `off` 를 선택하는 것이다.

엔드포인트를 노출하는 것은 활성화된 엔드포인트를 HTTP에 노출할지 아니면 JMX에 노출할지 선택하는 것이다. 

엔드포인트를 활성화하고 추가로 HTTP를 통해서 웹에 노출할지, 아니면 JMX를 통해서 노출할지 두 위치에 모두 노출 할지 노출 위치를 지정해주어야 한다.

물론 활성화가 되어있지 않으면 노출도 되지 않는다.

그런데 **엔드포인트는 대부분 기본으로 활성화** 되어 있다.

( `shutdown` 제외) 노출이 되어 있지 않을 뿐이다.

따라서 어떤 엔드포인트를 노출할지 선택하면 된다. 

참고로 HTTP와 JMX를 선택할 수 있는데, 보통 JMX는 잘 사용하지 않으므로 HTTP에 어떤 엔드포인트를 노출할지 선택하면 된다.


**application.yml - 모든 엔드포인트를 웹에 노출** 

```yaml
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

`"*"` 옵션은 모든 엔드포인트를 웹에 노출하는 것이다. 

참고로 `shutdown` 엔드포인트는 기본으로 활성화 되지 않기 때문에 노출도 되지 않는다.

**엔드포인트 활성화 + 엔드포인트 노출이 둘다 적용되어야 사용할 수 있다.**

**엔드포인트 활성화**

**application.yml - shutdown 엔드포인트 활성화**

```yaml
management:
  endpoint:
    shutdown:
      enabled: true
  endpoints:
    web:
      exposure:
        include: "*"
```

특정 엔드포인트를 활성화 하려면 `management.endpoint.{엔드포인트명}.enabled=true` 를 적용하면 된다. 

이제 Postman 같은 것을 사용해서 HTTP **POST**로 `http://localhost:8080/actuator/shutdown` 를 호출하면 다음 메시지와 함께 실제 서버가 종료되는 것을 확인할 수 있다.

참고로 HTTP GET으로 호출하면 동작하지 않는다.

물론 이 기능은 주의해서 사용해야 한다. 그래서 기본으로 비활성화 되어 있다.

**엔드포인트 노출**

스프링 공식 메뉴얼이 제공하는 예제를 통해서 엔드포인트 노출 설정을 알아보자

```yaml
management:
  endpoints:
    jmx:
      exposure:
        include: "health,info"
```

`jmx` 에 `health,info` 를 노출한다.

```yaml
management:
  endpoints:
   web:
    exposure:
      include: "*"
      exclude: "env,beans"
```

`web` 에 모든 엔드포인트를 노출하지만 `env` , `beans` 는 제외한다.

