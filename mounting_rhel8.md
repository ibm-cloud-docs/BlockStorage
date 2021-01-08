---

copyright:
  years: 2021
lastupdated: "2021-01-12"

keywords: MPIO, iSCSI LUNs, multipath configuration file, RHEL8, multipath, mpio, Linux, Red Hat Enterprise Linux 8

subcollection: BlockStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 2h

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}
{:step: data-tutorial-type='step'}

# Mount iSCSI LUN on Red Hat Enterprise Linux 8
{: #mountingRHEL8}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="2h"}

This tutorial guides you through how to mount a {{site.data.keyword.blockstoragefull}} volume on a server with the Red Hat Enterprise Linux&reg; 8 operating system. Complete the following steps to connect a Linux&reg;-based {{site.data.keyword.cloud}} Compute instance to a multipath input/output (MPIO) iSCSI storage volume. You're going to create two connections from one network interface of your host to two target IP addresses of the storage array.
{:shortdesc}

Before you begin, make sure the host that is accessing the {{site.data.keyword.blockstorageshort}} volume is authorized correctly.
{:important}

## Authorizing the host
{: #authhostrhel}

1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}. From the **menu**, select **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Locate the new volume and click the ellipsis (**...**).
4. Click **Authorize Host**.
5. To see the list of available devices or IP addresses, first, select whether you want to authorize access based on device types or subnets.
   - If you choose Devices, you can select from Bare Metal Server or Virtual server instances.
   - If you choose IP address, select the subnet where your host resides.
6. From the filtered list, select one or more hosts that are supposed to access the volume and click **Save**.

Alternatively, you can authorize the host through the SLCLI.
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to authorize.
  -v, --virtual-id TEXT     The ID of a virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of an IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```
{:codeblock}

```
# slcli block subnets-assign -h
Usage: slcli block subnets-assign [OPTIONS] ACCESS_ID
  Assign block storage subnets to the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
  --subnet-id INTEGER  ID of the subnets to assign; e.g.: --subnet-id 1234
  -h, --help           Show this message and exit.
```
{:codeblock}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{:important}

## Install the iSCSI and multipath utilities
{: #}
{: step}


## Set up the multipath.
{: #}
{: step}


## Update `/etc/iscsi/initiatorname.iscsi` file.
{: #}
{: step}


## Edit the CHAP settings.
{: #}
{: step}


## Discover the storage device and login.
{: #}
{: step}


## Verifying configuration
{: #}
{: step}


## Creating a file system (optional)
{: #}
{: step}

For more information about using the Device Mapper Multipath feature on RHEL 8, see [Configuring device mapper multipath](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/pdf/configuring_device_mapper_multipath/Red_Hat_Enterprise_Linux-8-Configuring_device_mapper_multipath-en-US.pdf){:external}.
