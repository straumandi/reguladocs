# 🛡️ RegulaDocs (MVP)

> **Automated Cyber Resilience Act (CRA) Compliance & SBOM Generator.**
> Turn your code dependencies into audit-ready technical documentation in seconds.

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)]()
[![Compliance](https://img.shields.io/badge/CRA-Ready-orange)]()

---

## 🧐 What is this?

**RegulaDocs** is a compliance engine designed for EU software agencies and SMEs.

With the upcoming **EU Cyber Resilience Act (CRA)**, software vendors must provide up-to-date SBOMs (Software Bill of Materials) and vulnerability assessments. Doing this manually in Excel is impossible.

RegulaDocs wraps industry-standard open-source tools (`Syft` & `Grype`) into a unified pipeline that outputs **human-readable, auditor-friendly PDF reports** compliant with CRA documentation requirements.

---

## 🚀 Key Features

- **📦 Auto-Discovery:** Detects packages in Node.js, Go, Python, Rust, and Java automatically.
- **🔍 Deep Scanning:** Generates a full SBOM (SPDX/CycloneDX format) using `Syft`.
- **🛡️ Vulnerability Matching:** Cross-references dependencies against the NVD (National Vulnerability Database) using `Grype`.
- **🚦 VEX-Light Logic:** Mechanism to mark vulnerabilities as "Ignored/False Positive" with justification (audit trail).
- **📄 PDF Generation:** Outputs a branded, professional PDF report suitable for clients and legal audits.

---

## 🏗️ Architecture

RegulaDocs operates as a pipeline to ensure data integrity between the code and the final report.

[Image of software supply chain security diagram]

1.  **Ingestion:** Scans the local directory or CI/CD environment.
2.  **Normalization:** Converts raw JSON output from scanners into a relational structure.
3.  **Assessment:** Applies VEX (Vulnerability Exploitability eXchange) rules to filter noise.
4.  **Reporting:** Renders the final artifact.

---

## ⚡ Quick Start (Local Mode)

### Prerequisites

- Node.js v18+
- Docker (optional, for containerized scanning)
- `syft` and `grype` installed (or use our bundled Docker image)

### Installation

```bash
# Clone the repo
git clone [https://github.com/straumandi/reguladocs.git](https://github.com/straumandi/reguladocs.git)
cd reguladocs

# Install dependencies
npm install

# Link command globally (optional)
npm link
```

### Usage

Run a scan on your current project and generate a report:

```bash
# Basic Scan
reguladocs scan . --output ./report.pdf

# Scan with "Ignore File" (VEX config)
reguladocs scan . --config .vex-config.json --output ./compliance-report.pdf
```

---

## ⚙️ Configuration (.vex-config.json)

To prevent "alert fatigue," you can define false positives or accepted risks. This file serves as your **audit trail**.

```json
{
  "project_name": "My Client App",
  "agency_name": "CodeWorks GmbH",
  "ignore_rules": [
    {
      "cve": "CVE-2023-1234",
      "package": "lodash",
      "status": "not_affected",
      "justification": "Vulnerable function is not used in production code path.",
      "approved_by": "Andreas A.",
      "date": "2025-05-20"
    }
  ]
}
```

---

## 🛠️ Tech Stack

- **Core:** Node.js (TypeScript)
- **Scanning Engine:** [Syft](https://github.com/anchore/syft) & [Grype](https://github.com/anchore/grype)
- **Database (SaaS ver.):** PostgreSQL (Supabase)
- **PDF Engine:** `pdfmake` / `puppeteer`

---

## 🗺️ Roadmap

- [ ] **v0.1:** Local CLI scanner & JSON parsing.
- [ ] **v0.2:** PDF Report generation with "Traffic Light" summary.
- [ ] **v0.3:** GitHub Action integration (CI/CD).
- [ ] **v1.0:** SaaS Web Dashboard with historical tracking & Agency White-labeling.

---

## ⚖️ Legal Disclaimer

**RegulaDocs provides technical documentation tools.**
We are not a law firm. While this tool assists in generating documentation required by the EU Cyber Resilience Act, the final responsibility for compliance and legal conformity rests solely with the operator/manufacturer of the software.

---

## 🤝 Contributing

This is currently a Proof of Concept (PoC). Pull requests are welcome, especially regarding improved PDF templates or cleaner VEX logic implementation.

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

---

**Built with ☕ and code in the EU.**
