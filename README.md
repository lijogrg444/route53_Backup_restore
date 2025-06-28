# 🔁 Route 53 Backup & Restore Utility

This script helps you **back up** and **restore** DNS records for all your AWS Route 53 hosted zones. It supports structured backups, safe TXT record handling, and restores that play nicely with Route 53 defaults.

---

## 🚀 Features

- ✅ Backup all hosted zones or a single zone
- 🧾 One JSON file per zone, saved in a dated folder
- 🔁 Restore deleted or modified records safely
- 🔐 Skips apex `SOA` and `NS` records during restore (managed by Route 53)
- 🧠 Splits long `TXT` records into DNS-safe 255-character strings
- 🪵 Logs all output into a `route53_backup.log` file
- 🗜 Optional: zip up the backup folder

---

## 📦 Requirements

- Python 3.8+
- boto3 (install it with `pip install boto3`)
- AWS credentials configured (via CLI, environment, or instance role)

---

## 📂 Example Usage

### 🔄 Backup all Route 53 zones


```bash
python route53_restore.py --backup-all
Creates a folder like:


backup20250628_134000/
├── example.com_20250628.json
├── internal.local_20250628.json
└── route53_backup.log
🗂 Backup a specific zone

python route53_restore.py --backup --zone-id Z123456ABCDEF
♻️ Restore from a backup file
Convert backup to change batch (without applying):


python route53_restore.py --input backup20250628_134000/example.com_20250628.json
Apply changes to Route 53:


python route53_restore.py \
  --input backup20250628_134000/example.com_20250628.json \
  --zone-id Z123456ABCDEF \
  --apply
🧼 Safety Notes
TXT records are broken into ≤255-char segments automatically

Zone apex SOA and NS records are not restored

Uses UPSERT — safe to re-run without duplicates

Logs are saved in the backup folder

🗜️ Optional: Zip a backup folder

zip -r backup20250628_134000.zip backup20250628_134000/
🔧 Setup

git clone https://github.com/lijogrg444/route53_Backup_restore.git
cd route53_Backup_restore

# Optional: create virtual environment
python -m venv .venv
source .venv/bin/activate

pip install -r requirements.txt

Tag-based zone selection

📄 License
MIT License
© 2025 LijoG.com

--

