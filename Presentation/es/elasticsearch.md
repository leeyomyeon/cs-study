(엘라스틱 서치 가이드북)[https://github.com/eskrug/es-kr-guide]

위 문서를 읽고 나름대로 정리하는 글

# 1 Elastic stack

Logstash, Kibana와 함께 사용 되면서 한동안 ELK Stack (Elasticsearch, Logstash, Kibana)이 유명함

## 1.1 Elasticsearch

Elastic Stack의 심장 Elasticsearch

뛰어난 검색 및 대규모 분신 시스템을 구축할 수 있는 다양한 기능 제공

특히 Full Text Search 와 BM25를 포함한 다양한 알고리즘을 제공

특징
**실시간 분석**: 하둡은 기본적으로 배치 기반인 반면 Es은 데이터가 계속 색인되고 near real time 속도로 색인된 데이터의 검색, 집계가 가능
**전문 검색 엔진**: Inverted file index라는 구조로 데이털르 저장, JSON 기반이며 유일한 형식이므로 입력 데이터를 JSON으로 가공해야함, Kibana에서는 기본적으로 CSV, Apache log, syslog 과 같은 형태가 변환을 지원하기도 함
**RESTFul API**: MSA에서는 REST API와 같은 표준 인터페이스가 중요함 따라서 HTTP 프로토콜로 처리함
**멀티네넌시**: Index라는 논리적인 집합 단위로 구성되며, 서로 다른 저장소에 분산되어 저장됨. 서로 다른 인덱스들을 별도의 커넥션 없이 하나의 질의로 묶어서 관리하고 검ㅅ색 결과들을 하나의 출력으로 도출할 수 있는 특성을 멀티테넌시라고 함

## 1.2 Logstash

Elasticsearch는 색인, 검색 기능만 제공하므로 수집을 담당하는 도구가 필요했는데 이것이 Logstash 별도의 프로젝트였으나 통합의 필요성을 느껴 통합하게 됨

Logstash는
입력(Inputs) ➡️ 필터(Filters) ➡️ 출력(Outputs)

의 과정을 거침

## 1.3 Kibana

시각화는 중요한 요소인데 ES가 HTTP 프로토콜 기반으로 되므로 손 쉽게 연동할 수 있음 그래서 Kibana라는 프로젝트가 인기를 끌어서 통합되었음
aggregation의 집계 기능을 이용해서 ES로부터 문서, 집계 결과 등을 불러와 시각화함
Discover, Visualize, Dashboard와 같은 기본 메뉴와 다양한 앱이 있는데 플러그인을 통해 App의 설치가 가능

## 1,4 Beats

Logstash가 데이터 수집기로 뛰어났지만 프로그램의 부피 및 ES가 클러스터로의 대용량 데이터 전송은 하나가 아닌 여러 시스템에서 수집을 하였는데, 각 단말에 Logstash를 설치하는것은 꽤 큰 부담 다라서 단말 시스템으로부터 데이터를 수집하고 필터기능없이 가볍게 원격 수집기를 개발하고 있었는데 Beates라는 경량 수집기가 탄생함
Beats는 Logstash 혹은 Elasticsearch로 데이터를 전송함

Packetbeat, Filebeat, Metricbeat 등 다양하게 있음

# 3. 클러스터

## 3.1 클러스터 세팅

Es의 노드들은 클라이언트 통신을 위핸 http 포트와 노드간 통신을 위해 사용하는 tcp 포트 (9300~9399)를 활용함

하나의 서버에 여러개의 노드를 실행하는 것도 가능하고 여러 서버에 하나의 클러스터로 실행하는 것도 가능
