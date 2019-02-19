---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 새 위치 및 기능
{: #news}

{{site.data.keyword.BluSoftlayer_full}}에서는 {{site.data.keyword.blockstoragefull}}의 새 버전을 소개합니다.

새 스토리지는 데이터 센터 선택 시에 사용 가능하며 비활성 데이터(data-at-rest)에 대한 디스크 레벨의 암호화가 사용되는 더 높은 IOPS의 플래시 스토리지로 지원됩니다. 업그레이드된 데이터 센터에서 프로비저닝된 모든 스토리지는 새 버전으로 자동 작성됩니다.

새 볼륨의 NFS 마운트 위치는 암호화되지 않은 볼륨의 마운트 위치와 다릅니다. 자세한 정보는 [암호화된 {{site.data.keyword.blockstorageshort}} 볼륨의 새 마운트 위치](#mountpoints) 섹션을 참조하십시오.
{:important}

## 새 위치
{: #new-locations}

새 {{site.data.keyword.blockstorageshort}}는 다음 영역 및 데이터 센터에서 사용할 수 있습니다.
<table role="presentation">
  <tr>
    <td><strong>US 2</strong></td>
    <td><strong>EU</strong></td>
    <td><strong>오스트레일리아</strong></td>
    <td><strong>캐나다</strong></td>
    <td><strong>라틴 아메리카</strong></td>
    <td><strong>아시아 태평양</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
        SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
    </td>
    <td>AMS01<br />
        AMS03<br />
	FRA02<br />
	FRA04<br />
	FRA05<br />
	LON02<br />
	LON04<br />
	LON05<br />
	LON06<br />
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
        SYD01<br />
        SYD04<br />
        SYD05<br />
        <br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
        TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
        SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
        HKG02<br />
	SEO01<br />
	SNG01<br />
        TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*표 1은 데이터 센터 가용성을 표시합니다. 각 지역에는 고유한 열이 있습니다. 일부 도시(예: Dallas, San Jose, Washington, Amsterdam, Frankfurt, London, Sydney)에는 데이터 센터가 여러 개 있습니다.*

## 새 기능 및 특성
{: #features}

- **[비활성 데이터(Data-At-Rest)에 대한 제공자 관리 암호화](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)**.
  모든 {{site.data.keyword.blockstorageshort}}는 추가 비용 없이 자동으로 암호화된 상태로 프로비저닝됩니다.
- **10 IOPS/GB 티어 옵션**.
  Endurance유형 {{site.data.keyword.blockstorageshort}}에 새 티어가 사용 가능하여 요구되는 대부분의 워크로드를 지원합니다.
- **모든 플래시 지원 스토리지.**
  모든 플래시 스토리지로 지원되는 2 IOPS/GB 이상에서 Endurance또는 Performance 유형으로 프로비저닝되는 모든 {{site.data.keyword.blockstorageshort}}.
- {{site.data.keyword.blockstorageshort}}가 포함된 **스냅샷 및 복제** 지원
- 한 달 미만으로 사용되는 스토리지를 위해 **시간별 비용 청구** 옵션이 추가되었습니다.
- {{site.data.keyword.blockstorageshort}}에 대해 **최대 48,000 IOPS**가 Performance로 프로비저닝됩니다.
- 계절별 로드 변경으로 인해 성능이 향상될 수 있도록 **IOPS 속도를 조정할 수 있습니다**. 이 기능에 대해 자세한 정보는 [여기](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS)를 참조하십시오.
- **[{{site.data.keyword.blockstorageshort}} 볼륨 복제 기능](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)**으로 데이터에 대한 복제본을 작성하십시오.
- 복제를 작성하거나 데이터를 더 큰 볼륨으로 수동으로 마이그레이션하지 않고도 GB 단위로 최대 12TB까지 **스토리지를 확장할 수 있습니다**. 이 기능에 대해 자세한 정보는 [여기](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity)를 참조하십시오.

## 암호화된 스토리지 볼륨의 새 마운트 위치
{: #mountpoints}

이 데이터 센터에서 프로비저닝되는 개선된 모든 스토리지 볼륨의 마운트 위치는 암호화되지 않은 볼륨의 위치와 다릅니다. [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}의 **볼륨 세부사항** 페이지에서 마운트 지점 정보를 확인하여 올바른 마운트 지점을 사용 중인지 확인하십시오. 또한 API 호출 `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 올바른 마운트 위치 정보를 가져올 수 있습니다.

새 기능에 모두 액세스할 수 있으려면 API를 통해 주문할 때 `Storage-as-a-Service Package 759`를 선택하십시오. API를 통해 {{site.data.keyword.blockstorageshort}}를 주문하는 데 관한 자세한 정보는 [order_block_volume ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){:new_window}을 참조하십시오.
{:important}

여기에서는 추가 데이터 센터가 업그레이드된 시기 및 {{site.data.keyword.blockstorageshort}}에 대해 추가 중인 새로운 기능에 대해서도 확인할 수 있습니다.
{:tip}
