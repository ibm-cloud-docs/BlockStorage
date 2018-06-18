---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} 정보

{{site.data.keyword.blockstoragefull}}는 컴퓨팅 인스턴스와 상관없이 프로비저닝 및 관리되는 지속적인 고성능 iSCSI 스토리지입니다. iSCSI 기반의 {{site.data.keyword.blockstorageshort}} LUN은 중복 다중 경로 I/O(MPIO) 연결을 통해 권한 부여된 디바이스에 연결됩니다.

{{site.data.keyword.blockstorageshort}}는 일치하지 않는 기능 세트를 사용하여 최고의 내구성 및 가용성을 제공합니다. 업계 표준 및 우수 사례를 사용하여 빌드되었습니다. {{site.data.keyword.blockstorageshort}는 일관성 있는 성능 기준을 제공하여 데이터 무결성을 보호하고 유지보수 이벤트 및 플랜되지 않은 실패를 통해 가용성은 유지하도록 디자인되었습니다.

## 핵심 기능

{{site.data.keyword.blockstorageshort}}의 다음 기능을 사용해 보십시오.

- **일관된 성능 기준선**
   - 개별 볼륨에 대해 프로토콜 레벨의 IOPS 할당을 통해 제공됩니다.
- **뛰어난 내구성 및 복원성**
   - 데이터 무결성을 보호하고 개별 디스크(RAID) 배열에 대한 운영 체제 레벨의 중복 배열을 작성 및 관리할 필요없이 유지보수 이벤트 및 플랜되지 않은 실패를 통해 가용성을 유지보수합니다.
- **비활성 데이터(Data-At-Rest) 암호화**([데이터 센터 선택 시 사용 가능](new-ibm-block-and-file-storage-location-and-features.html).)
   - 비활성 데이터(Data-At-Rest)에 대한 제공자 관리 암호화
- **모든 플래시 지원 스토리지**([데이터 센터 선택 시 사용 가능](new-ibm-block-and-file-storage-location-and-features.html).)
   - 2 IOPS/GB 이상으로 내구성(Endurance) 및 성능(Performance)을 사용하여 프로비저닝된 볼륨에 대한 모든 플래시 스토리지
- **스냅샷**([데이터 센터 선택](new-ibm-block-and-file-storage-location-and-features.html) 시)
   - 중단 없이 특정 시점의 데이터 스냅샷 캡처
- **복제**([데이터 센터 선택](/new-ibm-block-and-file-storage-location-and-features.html) 시)
   - 파트너 {{site.data.keyword.BluSoftlayer_full}} 데이터 센터에 스냅샷 자동 복사
- **고가용성의 연결**
   - 가용성을 극대화하기 위해 중복 네트워킹 연결 사용 - iSCSI 기반의 {{site.data.keyword.blockstorageshort}}는 다중 경로 I/O(MPIO) 사용
- **동시 액세스**
   - 클러스터된 구성을 위해 여러 호스트가 동시에 블록 볼륨(최대 8)에 액세스하는 것을 허용합니다.
- **클러스터된 데이터베이스**
   - 클러스터된 데이터베이스와 같은 고급 유스 케이스를 지원합니다.
     
## 시간별/월별 비용 청구

블록 LUN에 대해 시간별 또는 월별 비용 청구를 선택할 수 있습니다. LUN에 대해 선택한 비용 청구 유형은 해당 스냅샷 영역 및 복제본에 적용됩니다. 예를 들어, LUN을 시간별 비용 청구로 프로비저닝하는 경우, 모든 스냅샷 및 복제본 비용은 시간별로 청구됩니다. LUN을 월별 비용 청구로 프로비저닝하는 경우, 모든 스냅샷 및 복제본 비용은 월별로 청구됩니다. 

**시간별 비용 청구**를 사용하면 계정에서 블록 LUN이 존재하는 시간 계산은 LUN이 삭제되는 시점 또는 비용 청구 주기 종료 시점(둘 중 빠른 경우)에서 수행됩니다.  시간별 비용 청구는 며칠 정도 또는 한 달 미만으로 사용되는 스토리지에 적합합니다. 시간별 비용 청구는 [데이터 센터 선택](new-ibm-block-and-file-storage-location-and-features.html)에서 프로비저닝되는 스토리지에서만 사용할 수 있습니다. 

**월별 비용 청구**를 사용하는 경우, 비용 계산은 작성 날짜부터 비용 청구 주기 종료 시점까지 비례 배분되며 즉시 청구됩니다. 비용 청구 주기 종료 전에 LUN이 삭제되는 경우에는 환불되지 않습니다.  월별 비용 청구는 장시간(한달 이상) 저장 및 액세스해야 하는 데이터를 사용하는 프로덕션 워크로드에서 사용되는 스토리지에 적합합니다. 

### 성능(Performance):
<table>
 <tbody>
  <tr>
   <th>월별 가격</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>시간별 가격</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
  </tbody>
</table>
 
### 내구성(Endurance):
<table>
 <tbody>
  <tr>
   <th>IOPS 티어</th>
   <th>0.25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>월별 가격</th>
   <td>$0.10/GB</td>
   <td>$0.20/GB</td>
   <td>$0.35/GB</td>
   <td>$0.58/GB</td>
  </tr>
  <tr>
   <th>시간별 가격</th>
   <td>0.0002/GB</td>
   <td>$0.0003/GB</td>
   <td>$0.0005/GB</td>
   <td>$0.0009/GB</td>
  </tr>
  </tbody>
</table>



## 프로비저닝

{{site.data.keyword.blockstorageshort}} LUN은 20GB부터 12TB까지 프로비저닝 가능하며 프로비저닝 시에는 두 개의 옵션이 있습니다. <br/>
- 사전 정의된 성능 레벨과 스냅샷 및 복제와 같은 기능을 수행하는 **내구성(Endurance)** 티어를 프로비저닝합니다.
- 초당 할당된 입출력(I/O) 오퍼레이션(IOPS)이 있는 강력한 **성능(Performance)** 환경을 빌드합니다. 

### 내구성(Endurance) 티어

내구성(Endurance)은 다양한 애플리케이션의 요구를 지원하기 위해 세 개의 IOPS 성능 티어에서 사용할 수 있습니다. <br />

- **0.25 IOPS/GB**는 I/O 집약도가 낮은 워크로드를 대상으로 디자인되었습니다. 일반적으로 이런 워크로드는 특정 시점에 비활성 데이터의 비율이 높은 특성이 있습니다. 예를 들어 메일함 저장 또는 부서 레벨 파일 공유가 이에 해당합니다.

- **2 IOPS/GB**는 가장 일반적인 용도에 맞게 디자인되었습니다. 예를 들어 웹 애플리케이션 또는 하이퍼바이저를 위한 가상 머신 디스크 이미지를 지원하는 소규모 데이터베이스의 호스팅이 이에 해당합니다.

- **4 IOPS/GB**는 집약도 높은 워크로드를 대상으로 디자인되었습니다. 일반적으로 이런 워크로드는 특정 시점에 활성 데이터의 비율이 높은 특성이 있습니다. 예를 들어, 트랜잭션 데이터베이스 또는 그 외 성능에 민감한 데이터베이스가 이에 해당합니다.

- **10 IOPS/GB**는 NoSQL 데이터베이스로 작성된 워크로드 및 분석을 위한 데이터 처리와 같이 가장 수요가 많은 워크로드를 대상으로 디자인되었습니다.  이 티어는 [데이터 센터 선택](new-ibm-block-and-file-storage-location-and-features.html)에서 최대 4TB 크기의 스토리지 프로비저닝에 사용할 수 있습니다.

12TB 내구성(Endurance) 볼륨에 최대 48,000 IOPS가 사용 가능합니다.
 
워크로드에 대해 내구성(Endurance) {{site.data.keyword.blockstorageshort}}의 올바른 티어를 선택하는 것이 중요하며, 최대 성능 달성에 필요한 블록 크기, 이더넷 연결 속도, 호스트 수를 사용하는 것도 매우 중요합니다. 이들 중 하나라도 제대로 맞지 않는 경우, 처리 결과에 심각한 영향을 줄 수 있습니다.

 
### 성능(Performance)

성능(Performance)은 내구성(Endurance) 티어 내에는 잘 맞지 않는 성능 요구사항이 있는 높은 I/O의 애플리케이션을 지원하도록 설계된 {{site.data.keyword.blockstorageshort}}의 클래스입니다. 예측 가능한 성능은 프로토콜 레벨의 IOPS를 개별 볼륨에 할당하여 얻을 수 있습니다. 100 - 48,000 사이의 IOPS 속도는 20GB - 12TB 범위의 스토리지 크기로 프로비저닝할 수 있습니다. 

{{site.data.keyword.blockstorageshort}}용 성능(Performance)은 다중 경로 I/O(MPIO) iSCSI(Internet Small Computer System Interface) 연결을 통해 액세스하고 마운트합니다. 일반적으로 {{site.data.keyword.blockstorageshort}}는 단일 시스템에서 볼륨에 액세스하는 경우에 사용됩니다. 다중 볼륨은 더 큰 볼륨 및 더 높은 IOPS 수를 얻기 위해 호스트에 마운트되어 같이 스트라이프 가능합니다. 성능(Performance) 볼륨은 표 1의 크기 및 IOPS에 따라 주문 가능합니다(Linux, XEN, VMware 및 Windows 운영 체제).


<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>크기(GB)</th>
            <th>최소 IOPS</th>
            <th>최대 IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1,000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2,000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4,000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6,000 또는 10,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
          <tr>
            <td>1,000</td>
            <td>100</td>
            <td>6,000 또는 20,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
          <tr>
            <td>2,000-3,000</td>
            <td>200</td>
            <td>6,000 또는 40,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
          <tr>
            <td>4,000-7,000</td>
            <td>300</td>
            <td>6,000 또는 48,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
          <tr>
            <td>8,000-9,000</td>
            <td>500</td>
            <td>6,000 또는 48,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
          <tr>
            <td>10,000-12,000</td>
            <td>1,000</td>
            <td>6,000 또는 48,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
        </tbody>
</table>

<sup>![각주](/images/numberone.png)</sup> [데이터 센터 선택](new-ibm-block-and-file-storage-location-and-features.html)에서는 6,000 이상의 IOPS 한계를 사용할 수 있습니다.


성능(Performance) 볼륨은 프로비저닝된 IOPS 레벨과 계속 근접하게 수행되도록 디자인되었습니다. 일관성이 있으며 지정된 성능 레벨로 애플리케이션 환경을 크기 조정하고 스케일링하기 쉬워집니다. 또한 주어진 볼륨 크기 및 IOPS 개수 범위에서 이상적인 가격 대 성능 비율의 볼륨을 빌드하여 환경을 최적화할 수 있습니다.

### {{site.data.keyword.blockstorageshort}}에 대한 프로비저닝 IOPS 팁

내구성(Endurance) 및 성능(Performance) 모두의 IOPS는 50/50 읽기/쓰기 50% 랜덤 워크로드의 16KB 블록 크기를 기반으로 합니다. 16KB 블록은 볼륨에 한 번 쓰는 것에 해당합니다.

사용자 애플리케이션에서 사용되는 블록 크기는 스토리지 성능에 직접적인 영향을 줍니다. 애플리케이션에서 사용하는 블록 크기가 16KB보다 작은 경우, IOPS 한계가 처리량 한계 이전에 실현됩니다.  반대로, 애플리케이션에서 사용하는 블록 크기가 16KB보다 큰 경우, 처리량 한계가 IOPS 한계 이전에 실현됩니다.

블록 크기를 변경하면 다음과 같이 성능에 영향을 줍니다.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>블록 크기(KB)</th>
            <th>IOPS</th>
            <th>처리량(MB/s)</th>
          </tr>
          <tr>
            <td>4(일반적으로 Linux용)</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8(일반적으로 Oracle용)</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32(일반적으로 SQLServer용)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

사용자의 워크로드에 적합한 {{site.data.keyword.blockstorageshort}}를 선택하는 것이 중요하며 병목 현상을 방지하는 것도 중요합니다. 이더넷 연결 속도는 볼륨의 최대 예상 처리량보다 빨라야 합니다. 일반적으로 이더넷 연결이 포화 상태가 되지 않으려면 사용 가능한 대역폭의 70%를 넘지 않아야 합니다. 예를 들어, 6000 IOPS에 16KB의 블록 크기를 사용 중인 경우, 볼륨은 대략적으로 초당 94MB를 처리할 수 있습니다. LUN에 1Gbps 이더넷 연결을 사용 중인 경우, 서버가 최대 가용 처리량을 사용하려고 시도하면 병목 현상이 발생합니다. 1Gbps 이더넷 연결(초당 125MB)에 대한 이론적 한계의 70%는 초당 88MB만 처리할 수 있기 때문입니다.


또한, 볼륨을 사용 중인 호스트 수도 고려해야 합니다. 볼륨에 액세스하는 단일 호스트가 있는 경우, 사용 가능한 최대 IOPS를 실현하는 것은 어려울 수 있으며 특히 10,000과 같이 IOPS 수가 큰 경우에는 더욱 그렇습니다. 워크로드에 높은 처리량이 요구되는 경우, 단일 서버 병목 현상을 방지하기 위해 최소한 두 개 또는 세 개의 서버가 볼륨에 액세스하도록 구성하는 것이 바람직합니다.


최대 IOPS를 달성하려면 적절한 네트워크 리소스가 사용 가능해야 합니다. 이 밖에도 스토리지 외부의 사설 네트워크 사용과 호스트 측 및 애플리케이션별 고유 튜닝(IP 스택, 큐 깊이 등)도 고려해야 합니다.
