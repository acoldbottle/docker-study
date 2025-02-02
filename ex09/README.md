Docker-compose 스프링, 리엑트, 디비, Nginx

nginx dns 와 upstream

Docker Compose를 사용하는 경우, 서비스명은 내부 DNS 레지스트리에 등록되며 컨테이너 간의 통신에 사용됩니다. 이것은 도커 컨테이너 내부에서 동작하는 것으로 인식되어, /etc/hosts 파일과 유사한 역할을 하게 됩니다.

기본적으로 Docker Compose는 각 서비스를 별도의 네트워크에 연결하고, 서비스명으로 해당 서비스의 컨테이너를 찾을 수 있도록 DNS 레지스트리를 설정합니다. 따라서 컨테이너 내부에서 서비스명을 사용하면 내부 DNS에 의해 해당 서비스의 컨테이너 IP 주소를 찾아갈 수 있게 됩니다.

location에 dns이름을 정의하여 찾을 수는 없고 upstream에 dns이름을 정의하여 설정한뒤 location에서 upstream이름으로 경로를 설정할 수 있습니다. 이때 dns이름은 docker-compose에 정의된 서비스명입니다.

nginx URL 재작성

proxy_pass 지시문은 /api/로 들어오는 모든 요청을 http://backend으로 전달합니다. 이때, Nginx는 원본 요청의 URL 경로를 그대로 유지합니다. 따라서 100.5.5.5:80/api/product로 들어오는 요청은 실제로 http://backend/api/product로 전달됩니다.

api 주소를 지우고 싶다면?

location /api/ {
proxy_set_header X-Original-URI $request_uri;  # 현재 요청의 원본 URI를 저장합니다.
rewrite ^/api(/.*)$ $1 break;  # /api/를 제거합니다.
proxy_pass http://backend;
...
}
DB데이터가 도커 볼륨 설정으로 인해 계속 유지 된다면?

docker-compose down -v

DB데이터가 외부 볼륨 설정으로 인해 계속 유지 된다면?

store폴더 강제 삭제 혹은 테이블 초기화

프론트와 백엔드 배포를 따로 하는게 좋지 않을까?

1. 따로 배포하는 경우:

장점:

각 서비스를 독립적으로 확장 가능하며, 각각의 스케일링이 필요한 경우에 효율적입니다. 각 서비스를 별도로 관리하고 업데이트하므로 서비스 간의 의존성을 줄일 수 있습니다. 프론트엔드와 백엔드가 다른 언어 또는 기술 스택으로 개발되었거나 다른 인프라에서 호스팅되어야 하는 경우 유용합니다.

단점:

서비스 간의 통신과 CORS(Cross-Origin Resource Sharing) 관리가 필요합니다. 관리 및 배포가 두 개의 독립적인 애플리케이션으로 분리되어 추가 작업이 필요합니다.

2. docker-compose로 묶어서 배포하는 경우:

docker-compose는 로컬 테스트 용도

장점:

편리하게 여러 서비스를 하나의 설정으로 관리할 수 있으며, 단일 배포 구성 파일로 전체 애플리케이션을 쉽게 배포할 수 있습니다. 개발 환경과 유사한 환경에서 애플리케이션을 실행하고 테스트하는 데 용이합니다. 각 서비스의 종속성 및 구성이 일관되게 관리됩니다.

단점:

모든 서비스를 함께 확장해야 하므로 일부 서비스에 불필요한 스케일링이 발생할 수 있습니다. 서비스 간의 의존성이 높은 경우 복잡성이 증가할 수 있습니다. 서비스 간의 통신 및 CORS 관리가 필요하며, 보안 설정이 중요합니다. 어떤 방식을 선택하느냐는 프로젝트 요구 사항과 팀의 개발 및 관리 스킬에 따라 다릅니다. 즉, 각 프로젝트의 고유한 상황에 맞춰 결정해야 합니다. 일반적으로 프로젝트가 복잡한 경우나 서비스 간의 의존성이 낮은 경우에는 서비스를 따로 배포하는 것이 유용할 수 있습니다. 반면에 단순한 애플리케이션이고 통합 관리가 편리한 경우에는 docker-compose로 묶어서 배포하는 것이 간편할 수 있습니다.

도커 컴포즈 환경변수 관련 정리

https://getinthere.notion.site/8527704c25a942b4905559fd0350291c?pvs=4

도커 컴포즈의 목적

https://getinthere.notion.site/b5ef2bb4c1284cb7ab1b0b671100655e?pvs=4

nginx proxy 설정 정리

https://getinthere.notion.site/nginx-3a4de6a527ff4e9daf3cf4ae62b1b497?pvs=4