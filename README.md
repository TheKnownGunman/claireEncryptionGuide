# claireEncryptionGuide

Hi Benjamin, Esther loves you.

Here’s a clean, developer-friendly `README.md` file for your project — focused specifically on **setting up the secure connection** to the AWS RDS database via SSH tunneling using the `.bat` script.

---

## ✅ `README.md`

````markdown
# 🔐 Claire AI Encryption Connection Setup

This project uses **AWS KMS** for encryption and **PostgreSQL on RDS** inside a **private VPC subnet**, which is accessed securely through a **Bastion EC2 instance**.

This guide will help new developers **establish a secure connection** from their local machine to the private RDS instance using an SSH tunnel.

---

## 📦 Requirements

Before starting, make sure you have:

- [ ] Python installed
- [ ] `pip` available in terminal
- [ ] AWS credentials configured (`~/.aws/credentials` or environment)
- [ ] A PostgreSQL client library (e.g. `psycopg2`, or pgAdmin installed for manual DB access)
- [ ] `.env` file with the following entries:
  ```env
  PG_HOST=benjamindatabase-1.c41g6q6wsp1l.us-east-1.rds.amazonaws.com
  PG_PORT=15432
  PG_DATABASE=claireDatabase
  PG_USER=postgres
  PG_PASSWORD=your_db_password
  PG_SSL_CA=./rds-ca.pem

  AWS_REGION=us-east-1
  KMS_KEY_ID=arn:aws:kms:us-east-1:123456789012:key/your-key-id
````

* [ ] The following files (shared outside GitHub):

  * `bastion-key.pem` – your private SSH key
  * `rds-ca.pem` – Amazon RDS root certificate

---

## 🔗 Step-by-Step: Establish the Connection

### 1️⃣ Clone this repository

```bash
git clone https://github.com/YOUR-ORG/claire-encryption-test.git
cd claire-encryption-test
```

### 2️⃣ Add sensitive files

Copy the following files into the root of the project (do **not** commit them):

```
.env
bastion-key.pem
rds-ca.pem
```

> These files are listed in `.gitignore` and must be provided securely by the team lead.

---

### 3️⃣ Start the SSH Tunnel (One-Click)

Double-click the provided script:

```bash
run-encryption-test.bat
```

This script will:

* ✅ Open a secure SSH tunnel from `localhost:15432` to the private RDS instance
* ✅ Keep the tunnel open in a new window
* ❌ It **does not run any code** — it only establishes the secure connection

---

### 4️⃣ Run Your Application Separately

Once the tunnel is running, your local Python (or Node.js) app can connect using:

```python
# Example psycopg2 config
conn = psycopg2.connect(
    host="benjamindatabase-1.c41g6q6wsp1l.us-east-1.rds.amazonaws.com",
    port=15432,
    user=os.getenv("PG_USER"),
    password=os.getenv("PG_PASSWORD"),
    dbname=os.getenv("PG_DATABASE"),
    sslrootcert="rds-ca.pem",
    sslmode="verify-full"
)
```

---

## 🔒 Security Notes

* Never commit `.env`, `bastion-key.pem`, or `rds-ca.pem` to GitHub.
* These files should be distributed securely (e.g., via password-protected ZIP or Vault).
* All connections to the database are encrypted with TLS using the RDS root CA.

---

## ✅ Troubleshooting

| Problem                        | Solution                                                              |
| ------------------------------ | --------------------------------------------------------------------- |
| `bind ... Permission denied`   | Change `LOCAL_PORT` in `.bat` to an unused port (e.g., 15432)         |
| `ERR_TLS_CERT_ALTNAME_INVALID` | Ensure you're using the full RDS hostname, not `localhost`            |
| Can’t connect to DB            | Confirm SSH tunnel window is still open and `.env` values are correct |

---

## 🙌 Contact

For access to `.env`, keys, or debugging, contact **Benjamin (Team Lead)**.

```

---

Would you like me to generate a ZIP-ready starter pack of this entire structure with placeholders and git-safe files?
```
