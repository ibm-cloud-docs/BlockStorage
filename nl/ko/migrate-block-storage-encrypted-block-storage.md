---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}}를 암호화된 {{site.data.keyword.blockstorageshort}}로 마이그레이션

이제 데이터 센터 선택 시에 내구성(Endurance) 또는 성능(Performance)에 대해 암호화된 {{site.data.keyword.blockstoragefull}}가 사용 가능합니다. {{site.data.keyword.blockstorageshort}}를 암호화되지 않은 상태에서 암호화된 상태로 마이그레이션하는 방법에 대한 정보가 아래에서 제공됩니다. 제공자 관리 암호화 스토리지에 대한 자세한 정보는 [{{site.data.keyword.blockstorageshort}} 비활성 시 암호화(Encryption-At-Rest) 문서](block-file-storage-encryption-rest.html)를 참조하십시오. 업그레이드된 데이터 센터 및 사용 가능한 기능 목록을 보려면 [여기](new-ibm-block-and-file-storage-location-and-features.html)를 클릭하십시오.

선호하는 마이그레이션 경로는 두 LUN 모두에 동시 연결되고 임의 파일 LUN에서 다른 LUN으로 데이터를 직접 전송합니다. 스펙은 운영 체제 및 데이터가 복사 오퍼레이션 중에 변경되는지 여부에 따라 다릅니다.

사용자를 위해 더 공통되는 시나리오를 소개합니다. 이 경우, 이미 호스트에 암호화되지 않은 파일 LUN을 접속했다고 가정합니다. 그렇지 않으면, 이 태스크를 완료하기 위해 실행 중인 운영 체제에 가장 적합한 아래 지시사항에 따라 수행하십시오.

- [Linux에서 {{site.data.keyword.blockstorageshort}} 액세스](accessing_block_storage_linux.html)

- [Windows에서 {{site.data.keyword.blockstorageshort}} 액세스](accessing-block-storage-windows.html)

 
## 암호화된 LUN 작성

다음 단계를 사용하여 마이그레이션 프로세스를 수행하기 위해 암호화된 동일한 크기 이상의 LUN을 작성하십시오. 
암호화된 내구성(Endurance) 스토리지 LUN을 주문하십시오.

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 홈 페이지에서 **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.

2. {{site.data.keyword.blockstorageshort}} 페이지에서 **{{site.data.keyword.blockstorageshort}} 주문** 링크를 클릭하십시오.

3. **내구성(Endurance)**을 선택하십시오.

4. 원본 LUN이 있는 데이터 센터를 선택하십시오. <br/> **참고**: 암호화는 데이터 센터 선택 시에만 사용 가능합니다.

5. 원하는 IOPS 티어를 선택하십시오.

6. 원하는 스토리지 영역 크기(GB)를 선택하십시오. TB의 경우, 1TB는 1000GB와 동일하며 12TB는 12000GB와 동일합니다.

7. 스냅샷에 대해 원하는 스토리지 영역 크기(GB)를 입력하십시오.

8. 드롭 다운 목록에서 VMware OS를 선택하십시오.

9. 주문을 제출하십시오.

## 암호화된 성능(Performance) 스토리지 LUN을 주문하십시오.

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 홈 페이지에서 **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.

2. **{{site.data.keyword.blockstorageshort}} 주문**을 클릭하십시오.

3. **성능(Performance)**을 선택하십시오.

4. 원본 LUN이 있는 데이터 센터를 선택하십시오. 암호화는 별표(`*`)가 있는 데이터 센터에서만 사용 가능합니다.

5. 원하는 스토리지 영역 크기(GB)를 선택하십시오. TB의 경우, 1TB는 1000GB와 동일하며 12TB는 12000GB와 동일합니다.

6. 원하는 IOPS 크기(간격은 100)를 입력하십시오.

7. 드롭 다운 목록에서 VMware OS를 선택하십시오.

8. 주문을 제출하십시오.

스토리지는 1분 내에 프로비저닝되며 {{site.data.keyword.slportal}}의 {{site.data.keyword.blockstorageshort}} 페이지에 표시됩니다.

 
## 호스트에 새 볼륨 연결

"권한 부여"된 호스트는 볼륨에 대한 액세스 권한이 부여된 호스트입니다. 호스트 권한이 없는 경우, 시스템에서 스토리지에 액세스하거나 이를 사용할 수 없습니다. 볼륨에 액세스하도록 호스트에 권한을 부여하면 사용자 이름, 비밀번호, 다중 경로 I/O(MPIO) iSCSI 연결을 마운트하는 데 필요한 iSCSI 규정된 이름(IQN)이 생성됩니다.

1. **스토리지**  > **{{site.data.keyword.blockstorageshort}}**를 클릭하고 LUN 이름을 클릭하십시오.

2. 페이지의 **권한 부여된 호스트** 섹션으로 스크롤하십시오.

3. 페이지 오른쪽에 있는 **호스트 권한 부여** 링크를 클릭하십시오. 볼륨에 액세스할 수 있는 호스트를 선택하십시오.

 
## 스냅샷 및 복제

원본 LUN에 대해 스냅샷 및 복제가 설정되어 있습니까? 설정된 경우, 복제, 스냅샷 영역을 설정해야 하며 원본 볼륨과 동일한 설정으로 암호화된 새 LUN에 대해 스냅샷 스케줄을 작성해야 합니다. 

복제 대상 데이터 센터가 암호화로 업그레이드되지 않은 경우, 해당 데이터 센터를 업그레이드하지 않으면 암호화된 볼륨에 대해 복제를 설정할 수 없습니다.

 
## 데이터 마이그레이션

원본 및 암호화된 {{site.data.keyword.blockstorageshort}} LUN 모두에 연결되어 있어야 합니다. 
- 연결되지 않은 경우에는 위의 단계 및 다른 게시물에서 참조되는 단계를 모두 올바르게 수행해야 합니다. 두 LUN에 연결하여 지원을 위한 지원 티켓을 여십시오.

### 데이터 고려사항

이 시점에서 원본 {{site.data.keyword.blockstorageshort}} LUN에 있는 데이터 유형 및 암호화된 LUN에 이를 복사하는 가장 좋은 방법을 고려해야 합니다. 백업, 정적 컨텐츠, 복사 중에 변경되지 않는 것이 있는 경우, 특별히 고려해야 할 사항은 없습니다.

{{site.data.keyword.blockstorageshort}}에서 데이터베이스 또는 가상 머신을 실행 중인 경우, 데이터 손상이 발생하지 않도록 원본 LUN에 있는 데이터가 복사 중에 수정되지 않도록 하십시오. 대역폭 관련 문제가 있는 경우, 최대 활동 시간이 아닌 시간에 마이그레이션을 수행해야 합니다. 이런 고려사항에 관련된 도움이 필요한 경우에는 지원 티켓을 여십시오.
 
### Microsoft Windows

원본 {{site.data.keyword.blockstorageshort}} LUN에서 암호화된 LUN으로 데이터를 복사하려면 새 스토리지를 포맷하고 Windows Explorer를 사용하여 파일을 복사하십시오.

 
### Linux

rsync를 사용하여 데이터를 복사하는 것도 고려할 수 있습니다. 다음은 예제 명령입니다.

``[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/encrypted/block/storage
``

경로가 제대로 설정되도록 --dry-run 플래그와 같이 위의 명령은 사용하도록 권장됩니다. 이 프로세스가 인터럽트되는 경우, 새 위치의 시작부터 복사되도록 복사 중인 최종 대상 파일을 삭제할 수 있습니다.

--dry-run 플래그를 사용하지 않고 이 명령을 완료하면 데이터가 암호화된 {{site.data.keyword.blockstorageshort}} LUN으로 복사되어야 합니다. 누락된 항목이 없도록 위로 스크롤하여 명령을 다시 실행해야 합니다. 누락된 항목이 없는지 확인하기 위해 두 위치를 수동으로 검토할 수도 있습니다.

마이그레이션이 완료되면 프로덕션을 암호화된 LUN으로 이동하고 원본 LUN을 구성에서 분리하고 삭제할 수 있습니다. 삭제하면 원본 LUN에 연관되어 있는 대상 사이트에서 모든 스냅샷과 복제본도 제거됩니다.
