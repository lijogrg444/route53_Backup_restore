A Python tool to back up and restore Amazon Route 53 DNS zones with safety checks, structured output, and detailed logging. Perfect for routinely safeguarding DNS records or migrating them between accounts.

✨ Features

Capability

Details

Backup single zone

--backup --zone-id Z123… – saves one JSON file per zone

Backup all zones

--backup-all – one JSON per zone in a dated folder

Restore

Converts a backup to an AWS‑ready change batch and can apply it (--apply)

Safe TXT handling

Splits long TXT strings into ≤ 255‑char chunks

Skips apex SOA/NS

Avoids Route 53 conflicts on restore

Logging

Timestamped log saved in the same backup folder

Structured folders

All output lives in backupYYYYMMDD_HHMMSS/

🛠 Requirements

Python 3.8+

boto3 (pip install boto3)

AWS credentials (CLI profile, environment variables, or EC2/IAM role)

🚀 Installation

# Clone repository
git clone https://github.com/your‑org/route53‑backup.git
cd route53‑backup

# (Optional) create a virtual environment
python -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt  # just boto3 for now

⚡ Quick Start

Backup all hosted zones

python route53_restore.py --backup-all

Creates a folder such as:

backup20250626_154510/
├── example.com_20250626.json
├── internal.local_20250626.json
└── route53_backup.log

Backup a single zone

python route53_restore.py --backup --zone-id Z123456ABCDEF

Restore records (convert only)

python route53_restore.py --input backup20250626_154510/example.com_20250626.json
# -> restore.json generated in current directory

Restore records (and apply to Route 53)

python route53_restore.py \
  --input backup20250626_154510/example.com_20250626.json \
  --zone-id Z123456ABCDEF \
  --apply

📂 Output Structure

backupYYYYMMDD_HHMMSS/
├── <zone1>_YYYYMMDD.json
├── <zone2>_YYYYMMDD.json
└── route53_backup.log

Log file mirrors everything printed to the console.

🔒 Safety Details

TXT Records – Long strings are chunked automatically so each quoted segment is ≤ 255 characters.

SOA/NS at Apex – Skipped during restore; Route 53 creates and manages them.

UPSERT Actions – Existing records are updated, new ones are created, nothing is deleted.

🗜 Optional: Compress Backups

After a backup:

zip -r backup20250626_154510.zip backup20250626_154510/

🧩 Extending / Contributing

Fork the repo & create a feature branch.

Add tests (coming soon: pytest).

Open a pull request.

📄 License

MIT License © 2025 Your Name or Company
