---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-10"

---
{:new_window: target="_blank"}

# {{site.data.keyword.blockstorageshort}} 시작하기

{{site.data.keyword.blockstoragefull}}는 컴퓨팅 인스턴스와 상관없이 프로비저닝 및 관리되는 지속적인 고성능 iSCSI 스토리지입니다. iSCSI 기반의 {{site.data.keyword.blockstorageshort}} LUN은 중복 다중 경로 I/O(MPIO) 연결을 통해 권한 부여된 디바이스에 연결됩니다.

{{site.data.keyword.blockstorageshort}}는 일치하지 않는 기능 세트를 사용하여 최고의 내구성 및 가용성을 제공합니다. 업계 표준 및 우수 사례를 사용하여 빌드되었습니다. {{site.data.keyword.blockstorageshort}}는 일관성 있는 성능 기준을 제공하여 데이터 무결성을 보호하고 유지보수 이벤트 및 플랜되지 않은 실패를 통해 가용성은 유지하도록 디자인되었습니다.

## 핵심 기능

{{site.data.keyword.blockstorageshort}}의 다음 기능을 사용해 보십시오.

- **일관된 성능 기준선**
   - 개별 볼륨에 대해 프로토콜 레벨의 IOPS 할당을 통해 제공됩니다.
- **뛰어난 내구성 및 복원성**
   - 데이터 무결성을 보호하고 개별 디스크(RAID) 배열에 대한 운영 체제 레벨의 중복 배열을 작성 및 관리할 필요없이 유지보수 이벤트 및 플랜되지 않은 실패를 통해 가용성을 유지보수합니다.
- **비활성 데이터(Data-At-Rest) 암호화**([데이터 센터 선택 시 사용 가능](new-ibm-block-and-file-storage-location-and-features.html))
   - 비활성 데이터(Data-At-Rest)에 대한 제공자 관리 암호화
- **모든 플래시 지원 스토리지**([데이터 센터 선택 시 사용 가능](new-ibm-block-and-file-storage-location-and-features.html))
   - 2 IOPS/GB 이상 레벨로 Endurance및 Performance를 사용하여 프로비저닝된 볼륨에 대한 모든 플래시 스토리지
- **스냅샷**([데이터 센터 선택 시 사용 가능](new-ibm-block-and-file-storage-location-and-features.html))
   - 중단 없이 특정 시점의 데이터 스냅샷 캡처
- **복제**([데이터 센터 선택 시 사용 가능](new-ibm-block-and-file-storage-location-and-features.html))
   - 파트너 {{site.data.keyword.BluSoftlayer_full}} 데이터 센터에 스냅샷 자동 복사
- **고가용성의 연결**
   - 가용성을 극대화하기 위해 중복 네트워킹 연결 사용 
   - iSCSI 기반 {{site.data.keyword.blockstorageshort}}에서는 다중 경로 I/O(MPIO)를 사용합니다.
- **동시 액세스**
   - 클러스터된 구성을 위해 여러 호스트가 동시에 블록 볼륨(최대 8)에 액세스하는 것을 허용합니다.
- **클러스터된 데이터베이스**
   - 클러스터된 데이터베이스와 같은 고급 유스 케이스를 지원합니다.
     
## 비용 청구

블록 LUN에 대해 시간별 또는 월별 비용 청구를 선택할 수 있습니다. LUN에 대해 선택한 비용 청구 유형은 해당 스냅샷 영역 및 복제본에 적용됩니다. 예를 들어, LUN을 시간별 비용 청구로 프로비저닝하는 경우, 모든 스냅샷 및 복제본 비용은 시간별로 청구됩니다. LUN을 월별 비용 청구로 프로비저닝하는 경우, 모든 스냅샷 및 복제본 비용은 월별로 청구됩니다. 

**시간별 비용 청구**를 사용하면 계정에서 블록 LUN이 존재하는 시간은 LUN이 삭제되는 시점 또는 비용 청구 주기 종료 시점(둘 중 빠른 경우)에 계산합니다. 시간별 비용 청구는 며칠 정도 또는 한 달 미만으로 사용되는 스토리지에 적합합니다. 시간별 비용 청구는 [데이터 센터 선택](new-ibm-block-and-file-storage-location-and-features.html)에서 프로비저닝된 스토리지에만 사용할 수 있습니다. 

**월별 비용 청구**를 사용하는 경우, 비용 계산은 작성 날짜부터 비용 청구 주기 종료 시점까지 비례 배분되며 즉시 청구됩니다. 비용 청구 주기 종료 전에 LUN이 삭제되는 경우에는 환불되지 않습니다. 월별 비용 청구는 장시간(한달 이상) 저장 및 액세스해야 하는 데이터를 사용하는 프로덕션 워크로드에서 사용되는 스토리지에 적합합니다. 

**Performance**
<table>
  <caption>표 1에는 월별 및 시간별로 비용을 청구하는 Performance 스토리지 가격이 표시되어 있습니다.</caption>
  <tr>
   <th>월별 가격</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>시간별 가격</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
</table>
 
**Endurance**
<table>
  <caption>표 2에는 월별 및 시간별 비용 청구 옵션을 사용하는 각 티어의 Endurance 스토리지 가격이 표시되어 있습니다.</caption>
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
</table>



## 프로비저닝

{{site.data.keyword.blockstorageshort}} LUN은 20GB부터 12TB까지 프로비저닝 가능하며 다음과 같은 두 개의 옵션이 있습니다. <br/>
- 사전 정의된 성능 레벨과 스냅샷 및 복제와 같은 기타 기능을 제공하는 **Endurance** 티어를 프로비저닝합니다.
- 초당 할당된 입출력(I/O) 오퍼레이션(IOPS)이 있는 강력한 **Performance** 환경을 빌드합니다. 

### Endurance 티어를 사용하여 프로비저닝

Endurance{{site.data.keyword.blockstorageshort}}는 다양한 애플리케이션의 요구를 지원하기 위해 4개의 IOPS 성능 티어에서 사용할 수 있습니다. <br />

- **0.25 IOPS/GB**는 I/O 집약도가 낮은 워크로드를 대상으로 디자인되었습니다. 일반적으로 해당 워크로드는 임의 시점에 비활성 데이터의 비율이 높은 특성이 있습니다. 예를 들어 메일함 저장 또는 부서 레벨 파일 공유가 이에 해당합니다.

- **2 IOPS/GB**는 가장 일반적인 용도에 맞게 디자인되었습니다. 예를 들어 웹 애플리케이션 또는 하이퍼바이저를 위한 VM 디스크 이미지를 지원하는 소규모 데이터베이스의 호스팅이 이에 해당합니다.

- **4 IOPS/GB**는 집약도 높은 워크로드를 대상으로 디자인되었습니다. 일반적으로 해당 워크로드는 임의 시점에 활성 데이터의 비율이 높은 특성이 있습니다. 예를 들어, 트랜잭션 데이터베이스 또는 그 외 성능에 민감한 데이터베이스가 이에 해당합니다.

- **10 IOPS/GB**는 NoSQL 데이터베이스로 작성된 워크로드 및 분석을 위한 데이터 처리와 같이 가장 수요가 많은 워크로드를 대상으로 디자인되었습니다. 이 티어는 [데이터 센터 선택](new-ibm-block-and-file-storage-location-and-features.html) 시에만 최대 4TB의 스토리지 프로비저닝에 사용할 수 있습니다.

12TB Endurance볼륨에 최대 48,000 IOPS가 사용 가능합니다.
 
워크로드에 맞는 Endurance 티어를 선택하는 것이 중요합니다. 최대 성능 달성에 필요한 올바른 블록 크기, 이더넷 연결 속도 및 호스트 수를 사용하는 것도 매우 중요합니다. 이들 중 하나라도 제대로 맞지 않는 경우, 처리 결과에 심각한 영향을 줄 수 있습니다.

 
### Performance를 사용하여 프로비저닝

Performance는 Endurance티어 내에는 잘 맞지 않는 성능 요구사항이 있는 높은 I/O의 애플리케이션을 지원하도록 설계된 {{site.data.keyword.blockstorageshort}}의 클래스입니다. 예측 가능한 성능은 프로토콜 레벨의 IOPS를 개별 볼륨에 할당하여 얻을 수 있습니다. 100 - 48,000의 다양한 IOPS 속도는 20GB - 12TB 범위의 스토리지 크기로 프로비저닝할 수 있습니다. 

{{site.data.keyword.blockstorageshort}}용 Performance는 다중 경로 I/O(MPIO) iSCSI(internet Small Computer System Interface) 연결을 통해 액세스하고 마운트합니다. 일반적으로 {{site.data.keyword.blockstorageshort}}는 단일 서버에서 볼륨에 액세스하는 경우에 사용됩니다. 다중 볼륨은 더 큰 볼륨 및 더 높은 IOPS 수를 얻기 위해 호스트에 마운트되어 같이 스트라이프 가능합니다. Performance 볼륨은 표 3의 크기 및 IOPS 속도에 따라 주문 가능합니다(Linux, XEN 및 Windows 운영 체제).


<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>표 3에서는 성능 스토리지의 크기와 IOPS 조합을 표시합니다.<br/><sup><img src="/images/numberone.png" alt="각주" /></sup> 6,000이 넘는 IOPS 한계는 데이터 센터 선택 시 사용할 수 있습니다.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
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
</table>


Performance 볼륨은 프로비저닝된 IOPS 레벨과 계속 근접하게 작동하도록 디자인되었습니다. 일관성이 있으며 특정 성능 레벨로 애플리케이션 환경을 크기 조정하고 스케일링하기 쉬워집니다. 또한 이상적인 가격 대 성능 비율의 볼륨을 빌드하여 환경을 최적화할 수 있습니다.

### 프로비저닝 고려사항

**블록 크기**

Endurance및 Performance 모두의 IOPS는 50/50 읽기/쓰기 50% 랜덤 워크로드의 16KB 블록 크기를 기반으로 합니다. 16KB 블록은 볼륨에 한 번 쓰는 것에 해당합니다.

애플리케이션에서 사용하는 블록 크기는 스토리지 성능에 직접적인 영향을 줍니다. 애플리케이션에서 사용하는 블록 크기가 16KB보다 작은 경우, IOPS 한계가 처리량 한계 이전에 실현됩니다. 반대로, 애플리케이션에서 사용하는 블록 크기가 16KB보다 큰 경우, 처리량 한계가 IOPS 한계 이전에 실현됩니다.

<table>
  <caption>표 4에는 블록 크기와 IOPS가 처리량에 미치는 영향을 보여주는 예가 있습니다.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>블록 크기(KB)</th>
            <th>IOPS</th>
            <th>처리량(MB/s)</th>
          </tr>
        </thead>
        <tbody>
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

**권한 부여된 호스트**

또한, 볼륨을 사용 중인 호스트 수도 고려해야 합니다. 볼륨에 액세스하는 단일 호스트가 있는 경우, 사용 가능한 최대 IOPS를 실현하는 것은 어려울 수 있으며 특히 10,000과 같이 IOPS 수가 큰 경우에는 더욱 그렇습니다. 워크로드에 높은 처리량이 요구되는 경우, 단일 서버 병목 현상을 방지하기 위해 최소한 두서너 개의 서버가 볼륨에 액세스하도록 구성하는 것이 바람직합니다.

**네트워크 연결**

이더넷 연결 속도는 볼륨의 최대 예상 처리량보다 빨라야 합니다. 일반적으로 이더넷 연결이 포화 상태가 되지 않으려면 사용 가능한 대역폭의 70%를 넘지 않아야 합니다. 예를 들어, 6000 IOPS에 16KB의 블록 크기를 사용 중인 경우, 볼륨은 대략적으로 94MBps 처리량을 처리할 수 있습니다. LUN에 1Gbps 이더넷 연결을 사용 중인 경우, 서버가 최대 가용 처리량을 사용하려고 시도하면 병목 현상이 발생합니다. 1Gbps 이더넷 연결(초당 125MB)에 대한 이론적 한계의 70%는 초당 88MB만 처리할 수 있기 때문입니다.

최대 IOPS를 달성하려면 적절한 네트워크 리소스가 사용 가능해야 합니다. 이 밖에도 스토리지 외부의 사설 네트워크 사용과 호스트 측 및 애플리케이션 특정 튜닝(IP 스택 또는 큐 깊이 및 기타 설정)도 고려해야 합니다.

스토리지 트래픽은 공용 Virtual Server의 총 네트워크 사용에 포함됩니다. 서비스에서 부과할 수 있는 한계를 이해하려면 [Virtual Server 문서](https://console.bluemix.net/docs/vsi/vsi_public.html#public-virtual-servers)를 참조하십시오. 

## 주문 제출

주문을 제출할 준비가 되면 [여기](provisioning-block_storage.html) 지시사항을 따르십시오.

## 새 스토리지 연결

프로비저닝 요청이 완료되면 새 스토리지에 액세스하고 연결을 구성하도록 호스트에 권한을 부여하십시오. 호스트의 운영 체제에 따라 해당 링크를 따르십시오.
- [Linux에서 MPIO iSCSI LUN에 연결](accessing_block_storage_linux.html)
- [Microsoft Windows에서 MPIO iSCSI LUNS 연결](accessing-block-storage-windows.html)
- [cPanel을 사용하여 Block Storage 구성](configure-backup-cpanel.html)
- [Plesk를 사용하여 Block Storage 구성](configure-backup-plesk.html)

