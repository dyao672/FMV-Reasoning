# HLE-Diamond Parquet shards

Upload this folder’s files to `data/hle-diamond/` in FMV-Reasoning using GitHub’s **Add file → Upload files**. Each Parquet file is below 20 MiB and can be read independently. The original source file is unchanged.

Read the full dataset in its original row order (requires `pip install pyarrow`):

```python
from pathlib import Path
import pyarrow as pa
import pyarrow.parquet as pq

folder = Path("data/hle-diamond")
table = pa.concat_tables([pq.read_table(p) for p in sorted(folder.glob("test-*.parquet"))])
# Optional: dataframe = table.to_pandas()
# Optional: pq.write_table(table, "test-reassembled.parquet", compression="zstd")
```

`manifest.json` records row ranges, sizes, and SHA-256 checksums. Every shard was read back and compared with its source rows. Reassembly preserves the data and schema, but does not reproduce the original file byte for byte.
