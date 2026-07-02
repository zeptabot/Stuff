# The PQC Implementer's Playbook: A 4-Year Roadmap from CSC148 to Financial Independence

## TL;DR
- **The path is real and well-paved.** A Toronto stats student with proof-based math can become a hireable PQC implementation engineer in ~24–30 months by mastering **C → Rust**, cryptographic-engineering fundamentals, and the lattice/hash/code-based math behind the FIPS 203/204/205 + HQC standards — then proving it through **liboqs (Open Quantum Safe)** contributions and three portfolio projects. The single highest-leverage move is contributing real code to liboqs, the C library led out of the University of Waterloo by Douglas Stebila and Michele Mosca.
- **Compensation trajectory:** Entry-level Toronto crypto/security SWE ~CA$80–110k total comp; remote-US PQC roles pay roughly double (per Levels.fyi, the median Software Engineer total compensation at **SandboxAQ in the US is $180,000**, with the headline package totaling ~$207K and a top reported ~$492K). A realistic arc: ~CA$85–110k gross at age 21 → US$200k+ remote by late 20s if you take the US-remote route.
- **Financial independence by ~33–40 is realistic** at a 50–60% savings rate using a maxed TFSA (XEQT/VEQT), then FHSA and RRSP, targeting a ~CA$1.0–1.5M portfolio (4% rule on $40–60k/yr expenses). Open and start funding the TFSA now; it is the highest-priority account.

---

## Key Findings

1. **The standards are locked in, which makes this a hiring boom, not a research gamble.** NIST published **FIPS 203 (ML-KEM), FIPS 204 (ML-DSA) and FIPS 205 (SLH-DSA) on August 13, 2024** (effective August 14, 2024), and **on March 11, 2025 announced the selection of HQC** as a fifth, backup KEM algorithm — project lead Dustin Moody: *"We are announcing the selection of HQC because we want to have a backup standard that is based on a different math approach than ML-KEM."* FALCON (FIPS 206) is in draft. Per **NIST IR 8547, RSA/ECDSA/ECDH/DH are deprecated after 2030 and disallowed after 2035 for federal use**, following NSM-10's 2035 federal migration target — a decade-long, legally-mandated migration wave that creates sustained demand for *implementers*, not just theorists.
2. **Implementation skill > credentials.** PQShield explicitly states it assesses *"seniority primarily based on your depth of knowledge, hands-on experience, and problem-solving skills rather than just years worked,"* and asks for the ability to *"read and implement specifications from cryptographic research papers"* plus side-channel/constant-time hardening. This is ideal for a non-traditional candidate with a strong GitHub.
3. **Canada has a genuine PQC cluster** (Waterloo/Ottawa): evolutionQ, Crypto4A, Quantropi, plus the OQS project itself at Waterloo, plus the big banks (RBC opened a cyber lab and funded post-quantum research at the University of Waterloo). The highest pay, however, is remote-US (SandboxAQ, AWS Cryptography, IBM Research).
4. **Georgia Tech OMSCS is the flexibility/credential sweet spot.** Per the GA Tech Bursar OMSCS schedule, tuition is **$195 per semester credit hour**, which with the technology fee brings the 30-credit-hour degree to **under $7,000** total — fully online, with CS 6260 Applied Cryptography. It is far cheaper than Waterloo's research MMath, but it requires a bachelor's first, so it's a post-graduation move.
5. **Tax-sheltered compounding is the engine.** The **TFSA annual room is $7,000 for 2026** (cumulative $109,000 for anyone 18+ since 2009 — the student, being ~20, has less; verify in CRA My Account); the FHSA is $8,000/yr ($40k lifetime). Both should be prioritized over RRSP while income is modest.

---

## Phase Overview (the timeline at a glance)
- **Months 1–3:** C fundamentals + crypto-math foundations + Boneh "Cryptography I"
- **Months 4–6:** Finish C, start Rust, master symmetric crypto & RSA/ECC; first portfolio project
- **Months 7–12:** Rust proficiency, PQC algorithms deep-dive, first liboqs contribution; second project
- **Months 13–24:** Constant-time/side-channel skills, third project, internship/co-op hunt, networking
- **Months 25–36:** Graduate; full-time PQC/crypto role or OMSCS + work; ramp savings
- **Year 4+:** Mid-level pivot to remote-US comp; aggressive FI investing

---

## TRACK A: TECHNICAL EDUCATION & PORTFOLIO BLUEPRINT

### A1. The self-directed curriculum (CSC148 → hireable)

#### Languages: C → C++ → Rust
PQC implementation work is overwhelmingly **C and Rust** (liboqs is C; RustCrypto and much new work is Rust; embedded/HSM work touches C++ and assembly). Progression for someone with strong math but only CSC148-level CS:

- **C (Months 1–4, ~3–4 months):** The priority language — liboqs, PQClean, and NIST reference implementations are C. Resources: *The C Programming Language* (Kernighan & Ritchie) for the canon; *Effective C* (Robert Seacord, No Starch) for modern, security-aware C; Beej's Guide to C Programming (free). Build: reimplement SHA-256 and AES from the spec. Memory management and pointers are the main hurdle coming from Python-like CSC148.
- **C++ (Months 5–6, optional/light, ~1–2 months):** Only enough to *read* it (liboqs-cpp bindings, some HSM codebases). Use *A Tour of C++* (Stroustrup). Don't over-invest; Rust is the better use of time.
- **Rust (Months 5–12, ~4–6 months overlapping):** The strategic bet for new PQC code. Resources: *The Rust Programming Language* ("the book," free online); *Programming Rust* (Blandy/Orendorff) for depth; then the RustCrypto org with the `subtle` crate (constant-time traits from dalek-cryptography) and `zeroize`. A math background accelerates Rust's type-system curve.

Realistic total: a focused student doing ~10–15 hrs/week reaches "can contribute to a C crypto library" in ~6 months and "can write idiomatic Rust crypto code" in ~12 months.

#### Discrete math / number theory / abstract algebra
The student has linear algebra (100) and calculus (92) but needs **number theory, abstract algebra (groups/rings/fields), and discrete probability**. Fastest path:
- **Victor Shoup, *A Computational Introduction to Number Theory and Algebra*** (free legal PDF at shoup.net/ntb) — purpose-built for crypto/CS readers; covers number theory, groups, rings, fields, and discrete probability. The single best free option for this exact background.
- For lattice math specifically: **Chris Peikert, *A Decade of Lattice Cryptography*** (free, eprint.iacr.org/2015/939) — the definitive survey of the SIS and LWE problems and their ring/module variants underpinning ML-KEM and ML-DSA. Supplement with Peikert's "Lattices in Cryptography" course notes (GitHub: cpeikert/LatticesInCryptography).
- Leverage the CS/Math minors: take the university's number-theory and abstract-algebra courses for credit — they double as résumé anchors and degree progress.

#### Cryptography fundamentals (master before PQC)
- **Best starting course:** **Dan Boneh's "Cryptography I" (Stanford, Coursera)** — free to audit, 4.8 rating; covers symmetric encryption, integrity, public-key, key exchange.
- **Best practitioner book:** **Jean-Philippe Aumasson, *Serious Cryptography* (2nd ed., No Starch)** — *"a must read for anyone wanting to enter cryptographic engineering"*; covers AES, hash functions, RSA, ECC, TLS, and a post-quantum chapter. (Aumasson co-designed the NIST-standardized SLH-DSA/SPHINCS+ family and BLAKE2/3.)
- **Best rigorous textbook:** **Katz & Lindell, *Introduction to Modern Cryptography*** (free author PDF) — for provable-security foundations that suit the student's proof skills.
- **Companion free text:** **Boneh & Shoup, *A Graduate Course in Applied Cryptography*** (cryptobook.us).
- Per-primitive: AES & block-cipher modes, SHA-2/SHA-3 & Keccak, RSA, Diffie-Hellman, ECC (Curve25519) — all covered across Boneh + Aumasson.

#### PQC-specific knowledge
Master the math basis and implementation properties of each standardized algorithm:
- **ML-KEM (FIPS 203, ex-CRYSTALS-Kyber):** Module-Learning-With-Errors (lattice). Primary KEM replacing RSA/ECDH key exchange. Public keys 800–1,568 bytes. Parameter sets ML-KEM-512/768/1024.
- **ML-DSA (FIPS 204, ex-CRYSTALS-Dilithium):** Module-LWE + Module-SIS (lattice). General-purpose signature. **ML-DSA-65** (security category 3) is the enterprise default and produces a **~3,300-byte signature** (NIST FIPS 204 lists 3,309 bytes; 1,952-byte public key, 4,032-byte private key); **ML-DSA-87** (~4,627-byte signature) is required under CNSA 2.0. These are roughly 50× the size of an ECDSA P-256 signature — a real bandwidth/storage consideration in TLS certificate chains.
- **SLH-DSA (FIPS 205, ex-SPHINCS+):** Stateless hash-based; security rests only on hash-function collision resistance — a conservative, lattice-independent backup. Larger signatures (7,856–49,856 bytes).
- **HQC (selected March 11, 2025; draft ~2026, final ~2027):** Code-based KEM (Hamming Quasi-Cyclic), a mathematically different backup to ML-KEM in case lattices fall.
- **Implementation-specific skills (the differentiator):** constant-time programming (no secret-dependent branches or memory access), side-channel resistance (timing, power, fault injection), and assembly optimization (ARM/RISC-V, AVX2). Use the `subtle` and `zeroize` Rust crates; study cryptographic-constant-time (CCT) discipline. PQShield postings explicitly require hardening *"against side-channel analysis (SCA), fault injection attacks (FIA)… through constant-time design."*

### A2. Three concrete portfolio projects

**Project 1 — "From-Scratch ML-KEM in C with Known-Answer Tests" (Months 4–9)**
- *Description:* Implement ML-KEM-768 from the FIPS 203 spec in C, validating against NIST KAT vectors.
- *Stack:* C, NIST KATs, GitHub Actions CI, valgrind.
- *Demonstrates:* Reading a cryptographic spec and producing correct, testable C — exactly what PQShield asks for (*"read and implement specifications from cryptographic research papers"*).
- *Time:* ~10–12 weeks part-time. *Presentation:* Detailed README with the math, a correctness section showing KAT pass, and an honest "not constant-time yet" note.

**Project 2 — "Constant-Time Hardening & Benchmark Suite" (Months 10–16)**
- *Description:* Take Project 1 (or a RustCrypto primitive), make it constant-time, and build a timing-leakage test harness; benchmark against liboqs.
- *Stack:* Rust + `subtle` + `zeroize`, or C with `dudect`/`valgrind ctgrind`; criterion for benchmarks.
- *Demonstrates:* The single most valued implementer skill — side-channel awareness and constant-time discipline.
- *Time:* ~6–8 weeks. *Presentation:* Blog post + graphs of timing distributions before/after.

**Project 3 — "Hybrid PQC TLS Demo / liboqs Integration" (Months 16–24)**
- *Description:* Integrate ML-KEM into a TLS 1.3 handshake via the OQS provider for OpenSSL 3, or build a hybrid (X25519 + ML-KEM) key-exchange demo; ideally land a merged PR to liboqs or oqs-provider.
- *Stack:* C/Rust, liboqs, oqs-provider, OpenSSL 3.
- *Demonstrates:* Real-world protocol integration + open-source collaboration on the canonical PQC codebase.
- *Time:* ~8–10 weeks. *Presentation:* Link the merged PR prominently on GitHub and LinkedIn.

**GitHub/LinkedIn presentation:** Pin all three repos; write a short technical blog (the same content doubles as interview talking points). On LinkedIn, frame each as *"Implemented FIPS 203 ML-KEM in C with full KAT validation"* with the repo link. A merged liboqs PR in your "Featured" section is worth more than any certificate.

### A3. Flexible degree/credential paths

**Georgia Tech OMSCS** — Strongly recommended *after* the bachelor's.
- Suitability: Good. **Under $7,000 total** for 30 credit hours ($195/credit-hour + tech fee), fully online, unmatched flexibility (1–3 courses/term, up to 9 years).
- Specialization: **Computing Systems** (it houses the cybersecurity-administered crypto courses). Key courses: **CS 6260 Applied Cryptography** (Katz & Lindell text, all number theory taught in-course, proof-heavy — ideal for this student; reviewers call it among the most rewarding and most math-heavy in the program), CS 6238 Secure Computer Systems, CS 6262 Network Security, and CS 7560 Theory of Cryptography.
- Admission: bachelor's in CS or related (math counts), GPA ≥3.0 preferred; résumé + recommendations. Deadlines: March 1 (Fall), August 15 (Spring). **Work experience cannot substitute for the degree** — so this is a post-undergrad move.
- Caveat: the program is rigorous and self-paced; a widely-cited student review pegs the overall graduation rate below 50%.

**Waterloo cryptography/quantum programs** — Prestigious but less flexible.
- The **MMath in Computer Science (Quantum Information)** and the Cheriton **MMath in CS** (with a "Cryptography, Security and Privacy" field) require an **honours bachelor's with ≥78% standing**, are **on-campus**, thesis-based, 24 months, with admission to the thesis option only. As a stats major, the student would need strong CS grades and ideally a faculty supervisor (the program advises contacting supervisors before applying).
- Better if the student wants research/PhD optionality or to work directly inside the OQS/IQC ecosystem; less suited to "maximum scheduling flexibility + aggressive FI."

**Other online/part-time options.** OMSCS and University of London lead on flexible/online value. UT Austin MSCS Online (>US$10k) and UIUC MCS (>US$20k) are pricier alternatives; Stanford's online offerings are expensive (>US$40k for a full program). For *flexibility + cost + brand*, OMSCS wins.

**Certifications — mostly not worth it for an implementer.** CISSP, CompTIA Security+, and ISC2 credentials target security *operations/governance/compliance* roles, not cryptographic engineering. They will not substitute for the math + code portfolio and are a low priority. (Exception: if a specific employer, e.g., a bank's governance team, requires one, get it then.) Worth *knowing about* (not certifying in) for embedded PQC: **FIPS 140-3, Common Criteria, SESIP, PSA Certified** — these appear in PQShield/Crypto4A postings.

---

## TRACK B: CYBERSECURITY INDUSTRY CRACKING STRATEGY

### B1. Corporate vs. startup comparison for PQC roles

| Pillar | Corporate/Enterprise (banks, IBM, Cisco, Entrust, Thales, AWS) | Funded PQC startup (SandboxAQ, PQShield, evolutionQ, Crypto4A) |
|---|---|---|
| **Total comp** | Higher floor, cash-weighted. Per Levels.fyi, **IBM SWE US median $180K** (Associate/Band 6 median $133,657; Senior Engineer median $224,312). AWS/Amazon Security Engineer median ~US$340k. Canadian bank new-grad SWE ~CA$80–85k base. | Wide variance, equity-heavy. SandboxAQ SWE median **$180K** (package ~$207K; top ~$492K). QuSecure ~US$93–128k (thin/low data). Canadian startups (Entrust Ottawa proxy) entry ~CA$76–92k. |
| **Flexibility/scheduling** | Banks more rigid/hybrid-in-office; AWS/IBM variable. | Generally more remote & flexible; PQShield offers "flexible and hybrid working"; SandboxAQ is largely remote. |
| **Résumé/visibility discretion** | Lower public visibility; you're one of thousands — good for staying low-profile. | Higher visibility (small teams, you're identifiable). |
| **Exit velocity** | Strong résumé brand (IBM/AWS); slower specialization. | Faster, deeper PQC specialization; great for becoming a recognized implementer quickly. |

**Verdict for a non-traditional background:** Start where the *bar is skills, not pedigree* — a funded PQC startup (PQShield-style, which explicitly weighs demonstrated skill over years) or an open-source-visible team. Startups reward the GitHub portfolio most directly and specialize you fastest. Then, with 2–4 years and a track record, pivot to **remote-US big-tech crypto (AWS/IBM/SandboxAQ)** for the compensation jump. This sequencing maximizes both entry probability and lifetime earnings.

### B2. Top 10 employers hiring PQC talent (2025–2026)

**Corporate / enterprise (5):**
1. **IBM (incl. IBM Research)** — Remote/hybrid US & global. Roles: cryptography research, PQC migration engineering. Comp (Levels.fyi): SWE US median **$180K**; Senior Engineer median **$224,312**; Research Scientist senior bands up to ~US$360–368k. Looks for: research depth and/or strong implementation. Door-in: IBM contributes to liboqs and helped author the NIST standards.
2. **AWS Cryptography (Amazon)** — Remote-eligible US. Roles: cryptography/security software engineer. Benchmark: Amazon Security Engineer median ~US$340k; SWE L4 entry ~US$186k. Looks for: strong C/Rust + systems. Door-in: AWS funds and contributes to OQS/liboqs.
3. **Cisco** — Integrating quantum-safe networking; hybrid/remote US. US security-SWE benchmark ~US$198k avg. Looks for: protocol + crypto integration.
4. **Entrust** — Ottawa/Kanata ON + Minnesota; PQC-ready certificate authority/PKI. Canada SWE median ~CA$94k (Levels.fyi); Ottawa SWE intern ~CA$26/hr. Looks for: PKI, crypto engineering. Door-in: Ottawa co-op/intern pipeline.
5. **Canadian banks (RBC, TD)** — Toronto. RBC opened a cyber lab and funded post-quantum research at the University of Waterloo (CryptoWorks21). Roles: cryptography governance analyst, security SWE; new-grad ~CA$80–85k base. Looks for: governance OR engineering; **Tech@RBC** student/new-grad programs are the door-in.

**Funded startups (5):**
6. **SandboxAQ** (US, largely remote incl. Canada) — Roles: cryptography software engineer (user-space Linux, network/protocol). Comp (Levels.fyi): SWE median **$180K**, package ~$207K, top ~$492K, 4-yr equity. **SandboxAQ Residency Program** for STEM grad students + internships; contributes to liboqs. Confirmed fully remote in North America (US & Canada).
7. **PQShield** (Oxford UK HQ; US/Japan/EU teams; remote-heavy but UK-centric) — Roles: Cryptography Software Engineer (classical + PQC, embedded, ARM/RISC-V, side-channel hardening). Assesses skill over tenure; offers share options, flexible/hybrid work, a 6-month product internship. Was involved in *all* the NIST standards and claims the highest concentration of crypto PhDs in industry. (Note: openings are largely UK-based; not a confirmed North-America fully-remote employer.)
8. **evolutionQ** (Waterloo/Kitchener, Canada) — Co-founded by Michele Mosca; quantum-risk + PQC solutions; contributes programmer time to liboqs. Regional SWE TC proxy ~CA$126k avg (Waterloo region). Door-in: OQS contributions + Waterloo network.
9. **Crypto4A** (Ottawa) — Quantum-safe HSMs (first in the world to submit a PQC-capable HSM for FIPS 140-3 Level 3, March 2025). Roles: secure Linux/web in Java/Python/Rust, hardware crypto. **Strong co-op program** (100+ co-op students since 2019; recruits every term) — the single best Canadian door-in for this student.
10. **Quantropi** (Ottawa) — Quantum-secure comms (QiSpace: MASQ/QEEP/SEQUR); ~28 employees, all-Canadian. Roles: crypto/software engineering. Door-in: small team, direct outreach.
- *Also track: QuSecure (US, remote roles), Thales, Utimaco.*

### B3. Employment-history strategy (no formal work history)
- **Open-source as work experience:** A merged PR to **liboqs / oqs-provider** is the gold standard — it is the canonical PQC library, led from Waterloo, with contributors including AWS, Cisco, IBM, evolutionQ, SandboxAQ, and Microsoft. Frame it as: *"Open-source contributor, Open Quantum Safe (liboqs) — implemented/optimized [X]; PR merged [link]."* Legitimate, verifiable experience hiring managers at these very companies recognize.
- **Freelance tutoring → consulting framing:** *"Mathematics & Programming Tutor (self-employed), 2024–present — taught linear algebra, discrete math, and C/Python to 20+ university students."* Quantify (students, hours, outcomes). For any paid technical work, use "Independent Software Consultant."
- **University enrollment & academic projects as anchors:** List the degree (in progress), relevant coursework (number theory, abstract algebra, applied crypto), and course projects as portfolio items. "Expected graduation 20XX" signals trajectory.
- **The experience-gap question:** Pivot to demonstrated output: *"I don't have prior industry titles, but I've implemented FIPS 203 from spec, contributed to liboqs, and can walk you through constant-time hardening of ML-KEM right now."* Show, don't tell.
- **Résumé/LinkedIn phrasing:** Lead with "Projects" and "Open Source" sections above any thin "Experience" section. Use concrete verbs + spec names + links. Headline: *"Aspiring PQC Implementation Engineer | C/Rust | Contributor, Open Quantum Safe | FIPS 203/204 implementations."*

### B4. Networking strategy (PQC-specific, low in-person)
- **Communities/venues that matter:** **Real World Crypto (RWC)** (IACR; RWC 2026 in Taipei, March 9–11; **student presenters can have registration covered by the Cryptography Research Fund**), **PQCrypto** conference, the **NIST PQC Forum mailing list** (where the actual standardization discussion happens — read it, then contribute), **IACR ePrint Archive** (new lattice/implementation papers), CCS, the **OQS community/Discord**, and the **Open Source Cryptography Workshop (OSCW)**, an RWC affiliate.
- **Without in-person:** Be active on OQS GitHub (issues tagged "good first issue" / "help wanted"), the NIST PQC Forum, and Rust crypto channels. A thoughtful issue comment or PR beats a conference handshake.
- **Cold-email playbook:** Email researchers/engineers at target firms with (1) a specific, informed reference to their work or a liboqs issue, (2) one concrete thing you built (link), (3) a single specific question or ask. Keep it ~5 sentences. The Douglas Stebila / Michele Mosca OQS ecosystem (Waterloo) is the most natural geographic and topical target for a Toronto student.

---

## TRACK C: FINANCIAL FREEDOM ACCELERATOR MODEL

### C1. Salary projections (Toronto CAD / remote-US USD)

| Level (yrs exp) | Toronto CAD (total comp) | Remote-US USD (total comp) |
|---|---|---|
| Entry (0–2) | CA$80,000–110,000 | US$150,000–200,000 (SandboxAQ SWE median **$180,000**) |
| Mid (2–4) | CA$100,000–145,000 | US$200,000–280,000 |
| Senior (4–6) | CA$140,000–190,000 | US$300,000–490,000 (AWS/IBM senior bands) |

ERI pegs a Canadian "Cryptography Engineer" average at ~CA$140k (range ~CA$97k–171k); Glassdoor's US cryptography-engineer figure runs ~US$140–230k, and Levels.fyi's cross-company "Crypto Engineer" median is ~US$196k. **Trajectory entering at age 20–21:** ~CA$85–110k gross (with tutoring) in years 1–2 → CA$120–145k mid → either CA$140–190k senior in Toronto **or US$200k+ remote-US**, which is the single biggest lever on the FI timeline.

### C2. Canadian tax-shelter strategy

**TFSA (first priority).**
- **2026 annual room: $7,000.** Cumulative room since 2009 for a continuously-eligible adult is $109,000 — but at ~20, the student has accumulated room only since turning 18; confirm the exact figure in CRA My Account.
- Strategy: hold a low-fee all-equity ETF for decades of tax-free growth. **XEQT** (iShares all-equity, MER ~0.20%, ~9,000 stocks, globally diversified) is the strong default; **VEQT** (Vanguard; management fee cut to 0.17% in Nov 2025, matching XEQT) is equivalent; **VFV** (S&P 500, MER 0.09%) for concentrated US exposure. For a hands-off, long-horizon investor, XEQT/VEQT win on diversification.
- Automate: bi-weekly auto-buy on payday (dollar-cost averaging), DRIP on.
- Illustrative tax-free growth of maxing ~$7k/yr at ~7% real: ~$100k in 10 yrs, ~$300k in 20 yrs, ~$700k+ in 30 yrs.

**RRSP (later).**
- Room = 18% of prior-year earned income up to the annual max, plus carry-forward. Most valuable when income is *high* (the deduction is worth more at a higher marginal rate). For a 20-year-old earning CA$80–110k, **fill TFSA and FHSA first**; start RRSP seriously once income crosses ~CA$100k. The deduction can be *carried forward* — contribute now if cash allows but *claim* the deduction in a higher-income year.

**FHSA (highly relevant).**
- $8,000/yr, $40,000 lifetime; **tax-deductible like an RRSP AND tax-free on qualifying withdrawal like a TFSA** — the best of both. Carry-forward up to $8,000. Combines with the RRSP Home Buyers' Plan (up to $60,000). If homeownership never happens, the FHSA rolls to an RRSP tax-free (without using RRSP room) — so opening one early starts the 15-year clock and is nearly a free option.

**Optimal order of operations (young Canadian, tech + tutoring income):**
1. Emergency fund (3–6 months) in a HISA.
2. **TFSA to max** (XEQT/VEQT) — flexible, tax-free, re-contributable.
3. **FHSA to max** ($8k/yr) — deduction + tax-free withdrawal; open it now.
4. **RRSP** once income is high (use the deduction in peak years); great for a high Toronto salary or USD-remote income.
5. Taxable/non-registered (Questrade/Wealthsimple/IBKR) after registered accounts are full.

### C3. Financial independence timeline model

**FI target (4% rule):** annual expenses × 25.
- $40,000/yr → **$1.0M**
- $50,000/yr → **$1.25M**
- $60,000/yr → **$1.5M**

**Modeled accumulation** (income arc: Yr1–2 ~CA$85–110k incl. CA$15–20k tutoring; Yr3–4 CA$100–130k; Yr5+ CA$130–170k or US$140–200k remote), ~7% real return:

| Savings rate | Approx. annual savings (blended) | Years to ~$1.0M | Years to ~$1.25M |
|---|---|---|---|
| 40% | ~CA$40–55k | ~14–16 yrs | ~16–18 yrs |
| 50% | ~CA$50–70k | ~11–13 yrs | ~13–15 yrs |
| 60% | ~CA$60–85k | ~9–11 yrs | ~11–13 yrs |

**Realistic FI age:** Starting serious earning at ~22–23 and saving 50–60% (achievable for a single high earner with no dependents, especially on a remote-US salary while paying Canadian costs), **FI (the $1.0–1.25M band) is realistic by ~33–38**, and comfortably by 40 even at a 40% rate. The remote-US salary path (US$200k+ converted to CAD) is the single biggest accelerant and can pull FI into the early 30s. The TFSA's tax-free compounding shortens the timeline materially because there's no drag from tax on growth or withdrawal.

### C4. Dual-income optimization (tech job + tutoring/consulting)
- **HST registration:** Register for GST/HST once self-employment revenue exceeds **$30,000 in a single calendar quarter or over four rolling consecutive quarters** (not a calendar year). Below that you're a "small supplier" and need not register — but **voluntary registration** lets you claim Input Tax Credits on business purchases (laptop, software). For modest tutoring income, staying under $30k keeps it simple; register voluntarily if you have big deductible purchases. (Note: tutoring of standard academic subjects may be GST/HST-exempt — confirm with an accountant.)
- **Self-employment deductions (Form T2125):** home-office portion, computer/software, professional development, accounting fees, a share of internet/phone — any reasonable expense to earn the income. **Set aside 25–30% of net self-employment income for taxes** (nothing is withheld automatically).
- **Clean multi-source handling:** keep a separate bank account for tutoring income + any GST/HST collected; track everything; file T2125 with your T1; keep employment (T4) and self-employment (T2125) records cleanly separated.
- **Platforms:** **Wealthsimple** (commission-free ETF buys incl. fractional shares from $1; simplest for automated XEQT/VEQT in TFSA/FHSA/RRSP) is the recommended default for a young automate-and-forget investor; **Questrade** (free ETF purchases, charged sells) is a solid all-rounder; **Interactive Brokers** is best if you're paid in USD by a remote-US employer (lowest FX cost, native US-dollar holdings).

---

## Recommendations (staged & concrete)

**Now → Month 3:** Open a TFSA at Wealthsimple and start an auto bi-weekly XEQT buy (even $50). Start C (K&R + *Effective C*) and Boneh "Cryptography I." Begin Shoup's number-theory book. Register the tutoring as self-employment; track income and expenses.
- *Threshold to advance:* can write/debug pointer-heavy C; finished Boneh "Cryptography I."

**Months 4–12:** Build Project 1 (ML-KEM in C). Take university number theory + abstract algebra for credit. Start Rust + RustCrypto/`subtle`. Read *Serious Cryptography*. Make your first liboqs issue/PR.
- *Threshold:* one merged or in-review OQS contribution; Project 1 public with passing KATs.

**Months 13–24:** Projects 2–3 (constant-time hardening; hybrid PQC TLS). Apply to the **Crypto4A co-op** (recruits every term) and Entrust/RBC student programs. Get active on the NIST PQC Forum + OQS Discord. Open an FHSA; raise TFSA contributions as income grows.
- *Threshold:* a co-op/internship offer OR a second substantive OQS PR.

**Months 25–36:** Graduate; take a full-time PQC/crypto role (Canadian startup or co-op-to-FT). Apply to OMSCS (Computing Systems; CS 6260) for part-time credentialing. Push savings rate to 50%+.
- *Threshold:* CA$80k+ role secured; TFSA maxed.

**Year 4+:** Pivot toward **remote-US comp** (SandboxAQ/AWS/IBM) once you have a track record — the FI accelerant. Max TFSA + FHSA every year; begin RRSP in peak-income years. Reassess the FI math annually.
- *Benchmark that changes the plan:* if remote-US TC clears US$200k, FI by the early-to-mid 30s is in reach at a 50%+ savings rate; if you stay Toronto-only, target the CA$140–190k senior band and a 55–60% rate.

---

## Caveats
- **Salary data is noisy.** Levels.fyi samples for SandboxAQ/QuSecure/Entrust are small; QuSecure's reported ~US$93–128k looks low for US crypto work and may reflect equity-heavy or older/junior data. PQShield, evolutionQ, Crypto4A, and Quantropi publish **no** public salary data — those figures are regional/role proxies. Treat all comp numbers as ranges, not promises.
- **PQC startup roles often expect more than entry-level skill.** Many "PQC engineer" postings want 2+ years or deep demonstrated ability; the realistic *first* job may be a co-op, an adjacent security/SWE role, or a generalist position you steer toward crypto. The portfolio is what compresses this.
- **FI projections are illustrative**, assuming ~7% real returns (not guaranteed), consistent high savings, no major life-cost changes, and that a remote-US salary is attainable (immigration/employer-of-record and CAD/USD FX add complexity). Markets can deliver a "lost decade."
- **Credentials timing:** OMSCS and Waterloo both require the bachelor's first; neither is a shortcut around finishing the undergrad degree.
- **HQC and FALCON are not yet final** (HQC draft ~2026, final ~2027; FALCON/FIPS 206 in draft) — learn them, but production work today centers on ML-KEM/ML-DSA/SLH-DSA.
- **Tax specifics change**; confirm current-year TFSA/FHSA/RRSP room via CRA My Account, and get a one-time accountant consult once dual income is steady (including whether your tutoring is GST/HST-exempt).