---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.blockstorageshort}} 관리
{: #managingstorage}

[{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}을 통해 {{site.data.keyword.blockstoragefull}} 볼륨을 관리할 수 있습니다.

## {{site.data.keyword.blockstorageshort}} LUN 세부사항 보기

추가 스냅샷 및 스토리지에 추가된 복제 기능을 비롯하여 선택한 스토리지 LUN의 키 정보에 대한 요약을 볼 수 있습니다.

1. **스토리지**, **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
2. 목록에서 해당 LUN 이름을 클릭하십시오.

또는 SL CLI에서 다음 명령을 사용할 수 있습니다.
```
# slcli block volume-detail --help
사용법: slcli block volume-detail [OPTIONS] VOLUME_ID

옵션:
  -h, --help  이 메시지를 표시하고 종료합니다.
```

## {{site.data.keyword.blockstorageshort}}에 액세스하도록 호스트 권한 부여

"권한 부여된" 호스트는 특정 LUN에 대한 액세스 권한이 부여된 호스트입니다. 호스트 권한이 없는 경우, 시스템에서 스토리지에 액세스하거나 이를 사용할 수 없습니다. LUN에 액세스하도록 호스트에 권한을 부여하면 사용자 이름, 비밀번호, 다중 경로 I/O(MPIO) iSCSI 연결을 마운트하는 데 필요한 iSCSI 규정된 이름(IQN)이 생성됩니다.

사용자의 스토리지와 동일한 데이터 센터에 있는 호스트에 권한 부여하고 연결할 수 있습니다. 다수의 계정을 보유할 수는 있지만, 한 계정의 호스트에 권한 부여하여 다른 계정의 스토리지에 액세스할 수는 없습니다.
{:important}

1. **스토리지** -> **{{site.data.keyword.blockstorageshort}}**를 클릭하고 LUN 이름을 클릭하십시오.
2. 페이지의 **권한 부여된 호스트** 섹션으로 스크롤하십시오.
3. 오른쪽에 있는 **호스트 권한 부여**를 클릭하십시오. 특정 LUN에 액세스할 수 있는 호스트를 선택하십시오.

또는 SL CLI에서 다음 명령을 사용할 수 있습니다.
```
# slcli block access-authorize --help
사용법: slcli block access-authorize [OPTIONS] VOLUME_ID

옵션:
  -h, --hardware-id TEXT    권한 부여할 하나의 SoftLayer_Hardware ID
  -v, --virtual-id TEXT     권한 부여할 하나의 SoftLayer_Virtual_Guest ID
  -i, --ip-address-id TEXT  권한 부여할 하나의 SoftLayer_Network_Subnet_IpAddress
                            ID
  --ip-address TEXT         권한 부여할 IP 주소
  --help                    이 메시지를 표시하고 종료합니다.
```

## {{site.data.keyword.blockstorageshort}} LUN에 액세스할 권한 부여된 호스트 목록 보기

1. **스토리지** -> **{{site.data.keyword.blockstorageshort}}**를 클릭하고 LUN 이름을 클릭하십시오.
2. 아래로 스크롤하여 **권한 부여된 호스트** 섹션으로 이동하십시오.

여기에 현재 LUN에 액세스하도록 권한 부여된 호스트 목록이 표시됩니다. 연결하는 데 필요한 인증 정보(사용자 이름, 비밀번호 및 IQN 호스트)도 표시됩니다. 대상 주소는 **스토리지 세부사항** 페이지에 나열되어 있습니다. NFS의 경우 대상 주소는 DNS 이름으로 설명되고, iSCSI의 경우 대상 발견 포털의 IP 주소로 설명됩니다.

또는 SL CLI에서 다음 명령을 사용할 수 있습니다.
```
# slcli block access-list --help
사용법: slcli block access-list [OPTIONS] VOLUME_ID

옵션:
  --sortby TEXT   열 정렬 기준
  --columns TEXT  열 표시. 옵션: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      이 메시지를 표시하고 종료합니다.
```

## 호스트가 권한 부여된 {{site.data.keyword.blockstorageshort}} 보기

연결에 필요한 정보(LUN 이름, 스토리지 유형, 대상 주소, 용량, 위치)를 포함하여 호스트가 액세스 권한이 있는 LUN을 볼 수 있습니다.

1. [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://control.softlayer.com/){:new_window}에서 **디바이스** -> **디바이스 목록**을 클릭하고 적절한 디바이스를 클릭하십시오.
2. **스토리지** 탭을 선택하십시오.

이 특정 호스트가 액세스할 수 있는 스토리지 목록이 표시됩니다. 목록은 스토리지 유형(블록, 파일, 기타)별로 그룹화되어 표시됩니다. **조치**를 클릭하여 추가 스토리지를 권한 부여하거나 액세스 권한을 제거할 수 있습니다.

## {{site.data.keyword.blockstorageshort}} 마운트 및 마운트 해제

호스트의 운영 체제에 따라 해당되는 지시사항을 따르십시오.

- [Linux에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [CloudLinux에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Microsoft Windows에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [cPanel을 사용하여 Block Storage 구성](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [cPanel을 사용하여 Block Storage 구성](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)


## {{site.data.keyword.blockstorageshort}}에 대한 호스트 액세스 권한 취소

호스트에서 특정 스토리지 LUN으로 액세스를 중지하려는 경우, 액세스 권한을 취소할 수 있습니다. 액세스를 취소하면 LUN에서 호스트 연결이 삭제됩니다. 운영 체제 및 애플리케이션은 LUN과 더 이상 통신할 수 없게 됩니다.

호스트 측의 문제를 방지하려면, 드라이버가 누락되거나 데이터가 손상되지 않도록 액세스를 취소하기 전에 운영 체제에서 스토리지 LUN을 마운트 해제하십시오.
{:important}

**디바이스 목록** 또는 **스토리지 보기**에서 액세스 권한을 취소할 수 있습니다.

### 디바이스 목록에서 액세스 권한 취소

1. [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}에서 **디바이스**, **디바이스 목록**을 클릭하고 적절한 디바이스를 두 번 클릭하십시오.
2. **스토리지** 탭을 선택하십시오.
3. 이 특정 호스트가 액세스할 수 있는 스토리지 목록이 표시됩니다. 목록은 스토리지 유형(블록, 파일, 기타)별로 그룹화되어 표시됩니다. LUN 이름 옆에 있는 **조치를 선택하고 **액세스 권한 취소**를 클릭하십시오.
4. 조치는 실행 취소할 수 없기 때문에 LUN에 대한 액세스 권한을 취소할 것인지 확인하십시오. **예**를 클릭하여 LUN 액세스 권한을 취소하거나 **아니오**를 클릭하여 조치를 취소하십시오.

특정 호스트에서 여러 LUN의 연결을 끊으려면 각 LUN에 대해 액세스 취소 조치를 반복 수행해야 합니다.
{:tip}


### 스토리지 보기에서 액세스 권한 취소

1. **스토리지**, **{{site.data.keyword.blockstorageshort}}**를 클릭하고 액세스 권한을 취소하려는 LUN을 선택하십시오.
2. **권한 부여된 호스트**로 스크롤하십시오.
3. 액세스 권한이 취소되는 호스트 옆에 있는 **조치**를 클릭하고 **액세스 취소**를 선택하십시오.
4. 조치는 실행 취소할 수 없기 때문에 LUN에 대한 액세스 권한을 취소할 것인지 확인하십시오. **예**를 클릭하여 LUN 액세스 권한을 취소하거나 **아니오**를 클릭하여 조치를 취소하십시오.

특정 LUN에서 여러 호스트의 연결을 끊으려면 각 호스트에 대해 액세스 취소 조치를 반복 수행해야 합니다.
{:tip}

### SL CLI를 통해 액세스 권한 취소

또는 SL CLI에서 다음 명령을 사용할 수 있습니다.
```
# slcli block access-revoke --help
사용법: slcli block access-revoke [OPTIONS] VOLUME_ID

옵션:
  -h, --hardware-id TEXT    권한을 취소할 하나의 SoftLayer_Hardware ID

  -v, --virtual-id TEXT     권한을 취소할 하나의 SoftLayer_Virtual_Guest ID

  -i, --ip-address-id TEXT  권한을 취소할 하나의 SoftLayer_Network_Subnet_IpAddress ID

  --ip-address TEXT         권한을 취소할 IP 주소
  --help                    이 메시지를 표시하고 종료합니다.
```

## 스토리지 LUN 취소

특정 LUN이 더 이상 필요하지 않은 경우에는 언제든 취소할 수 있습니다.

스토리지 LUN을 취소하려면 우선 모든 호스트에서 액세스 권한을 취소해야 합니다.
{:important}

1. **스토리지**, **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
2. 취소되는 LUN에 대해 **조치**를 클릭하고 **{{site.data.keyword.blockstorageshort}} 취소**를 선택하십시오.
3. LUN을 즉시 취소하는지 또는 LUN이 프로비저닝되는 지정일에 취소할 것인지 확인하십시오.

   지정일에 LUN을 취소하는 옵션을 선택하는 경우, 해당 지정일 이전에 취소 요청을 무효화할 수 있습니다.
   {:tip}
4. **계속** 또는 **닫기**를 클릭하십시오.
5. **수신확인** 선택란을 클릭하고 **확인**을 클릭하십시오.

또는 SL CLI에서 다음 명령을 사용할 수 있습니다.
```
# slcli block volume-cancel --help
사용법: slcli block volume-cancel [OPTIONS] VOLUME_ID

옵션:
  --reason TEXT  취소에 대한 선택적 이유
  --immediate    청구일 대신에 즉시 블록 스토리지 볼륨 취소
  -h, --help     이 메시지를 표시하고 종료합니다.
```
