---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-30"

---

{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# 재해 복구를 위한 복제본 볼륨 복제

1차 사이트에서 가동 중단을 유발하는 파국적 장애나 재해가 발생하는 경우, 고객은 다음의 조치를 취하여 2차 사이트의 데이터에 빠르게 액세스할 수 있습니다.  

## 2차 사이트에서 복제본 볼륨의 중복으로 장애 복구

1. [IBM Cloud 콘솔](https://{DomainName}/catalog/){:new_window}에 로그인하여 맨 위 왼쪽의 **메뉴** 아이콘을 클릭하십시오. **일반 인프라**를 선택하십시오.  

   또는 [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}에 로그인할 수 있습니다.
2. **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
3. 목록에서 LUN의 복제본을 클릭하여 **세부사항** 페이지를 보십시오.
4. **세부사항** 페이지에서 아래로 스크롤하여 기존 스냅샷을 선택하고 **조치** > **중복**을 클릭하십시오.
5. 필요에 따라 새 볼륨의 IOP 또는 용량(크기 늘리기)을 업데이트하십시오.
6. 필요하면 새 볼륨의 스냅샷 영역을 업데이트하십시오.
7. **계속**을 클릭하여 복제에 대한 주문을 제출하십시오.

볼륨이 작성되는 즉시, 이는 호스트에 연결되어 읽기/쓰기 오퍼레이션을 수행할 수 있습니다. 원래 볼륨에서 복제로 데이터가 복사되는 동안 세부사항 페이지는 복제가 진행 중임을 표시합니다. 복제 프로세스가 완료되면 새 볼륨은 원본과 완전히 독립적인 볼륨이 되며, 정상적으로 스냅샷 및 복제로 관리될 수 있습니다.

## 원래 1차 사이트로 페일백

프로덕션을 원래 1차 사이트로 리턴하려면 다음 단계를 수행해야 합니다.

1. [IBM Cloud 콘솔](https://{DomainName}/catalog/){:new_window}에 로그인하여 맨 위 왼쪽의 **메뉴** 아이콘을 클릭하십시오. **일반 인프라**를 선택하십시오.  

   또는 [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}에 로그인할 수 있습니다.
2. **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
3. LUN 이름을 클릭하고 스냅샷 스케줄을 작성하십시오(아직 존재하지 않는 경우).  

   스냅샷 스케줄에 대한 자세한 정보는 [스냅샷 관리](working-with-snapshots.html#adding-a-snapshot-schedule)를 참조하십시오.
   {:tip}
4. **복제본**을 클릭하고 **복제본 구매**를 클릭하십시오.
5. 복제가 따라야 할 기존 스냅샷 스케줄을 선택하십시오. 목록에는 모든 활성 스냅샷 스케줄이 포함됩니다.  
6. **위치**를 클릭하고 원래 프로덕션 사이트였던 데이터 센터를 선택하십시오.
7. **계속**을 클릭하십시오.
8. **마스터 서비스 계약을 읽었습니다…** 선택란을 클릭하고 **주문하기**를 클릭하십시오.

복제가 완료되면 새 복제본의 중복 볼륨을 작성해야 합니다.
{:important}

1. **스토리지** > **{{site.data.keyword.blockstorageshort}}**로 되돌아가십시오.
2. 목록에서 LUN의 복제본을 클릭하여 **세부사항** 페이지를 보십시오.
3. **세부사항** 페이지에서 아래로 스크롤하여 기존 스냅샷을 선택하고 **조치** > **중복**을 클릭하십시오.
4. 필요에 따라 새 볼륨의 IOP 또는 용량(크기 늘리기)을 업데이트하십시오.
5. 필요하면 새 볼륨의 스냅샷 영역을 업데이트하십시오.
6. **계속**을 클릭하여 복제에 대한 주문을 제출하십시오.

복제 프로세스가 완료되면 데이터를 다시 원래 1차 사이트로 가져오는 데 사용된 볼륨 및 복제를 취소할 수 있습니다. 복제는 1차 스토리지가 되며, 원래 2차 사이트로의 복제가 다시 설정될 수 있습니다.
