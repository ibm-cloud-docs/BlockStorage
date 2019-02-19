---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Managing storage limits
{: #managingstoragelimits}

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}} volumes globally.

If you're unsure how many volumes you have, you can list your volumes for each data center by using the following `slcli` command.
```
# slcli block volume-count --help
Usage: slcli block volume-count [OPTIONS]

Options:
  -d, --datacenter TEXT  Datacenter shortname
  --sortby TEXT          Column to sort by
  -h, --help             Show this message and exit.
```

You can request a limit increase by submitting a ticket in the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}. When the request is approved, you get a volume limit that is set for a specific data center.  

To request a limit increase, open a ticket and direct it to your sales representative.

In the ticket, provide the following information:

- **Ticket Subject**: Request to Increase Data Center Volume Count Storage Limit

- **What is the use case for the additional volumes request?** <br />
*For example, your answer might be something similar to a new VMware datastore, a new development and testing (dev/test) environment, an SQL database, or logging.*

- **How many extra Block volumes are needed by type, size, IOPS, and location?** <br />
*For example, your answer might be something similar to "25x Endurance 2 TB @ 4 IOPS in DAL09" or "25x Performance 4 TB @ 2 IOPS in WDC04".*

- **How many extra File volumes are needed by type, size, IOPS, and location?** <br />
*For example, your answer might be something similar to "25x Performance 20 GB @ 10 IOPS in DAL09" or "50x Endurance 2 TB @ 0.25 IOPS in SJC03".*

- **Provide an estimate of when you expect or plan to provision all of the requested volume increase.** <br />
 "*For example, your answer might be something similar to "90 days".*

- **Provide a 90-day forecast of expected average capacity usage of these volumes.** <br />
*For example, your answer might be something similar to "expect 25 percent to be used in 30 days, 50 percent to be used in 60 days and 75 percent to be used in 90 days".*

Respond to all questions and statements in your request. They are required for processing and approval.
{:important}

You're going to be notified of the update to your limits through the ticket process.
