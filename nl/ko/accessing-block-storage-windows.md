---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"
---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# Microsoft Windows에서 iSCSI LUNS 연결
{: #mountingWindows}

시작하기 전에 {{site.data.keyword.blockstoragefull}} 볼륨에 액세스하는 호스트의 권한이 [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}을 통해 부여되는지 확인하십시오.

1. {{site.data.keyword.blockstorageshort}} 나열 페이지에서 새 볼륨을 찾고 **조치**를 클릭하십시오. **호스트 권한 부여**를 클릭하십시오.
2. 목록에서 볼륨에 대한 액세스 권한이 있는 호스트를 선택하고 **제출**을 클릭하십시오.

또는 SLCLI를 통해 호스트에 권한을 부여할 수 있습니다.
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
{:codeblock}

## {{site.data.keyword.blockstorageshort}} 볼륨 마운트

다음은 Windows 기반의 {{site.data.keyword.BluSoftlayer_full}} 컴퓨팅 인스턴스를 다중 경로 입력/출력(MPIO) iSCSI(internet Small Computer System Interface) LUN(Logical Unit Number)에 연결하는 데 필요한 단계입니다. 예제는 Windows Server 2012를 기반으로 합니다. 단계는 운영 체제(OS) 공급업체 문서에 따라 다른 Windows 버전에 맞게 조정 가능합니다.

### MPIO 기능 구성

1. 서버 관리자를 시작하고 **관리**, **역할 및 기능 추가**를 찾아보십시오.
2. **다음**을 클릭하여 기능 메뉴로 이동하십시오.
3. 아래로 스크롤하여 **다중 경로 I/O**를 선택하십시오.
4. **설치**를 클릭하여 호스트 서버에서 MPIO를 클릭하십시오.
![서버 관리자의 역할 및 기능 추가](/images/Roles_Features.png)

### MPIO에 대한 iSCSI 지원 추가

1. **시작**을 클릭하고, **관리 도구**를 지시하고, **MPIO**를 클릭하여 MPIO 특성 창을 여십시오.
2. **다중 경로 검색**을 클릭하십시오.
3. **iSCSI 디바이스에 대한 지원 추가**를 선택하고 **추가**를 클릭하십시오. 컴퓨터 다시 시작이 프롬프트되면 **예**를 클릭하십시오.

Windows Server 2008에서, iSCSI에 대한 지원을 추가하면 우선 iSCSI 대상에 대한 연결이 필요한 MPIO에 대한 모든 iSCSI를 MSDSM(Microsoft Device Specific Module)에서 청구할 수 있습니다.
{:note}

### iSCSI 이니시에이터 구성

1. 서버 관리자에서 iSCSI 이니시에이터를 실행하고 **도구**, **iSCSI 이니시에이터**를 선택하십시오.
2. **구성** 탭을 클릭하십시오.
    - 이니시에이터 이름 필드는 이미 `iqn.1991-05.com.microsoft:`와 유사한 입력으로 채워져 있을 수도 있습니다.
    - **변경**을 클릭하여 기존 값을 iSCSI 규정된 이름(IQN)으로 대체하십시오.
    ![iSCSI 이니시에이터 특성](/images/iSCSI.png)

      IQN 이름은 [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}의 {{site.data.keyword.blockstorageshort}} 세부사항 화면에서 얻을 수 있습니다.
      {: tip}

    - **발견** 탭을 클릭하고 **포털 발견**을 클릭하십시오.
    - iSCSI 대상의 IP 주소를 입력하고 포트는 기본값인 3260으로 두십시오.
    - **고급** 탭을 클릭하여 고급 설정 창을 여십시오.
    - **CHAP 로그온 사용**을 선택하여 CHAP 인증을 켜십시오.
    ![CHAP 로그인 사용](/images/Advanced_0.png)

    이름 및 대상 시크릿 필드는 대소문자를 구분합니다.
    {:important}
         - **이름** 필드에서 기존 항목을 삭제하고 [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}에서 사용자 이름을 입력하십시오.
         - **대상 시크릿** 필드에서 [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}의 비밀번호를 입력하십시오.
    - **고급 설정** 및 **대상 포털 발견** 창에서 **확인**을 클릭하고 기본 iSCSI 이니시에이터 특성 화면으로 돌아가십시오. 인증 오류가 수신되면 사용자 이름 및 비밀번호 입력을 확인하십시오.
      ![비활성 대상](/images/Inactive_0.png)

    대상의 이름은 발견된 대상 섹션에 `비활성 상태`로 표시됩니다.
    {:note}


### 대상 활성화

1. **연결**을 클릭하여 대상에 연결하십시오.
2. **다중 경로 사용** 선택란을 선택하여 대상에 대해 다중 경로 IO가 사용되도록 설정하십시오.<br/>
   ![다중 경로 사용](/images/Connect_0.png)
3. **고급**을 클릭하고 **CHAP 로그온 사용**을 선택하십시오.</br>
   ![CHAP 사용](/images/chap_0.png)
4. 이름 필드에 사용자 이름을 입력하고 대상 시크릿 필드에 비밀번호를 입력하십시오.

   이름 및 대상 시크릿 필드 값은 {{site.data.keyword.blockstorageshort}} 세부사항 화면에서 가져올 수 있습니다.
   {:tip}
5. **iSCSI 이니시에이터 특성** 창이 표시될 때까지 **확인**을 클릭하십시오. **발견된 대상** 섹션의 대상 상태는 **비활성**에서 **연결됨**으로 변경됩니다.
![연결된 상태](/images/Connected.png)


### iSCSI 이니시에이터에서 MPIO 구성

1. iSCSI 이니시에이터를 실행하고 대상 탭에서 **특성**을 클릭하십시오.
2. 특성 창에서 **세션 추가**를 클릭하여 대상 연결 창을 여십시오.
3. 대상에 연결 대화 상자에서 **다중 경로 사용** 선택란을 선택하고 **고급**을 클릭하십시오.
  ![대상](/images/Target.png)

4. 고급 설정 창에서 다음을 수행하십시오. ![설정](/images/Settings.png)
   - 로컬 어댑터 목록에서 Microsoft iSCSI 이니시에이터를 선택하십시오.
   - 이니시에이터 IP 목록에서 호스트의 IP 주소를 선택하십시오.
   - 대상 포털 IP 목록에서 디바이스 인터페이스의 IP를 선택하십시오.
   - **CHAP 로그온 사용** 선택란을 클릭하십시오.
   - 포털 창에서 가져온 이름 및 대상 시크릿 값을 입력하고 **확인**을 클릭하십시오.
   - 대상 연결 창에서 **확인**을 클릭하여 특성 창으로 돌아가십시오.

5. **특성**을 클릭하십시오. 특성 대화 상자에서 **세션 추가**를 다시 클릭하여 두 번째 경로를 추가하십시오.
6. 대상에 연결 창에서 **다중 경로 사용** 선택란을 선택하십시오. **고급**을 클릭하십시오.
7. 고급 설정 창에서 다음을 수행하십시오.
   - 로컬 어댑터 목록에서 Microsoft iSCSI 이니시에이터를 선택하십시오.
   - 이니시에이터 IP 목록에서 호스트에 대응되는 IP 주소를 선택하십시오. 이 경우에는 스토리지 디바이스의 2개 네트워크 인터페이스를 호스트의 단일 네트워크 인터페이스에 연결합니다. 따라서 이 인터페이스는 첫 번째 세션에 대해 제공된 인터페이스와 동일합니다.
   - 대상 포털 IP 목록에서 스토리지 디바이스에서 사용으로 설정되어 있는 두 번째 데이터 인터페이스의 IP 주소를 선택하십시오. 

     [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}의 {{site.data.keyword.blockstorageshort}} 세부사항 화면에서 두 번째 IP 주소를 찾을 수 있습니다.
      {: tip}
   - **CHAP 로그온 사용** 선택란을 클릭하십시오.
   - 포털 창에서 가져온 이름 및 대상 시크릿 값을 입력하고 **확인**을 클릭하십시오.
   - 대상 연결 창에서 **확인**을 클릭하여 특성 창으로 돌아가십시오.
8. 이제 특성 창의 ID 분할창에는 두 개 이상의 세션이 표시됩니다. iSCSI 스토리지에 대해 둘 이상의 세션이 있습니다.

   호스트에 ISCSI 스토리지에 연결될 다수의 인터페이스가 있는 경우에는 이니시에이터 IP 필드에서 기타 NIC의 IP 주소로 다른 연결을 설정할 수 있습니다. 그러나 연결을 시도하기 전에 [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}에서 두 번째 이니시에이터 IP 주소에 권한을 부여하십시오.
   {:note}
9. 특성 창에서 **디바이스**를 클릭하여 디바이스 창을 여십시오. 디바이스 인터페이스 이름이 `mpio`로 시작됩니다. <br/>
  ![디바이스](/images/Devices.png)

10. **MPIO**를 클릭하여 **디바이스 세부사항** 창을 여십시오. 이 창에서 MPIO에 대한 로드 밸런싱 정책을 선택할 수 있으며 iSCSI 경로가 표시됩니다. 이 예제에서는 두 개의 경로가 서브셋 로드 밸런싱 정책이 포함된 라운드로빈 방식으로 MPIO에 대해 사용 가능한 것으로 표시됩니다.
  ![디바이스 세부사항 창에서는 서브셋 로드 밸런싱 정책이 포함된 라운드로빈 방식으로 MPIO에 사용 가능한 두 경로를 표시함](/images/DeviceDetails.png)

11. **확인**을 여러 번 클릭하여 iSCSI 이니시에이터를 종료하십시오.



## Windows 운영 체제에서 MPIO가 제대로 구성되었는지 확인
{: #verifyMPIOWindows}

Windows MPIO가 제대로 구성되었는지 확인하려면 우선 MPIO 추가 기능이 사용 가능한지 확인한 후 서버를 다시 시작해야 합니다.

![Roles_Features_0](/images/Roles_Features_0.png)

다시 시작이 완료되고 스토리지 디바이스가 추가되면 MPIO가 구성되어 작동 중인지 확인할 수 있습니다. 확인하려면 **대상 디바이스 세부사항**에서 **MPIO**를 클릭하십시오.
![DeviceDetails_0](/images/DeviceDetails_0.png)

MPIO가 올바르지 않게 구성되면, 네트워크 가동 중단이 발생하거나 {{site.data.keyword.BluSoftlayer_full}} 팀이 유지보수를 수행하는 경우 스토리지 디바이스는 연결이 끊어지고 사용 안함으로 표시될 수 있습니다. MPIO를 사용하면 이런 상황에서도 추가 레벨의 연결이 가능하며 LUN에 대해 읽기/쓰기 조작이 활성화된 세션이 계속 유지됩니다.

## {{site.data.keyword.blockstorageshort}} 볼륨 마운트 해제
{: #unmounting}

다음은 MPIO iSCSI LUN에 대해 Windows 기반의 {{site.data.keyword.Bluemix_short}} 컴퓨팅 인스턴스 연결을 끊기 위해 필요한 단계입니다. 예제는 Windows Server 2012를 기반으로 합니다. 단계는 OS 공급업체 문서에 따라 다른 Windows 버전에 대해 조정 가능합니다.

### iSCSI 이니시에이터 시작

1. **대상** 탭을 클릭하십시오.
2. 제거하려는 대상을 선택하고 **연결 끊기**를 클릭하십시오.

### 대상 제거
iSCSI 대상에 더 이상 액세스할 필요가 없는 경우 이 단계는 선택사항입니다.

1. iSCSI 이니시에이터에서 **발견**을 클릭하십시오.
2. 스토리지 볼륨에 연관된 대상 포털을 강조표시하고 **제거**를 클릭하십시오.
