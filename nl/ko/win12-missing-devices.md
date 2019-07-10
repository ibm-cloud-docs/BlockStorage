---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block storage, auxiliary storage, missing routes, mpio, multipath, windows, troubleshooting

subcollection: BlockStorage

---
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Windows 2012 R2 - 다중 iSCSI 디바이스
{: #troubleshootingWin12}

동일한 호스트에서 iSCSI 디바이스를 세 개 이상 사용하는 경우 이 프로시저가 유용할 수 있습니다. 모든 iSCSI가 동일한 스토리지 디바이스에서 연결된 경우에 특히 유용합니다.
디스크 관리자에 두 개의 디바이스만 표시되는 경우 모든 서버 노드의 iSCSI 이니시에이터에 있는 각 디바이스에 수동으로 연결해야 합니다.
{:tsSymptoms}
{:tsResolve}


1. Windows iSCSI 이니시에이터를 여십시오.
2. **대상** 탭에서 **디바이스**를 클릭하십시오.

   ![iSCSI 이니시에이터 특성](/images/win12-ts1.png)
3. 표시되는 장치의 수를 확인하십시오. 권한 부여된 네 개의 디바이스가 아니라 두 개만 표시되면 다음 단계를 진행하십시오.
4. **대상**과 **연결**을 순서대로 클릭하십시오.
5. **다중 경로**와 **고급**을 순서대로 선택하십시오.
6. Microsoft iSCSI 이니시에이터를 로컬 어댑터로 선택하십시오. 이니시에이터 IP는 서버에 속합니다.
7. 대상 포털 IP 목록에 표시된 첫 번재 IP 주소를 선택하십시오.

   ![고급 설정, IP 주소](/images/win12-ts3.png)

   나열되는 모든 IP 주소에 대해 이 단계를 반복해야 합니다.
   {:tip}

8. **CHAP 사용** 상자를 선택하고 서버의 CHAP ID 및 비밀번호를 입력하십시오.

   ![고급 설정, CHAP](/images/win12-ts4.png)
9. **확인**을 클릭하십시오.
10. iSCSI 이니시에이터에 입력한 IP마다 5-9단계를 반복하십시오. 완료되면 **디바이스** 탭을 클릭하고 결과를 검토하십시오. 설정한 모든 LUN이 두 번 표시되어야 합니다.

    ![디바이스 탭](/images/win12-ts5.png)
11. **확인**을 클릭하십시오.
12. 디스크 관리자를 열고 이제 드라이브가 표시되는지 확인하십시오.

    ![디바이스 관리자](/images/win12-ts6.png)
