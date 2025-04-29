## 🧵 Sewing Patterns Viewer

This site loads and displays a CSV of your sewing patterns using [MinIO](https://min.io/) as local object storage and [MkDocs](https://www.mkdocs.org/) for the site framework.

---

### 1: Clone the Repository

```bash
git clone git@github.com:chesmore/sewing-inventory.git
cd sewing-inventory
```

### 2: Install and Run MinIo

```bash
# Install MinIO
brew install minio/stable/minio
# Start MinIO server
minio server ~/minio-data
```

After starting the MinIO server, you should see output similar to:

- Web UI: http://127.0.0.1:61745
- API: http://127.0.0.1:9000

Login credentials:
- Username: minioadmin
- Password: minioadmin

### 3: Upload Your CSV File

Open http://127.0.0.1:61745 in your browser.

Login using the credentials above.

Create a new bucket called sewing-data.

Upload your sewing patterns CSV file to the sewing-data bucket.

### 4: Grant Public Read Access to the Bucket

By default, MinIO buckets are private. To make your CSV accessible from the browser or JavaScript, you need to grant anonymous read/download access to the bucket.

Install the MinIO client (mc):

```bash
brew install minio/stable/mc
```

Connect to Your Local MinIO Server
```
mc alias set localminio http://127.0.0.1:9000 minioadmin minioadmin
```

Make the Bucket Public
```bash
mc anonymous set download localminio/sewing-data
```

Now your CSV file is publicly accessible at:
```
http://127.0.0.1:9000/sewing-data/sewing-patterns.csv
```

### 5: Serve the MkDocs Site

```bash
# Install MkDocs
pip install mkdocs
# Serve the MkDocs site
mkdocs serve
```

This will start a local server at http://127.0.0.1:8000.
Go to the "Patterns" tab to see your uploaded CSV rendered as a sortable table.

### 6: Data Format
Your data should be in CSV format, and here is an example:
```csv
Pattern Name,Designer,Category,Link
Sewing Pattern 1,Designer A,Dress,http://example.com/pattern1
Sewing Pattern 2,Designer B,Top,http://example.com/pattern2
Sewing Pattern 3,Designer C,Skirt,http://example.com/pattern3
```
