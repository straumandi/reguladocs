The Technical Blueprint (Architecture & Stack)
Goal: Build a scalable scanning pipeline that converts raw code into a legal document without crashing on large repositories.

1. The Tech Stack
   Core Engine: Syft (SBOM generation) + Grype (Vulnerability matching). Both are open-source (Apache 2.0) and CLI-based.

**MVP (CLI-First): Go**

Why: Single binary distribution, native concurrency for parsing large JSON files, easy Docker packaging, and potential to use Syft/Grype as Go libraries instead of shelling out.

**Future SaaS Backend: Go + PostgreSQL**

Database: PostgreSQL on Railway/Render (not Supabase - avoid vendor lock-in).

Why: Relational data for Projects -> Scans -> Components -> Vulnerabilities -> VEX Decisions.

Infrastructure: Railway (MVP hosting, $5/month) -> AWS (scale phase).

Critical: For SaaS version, scans run as background jobs. For CLI MVP, they run synchronously - simpler but works perfectly for initial users.

**Future Frontend: Next.js (React) + Tailwind CSS** (only if SaaS demand exists).

2. Architecture Diagram (Conceptual)
   Trigger: User connects GitHub App -> Webhook sends payload to API.

Queue: API pushes job scan_repo_123 to AWS SQS.

Worker:

Pull Docker container (ECS/Fargate).

git clone the target repo (using a temporary token).

Run syft . -o json (Generate SBOM).

Run grype sbom.json -o json (Find CVEs).

Normalization: Parse JSONs and insert/upsert into Supabase (this is the hardest part—mapping raw data to your DB schema).

Notify: Update status in DB, send email/Slack notification to user.

3. The "Tech-Moat" Features
   Smart Diffing: Don't just overwrite data. Compare the new scan with the old one. Did a new vulnerability appear? Did an old one disappear? This history tracking is required for audits.

The "VEX" Implementation: VEX (Vulnerability Exploitability eXchange) is the standard for saying "We are not affected by this."

Build a feature where users click "Ignore" on a CVE, select a reason (e.g., "Function not used"), and this decision is saved and re-applied on the next scan. This prevents "alert fatigue."
