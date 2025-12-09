# ReconPipeline

Automated reconnaissance pipeline for subdomain enumeration, live host detection, JS analysis, and secret extraction.

## What It Does

1. **Subdomain Enumeration** - Aggregates results from subfinder, amass, assetfinder, sublist3r, chaos
2. **Live Host Detection** - Probes with httpx to identify active hosts
3. **JS Discovery** - Extracts JavaScript URLs from HTML and Wayback archives
4. **Secret Extraction** - Greps JS files for API keys, tokens, credentials
5. **GF Pattern Matching** - Scans for XSS, SSRF, SQLi, LFI, RCE patterns
6. **Nuclei Prep** - Outputs clean target list ready for nuclei scanning

## Installation

### Required Tools

```bash
# Go tools
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
go install -v github.com/owasp-amass/amass/v4/...@master
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
go install -v github.com/lc/gau/v2/cmd/gau@latest

# System packages
sudo apt install curl ripgrep
```

### Optional Tools (recommended)

```bash
go install -v github.com/tomnomnom/assetfinder@latest
go install -v github.com/projectdiscovery/chaos-client/cmd/chaos@latest
go install -v github.com/tomnomnom/gf@latest
go install -v github.com/s0md3v/uro@latest
pip3 install sublist3r
```

## Usage

```bash
chmod +x recon_pipeline_enhanced.sh
./recon_pipeline_enhanced.sh target.com
```

## Output Files

```
recon_target.com_YYYY-MM-DD_HHMMSS/
├── final_subdomains.txt    # Deduplicated subdomains (pruned to live hosts)
├── httpx-detection.txt     # URL | status | title | tech stack
├── live-subdomain.txt      # Live hosts only
├── js.txt                  # Discovered JS URLs
├── js_files/               # Downloaded JS files
├── js-sensitive.txt        # Secrets found in JS
├── gf_output.txt           # GF pattern matches
├── nuclei_targets.txt      # Ready for nuclei
└── pipeline.log            # Execution log
```

## Post-Recon

Run nuclei on discovered targets:

```bash
nuclei -l recon_target.com_*/nuclei_targets.txt -t cves/ -o nuclei_results.txt
```

## License

MIT
