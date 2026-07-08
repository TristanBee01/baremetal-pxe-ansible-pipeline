# baremetal-pxe-ansible-pipeline
# Bare-Metal Automated Quality Gates & Provisioning Pipeline

## Overview
This repository contains the CI/CD pipeline definitions and Ansible playbooks used to automate the validation and provisioning of bare-metal GPU nodes. 

As a Technical Product Manager, I designed this workflow to act as the "First Customer" for our physical infrastructure, ensuring that every server passes strict hardware-level acceptance criteria before it is handed over to the Kubernetes virtualization layer (KVM).

## The Pipeline Workflow
1.  **IPMI / Redfish Interrogation:** The pipeline triggers a baseboard management controller (BMC) check to validate hardware inventory (e.g., verifying 8x H200s and correct NVMe configurations are physically present and healthy).
2.  **PXE Boot & Image Provisioning:** Ansible orchestrates the PXE boot sequence, deploying the base operating system and injecting the necessary NVIDIA drivers and CUDA toolkits.
3.  **The Quality Gate (Automated Testing):** A GitHub Actions/GitLab CI runner executes a synthetic GPU burn-in test and PCIe bandwidth check. 
4.  **Acceptance Criteria:** If GPU thermals exceed 85°C or NVMe IOPS drop below the predefined SLO, the pipeline fails, flagging the node as "Degraded" and preventing it from entering the production cluster pool.

## Key Files
*   `.github/workflows/bare-metal-validation.yml`: The CI/CD pipeline definition.
*   `ansible/playbooks/pxe-gpu-provision.yml`: The automation script for OS and driver installation.
