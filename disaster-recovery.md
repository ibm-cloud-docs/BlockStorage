---

copyright:
  years: 2018, 2023
lastupdated: "2022-01-06"

keywords: Block Storage, inaccessible primary volume, duplicate of a replica volume, Disaster Recovery, volume duplication, replication, failover, failback

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Disaster Recovery - Fail over from an inaccessible primary volume
{: #dr-inaccessible}

If a catastrophic failure or disaster causes an outage on the primary site, customers can perform the following actions to quickly access their data on the secondary site. When the primary volume is inaccessible, you can force a failover to the remote replica.
{: shortdesc}

Authorized hosts and volumes must be in the same data center. For example, you can't have a replica volume in London and the host in Amsterdam. Both must be in London or both must be in Amsterdam.
{: note}

This action breaks the replication relationship and restoring the connection between the primary and the replica location can be time-consuming.
{: important}

## Fail over to the replica volume in the UI
{: #DRFailoverUI}
{: ui}

1. Go to your list of {{site.data.keyword.blockstoragefull}}. From the **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") menu, click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Locate and click the volume name.
3. Click **Actions**  ![Actions icon](../icons/action-menu-icon.svg "Actions")> **Failover**.
4. When the primary location is unavailable, the option of Disaster Recovery Failover becomes active. Check the box to confirm you understand the failover cannot be undone without a support case.
5. Click **Yes** to proceed.

## Fail over to the replica volume by using the SL CLI
{: #drfailoverCLI}
{: cli}

Use the following command to fail a block volume over to a specific replicant volume.
   ```python
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
   ```sh
   {
    "parameters": [replicavolumeid]
   }
   ```

### SOAP API
{: #drsoapapi}

* URL - `https://api.softlayer.com/soap/v3/SoftLayer_Network_Storage`
* Request body
   ```sh
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

## Fail back to the original primary site in the UI
{: #DRFailback2originalUI}
{: ui}

After a disaster event, {{site.data.keyword.cloud}} begins remediation work to return the impacted locations to normal operations. When the site is restored, you can initiate a Failback to the original site by clicking **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [{{site.data.keyword.cloud}} console](/classic/storage/file){: external}.

1. Click your active volume ("target").
2. In the upper right, click **Replica** and click **Actions**.
3. Select **Failback**. When the primary location is marked unavailable, the option of Disaster Recovery Failback becomes active.

   During the Disaster Recovery Failover, the system is forced to fail over to the replica site and the replication relationship is severed. To be able to fail back to the original site after the site is restored to normal operations, the system must reestablish the replication bond. This operation can take considerable amount of time. Expect a message that shows the failover is in progress. Additionally, an icon appears next to your volume on the **{{site.data.keyword.blockstorageshort}}** that indicates that an active transaction is occurring. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete. During the Failback process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in replication history.
   {: note}

4. In the upper right, click **View All {{site.data.keyword.blockstorageshort}}**.
5. Click your replica volume ("source"). This volume now has an **Inactive** status.
6. Mount and attach your storage volume to the host. For more information, [Connecting your storage](/docs/BlockStorage?topic=BlockStorage-orderingthroughConsole#mountingnewLUN).

If you need further assistance, create a [support case](https://cloud.ibm.com/unifiedsupport/supportcenter){: external}.

## Fail back from the SLCLI
{: #DRFailback2originalCLI}
{: cli}

To fail back a file volume from a specific replicant volume, use the following command.
```python
# slcli block replica-failback --help
Usage: slcli block replica-failback [OPTIONS] VOLUME_ID

Options:
 --replicant-id TEXT  ID of the replicant volume
 -h, --help           Show this message and exit.
```

During the Disaster Recovery Failover, the system is forced to fail over to the replica site and the replication relationship is severed. To be able to fail back to the original site after the site is restored to normal operations, the system must reestablish the replication bond. This operation can take considerable amount of time. During the Failback process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in the replication history.
{: note}

When the original volume is active, you can mount and attach it to the host. For more information, [Connecting your storage](/docs/BlockStorage?topic=BlockStorage-orderingthroughConsole#mountingnewLUN).

If you need further assistance, create a [support case](https://cloud.ibm.com/unifiedsupport/supportcenter){: external}.
