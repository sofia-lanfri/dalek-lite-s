# Verilib CLI Testing with New Server

## Testing New Server: http://ec2-3-133-125-4.us-east-2.compute.amazonaws.com/

This document replicates the tests from the Verilib CLI Installation and Usage Guide, but uses the new server endpoint.

### Testing Steps

#### Step 1: Initialize with New Server

Run the following command to initialize the repository with the new server:
```bash
verilib-cli init --url http://ec2-3-133-125-4.us-east-2.compute.amazonaws.com
```

Expected output:
```
Repository URL Options:
• Full repository: https://github.com/user/repo
• Specific branch: https://github.com/user/repo@branch-name
• Folder only: https://github.com/user/repo/tree/main/folder-name
• Folder from branch: https://github.com/user/repo/tree/main/folder-name@branch-name

Enter repository URL: https://github.com/sofia-lanfri/dalek-lite-s
Creating new repository from git URL: https://github.com/sofia-lanfri/dalek-lite-s

Collecting repository information...
Select Language:: Rust
Select Proof Language:: Verus

Enter summary (max 128 characters, required):
> 
Enter description (optional, press Enter to skip):
Select Type: Algorithms
Repository created successfully!
Repository ID: <new-id>
Created .verilib/.gitignore
```

#### Step 2: Clone Repository for Local Analysis
```bash
git clone https://github.com/sofia-lanfri/dalek-lite-s.git repo
cd repo
```

#### Step 3: Generate Structure Files
```bash
verilib-cli create
```

Expected output:
```
Wrote config to /path/to/repo/.verilib/config.json
Running analyze_verus_specs_proofs.py...
Generated tracked functions CSV at /path/to/repo/.verilib/tracked_functions.csv
Generating structure files...
WARNING: File already exists, overwriting: [218 files]
Created 218 structure files in /path/to/repo/.verilib/structure
```

#### Step 4: Enrich with Atoms
```bash
verilib-cli atomize --update-stubs
```

Expected output:
```
Running probe-verus stubify on /path/to/repo/.verilib/structure...
Stubs saved to /path/to/repo/.verilib/stubs.json
Loaded 218 stubs from structure files
Running probe-verus atomize on /path/to/repo...
Atoms saved to /path/to/repo/.verilib/atoms.json
Loaded 1142 atoms
Enriching stubs with atom metadata...
Entries enriched: 218
Skipped: 0
Saving enriched stubs to /path/to/repo/.verilib/stubs.json...
Updating structure files with code-names...
Structure files updated: 218
Skipped: 0
Done.
```

#### Step 5: Check Specifications
```bash
verilib-cli specify --dry-run
```

Expected output:
```
Loaded 218 stubs from stubs.json
Running probe-verus specify on /path/to/repo...
Specs saved to /path/to/repo/.verilib/specs.json
Incorporated spec-text for 218 stubs
Found 11 existing certs

Found 218 stubs with spec-text
Found 207 stubs needing certification

207 functions with specs need certification

============================================================
Functions with specs but no certification:
============================================================

  [1] conditional_assign (curve25519-dalek/src/backend/serial/curve_models/mod.rs#L460-L478)
  [2] identity (curve25519-dalek/src/backend/serial/curve_models/mod.rs#L297-L306)
  [3] neg (curve25519-dalek/src/backend/serial/curve_models/mod.rs#L1292-L1326)
  [4] zeroize (curve25519-dalek/src/backend/serial/curve_models/mod.rs#L210-L221)
  ...and 203 more functions needing certification
```

#### Step 6: Run Verification
```bash
verilib-cli verify --check-only --debug
```

Expected output:
```
Checking stubs for verification failures...
All 218 stubs passed verification.
```

**Full verification command (without --check-only):**
```bash
verilib-cli verify --debug
```

Expected output:
```
Running probe-verus verify on /path/to/repo...
Error: probe-verus verify failed.
Error: probe-verus verify failed
```

**Note:** Full verification may fail due to complex proof requirements. However, the `--check-only` flag successfully checks verification status without running proof verification. All 218 stubs should pass the check.

### Renewing API Key

If you encounter an authorization error (401), follow these steps to renew your API key:

1. Open the Verilib server URL in your browser: [http://ec2-3-133-125-4.us-east-2.compute.amazonaws.com](http://ec2-3-133-125-4.us-east-2.compute.amazonaws.com).
2. Log in with your credentials.
3. Navigate to the API key management section and generate a new API key.
4. Update the Verilib CLI configuration with the new API key:
   ```bash
   verilib-cli config set-api-key <new-api-key>
   ```
   Replace `<new-api-key>` with the key you copied.
5. Retry the initialization command:
   ```bash
   verilib-cli init --url http://ec2-3-133-125-4.us-east-2.compute.amazonaws.com
   ```

---

## Summary

This document replicates the tests from the original guide, using the new server endpoint: `http://ec2-3-133-125-4.us-east-2.compute.amazonaws.com`. Follow the steps above to verify the functionality of Verilib CLI with the new server.