# Prerequisites and Local Environment
Before beginning the provisioning process, you must verify that the host machine meets the technical requirements for running virtual machines and has the necessary orchestration tools.

# Hardware Virtualization Check
Your host machine’s processor must support hardware virtualization (Intel VT-x or AMD-V), and this feature must be enabled in the BIOS/UEFI. You can verify this using the following commands, depending on your operating system:

In this guide, we’ve adapted the official prerequisites to use Multipass as the local environment instead of a traditional jumpbox, while maintaining the step-by-step process for deploying the cluster.

# Hardware Virtualization Check:
Get-ComputerInfo | Select-Object HyperVRequirement*

PowerShell:
Multipass version (Note: If it is not installed, you can download it from the official Multipass website or via Winget by running: winget install Canonical.Multipass).

# Tools and Environment
For this deployment, we’ll use Multipass on your local machine to manage the Ubuntu 24.04 virtual machines, eliminating the need to configure complex external SSH connections thanks to Multipass’s direct shell management.

In my case, the cluster requirements for an 8-GB system will be as follows:

Name	Description	CPU	RAM	Storage
server/jumpbox	Kubernetes server	    2	2GB	    10GB
node-0	Kubernetes worker node	1	1GB 	10GB
node-1	Kubernetes worker node	1	1GB	    10GB
