# 클릭하우스 and 타빅스 컨테이너 배포시

#### 해결 방법

1.  **호스트 경로의 권한 설정**

    * `/data/PDTIAI_platform/DTIAI_CLICKHOUSE/click_data` 디렉토리의 소유자와 권한을 변경하여 ClickHouse 컨테이너가 해당 디렉토리에 접근하고 쓸 수 있도록 합니다.

    ```bash
    bash코드 복사sudo chown -R 101:101 /data/PDTIAI_platform/DTIAI_CLICKHOUSE/click_data
    sudo chmod -R 755 /data/PDTIAI_platform/DTIAI_CLICKHOUSE/click_data
    ```

    * 여기서 `101:101`은 ClickHouse 컨테이너의 기본 사용자 및 그룹 ID입니다. 컨테이너가 디렉토리에 접근할 수 있도록 소유자와 그룹을 설정합니다.
2.  **다른 디렉토리 권한 설정**

    * 마운트된 로그 경로 (`/data/PDTIAI_platform/DTIAI_CLICKHOUSE/log`) 및 설정 파일 경로 (`config.xml`, `users.xml`)도 동일하게 접근 권한을 부여하는 것이 좋습니다. 설정 파일은 읽기 권한만 필요할 수 있지만, 로그와 데이터 경로는 읽기 및 쓰기 권한이 필요합니다.

    ```bash
    bash코드 복사sudo chown -R 101:101 /data/PDTIAI_platform/DTIAI_CLICKHOUSE/log
    sudo chmod -R 755 /data/PDTIAI_platform/DTIAI_CLICKHOUSE/log
    ```
3.  **컨테이너 재시작**

    * 권한 설정을 완료한 후에 Docker Compose나 Docker 명령어를 사용하여 컨테이너를 다시 시작합니다.

    ```bash
    bash코드 복사docker-compose down
    docker-compose up -d
    ```

이렇게 하면 ClickHouse 컨테이너가 호스트의 `/var/lib/clickhouse` 경로에 접근할 수 있으며, `Permission denied` 오류 없이 정상적으로 디렉토리를 생성하고 사용할 수 있습니다.
