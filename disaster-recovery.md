---

copyright:
  years: 2018, 2021
lastupdated: "2021-09-01"

keywords: Block Storage, inaccessible Primary volume, duplicate of a replica volume, Disaster Recovery, volume duplication, replication, failover, failback

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Disaster Recovery - Fail over from an inaccessible primary volume
{: #dr-inaccessible}

If a catastrophic failure or disaster causes an outage on the primary site, customers can perform the following actions to quickly access their data on the secondary site. When the primary volume is inaccessible, you can force a failover to the remote replica.
{: shortdesc}

Authorized hosts and volumes must be in the same data center. For example, you can't have a replica volume in London and the host in Amsterdam. Both must be in London or both must be in Amsterdam.
{: note}

This action breaks the replication relationship and cannot be undone without manual intervention from the support team.
{: important}

## Fail over to the replica volume in the UI
{: #DRFailoverUI}
{: ui}

1. Go to your list of {{site.data.keyword.blockstoragefull}}. From the **Classic Infrastructure** menu, click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Locate and click the volume name.
3. Click **Actions** > **Failover**.
4. When the primary location is unavailable, the option of Disaster Recovery Failover becomes active. Check the box to confirm you understand the failover cannot be undone without a support case.
5. Click **Yes** to proceed.

## Fail over to the replica volume by using the SL CLI
{: #drfailoverCLI}
{: cli}

Use the following command to fail a block volume over to a specific replicant volume.
  ```
  # slcli block disaster-recovery-failover --help
  Usage: slcli block disaster-recovery-failover [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  -h, --help           Show this message and exit.
  ```

## Fail over to the replica volume by using the API
{: #drfailoverAPI}
{: api}

### REST API
{: #drrestapi}
* URL - `https://USERNAME:APIKEY@api.softlayer.com/rest/v3/SoftLayer_Network_Storage/primaryvolumeId/disasterRecoveryFailoverToReplicant`
* Request body
  ```
  {
    "parameters": [replicavolumeid]
  }
  ```

### SOAP API
{: #drsoapapi}
* URL - `https://api.softlayer.com/soap/v3/SoftLayer_Network_Storage`
* Request body
  ```
  <?xml version="1.0" encoding="UTF-8"?>
  <SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://api.service.softlayer.com/soap/v3.1/">
   <SOAP-ENV:Header>
    <ns1:authenticate>
     <username>USERNAME</username>
     <apiKey>APIKEY</apiKey>
    </ns1:authenticate>
    <ns2:SoftLayer_Network_StorageInitParameters>
     <id>primary Volume Id</id>
    </ns2:SoftLayer_Network_StorageInitParameters>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
    <ns1:disasterRecoveryFailoverToReplicant>
      <replicantId xsi:type="int">replica Volume ID</replicantId>
    </ns1:disasterRecoveryFailoverToReplicant>
   </SOAP-ENV:Body>
  </SOAP-ENV:Envelope>
  ```

## Fail back to the original primary site

If you want to return production to the original primary site, create a [support case](https://cloud.ibm.com/unifiedsupport/supportcenter){: external}. For more information about opening a support case or case severities and response times, see [Viewing support cases](/docs/get-support?topic=get-support-managing-support-cases) or [Escalating support cases](/docs/get-support?topic=get-support-escalation).
