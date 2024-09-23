# 백업 방법

wget 설치



## **기본**

```
wget https://github.com/Altinity/clickhouse-backup/releases/download/v2.6.1/clickhouse-backup-linux-amd64.tar.gz

```

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>



```
// tar로 압출 풀기
tar -xvzf <해당압축파일>
mv clickhouse-backup /usr/local/bin/
// 파일에 실행 권한을 부여합니다.
chmod +x /usr/local/bin/clickhouse-backup
```



```
// 설치 확인 
clickhouse-backup --version
```



```
//방법1 전체 백업
clickhouse-backup create <backup_name>

```



## 특정 테이블만



**특정 테이블 백업**: 원하는는 테이블을 백업하고자 할 때는 테이블 목록을 쉼표(,)로 구분하여 나열합니다.

```bash
clickhouse-backup create --tables=db_name.table1,db_name.table2 my_tables_backup
```

예시:

```bash
clickhouse-backup create --tables=default.table1,default.table2 multiple_tables_backup
```



**특정 테이블만 복원**:

백업된 특정 테이블만 복원할 때도 동일하게 특정 테이블을 지정할 수 있습니다.

```bash
clickhouse-backup restore --tables=db_name.table_name my_table_backup
```

예시:

```bash
clickhouse-backup restore --tables=default.testCreateusers testCreateusers_backup
```

##

## 전체복원

```
// 전체복원
 clickhouse-backup restore /var/lib/clickhouse/backup/chouse_multiple_backup20240919
```

## **도커 컨테이너로 DB 쓸때**

**`clickhouse-backup`** 도구가 표준 패키지 리포지토리에서 제공되지 않기 때문에 `apt-get`으로 설치하려고 할 때 오류가 발생한 것입니다. 이 경우, **`clickhouse-backup`** 도구는 **GitHub**에서 직접 설치해야 합니다.

#### `clickhouse-backup` 설치 방법 (Docker 컨테이너 내에서)

1.  **`clickhouse-backup` 다운로드 및 설치** `clickhouse-backup`은 GitHub에서 제공되므로, 이를 다운로드한 후 설치할 수 있습니다.

    먼저, ClickHouse Docker 컨테이너에 접속합니다.

    ```bash
    docker exec -it <container_name> bash
    ```

    그런 다음, `clickhouse-backup` 바이너리를 GitHub에서 다운로드합니다.

    ```bash
    wget https://github.com/Altinity/clickhouse-backup/releases/download/v2.3.3/clickhouse-backup-v2.3.3-linux-amd64.tar.gz
    ```
2.  **다운로드한 파일 압축 해제**

    다운로드한 파일을 압축 해제합니다.

    ```bash
    tar -xvzf clickhouse-backup-v2.3.3-linux-amd64.tar.gz
    ```
3.  **실행 파일을 이동**

    `clickhouse-backup` 바이너리를 `/usr/local/bin` 또는 시스템 경로에 이동하여 쉽게 실행할 수 있도록 합니다.

    ```bash
    mv clickhouse-backup /usr/local/bin/
    chmod +x /usr/local/bin/clickhouse-backup
    ```
4.  **버전 확인**

    `clickhouse-backup` 명령을 사용하여 설치가 완료되었는지 확인합니다.

    ```bash
    clickhouse-backup --version
    ```

    정상적으로 설치되었다면, 도구의 버전이 출력됩니다.

**5. 백업 생성**

컨테이너 내에서 `clickhouse-backup`을 사용하여 백업을 생성할 수 있습니다.

```bash
clickhouse-backup create my_backup_name
```





**6. 백업 파일을 호스트로 복사**

백업 파일을 Docker 컨테이너에서 호스트 시스템으로 복사하려면 다음 명령을 사용합니다.

```bash
docker cp <container_name>:/var/lib/clickhouse/backup /path/to/local/backup/
```

#### 요약:

* \*\*`clickhouse-backup`\*\*은 **GitHub**에서 다운로드하여 설치해야 합니다.
* Docker 컨테이너 내부에서 직접 도구를 설치하고 사용한 후, 백업을 생성하고 이를 호스트 시스템으로 복사할 수 있습니다.

이 방법을 통해 `clickhouse-backup`을 정상적으로 설치하고 ClickHouse 데이터를 백업할 수 있습니다.







## **다른 복사 방법 사용**&#x20;

`docker cp` 명령이 정상적으로 작동하지 않을 경우, 컨테이너에서 파일을 직접 압축한 다음, 압축된 파일을 복사하는 방법도 있습니다. 이 방법으로 복사 속도를 높일 수 있습니다.

**해결 방법**:

1.  **컨테이너 내에서 파일을 압축**합니다.

    ```bash
    docker exec some-clickhouse-server tar -czvf /var/lib/clickhouse/backup.tar.gz /var/lib/clickhouse/backup/chouse_multiple_backup20240919
    ```
2.  **압축된 파일을 복사**합니다.

    ```bash
    sudo docker cp some-clickhouse-server:/var/lib/clickhouse/backup.tar.gz /var/lib/clickhouse/
    ```
3.  **호스트 시스템에서 압축 해제**:

    ```bash
    tar -xzvf /var/lib/clickhouse/backup.tar.gz -C /var/lib/clickhouse/
    ```

#### 요약:

* `docker cp` 명령이 실행 중이지만 복사 상태가 보이지 않을 때는, **파일 크기를 실시간으로 확인**하여 복사가 진행 중인지 확인하세요.
* 복사 작업이 너무 오래 걸리거나 멈춘 것 같으면, **압축 후 복사**하는 방법으로 전환할 수 있습니다.
* **Docker 로그**를 확인하여 문제가 있는지 확인하고, 필요시 작업을 나누어서 처리하세요.

이 방법을 통해 문제를 해결하고 복사를 완료할 수 있을 것입니다.
