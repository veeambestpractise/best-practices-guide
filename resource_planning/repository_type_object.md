# Object Storage Repository

The Object Storage Repository cannot be used on it's own but has to be configured as [Capacity Tier](./repository_sobr_capacity_tier.md) in the [Scale-out Backup Repository](./repository_sobr.md).

## Lifecycle Rules & Tiering
Do **not configure any tiering or lifecycle rules** on object storage buckets used for Veeam Object Storage Repositories. This is currently **not supported**.

In fact there are currently some reasons why this is not (yet) supported:

1) Tiering and lifecycle rules in object storages are based on object age. However, with Veeam's implementation even a very old block could still be relevant for the latest offloaded backup file when the block was not changed between the restore points. An object storage vendor would not know which blocks are still relevant and which not so could not make any clever tiering decisions.
2) The storage vendor APIs are not transparent. E.g. to access Amazon S3 storage or Amazon Glacier storage requires the use of different APIs. When tiering/lifecycle management is done on Amazon side we do not know of that and could not access blocks transparently.

## Manual Deletion
Do **not delete manually** from an object storage bucket used for a Veeam Object Repository. Veeam will take care of deleteing unneeded objects based on your configured retention period.

You can safely delete everything manually when you've decomissioned the object storage repository completely (unconfigured in VBR).

## Security
Create an own bucket and own user where possible for the Object Storage Repository.

## Cost Considerations
When using public cloud object storage always consider all costs. 

Putting data to the object storage requires API PUT calls. These calls normally cost by the thousand.

When data is at rest the used resources are normally priced by GB/month.

Do never forget prices for restores. These prices do include API requests (GET) but also the egress traffic cost from the cloud datacenter which can be immense depending on how much data is required to be pulled from the cloud.
Veeam tries to leverage the locally available data as far as possible to reduce cost, but blocks which are not present on prem have to be pulled for restores.

