# Accelerated Evaluation

Many customers decide to do a small scale Proof of Concept (POC) after seeing their first live demonstration and presentation meetings with partners or Veeam System Engineers. The idea is to get started with the interface of Veeam Backup & Replication and to test if everything works as expected/presented within the customer’s environment.

As enterprise environments are sometimes very complicated from the firewall and storage perspective, in most cases customers decide to do a POC in small test environments. Typically, a test environment includes:

* ESXi hosts, vCenter Server, Veeam Backup & Replication server
* 10-20 VMs running various business applications

It is possible to carry out a Veeam Backup & Replication POC in such environment with only a single Veeam backup server on a VM with 8 cores and 8-16GB of RAM. (Since this test is focused on the user interface experience, no special preparation is needed from the performance perspective.)

Customers often drive this POC themselves. To assist customers with this task, Veeam has published a good Evaluator's Guide that includes configuration screenshots with minimal required background information.

One thing to remember when running a POC with Veeam is that you want to test something with meaning, testing a backup because it backs up is important however having a goal is also important.

Even for a small POC a plan is essential, write up can be as simple as:

* How many machines, set a specific number and record their names.
* What applications are you testing and why, what is the criteria for success on each machine.
* What types of recovery are you going to test and why (Veeam currently has 57 ways to recover).
* What are your expectations from the testing process.
* What functionality do you want to see in action.

We all know Veeam will protect virtual machines, the aim of your POC should be to see how well it lives up to your expectation at doing specific types of protection and recovery.

See Veeam Helpcenter for Evaluator's Guide:
* [VMware vSphere environments](http://helpcenter.veeam.com/evaluation/backup/vsphere/en/)
* [Microsoft Hyper-V environments](http://helpcenter.veeam.com/evaluation/backup/hyperv/en/)
