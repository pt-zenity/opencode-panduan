# Panduan Lengkap OpenCode Pentester — Bahasa Indonesia

Panduan **opencode-pentester v1.13.0** dengan langkah instalasi, contoh perintah terpisah, audit keamanan, agen AI, dan pelaporan.

**[Buka panduan HTML interaktif (tombol salin perintah)](index.html)** · [Repositori proyek asli](https://github.com/humaidhahm/opencode-pentester)

> **Penting:** Jalankan pengujian keamanan hanya pada sistem milik sendiri atau yang telah memberikan izin tertulis. Contoh perintah berasal dari panduan dan perlu disesuaikan dengan lingkungan serta cakupan izin Anda.

## Daftar isi

- [Apa itu opencode-pentester?](#bagian-2)
- [Instalasi](#bagian-3)
- [Struktur Direktori](#bagian-4)
- [Cara Penggunaan](#bagian-5)
- [17 Kategori Audit OWASP yang Dicakup](#bagian-6)
- [12 Agen AI yang Tersedia](#bagian-7)
- [Mengelola Hasil Temuan](#bagian-8)
- [Membuat Laporan](#bagian-9)
- [Konfigurasi Lingkungan](#bagian-10)
- [Model LLM yang Direkomendasikan](#bagian-11)
- [Privasi Data dan Keamanan](#bagian-12)
- [Peringatan Hukum dan Etika](#bagian-13)
- [Contoh Alur Kerja Lengkap](#bagian-14)
- [Pemecahan Masalah](#bagian-15)
- [Referensi Lanjutan](#bagian-16)


<a id="bagian-2"></a>

## Apa itu opencode-pentester?

opencode-pentester adalah kerangka kerja pengujian penetrasi berbasis AI yang terintegrasi dengan OpenCode. Framework ini mengubah asisten pemrograman AI menjadi tim pentest lengkap dengan 12 agen AI khusus, 69 kategori serangan, 17 kategori audit OWASP, dan 75 security tools yang terpasang otomatis.


<a id="bagian-3"></a>

## Instalasi

### Prasyarat

- OpenCode sudah terpasang

- Linux atau macOS (Windows via WSL2)

- Akses sudo (untuk instalasi tools)

- Otorisasi tertulis jika ingin menguji sistem milik orang lain

### Langkah Instalasi

**1. Clone repositori**

```bash
git clone https://github.com/humaidhahm/opencode-pentester.git
```

**1. Clone repositori**

```bash
cd opencode-pentester
```

**2. Jalankan installer (install tools + daftarkan skill + daftarkan agent)**

```bash
sudo bash install.sh
```

### Opsi Installer

Perintah                                            Fungsi

**Perintah**

```bash
sudo bash install.sh                                Instalasi lengkap (tools + skill + agent)
```

**Perintah**

```bash
bash install.sh --global-only                       Daftarkan skill & agent saja, tanpa install tools
```

**Perintah**

```bash
bash install.sh --tools-only                        Install tools saja
```

**Perintah**

```bash
bash install.sh --local-only                        Refresh salinan skill di dalam repo saja
```

**Perintah**

```bash
bash install.sh --uninstall                         Hapus semua komponen yang terinstall
```

### Verifikasi Instalasi

**Cek tool yang terinstall**

```bash
bash db/doctor.sh
```

**Cek inventori lengkap**

```bash
bash db/doctor.sh --inventory
```

**Konfirmasi skill terdaftar di OpenCode**

```bash
opencode run "List available skills" --agent pentester
```


<a id="bagian-4"></a>

## Struktur Direktori

opencode-pentester/

**Perintah**

```text
├── agents/                     # Agent defensif (security-auditor, scope-guard)
```

**Perintah**

```text
├── db/                         # Database SQLite, skrip findings, doctor
```

**Perintah**

```text
│   ├── doctor.sh               # Cek ketersediaan tools
```

**Perintah**

```text
│   ├── findings.sh             # Kelola findings database
```

**Perintah**

```text
│   ├── handoff.sh              # Generate laporan
```

**Perintah**

```text
│   ├── pipeline.sh             # Jalankan pipeline otomatis
```

**Perintah**

```text
│   └── schema.sql              # Skema database
```

**Perintah**

```text
├── docs/                       # Dokumentasi referensi
```

**Perintah**

```text
├── pipelines/                  # Pipeline YAML DAG otomatis
```

**Perintah**

```text
│   ├── bounty_sweep.md         # Sweep bug bounty cepat
```

**Perintah**

```text
│   ├── web_app_scan.md         # Scan aplikasi web
```

**Perintah**

```text
│   ├── sast_scan.md            # Static analysis
```

**Perintah**

```text
│   └── injection_test.md       # Uji injeksi
```

**Perintah**

```text
├── playbooks/                  # Panduan engagement per domain
```

**Perintah**

```text
├── plugins/pentest/agents/     # 11 agen ofensif
```

**Perintah**

```text
├── templates/                  # Template laporan
```

**Perintah**

```text
├── SKILL.md                   # Definisi skill utama
```

**Perintah**

```text
├── AI-CHECKLIST.MD            # Checklist audit 17 kategori
```

**Perintah**

```text
├── AGENTS.md                  # Security rules untuk AI
```

**Perintah**

```text
├── settings.example.env       # Contoh konfigurasi environment
```

**Perintah**

```text
└── install.sh                 # Installer utama
```


<a id="bagian-5"></a>

## Cara Penggunaan

MODE 1 — Pengujian Penetrasi Ofensif (Target Jaringan) Gunakan mode ini ketika Anda memiliki otorisasi tertulis untuk menguji sistem target (URL, IP, hostname).

### Contoh Penggunaan Dasar

**Pentest dasar dengan budget waktu**

```bash
opencode run "I have authorization to pentest https://target.com. Use opencode-pentester. Time bu
```

**Pentest dengan budget lebih besar**

```bash
opencode run "I have authorization to pentest https://target.com. Use opencode-pentester. Time bu
```

**Red team engagement penuh**

```bash
opencode run "Authorized red team engagement against https://target.com. Budget: 480 minutes. Tho
```

**Bug bounty hunting**

```bash
opencode run "I have authorization to test https://target.com as part of their bug bounty program
```

### Menggunakan Agent Spesifik

**Gunakan agent pentester langsung**

```bash
opencode run "Pentest https://target.com" --agent pentester
```

**Gunakan agent security auditor**

```bash
opencode run "Audit this project for security vulnerabilities" --agent security-auditor
```

### Menggunakan Pipeline Otomatis

**Sweep bug bounty cepat**

```bash
bash db/pipeline.sh run bounty_sweep \
--param target_url=https://target.com \
--flag bug_bounty_scope_confirmed
```

**Scan aplikasi web lengkap**

```bash
bash db/pipeline.sh run web_app_scan \
--param target_url=https://target.com
```

**Uji injeksi (SQL, NoSQL, Command)**

```bash
bash db/pipeline.sh run injection_test \
--param target_url=https://target.com
```

**Analisis SSL/TLS**

```bash
bash db/pipeline.sh run ssl_analysis \
--param target_url=https://target.com
```

**Lihat semua pipeline yang tersedia**

```bash
bash db/pipeline.sh list
```

MODE 2 — Audit Kode Sumber (Audit Kode Defensif) Gunakan mode ini untuk mengaudit kode program Anda sendiri sebelum deployment, tanpa perlu target jaringan.

### Langkah Persiapan

**Salin file AGENTS.md ke project Anda**

```bash
cp AGENTS.md /path/to/your/project/
```

**Masuk ke direktori project**

```bash
cd /path/to/your/project/
```

### Menjalankan Audit

**Audit otomatis lengkap (17 kategori OWASP)**

```bash
opencode run "Run the security audit defined in AI-CHECKLIST.MD against this project"
```

**Audit dengan agent khusus**

```bash
opencode run "Audit this project for security vulnerabilities" --agent security-auditor
```

**Audit kategori tertentu saja**

```bash
opencode run "Check this project for SQL injection and XSS vulnerabilities"
```

**Audit untuk aplikasi Next.js**

```bash
opencode run "Run security audit on this Next.js project. Check for exposed secrets, unprotected
```

### Audit via Pipeline CI/CD

**Static Analysis (SAST)**

```bash
bash db/pipeline.sh run sast_scan --param source_dir=./src
```

**Software Composition Analysis (SCA) — cek dependency berbahaya**

```bash
bash db/pipeline.sh run sca_scan --param source_dir=.
```


<a id="bagian-6"></a>

## 17 Kategori Audit OWASP yang Dicakup

No                             Kategori                      Yang Diperiksa 1                              Misconfigured DB/RLS          Supabase RLS, Firebase Rules, policy USING (true) 2                              Unprotected API Routes        Auth middleware, endpoint tanpa autentikasi 3                              Committed Secrets             API key, token, password di repo git 4                              IDOR / Access Control         Verifikasi kepemilikan resource 5                              Secrets in Frontend           Variabel NEXT_PUBLIC_, VITE_, REACT_APP_ No                              Kategori                            Yang Diperiksa 6                               SSRF                                Fetch URL dari user, IP range internal 7                               CSRF                                Token validasi, SameSite cookie 8                               Security Headers / CSP              Helmet, CSP, HSTS, X-Frame-Options 9                               Wildcard CORS                       origin: &#x27;*&#x27; dengan credentials 10                              Missing Rate Limiting               Login, register, password reset 11                              SQL Injection                       Query concatenation, ORM raw queries 12                              XSS                                 dangerouslySetInnerHTML, innerHTML, v-html 13                              Unverified Webhooks                 Stripe signature, webhook idempotency 14                              Insecure File Uploads               Magic bytes, UUID rename, storage domain 15                              Verbose Errors                      Stack trace di response, debug mode production 16                              Weak Password Hashing               MD5/SHA1 untuk password, gunakan bcrypt/Argon2 17                              Slopsquatted Packages               Package palsu / typosquatting di dependencies


<a id="bagian-7"></a>

## 12 Agen AI yang Tersedia

### Agen Defensif

Agen               Fungsi security-auditor   Audit kode sumber untuk 17 kategori kerentanan scope-guard        Penjaga batas scope engagement

### Agen Ofensif

Agen                                                Fungsi pentester-orchestrator                              Membaca scope, buat rencana serangan pentester-executor                                  Eksekutor generik (SSRF, JWT, path traversal) recon-agent                                         OSINT, subdomain, port, crawling, direktori xss-tester                                          Uji XSS dengan bukti Playwright csrf-tester                                         Uji kerentanan CSRF injection-tester                                    SQL/NoSQL/Command injection via sqlmap cve-tester                                          Riset CVE dan validasi PoC network-pentest-agent                               SMB/SNMP/LDAP, Active Directory, pivoting cloud-security-agent                                Audit AWS/Azure/GCP, container, Kubernetes post-exploitation-agent                             PrivEsc, lateral movement, credential extraction password-attack-agent                               Brute force, hash cracking, credential spraying


<a id="bagian-8"></a>

## Mengelola Hasil Temuan

Semua temuan disimpan di database SQLite yang persisten lintas sesi.

**Lihat semua kerentanan yang ditemukan**

```bash
bash db/findings.sh list vulns
```

**Lihat semua host yang ditemukan**

```bash
bash db/findings.sh list hosts
```

**Lihat semua engagement**

```bash
bash db/findings.sh list engagements
```

**Log tool run secara manual**

```bash
bash db/findings.sh log tool \
--agent recon-agent \
--tool nmap \
--command "nmap -sV -p 80,443 example.com" \
--target example.com \
--phase recon \
--exit 0 \
--duration 1520 \
--summary "Port scan complete, 2 ports open"
```

**Cek attack chains (rangkaian serangan berkorelasi)**

```bash
bash db/chains.sh list
```

**Cek leads (jalur yang belum dieksplorasi)**

```bash
bash db/leads.sh list
```


<a id="bagian-9"></a>

## Membuat Laporan

**Generate laporan otomatis**

```bash
bash db/handoff.sh
```

**Lihat laporan yang dihasilkan**

```bash
cat outputs/$(basename $(pwd))/report.md
```

**Cek apakah laporan sudah stale (ada findings baru setelah laporan dibuat)**

```bash
bash db/handoff.sh --diff
```

### Format Laporan

Laporan dihasilkan dalam format Markdown dengan struktur 8 bagian: 1. Executive Summary — ringkasan eksekutif 2. Scope & Methodology — lingkup dan metodologi 3. Findings Summary — tabel ringkasan temuan 4. Critical & High Findings — temuan kritis dengan PoC 5. Medium & Low Findings — temuan medium/low 6. Attack Chains — rantai serangan berkorelasi 7. Remediation Roadmap — rencana perbaikan prioritas 8. Appendix — log tools, evidence artifacts


<a id="bagian-10"></a>

## Konfigurasi Lingkungan

Salin settings.example.env ke .env lalu sesuaikan:

cp settings.example.env .env

Isi konfigurasi penting di .env:

**Lokasi database findings**

```bash
PENTEST_DB="$HOME/.opencode-pentester/findings.db"
```

**Nama engagement aktif**

```bash
PENTEST_ENGAGEMENT="nama-engagement-anda"
```

**Timeout per tool (detik)**

```bash
PENTEST_TIMEOUT="300"
```

**Jumlah tool yang berjalan paralel**

```bash
PENTEST_PARALLEL="4"
```

**Scope strict — larang eksekusi di luar target**

```bash
PENTEST_SCOPE_STRICT="true"
```

**Izinkan mode destruktif (hati-hati!)**

```bash
PENTEST_DESTRUCTIVE="false"
```

**Model LLM yang digunakan**

```bash
PENTEST_LLM_MODEL="claude-sonnet-4-20250514"
```

**Format laporan**

```bash
PENTEST_REPORT_FORMAT="markdown"
```


<a id="bagian-11"></a>

## Model LLM yang Direkomendasikan

Model                                             Keterangan opencode/deepseek-v4.1-flash-free (variant:       Direkomendasikan — gratis, performa tinggi max) opencode/mimo-v2.6-flash-free                     Alternatif cepat, gratis claude-sonnet-4-20250514                          Sangat capable, berbayar Catatan: Model gratis bisa mengalami rate limiting. Cek ketersediaannya di /models dalam OpenCode.


<a id="bagian-12"></a>

## Privasi Data dan Keamanan

- Findings database disimpan lokal di ~/.opencode-pentester/findings.db

- Output scan disimpan lokal di direktori outputs/

- Data prompt dikirim ke provider LLM yang Anda pilih

- Untuk engagement sensitif, gunakan model lokal (Ollama, LM Studio)

**Gunakan model lokal Ollama untuk privasi penuh**

```bash
opencode run "Pentest https://target.com" --model ollama/llama3
```


<a id="bagian-13"></a>

## Peringatan Hukum dan Etika

WAJIB dibaca sebelum menggunakan!

- Hanya gunakan pada sistem yang Anda miliki atau dengan otorisasi tertulis

- Selalu sebutkan otorisasi dalam prompt: “I have authorization to pentest…”

- Patuhi rules of engagement dan scope yang telah disepakati

- Ikuti prinsip responsible disclosure jika menemukan kerentanan

- Pelanggaran bisa berujung pada konsekuensi hukum


<a id="bagian-14"></a>

## Contoh Alur Kerja Lengkap

### Skenario: Bug Bounty Hunting

**1. Masuk ke direktori opencode-pentester**

```bash
cd opencode-pentester
```

**2. Jalankan OpenCode dengan prompt otorisasi**

```bash
opencode run "I have authorization to test https://target.com as part of their bug bounty program
```

**3. Setelah selesai, lihat temuan**

```bash
bash db/findings.sh list vulns
```

**4. Generate laporan**

```bash
bash db/handoff.sh
```

**5. Baca laporan**

```bash
cat outputs/target-com/report.md
```

### Skenario: Audit Kode Sebelum Deploy

**1. Masuk ke project Anda**

```bash
cd /path/to/my-nextjs-app
```

**2. Salin rules keamanan**

```bash
cp /opt/opencode-pentester/AGENTS.md .
```

**3. Jalankan audit lengkap**

```bash
opencode run "Run the security audit defined in AI-CHECKLIST.MD against this project. Check all 1
```

**4. Baca laporan yang dihasilkan**

```bash
cat security/reports/FULL_SCAN_2026.md
```

### Skenario: CI/CD Security Gate

**Static Analysis**

```bash
bash /opt/opencode-pentester/db/pipeline.sh run sast_scan \
--param source_dir=./src
```

**Dependency Check**

```bash
bash /opt/opencode-pentester/db/pipeline.sh run sca_scan \
--param source_dir=.
```

**Exit code non-zero jika ada temuan kritis**

```bash
echo "Exit code: $?"
```


<a id="bagian-15"></a>

## Pemecahan Masalah

Masalah                                         Solusi Skill tidak terdeteksi                          Jalankan ulang bash install.sh

--global-only

Tool tidak terpasang                           Jalankan bash install-tools.sh atau bash db/doctor.sh --install Database tidak ditemukan                        Jalankan bash db/migrate.sh untuk inisialisasi Tool gagal di tengah jalan                      Installer resilient — cek summary dan jalankan perintah repair yang ditampilkan Butuh uninstall                                 bash install.sh --uninstall


<a id="bagian-16"></a>

## Referensi Lanjutan

File                                            Isi SKILL.md                                        Referensi skill lengkap & lifecycle engagement AI-CHECKLIST.MD                                 Prompt audit 17 kategori OWASP AGENTS.md                                       Aturan keamanan untuk semua kode yang digenerate AI docs/AGENT-GUIDE.md                             Panduan penggunaan setiap agen docs/CUSTOMIZATION.md                           Cara menambah kategori, agen, atau tools baru docs/reference/ATTACK_INDEX.md                  Indeks 69 kategori serangan docs/reference/TOOL_REGISTRY.md                 Katalog 75 tools dengan command template DISCLAIMER.md                                   Syarat penggunaan legal CONTRIBUTING.md                                 Cara berkontribusi ke project

