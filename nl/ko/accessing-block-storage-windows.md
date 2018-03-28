---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Microsoft Windows에서 MPIO iSCSI LUNS 연결

시작하기 전에 {{site.data.keyword.blockstoragefull}} 볼륨에 액세스하는 호스트가 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}을 통해 권한 부여되었는지 확인하십시오. 

1. {{site.data.keyword.blockstorageshort}} 목록 페이지에서 새로 프로비저닝된 볼륨에 연관된 **조치** 단추를 클릭하고 **호스트 권한 부여**를 클릭하십시오. 
2. 목록에서 원하는 호스트를 선택하고 **제출**을 클릭하십시오. 볼륨에 액세스할 수 있도록 호스트에 권한이 부여됩니다. 

## {{site.data.keyword.blockstorageshort}} 볼륨 마운트 방법

다음은 Windows 기반의 {{site.data.keyword.BluSoftlayer_full}} 컴퓨팅 인스턴스를 다중 경로 입력/출력(MPIO) iSCSI(Internet Small Computer System Interface) LUN(Logical Unit Number)에 연결하는 데 필요한 단계입니다. 예제는 Windows Server 2012를 기반으로 합니다. 단계는 운영 체제(OS) 공급업체 문서에 따라 다른 Windows 버전에 맞게 조정 가능합니다. 

### MPIO 기능 구성

1. 서버 관리자를 실행하고 **관리**, **역할 및 기능 추가**로 이동하십시오. 
2. **다음**을 클릭하여 기능 메뉴로 이동하십시오. 
3. 아래로 스크롤하여 **다중 경로 I/O** 선택란을 클릭하십시오. 
4. **설치**를 클릭하여 호스트 서버에서 MPIO를 클릭하십시오. ![역할 및 기능 추가](/images/Roles_Features.png)

### MPIO에 대한 iSCSI 지원 추가

1. MPIO 특성을 여십시오. MPIO 특성을 열려면 **시작**을 클릭하고 관리 도구를 지시한 후 **MPIO**를 클릭하십시오. 
2. **다중 경로 검색** 탭을 클릭하십시오. 
3. **iSCSI 디바이스에 대한 지원 추가** 선택란을 선택하고 **추가**를 클릭하십시오. 컴퓨터 다시 시작이 프롬프트되면 **예**를 클릭하십시오.

**참고**: Windows Server 2008에서는 iSCSI에 지원을 추가하면 MSDSM(Microsoft Device Specific Module)이 우선 iSCSI 대상에 연결해야 하는 MPIO에 대해 모든 iSCSI를 청구할 수 있습니다. 

### iSCSI 이니시에이터 구성

1. 서버 관리자에서 iSCSI 이니시에이터를 실행하고 **도구**, **iSCSI 이니시에이터**를 선택하십시오.
2. **구성** 탭을 클릭하십시오. 
    - 이니시에이터 이름 필드는 이미 iqn.1991-05.com.microsoft와 유사한 입력으로 채워져 있을 수도 있습니다. 
    - **변경**을 클릭하여 기존 값을 iSCSI 규정된 이름(IQN)으로 대체하십시오. IQN 이름은 포털의 {{site.data.keyword.blockstorageshort}} 세부사항 화면에서 볼 수 있습니다. ![iSCSI 이니시에이터 특성](/images/iSCSI.png)
    - **발견** 탭을 클릭하고 **포털 발견**을 클릭하십시오.
    - iSCSI 대상의 IP 주소를 입력하고 포트는 기본값인 3260으로 두십시오.  
    - **고급** 탭을 클릭하여 고급 설정 창을 실행하십시오. 
    - **CHAP 로그온 사용**을 선택하여 CHAP 인증을 켜십시오. ![CHAP 로그인 사용](/images/Advanced_0.png)
    **참고:** 이름 및 대상 시크릿 필드는 대소문자를 구분합니다. 
         - 이름 필드에서 기존의 모든 입력은 삭제하고 포털의 사용자 이름을 입력하십시오. 
         - 대상 시크릿 필드에 포털의 비밀번호를 입력하십시오. <br/>
         **참고:** 이름 및 대상 시크릿 값은 포털의 {{site.data.keyword.blockstorageshort}} 세부사항 화면에서 사용자 이름 및 비밀번호로 각각 확인할 수 있습니다. 
    - 고급 설정 및 대상 포털 발견 창에서 **확인**을 클릭하고 기본 iSCSI 이니시에이터 특성 화면으로 돌아가십시오. 인증 오류가 수신되면 사용자 이름 및 비밀번호 입력을 다시 확인하십시오.
![비활성 대상](/images/Inactive_0.png)
    대상 이름은 발견된 대상 섹션에 비활성 상태로 표시됩니다.  
    
    다음 단계는 대상을 활성화하는 방법을 보여줍니다. 
    
### 대상 활성화

1. **연결**을 클릭하여 대상에 연결하십시오. 
2. **다중 경로 사용** 선택란을 선택하여 대상에 대해 다중 경로 IO가 사용되도록 설정하십시오. ![다중 경로 사용](/images/Connect_0.png)
3. **고급**을 클릭하고 **CHAP 로그온 사용**을 선택하십시오.
![CHAP 사용](/images/chap_0.png)
4. 이름 필드에 사용자 이름을 입력하고 대상 시크릿 필드에 비밀번호를 입력하십시오. <br/>
**참고:** 이름 및 대상 시크릿 필드 값은 포털의 {{site.data.keyword.blockstorageshort}} 세부사항 화면에서 사용자 이름 및 비밀번호로 각각 확인할 수 있습니다. 
5. iSCSI 이니시에이터 특성 창이 표시될 때까지 **확인**을 클릭하십시오. 발견된 대상 섹션의 대상 상태는 비활성에서 연결됨으로 변경됩니다. ![연결된 상태](/images/Connected.png) 


### iSCSI 이니시에이터에서 MPIO 구성

1. iSCSI 이니시에이터를 실행하고 대상 탭에서 **특성**을 클릭하십시오.
2. 특성 창에서 **세션 추가**를 클릭하여 대상 연결 창을 실행하십시오. 
3. **다중 경로 사용** 선택란을 선택하고 **고급...**을 클릭하십시오.
  ![대상](/images/Target.png) 
  
4. 고급 설정 창에서 다음을 수행하십시오. 
   - 로컬 어댑터 및 이니시에이터 IP 필드에 대해서는 기본값을 그대로 사용하십시오. iSCSI에 대해 여러 개의 인터페이스가 있는 호스트 서버의 경우, 이니시에이터 IP 필드에 대해 해당 값을 선택해야 합니다. 
   - 대상 포털 IP 드롭 다운 목록에서 iSCSI 스토리지의 IP를 선택하십시오. 
   - **CHAP 로그온** 선택란을 클릭하십시오. 
   - 포털 창에서 가져온 이름 및 대상 시크릿 값을 입력하고 **확인**을 클릭하십시오.
   - 대상 연결 창에서 **확인**을 클릭하여 특성 창으로 돌아가십시오. 이제 특성 창의 ID 창에는 둘 이상의 세션이 표시되어야 합니다. 이제 iSCSI 스토리지에 대해 둘 이상의 세션이 있습니다.
![설정](/images/Settings.png) 
   
5. 특성 창에서 **디바이스**를 클릭하여 디바이스 창을 실행하십시오. 디바이스 인터페이스 이름의 경우 디바이스 이름 맨 앞에 mpio가 있어야 합니다. <br/>
  ![디바이스](/images/Devices.png) 
  
6. **MPIO**를 클릭하여 디바이스 세부사항 창을 실행하십시오. 이 창에서 MPIO에 대한 로드 밸런싱 정책을 선택할 수 있으며 iSCSI 경로도 표시됩니다. 이 예제에서는 두 개의 경로가 서브셋 로드 밸런싱 정책이 포함된 라운드로빈 방식으로 MPIO에 대해 사용 가능한 것으로 표시됩니다.
![DeviceDetails](/images/DeviceDetails.png) 
  
7. **확인**을 여러 번 클릭하여 iSCSI 이니시에이터를 종료하십시오. 



## Windows OSes에서 MPIO가 제대로 구성되었는지 확인하는 방법

Windows MPIO가 제대로 구성되었는지 확인하려면 우선 MPIO 추가 기능이 사용 가능한지와 필수 재부팅이 완료되었는지 확인하십시오. 

![Roles_Features_0](/images/Roles_Features_0.png)

재부팅이 완료되면 성능(Performance) 스토리지 디바이스가 추가되고 MPIO가 구성되어 작동 중인지 확인할 수 있습니다. 확인하려면 **대상 디바이스 세부사항**에서 **MPIO**를 클릭하십시오.
![DeviceDetails_0](/images/DeviceDetails_0.png)

성능(Performance) 스토리지 디바이스에서 MPIO가 구성되지 않았고 {{site.data.keyword.BluSoftlayer_full}} 팀이 유지보수를 수행하거나 네트워크 가동 중단이 발생하는 경우, 스토리지 디바이스는 연결이 끊어지고 사용할 수 없게 됩니다. MPIO를 사용하면 이런 상황에서도 추가 레벨의 연결이 가능하며 LUN에 대해 읽기/쓰기가 활성화된 세션이 계속 유지됩니다. 

## {{site.data.keyword.blockstorageshort}} 볼륨 마운트 해제 방법

다음은 MPIO iSCSI LUN에 대해 Windows 기반의 Bluemix 컴퓨팅 인스턴스 연결을 끊기 위해 필요한 단계입니다. 예제는 Windows Server 2012를 기반으로 합니다. 단계는 OS 공급업체 문서에 따라 다른 Windows 버전에 대해 조정 가능합니다. 

### iSCSI 이니시에이터를 실행하십시오. 

1. **대상** 탭을 클릭하십시오. 
2. 제거하려는 대상을 선택하고 **연결 끊기**를 클릭하십시오.

### 선택적으로 iSCSI 대상에 더 이상 액세스할 필요가 없는 경우에는 다음을 수행하십시오. 

1. iSCSI 이니시에이터에서 **발견** 탭을 클릭하십시오. 
2. 스토리지 볼륨에 연관된 대상 포털을 강조표시하고 **제거**를 클릭하십시오.
