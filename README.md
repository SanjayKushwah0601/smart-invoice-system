# Smart Invoice System

Python project for working with vendor invoices, purchases, and store inventory data stored in a local SQLite database.

## Prerequisites

- [uv](https://docs.astral.sh/uv/) (recommended)
- Python **3.14+** (pinned in `.python-version`)

## Setup

```bash
# Clone the repo
git clone https://github.com/SanjayKushwah0601/smart-invoice-system.git
cd smart-invoice-system

# Create the virtualenv and install the package + dev deps (ipykernel)
uv sync --group dev
```

## Run

```bash
# CLI entrypoint
uv run smart-invoice-system

# Or as a module
uv run python -c "from smart_invoice_system import main; main()"
```

For notebooks / interactive work, use the project environment:

```bash
uv run python
# or select the .venv kernel after: uv sync --group dev
```

## Database

Local SQLite file (not committed; ignored via `.gitignore`):

| | |
|---|---|
| **Path** | `data/inventory.db` |
| **Size** | ~405 MB |
| **Engine** | SQLite |

Place or keep `inventory.db` at `data/inventory.db` before running analysis against it.

### Tables

| Table | Rows (approx.) | Purpose |
|---|---:|---|
| `purchases` | 2,372,474 | Line-level purchase / receiving records |
| `begin_inventory` | 206,529 | Starting on-hand inventory by store/brand |
| `end_inventory` | 224,489 | Ending on-hand inventory by store/brand |
| `purchase_prices` | 12,261 | Brand/vendor purchase price reference |
| `vendor_invoice` | 5,543 | Vendor invoice headers (qty, dollars, freight) |

### Column overview

- **`purchases`** — `InventoryId`, `Store`, `Brand`, `Description`, `Size`, `VendorNumber`, `VendorName`, `PONumber`, `PODate`, `ReceivingDate`, `InvoiceDate`, `PayDate`, `PurchasePrice`, `Quantity`, `Dollars`, `Classification`
- **`begin_inventory` / `end_inventory`** — `InventoryId`, `Store`, `City`, `Brand`, `Description`, `Size`, `onHand`, `Price`, `startDate` / `endDate`
- **`purchase_prices`** — `Brand`, `Description`, `Price`, `Size`, `Volume`, `Classification`, `PurchasePrice`, `VendorNumber`, `VendorName`
- **`vendor_invoice`** — `VendorNumber`, `VendorName`, `InvoiceDate`, `PONumber`, `PODate`, `PayDate`, `Quantity`, `Dollars`, `Freight`, `Approval`

### Quick check

```bash
sqlite3 data/inventory.db ".tables"
sqlite3 data/inventory.db "SELECT COUNT(*) FROM purchases;"
```

From Python:

```python
import sqlite3

conn = sqlite3.connect("data/inventory.db")
conn.row_factory = sqlite3.Row
rows = conn.execute("SELECT * FROM vendor_invoice LIMIT 5").fetchall()
```

## Project layout

```
smart-invoice-system/
├── data/
│   └── inventory.db          # local only (gitignored)
├── src/smart_invoice_system/ # package source
├── pyproject.toml
└── uv.lock
```
