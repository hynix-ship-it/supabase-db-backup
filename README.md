# Supabase DB Backup GitHub Action

This repository contains a GitHub Actions workflow to automatically back up a Supabase PostgreSQL database.

## Overview

The workflow performs the following steps:

1.  **Scheduled Trigger**: Runs automatically at a specified time.
2.  **Manual Trigger**: Can be run manually from the GitHub Actions tab.
3.  **Database Dump**: Dumps the database into three separate files:
    *   `roles.sql`: For database roles.
    *   `schema.sql`: For the database schema.
    *   `data.sql`: For the data, using `COPY` for efficiency.
4.  **Compression**: Compresses the SQL dump files into a `.tar.gz` archive.
5.  **Upload**: Uploads the compressed archive to a Supabase Storage bucket.

## Workflow File

The workflow is defined in `.github/workflows/db-backup.yml`.

## Schedule

The backup is scheduled to run daily at 17:00 UTC (00:00 WIB - Western Indonesia Time).

```yaml
on:
  schedule:
    - cron: "0 17 * * *" # 24:00 WIB (UTC+7)
```

## Configuration

To use this workflow, you need to set the following secrets in your GitHub repository settings (`Settings > Secrets and variables > Actions`):

*   `SUPABASE_DB_URL`: The connection string for your Supabase database. It should be in the format `postgresql://postgres:[YOUR-PASSWORD]@[AWS-REGION].sql.supabase.co:5432/postgres`.
*   `SUPABASE_URL`: The URL of your Supabase project.
*   `SUPABASE_SERVICE_ROLE_KEY`: Your Supabase project's `service_role` key.

The workflow uploads backups to a Supabase Storage bucket named `db-backups`. You can change this by editing the `BACKUP_BUCKET` environment variable in the workflow file.

## Manual Trigger

You can trigger the backup manually:

1.  Go to the "Actions" tab of your repository.
2.  Select the "supabase-db-backup" workflow.
3.  Click on the "Run workflow" dropdown and then "Run workflow".

## Backup Structure

The backups are stored in the `db-backups` bucket with the following structure:

```
daily/YYYY-MM-DD/supabase-backup-YYYY-MM-DDTHH-MM-SSZ.tar.gz
```

*   `YYYY-MM-DD` corresponds to the date in WIB (UTC+7).


