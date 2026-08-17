# Network-Automation-Platform
An automated network engineering platform and diagnostic toolkit for orchestrating multi-tenant Linux routing topologies, stateful firewalls, dynamic BGP/OSPF meshes, and LLM-assisted packet telemetry analysis.

------------------------------
## 🛠️ Platform Architecture Overview
This platform serves as an Infrastructure-as-Code (IaC) and observability framework for modern network engineering. It provides fully orchestrated, repeatable network topologies alongside a custom AI telemetry agent to accelerate root-cause analysis (RCA).

                      [ LLM Inference API ]
                                ▲
                                │ Telemetry Prompt
┌───────────────────────────────┼───────────────────────────────┐
│ PLATFORM ENGINE               │                               │
│                               ▼                               │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ tools/ai-packet-analyzer/                               │  │
│  │ ──► Ingests PCAPs ──► Parses Layers ──► Triage Report  │  │
│  └────────────────────────────────▲────────────────────────┘  │
│                                   │ Direct PCAP Feed          │
├───────────────────────────────────┼───────────────────────────┤
│ ORCHESTRATED ENVIRONMENTS         │                           │
│                                   │                           │
│  ┌─────────────────────────────┐  │  ┌─────────────────────┐  │
│  │ env/01-namespace-routing/   │  │  │ env/02-nftables/    │  │
│  │ • Multi-tenant VRF Isolation│──┼─►│ • State Inspection  │  │
│  │ • Kernel-level NAT / Routing│  │  │ • Egress Filtering  │  │
│  └─────────────────────────────┘  │  └─────────────────────┘  │
│                                   │                           │
│  ┌────────────────────────────────┴────────────────────────┐  │
│  │ env/03-dynamic-routing-mesh/                            │  │
│  │ • Containerlab Engine + FRRouting (FRR) vNiCs           │  │
│  │ • Dynamic BGP / OSPF Control Planes                     │  │
│  └─────────────────────────────────────────────────────────┘  │
└───────────────────────────────────────────────────────────────┘

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
## 💡 Why this README works for recruiters:

   1. Industry Verbiage: Replaces "lab deployment" with "environment orchestration", and "AI tool" with "Intelligent Observability Engine".
   2. Visual Anchor: The ASCII map directly shows that you understand complex systemic designs, not just isolated coding tasks.
   3. The Matrix Table: Instantly signals that you value usability, DevOps workflows, and infrastructure clean-up routines.
