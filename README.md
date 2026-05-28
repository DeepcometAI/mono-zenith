# Mono‑Zenith Kernel

**Mono‑Zenith** is a research and engineering fork of the Linux kernel, preserving the full upstream history while pursuing experimental kernel features for AI‑native workloads, heterogeneous compute (NPU/TPU), and ARM64‑first platforms. This repository is a complete mirror of the official Linux history and is intended as a development baseline and research platform under the DeepcometAI organization.

---

## Overview
**Purpose:** Provide a stable, auditable baseline that mirrors upstream Linux while enabling rapid experimentation with kernel primitives, scheduler extensions, and hardware acceleration for AI workloads.

**Scope:**  
- Preserve upstream commits, tags, and history for provenance.  
- Maintain a lightweight working branch for active development (`mono-zenith-1.0`) pinned to a chosen kernel snapshot.  
- Offer reproducible builds and CI for kernel snapshots and experimental subsystems.

**Audience:** Kernel developers, systems researchers, platform engineers, and contributors interested in AI‑native OS innovations.

---

## Project Structure
- **`master` / upstream branches** — full upstream history and tags (archived for provenance).  
- **`mono-zenith-1.0`** — primary development branch pinned to a chosen release snapshot.  
- **`docs/`** — design docs, architecture notes, and contributor guides.  
- **`patches/`** — curated experimental patches and RFCs.  
- **`.github/workflows/`** — CI workflows for builds, tests, and security scans.

---

## Versioning and Pinning
**Pinning strategy:** Work on a branch created from a specific upstream tag (example: `v7.1-rc5` or `v7.0`). This gives a reproducible baseline while keeping the full history available for merges.

Example commands to pin to a tag and push a working branch:
```bash
git fetch --all --tags
git checkout -b mono-zenith-1.0 v7.1-rc5
git push -u origin mono-zenith-1.0
```

**Shallow clones for day‑to‑day work:**
```bash
git clone --depth=1 https://github.com/DeepcometAI/mono-zenith.git
cd mono-zenith
git checkout mono-zenith-1.0
```

---

## Security and Vulnerability Handling
**Baseline security posture:** Mono‑Zenith preserves upstream fixes and expects maintainers to track upstream stable branches for security patches.

**Known issues & mitigation:** If you base work on older tags, verify whether critical fixes (for example, privilege escalation or crypto interface bugs) are present. When a security fix is required:
- Identify the upstream commit that introduced the patch.
- Cherry‑pick the commit into `mono-zenith-1.0` and run CI/security tests.
- Document the backport in `docs/security.md` with CVE references and test vectors.

**Responsible disclosure:** Report security issues to the maintainers via the repository’s security policy. Do not publish exploit details before a fix is available.

---

## Development Workflow (Codespaces recommended)
**Why Codespaces:** Linux case‑sensitive filesystem, no NTFS collisions, no need to download the full 6.3 GB history locally.

**Recommended flow:**
1. Open a Codespace on `mono-zenith-1.0`.  
2. Create a feature branch:
   ```bash
   git checkout -b feat/<short-descriptor>
   ```
3. Implement changes; keep commits small and focused.  
4. Run local build/test in Codespaces container.  
5. Push branch and open a Pull Request against `mono-zenith-1.0`.

**Local Windows notes:** If you must work locally on Windows and need full history, enable case sensitivity on the target folder before cloning:
```powershell
fsutil file setCaseSensitiveInfo D:\mono-zenith enable
git clone https://github.com/DeepcometAI/mono-zenith.git
```
Prefer shallow clones for routine tasks.

---

## Build and Test
**Basic build (x86_64 example):**
```bash
make defconfig
make -j$(nproc)
```

**ARM64 cross‑build example:**
```bash
export ARCH=arm64
export CROSS_COMPILE=aarch64-linux-gnu-
make defconfig
make -j$(nproc)
```

**Automated CI:** The repository includes GitHub Actions workflows to:
- Build configured snapshots.
- Run static analysis (sparse, clang‑tidy where applicable).
- Run unit tests and integration smoke tests in containerized runners.

---

## CI/CD and Release Process
- **Continuous Integration:** Every PR triggers build + static checks. Experimental patches must pass CI before review.  
- **Nightly snapshots:** Optional nightly builds of `mono-zenith-1.0` for integration testing.  
- **Releases:** Tag release candidates from `mono-zenith-1.0` with semantic tags like `mono-zenith-1.0-rc1`. Include a release note summarizing changes and security backports.

---

## Contributing
**How to contribute:**
1. Fork the repo and branch from `mono-zenith-1.0`.  
2. Follow the commit message format: `<subsystem>: short description` and include a longer body explaining rationale and tests.  
3. Add tests or reproduce steps for regressions.  
4. Open a PR with a clear description, test results, and any compatibility notes.

**Code style:** Follow upstream Linux kernel coding style. See `Documentation/process/coding-style.rst` for details.

**Review process:**  
- Small, focused patches get faster review.  
- Major design changes require an RFC in `docs/rfcs/` and a design review meeting.

---

## Governance and Licensing
**Governance:** DeepcometAI maintains Mono‑Zenith. Major design decisions are documented in `docs/governance.md`. Community contributors are welcome; maintainers reserve merge rights for core branches.

**License:** Mono‑Zenith inherits the **GPLv2** license from the Linux kernel. All contributions must be compatible with GPLv2.

---

## Troubleshooting and FAQ
**Q: Repo shows “no branches” after import.**  
A: Create and push a working branch (example `mono-zenith-1.0`) from a tag or `master` and set it as default in Settings.

**Q: Files collide on Windows.**  
A: Use Codespaces or enable case sensitivity on the local folder before cloning.

**Q: How to apply an upstream security patch?**  
A: Find the upstream commit, cherry‑pick into your branch, run CI, and document the backport in `docs/security.md`.

**Q: I need a specific kernel tag (e.g., v7.1-rc5).**  
A: Fetch tags and checkout the tag into a branch:
```bash
git fetch --all --tags
git checkout -b mono-zenith-1.0 v7.1-rc5
git push -u origin mono-zenith-1.0
```

---

## Appendices
### A. Useful Commands
```bash
# Fetch everything
git fetch --all --tags

# Create working branch from tag
git checkout -b mono-zenith-1.0 v7.1-rc5

# Push branch
git push -u origin mono-zenith-1.0

# Shallow clone
git clone --depth=1 https://github.com/DeepcometAI/mono-zenith.git
```

### B. Contact & Support
- **Maintainers:** See `MAINTAINERS` file for subsystem owners.  
- **Security contact:** Use the repository Security policy (link in GitHub UI).  
- **Community:** Open issues for non‑security discussions; use PRs for code.

---

## Changelog
Keep a `CHANGELOG.md` for notable changes, security backports, and release notes. Tag entries with dates and links to PRs/commits.

---
