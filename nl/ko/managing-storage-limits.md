---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, limit increase, global quota, quota increase

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 스토리지 한계 관리
{: #managingstoragelimits}

기본적으로 총 250개의 결합된 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}} 볼륨을 전 세계적으로 프로비저닝할 수 있습니다.

보유하고 있는 볼륨이 얼마인지 잘 모르는 경우 다음 `slcli` 명령을 사용하여 각 데이터 센터의 볼륨을 나열할 수 있습니다.
```
# slcli block volume-count --help
사용법: slcli block volume-count [OPTIONS]

옵션:
  -d, --datacenter TEXT  데이터 센터 단축 이름
  --sortby TEXT          열 정렬 기준
  -h, --help             이 메시지를 표시하고 종료합니다.
```

[{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}에서 티켓을 제출하여 한계 늘리기를 요청할 수 있습니다. 요청이 승인되면 특정 데이터 센터에 대해 설정되는 볼륨 한계가 표시됩니다.  

한계 증가를 요청하려면 티켓을 열고 이를 영업 담당자에게 보내십시오.

티켓에서 다음 정보를 제공하십시오.

- **티켓 제목**: 데이터 센터 볼륨 개수 스토리지 한계 증가 요청

- **추가 볼륨 요청의 유스 케이스란 무엇입니까?** <br />
*예를 들어, 사용자의 답변은 새 VMware 데이터 저장소, 새 개발/테스트 환경, SQL 데이터베이스 또는 로깅과 유사할 수 있습니다.*

- **유형, 크기, IOPS 및 위치에 따라 필요한 추가 블록 볼륨은 얼마입니까?** <br />
*예를 들어, 사용자의 답변은 "25x Endurance2TB @ 4 IOPS in DAL09" 또는 "25x Performance 4TB @ 2 IOPS in WDC04"과 유사할 수 있습니다. *

- **유형, 크기, IOPS 및 위치에 따라 필요한 추가 파일 볼륨은 얼마입니까?** <br />
*예를 들어, 사용자의 답변은 "25x Performance 20GB @ 10 IOPS in DAL09" 또는 "50x Endurance2TB @ 0.25 IOPS in SJC03"과 유사할 수 있습니다.*

- **요청된 모든 볼륨 늘리기의 프로비저닝에 대한 예상 또는 계획 시점을 제공하십시오.** <br />
 "*예를 들어, 사용자의 답변은 "90일"과 유사할 수 있습니다.*

- **해당 볼륨의 예상 평균 용량 사용량에 대한 90일의 예측을 제공하십시오.** <br />
*예를 들어, 사용자의 답변은 "30일 내에 25% 사용, 60일 내에 50% 사용, 90일 내에 75% 사용 예상" 등이 가능합니다.*

요청의 모든 질문과 명령문에 응답하십시오. 해당 사항은 처리와 승인에 필요합니다.
{:important}

사용자는 티켓 프로세스를 통해 한계에 대한 업데이트의 알림을 받습니다.
