# Medusa Demo

## Prerequisites

```bash
sudo dnf in git nodejs postgresql postgresql-contrib postgresql-server
```

## Medusa Setup

Install Medusa CLI.
```bash
npm install @medusajs/medusa-cli -g
```

Install dependencies.
```bash
cd my-medusa-store
npm install
```

### Configure Database - Local PostgreSQL DB

Setup PostgreSQL

```bash
sudo postgresql-setup initdb
```

Start PostgreSQL

```bash
sudo systemctl enable --now postgresql
```

Access the PostgreSQL console to create a new user and database for the Medusa server.

```bash
sudo -u postgres psql
```

To create a new user named `medusa_admin` run this command:

```sql
CREATE USER medusa_admin WITH PASSWORD 'medusa_admin_password';
```

Now, create a new database named `medusa_db` and make `medusa_admin` the owner.

```sql
CREATE DATABASE medusa_db OWNER medusa_admin;
```

Last, grant all privileges to `medusa_admin` and exit the PostgreSQL console.

```sql
GRANT ALL PRIVILEGES ON DATABASE medusa_db TO medusa_admin;
```

```sql
exit
```

Replace below line
```bash
sudo nano /var/lib/pgsql/data/pg_hba.conf
```
```bash
# IPv4 local connections:
host    all             all             127.0.0.1/32            indent
```

Add the connection string as the `DATABASE_URL` to your environment variables. Inside `my-medusa-store` create a `.env` file and add the following:

```
DATABASE_URL=postgres://medusa_admin:medusa_admin_password@localhost:5432/medusa_db
```

### Seed Database

Run migrations and seed data to the database by running the following command:

```bash
medusa seed --seed-file="./data/seed.json"
```

### Start your Medusa backend

```bash
medusa develop
```

The Medusa server will start running on port `7001`.

Test your server:
```bash
curl localhost:7001/store/products
```

Open up your browser and visit [`localhost:7001`](http://localhost:7001) to see the Medusa Admin Dashboard. Use the Email `admin@medusa-test.com` and password `supersecret` to log in.
