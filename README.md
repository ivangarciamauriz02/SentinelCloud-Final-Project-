# SentinelCloud

An automated active defense and network security system designed to enforce dynamic micro-segmentation for IoT devices within cloud environments. Developed as a Final Graduation Project (Grade: 8.0/10).

## Project Overview
Device insecurity in corporate IoT environments represents a critical attack vector. **SentinelCloud** mitigates this risk by continuously monitoring network logs and executing automated quarantine protocols via Python when anomalous behavior or a known threat signature is detected.

## Architecture & Flow
`Compromised IoT Device` ──> `Anomalous Traffic/Log` ──> `Python Detection Script` ──> `AWS API Call (Boto3)` ──> `Security Group Revocation` ──> `Isolation`

### Core Features:
*   **Automated Triage:** Real-time analysis of simulated network traffic logs.
*   **Active Defense:** Immediate automated isolation of compromised hosts by updating AWS Security Groups and ACLs in <5 seconds.
*   **Perimeter Hardening:** Implements strict least-privilege access rules dynamically.

## Tech Stack
*   **Language:** Python 3.x
*   **Cloud Infrastructure:** Amazon Web Services (AWS VPC, Security Groups, IAM Roles)
*   **SDK:** Boto3 (AWS SDK for Python)

## Core Code Highlight
Below is the core Python logic utilizing the `boto3` library to isolate an instance by revoking its network access permissions upon alert triggering:

```python
import boto3

def isolate_compromised_host(instance_id, security_group_id):
    """
    Revokes all ingress traffic for a specific Security Group to isolate the host.
    """
    ec2 = boto3.client('ec2', region_name='eu-west-3') # Madrid Region
    try:
        response = ec2.revoke_security_group_ingress(
            GroupId=security_group_id,
            IpPermissions=[
                {
                    'IpProtocol': '-1', # All protocols
                    'FromPort': -1,
                    'ToPort': -1,
                    'IpRanges': [{'CidrIp': '0.0.0.0/0'}]
                }
            ]
        )
        print(print(f"[!] SUCCESS: Instance {instance_id} has been isolated."))
        return response
    except Exception as e:
        print(f"[-] ERROR isolating instance: {e}")
