# 📖 Glossary — for anyone new to SOC/SOAR

This project sits at the intersection of a few different worlds (SOC
analysis, SOAR automation, cloud networking), so it uses a lot of jargon
very quickly. This page exists so nobody has to leave to go Google a
term mid-README. Skip it entirely if you already live in this world.

<details>
<summary><b>SOC — Security Operations Center</b></summary>

The team (or, here, the one-person home-lab version of a team) whose
job is to watch for security events, decide which ones matter, and
respond. In a real company this is a room full of analysts staring at
dashboards; here it's this repo.
</details>

<details>
<summary><b>SOAR — Security Orchestration, Automation and Response</b></summary>

Software that takes the "if this alert happens, do these five things"
logic that a human analyst would otherwise do by hand — look up a
hash, open a ticket, message someone — and runs it automatically.
**Shuffle** is the SOAR tool used in this lab.
</details>

<details>
<summary><b>IOC — Indicator of Compromise</b></summary>

Any piece of evidence that suggests something bad happened: a file
hash, an IP address, a domain name, a registry key. In this lab, the
IOC is a **SHA256 file hash** pulled from a Mimikatz detection.
</details>

<details>
<summary><b>Webhook</b></summary>

A URL that a piece of software can send data to the moment something
happens, instead of someone having to go check for updates manually.
Wazuh doesn't "poll" Shuffle for alerts — it **pushes** each alert to a
webhook URL the instant a rule fires.
</details>

<details>
<summary><b>Regex capture group</b></summary>

A pattern-matching rule that finds a piece of text inside a bigger
block of text and "captures" just that piece. Wazuh's raw hash field
looks like `MD5=...,SHA256=...,IMPHASH=...` — the regex node's job is
to reach into that string and pull out **only** the 64-character SHA256
value, discarding everything else.
</details>

<details>
<summary><b>Hash (MD5 / SHA256)</b></summary>

A short fixed-length fingerprint generated from a file's contents.
Same file → same hash, always. Security tools use hashes to identify
known-malicious files without needing to share the file itself —
you just compare fingerprints. SHA256 is the longer, more
collision-resistant sibling of MD5.
</details>

<details>
<summary><b>VirusTotal</b></summary>

A free-to-use file/URL/hash reputation service (owned by Google) that
aggregates results from 70+ antivirus engines. Send it a hash, it
tells you whether anyone else has ever seen that file and what the
antivirus world thinks of it — without you needing the actual file.
</details>

<details>
<summary><b>Alert vs. Case</b></summary>

An **alert** is a raw, unverified "something might be wrong" signal —
cheap to generate, expected to be noisy. A **case** is what an alert
becomes once a human (or automation) decides it's worth actually
investigating and tracking to resolution. TheHive's whole job is
managing that alert → case lifecycle.
</details>

<details>
<summary><b>TLP / PAP</b></summary>

**Traffic Light Protocol** and **Permissible Actions Protocol** —
standardized labels used in threat intel sharing to say how far a
piece of information is allowed to travel (TLP) and what kinds of
active response actions are permitted against it (PAP). You'll see
these as numeric fields (`"tlp": 2`) in the TheHive alert JSON later in
this repo.
</details>

<details>
<summary><b>OCI Security List vs. NSG vs. ufw</b></summary>

Three different firewalls stacked on top of each other on Oracle
Cloud, and traffic has to clear **all three** to reach a service:

1. **Security List** — a firewall attached to the whole subnet.
2. **NSG (Network Security Group)** — a firewall attached to the
   specific VM's network interface.
3. **ufw** — the firewall running *inside* the VM's own Linux OS.

Getting `ufw` right means nothing if the Security List or NSG is still
blocking the port upstream — see
[troubleshooting.md](troubleshooting.md#networking) for exactly how
this bit us.
</details>

<details>
<summary><b>Docker Compose</b></summary>

A way to describe a group of containers (e.g. "Wazuh manager +
indexer + dashboard" or "TheHive + Cassandra + Elasticsearch + MinIO")
in one file and bring them all up together with a single
`docker compose up -d`, instead of managing each container by hand.
</details>

<details>
<summary><b>Service account</b></summary>

A login that belongs to a piece of software, not a human. In this
lab, Shuffle doesn't authenticate to TheHive using the human analyst's
own login — it uses a dedicated `shuffle@test.com` **service** account
with an `analyst` profile, so the automation's access can be audited
and revoked separately from any person's.
</details>
