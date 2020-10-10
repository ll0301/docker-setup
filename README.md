## 1. 도커란 무엇인가?
* 도커는 격리된 환경에서 응용프로그램을 실행하기 위한 도구이다.   
* virtual machine과 유사하다.  
* 앱이 동일한 환경에서 실행된다.   
* 소프트웨어 배포 표준   

## 2. Container vs VM
* Containers   
-> 컨테이너는 코드와 종속성을 함께 패키징 하는 애플리케이션 계층의 추상화다.   
여러 컨테이너가 동일한 시스템에서 실행될 수 있고 os 커널을 다른 컨테이너와 공유할 수 있으며, 각각은 사용자 공간에서 격리된 프로세스로 실행된다.   
[Containeriazed Applications ...]   
ㄴ Docker   
ㄴ Host Operating System   
ㄴ Infrastructure   
   
* Virtual Machines (VM)   
-> VM은 하나의 서버를 여러 서버로 전환하는 물리적 하드웨어의 추상화다.   
하이퍼바이저를 통해 단일 시스템에서 여러 VM을 실행할 수 있다.   
각 VM에는 운영체제, 애플리케이션, 필요한 이진 파일 및 라이브러리의 전체 복사본이 포함되며, 여기에는 수십 GB가 사용된다. VM의 부팅속도도 느릴 수 있다.   
[VM / App / Guest Operating System] [VM / App / Guest Operating System]   
ㄴ Hypervisor   
ㄴ Infrastructure   

## 3. Docker 사용의 장점
-> 초단위로 컨테이너 실행   
-> 적은 리소스로 disk 공간 절약   
-> 적은 메모리 사용   
-> 전체 os가 필요하지 않음   
-> 효율적   
-> Testing   

## 4. Docker install
ubuntu   
https://blog.dalso.org/linux/ubuntu-20-04-lts/13118   

## 5. Docker Image
-> 이미지는 원하는 환경을 만들기 위한 템플릿이다.   
-> snapshot   
-> 애플리케이션을 실행하는 데 필요한 모든것이 있다.   
-> os / software / app code   
Container 는 이미지의 인스턴스를 실행한 것이다.   

## 6.  Pulling nginx image
hub.docker.com -> explore -> search for nginx   
$ sudo docker pull nginx   
$ sudo docker run nginx:latest   
$ sudo docker container ls   
$ sudo docker ps   
$ sudo docker images   
$ sudo docker run -d nginx:latest   
$ sudo docker stop 'Container ID'   

## 7. Exposing Port
$ sudo docker run -d -p 8080:80 nginx:latest   
-> http://localhost:8080/   
$ sudo docker run -d -p 3000:80 nginx:latest   
-> http://localhost:3000/   
$ sudo docker run -d -p 8080:80 -p 3000:80 nginx:latest   
$ sudo docker stop 'NAMES'   
$ sudo docker start 'NAMES'   

## 8. Managing Containers
$ sudo docker ps -a   
$ sudo docker rm 'CONTAINER ID'   
$ sudo docker rm 'NAMES'   
$ sudo docker ps -aq   
-> 아이디만 검색   
$ sudo docker rm $(sudo docker ps -aq)   
-> 전체삭제   
$ sudo docker rm -f $(sudo docker ps -aq)   
-> 실행중일때 강제삭제   
$ sudo docker run --name website -d -p 8080:80 -p 3000:80 nginx:latest   
-> 이름지정   
$ sudo docker stop website   
-> 지정된 이름으로 삭제   
$ sudo docker start website   
-> 지정된 이름으로 시작   
$ sudo docker run --name website-two -d -p 9000:80 nginx:latest   
$ sudo docker ps --format="ID\t{{.ID}}\nNAME\t{{.Names}}\nIMAGE\t{{.Image}}\nPORTS\t{{.Ports}}\nCOMMAND\t{{.Command}}\nCREATED\t{{.CreatedAt}}\nSTATUS\t{{.Status}}\n"   
-> ps custom   
$ export FORMAT="ID\t{{.ID}}\nNAME\t{{.Names}}\nIMAGE\t{{.Image}}\nPORTS\t{{.Ports}}\nCOMMAND\t{{.Command}}\nCREATED\t{{.CreatedAt}}\nSTATUS\t{{.Status}}\n"   
-> FORMAT 등록   
$ sudo docker ps --format=$FORMAT   
-> 짧게 사용하기    

## 9. Volumes
-> 호스트와 컨테이너 사이에 혹은 컨테이너 간에 데이터 공유를 허용한다. (파일 및 폴더)   
hub.docker.com -> explore -> nginx   
$ sudo docker run --name website -d -p 8080:80 nginx   
$ mkdir docker && cd docker   
$ mkdir website && cd website && touch index.html   
+ <h1> ... </h1>   
$ sudo docker stop website   
$ sudo docker rm website   
$ sudo docker run --name website -v $(pwd):/usr/share/nginx/html:ro -d -p 8080:80 nginx   
-> read only   
-> localhost:8080 접속시 index.html 보여줌   
$ sudo docker exec -it website bash   
$ cd /usr/share/nginx/html/   
$ touch about.html   
-> read only 메시지 나옴    
$ sudo docker rm -f website   
$ sudo docker run --name website -v $(pwd):/usr/share/nginx/html -d -p 8080:80 nginx   
bootstrap 예제 받고 docker/website 폴더안에 적용   
$ sudo docker run --name website-copy --volumes-from website -d -p 8081:80 nginxsudo docker run --name website-copy --volumes-from website -d -p 8081:80 nginx   
-> 복사   

## 10. Docker file

직접 이미지를 구축한다.   
dockerfile -> [create] -> image -> [run] -> container   
