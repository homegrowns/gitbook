# 백업 설정 파일

경로

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

```
<!-- Config that is used when server is run without config file. -->
<clickhouse>
    <logger>
        <level>trace</level>
        <log>/var/log/clickhouse-server/clickhouse-server.log</log>
        <errorlog>/var/log/clickhouse-server/clickhouse-server.err.log</errorlog>
        <size>1000M</size>
        <count>10</count>
        <console>true</console>
    </logger>

    <listen_host>0.0.0.0</listen_host>
    <http_port>8123</http_port>
    <tcp_port>9000</tcp_port>
    <mysql_port>9004</mysql_port>

    <path>/var/lib/clickhouse/</path>

    <backups>
         <allowed_disk>backup_disk</allowed_disk>
    </backups>

    <mlock_executable>true</mlock_executable>

    <users>
        <default>
            <password/>

            <networks>
                <ip>::/0</ip>
            </networks>

            <profile>default</profile>
            <quota>default</quota>

            <access_management>1</access_management>
            <named_collection_control>1</named_collection_control>
        </default>
    </users>

    <profiles>
        <default/>
    </profiles>

    <quotas>
        <default/>
    </quotas>

    <storage_configuration>
        <disks>
            <backup_disk>
                    <path>/mnt/backup/</path>
            </backup_disk>
        </disks>
    </storage_configuration>


</clickhouse>
```

이설정하고 clickhouseDB 설치경로에서&#x20;

아래 명령어 재시작(쿠베플로우 pod 에서wget 설치시)

./clickhouse server --config-file=/etc/clickhouse-server/config.xml



ps .도커컨테이너는 그냥  docker 재시작 명령어 하면될것 같다.
