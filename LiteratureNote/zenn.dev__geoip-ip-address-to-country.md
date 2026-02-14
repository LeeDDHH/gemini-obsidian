---
type: literature
source_url: https://zenn.dev/tomoakinagahara/articles/cc4e41499f256a
source_title: GeoIP：どこの国からアクセスしているか、IPアドレスから逆算する方法
created_at: 2026-02-14
updated_at: 2026-02-14
tags:
  - geoip
  - ip-address
  - geolocation
  - linux
  - php
  - mac
source_type: web
domain: zenn.dev
---

이 글은 IP 주소로부터 액세스 국가를 역산하는 GeoIP 기술에 대해 Linux, PHP, Mac, Windows 환경에서의 사용법과 GeoIP 및 GeoLiteCity의 차이점을 설명한다.

**Linux 환경:**
`sudo yum install GeoIP` 명령으로 설치하고 `geoiplookup 8.8.8.8`로 조회한다. 데이터베이스는 `/usr/share/GeoIP/GeoIP.dat`에 위치하며, `sudo geoipupdate`로 업데이트할 수 있으나 라이선스 구매가 필요하다.

**PHP 환경:**
`sudo yum install php-geoip`로 설치하고 `phpinfo()`로 활성화 여부를 확인한다. `geoip_country_code_by_name('8.8.8.8')`와 같은 함수로 국가 코드 및 이름을 얻을 수 있다. 구 버전 PHP에서는 데이터베이스 지정 함수가 있었으나, 최신 버전에서는 해당 함수가 없음을 지적하며 ChatGPT의 답변이 정확하지 않을 수 있음을 언급한다.

**Mac 환경:**
MacPorts를 통해 `GeoLiteCity`를 설치하여 사용할 수 있다. `geoiplookup 8.8.8.8`로 도시, 위도, 경도 등 상세 정보를 얻는다. 데이터베이스는 `/opt/local/share/GeoIP/GeoIPCity.dat`에 있다. 라이선스 문제로 GeoIP 라이브러리 직접 사용이 어렵고, 과거 무료로 제공되던 GeoLite 데이터베이스도 현재는 유료화되었음을 언급한다. 셸 명령어를 이용한 우회 방법도 제시하지만 라이선스 위반 가능성을 경고한다.

**Windows 환경:**
WSL(Windows Subsystem for Linux)을 사용하면 Ubuntu와 동일하게 적용 가능하다고 언급한다.

**GeoIP와 GeoLiteCity의 차이:**
MaxMind사에서 제공하는 두 데이터베이스의 주요 차이점은 다음과 같다.
*   **데이터 정확도:** GeoIP가 더 정확하고 상세한 위치 정보를 제공한다.
*   **사용 조건:** GeoIP는 상업적 이용 시 라이선스 비용이 필요하며, GeoLiteCity는 무료이다.
*   **데이터 업데이트 빈도:** GeoIP가 더 자주 업데이트된다.

결론적으로, 사용 목적과 예산에 따라 적절한 데이터베이스를 선택해야 한다. 상업적 이용이나 높은 정확도가 필요하면 GeoIP를, 비상업적 이용이나 무료 데이터가 필요하면 GeoLiteCity를 추천한다.
