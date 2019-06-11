# Enhanced Evaluation - Preparation
After having agreed and discussed the points in the Workshop Example section, proceed with the enhanced POC to demonstrate that Veeam Availability Suite can work in customer's environment with excellent speed.

Typically, the enhanced POC is carried out under the following conditions:
* The environment resembles the production environment, with all firewalls in place.
* Involved storage systems are similar to the production storage systems.
* Veeam storage integration is used whenever possible.
* 100-200 VMs will be back-upped/replicated to demonstrate the scalability and great load balancing power.
* All major applications are back-upped to test all restore scenarios.

## Preparation steps
1.	Prepare for the POC planning with the Veeam User Guide and this document.
2.  Complete a POC document including all your designs and plans, including chosen servers for the tests and why they are important. Set criteria for success on each machine and what is tested and expected outcome.
3.	Check out the necessary firewall ports and help the customer with the internal firewall change requests. Refer to the corresponding sections in the User Guide and this document.

  **Tip:** Perform firewall planning thoroughly: if something is misconfigured, this may block the entire POC. In most cases, it is not easy to detect problems and introduce firewall changes, when the POC is underway. However, it is a good idea to ask the customer to have the firewall administrator at hand in case you need an urgent modification.
4.	Create a separate vCenter Server account for Veeam ONE (read-only + datastore browsing + CIM) so that you are able to track what users do.
5.	If you want to use the storage integration feature, check out the corresponding chapter in this guide, set up the storage and the SAN network together with the storage administrators. **Limit the scope** of storage system rescan **to the volumes used** in the POC.
6.	If you want to use SureBackup, make sure that a virtualized Domain Controller is present if needed (e.g. for Microsoft Exchange).
7.	Let the customer prepare all used antivirus systems upfront so that you do not run into trouble. Check the [Antivirus section](../resource_planning/protecting_veeam_backup_and_replication_config.md) of this guide and Veeam [KB1999](https://veeam.com/kb1999).
8.	Ask the customer to prepare a decent performing storage system for the POC. Avoid low-end NAS appliances for enhanced evaluations.
9.	Let the customer prepare all operating systems and database installations. Set up Veeam Backup & Replication and backup infrastructure components together with the customer and place the folders correctly.
10. Ensure that the document relating to all the testing is accurate and up to date including all success criteria for each machine being tested. This will keep control for POC, each test and a schedule can be built around the testing avoiding random testing of features.
