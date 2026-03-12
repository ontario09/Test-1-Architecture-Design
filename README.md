# Dokumentasi Skema Database ERP (Multi-Company)

Dokumen ini menjelaskan struktur data untuk mendukung fitur Multi-tenancy, Rekonsiliasi Payment, dan Laporan Konsolidasi.

## Entity Relationship Diagram (ERD)

```mermaid
erDiagram
    COMPANIES ||--o{ USERS : "has"
    COMPANIES ||--o{ CHART_OF_ACCOUNTS : "defines"
    COMPANIES ||--o{ INVOICES : "issues"
    COMPANIES ||--o{ BANK_STATEMENTS : "uploads"

    COMPANIES {
        uuid id PK
        string name
        uuid parent_company_id FK "Untuk Konsolidasi"
    }

    CHART_OF_ACCOUNTS {
        uuid id PK
        uuid company_id FK
        string account_code
        string account_name
    }

    INVOICES {
        uuid id PK
        uuid company_id FK
        string invoice_number
        decimal total_amount
        string status
    }

    BANK_STATEMENTS {
        uuid id PK
        uuid company_id FK
        decimal transaction_amount
        string reference_code
        boolean is_reconciled
    }

    PAYMENT_RECONCILIATIONS {
        uuid id PK
        uuid company_id FK
        uuid invoice_id FK
        uuid bank_statement_id FK
        decimal matched_amount
        string match_method
    }

    INVOICES ||--o{ PAYMENT_RECONCILIATIONS : "matched_to"
    BANK_STATEMENTS ||--o{ PAYMENT_RECONCILIATIONS : "links_to"
