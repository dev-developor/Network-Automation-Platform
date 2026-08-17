------------------------------
## Network Automation & AIOps Platform
An automated network engineering platform and diagnostic toolkit for orchestrating isolated routing environments, stateful traffic filtering, dynamic BGP/OSPF meshes, and LLM-powered packet telemetry analysis.
[](https://github.com/your-username/network-automation-and-aiops/actions)

------------------------------

### 📐 Platform Architecture & Data Flow
This platform serves as an Infrastructure-as-Code (IaC) and observability framework for modern network engineering. It provides fully orchestrated, repeatable network topologies alongside a custom AI telemetry agent to accelerate root-cause analysis (RCA).

| Component Layer | Responsibility & Action | Outputs / Downstream |
| :--- | :--- | :--- |
| **1. Orchestrated Environments**<br>`/environments` | Orchestrates network namespaces, stateful firewalls, and dynamic BGP/OSPF meshes. | Generates direct raw network traffic and telemetry logs. |
| **2. Platform Engine**<br>`/tools/ai-packet-analyzer` | Ingests bulk `.pcap` feeds, parses protocol layers, and isolates anomalous packet drops. | Compiles tokenized telemetry prompts. |
| **3. External Intelligence**<br>`LLM Inference API` | Receives contextual diagnostics from the local analyzer tool. | Returns production-ready incident triage reports. |

#### 🔄 Pipeline Operations
1. **Infrastructure Execution:** Traffic flows from **`01-namespace-routing`** through the **`02-nftables`** stateful firewall.
2. **Telemetry Capture:** Network topologies running in **`03-dynamic-routing-mesh`** generate diagnostic packet captures (`.pcap`).
3. **AI Diagnostic Loop:** The **`ai-packet-analyzer`** processes the captures and queries the **`LLM Inference API`** to isolate structural faults.


------------------------------
## 🚀 Key Modules & Capabilities## 1. Automated Infrastructure & Environments (/environments)

* Isolated Namespace Routing (/01-isolated-namespace-routing): Implements lightweight network virtualization using Linux network namespaces (netns), virtual ethernet pairs (veth), and IP forwarding engines to simulate multi-tenant VRF isolation and NAT gateways.
* Stateful Network Firewalls (/02-nftables-stateful-firewall): Utilizes nftables syntax for stateful packet inspection, defining explicit input/output hooks, dynamic egress filtering, and rate-limiting connection track thresholds.
* Dynamic Routing Meshes (/03-dynamic-routing-mesh): Leverages Containerlab to spin up multi-node virtual topologies executing FRRouting (FRR) instances. Fully configures autonomous systems running concurrent BGP peerings and multi-area OSPF control planes.

## 2. Intelligent Observability Engine (/tools)

* AI-Assisted Packet Telemetry CLI (/ai-packet-analyzer): A sophisticated diagnostic agent built with Python, pyshark (TShark engine), and LiteLLM/OpenAI.
* Streams local interfaces or ingests bulk raw .pcap files, identifies anomalous packet behavior (e.g., TCP out-of-order, retransmissions, DNS failures), and crafts contextual tokenized prompts for LLMs to generate instant incident triage reports.

------------------------------
## ⚡ Quick Start & Orchestration
The platform features an idempotent automated orchestration layer powered by a root Makefile. This ensures single-command environment deployment, structural testing, and teardown.
## Prerequisites

* Linux Kernel 5.4+ (with nftables and netns capabilities enabled)
* Docker & Containerlab CLI
* Python 3.10+ & Wireshark/TShark dependencies

## Deployment Matrix

| Action | Target Command | Infrastructure State |
|---|---|---|
| Bootstrap Platform | make install-deps | Installs local python and linting dependencies. |
| Orchestrate Mesh | make deploy-all | Provisions all namespaces, rules, and container topologies. |
| Execute Diagnostics | make run-analyzer | Ingests sample PCAPs into the AI troubleshooting engine. |
| Platform Wipe | make clean | Executes teardown scripts, flushes tables, and destroys meshes. |

------------------------------
## 🔬 In-Depth Engineering Deep Dives
For comprehensive data-plane analysis, traffic matrix definitions, and granular control plane validations, reference the engineering documentation inside the repository:

* Network Architecture & Packet Flow Deep Dive — Step-by-step trace of how encapsulation and firewall states transition across nodes.
* AI Diagnostic Methodology — Explains the heuristics-to-LLM pipeline for packet payload context extraction.

------------------------------
## 🛡️ CI/CD & Code Quality
This repository enforces production-ready standards via automated GitHub Actions pipelines (.github/workflows/lint-and-test.yml):

* Shell Quality: Validated using ShellCheck for runtime configuration scripts.
* Python Engine Quality: Validated via Flake8 linting and standard functional unit testing.

------------------------------
