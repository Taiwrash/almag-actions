# Almag Actions

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Almag%20Actions-blue.svg?logo=github&style=flat-square)](https://github.com/marketplace/actions/almag-actions)

Normalize and upload security scan reports to your Almag instance. This action allows you to aggregate findings from various security tools into a single, intelligent dashboard.

## Supported Tools

Almag currently supports parsing reports from:
- **Trivy** (Vulnerabilities, Misconfigurations)
- **Gitleaks** (Secrets)
- **Grype** (Vulnerabilities)
- **Semgrep** (SAST)
- **VulnCheck** (KEV correlation)
- **Generic SARIF** (Static analysis, linting, etc.)

## Usage

Add this action to your GitHub workflow after your security scanning step.

```yaml
- name: Upload Report to Almag
  uses: almag-security/almag-actions@v1
  with:
    almag-url: 'https://almag.yourcompany.com'
    token: ${{ secrets.ALMAG_TOKEN }}
    tool: 'trivy'
    report-file: 'reports/trivy-results.json'
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `almag-url` | The base URL of your Almag server instance. | **Yes** | N/A |
| `token` | Your Almag API Key (`almag_sk_...`) or JWT. | **Yes** | N/A |
| `tool` | The name of the tool that generated the report. One of: `trivy`, `gitleaks`, `grype`, `semgrep`, `vulncheck`, `sarif`. | **Yes** | N/A |
| `report-file` | Path to the generated report file (must exist). | **Yes** | N/A |

## Examples

### Trivy Scan Integration

```yaml
jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Trivy
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: 'my-app:latest'
          format: 'json'
          output: 'trivy.json'

      - name: Send findings to Almag
        uses: almag-security/almag-actions@v1
        with:
          almag-url: ${{ secrets.ALMAG_URL }}
          token: ${{ secrets.ALMAG_TOKEN }}
          tool: 'trivy'
          report-file: 'trivy.json'
```

### Semgrep Integration

```yaml
- name: Semgrep SAST
  run: semgrep --config auto --json --output semgrep.json

- name: Upload to Almag
  uses: almag-security/almag-actions@v1
  with:
    almag-url: ${{ secrets.ALMAG_URL }}
    token: ${{ secrets.ALMAG_TOKEN }}
    tool: 'semgrep'
    report-file: 'semgrep.json'
```

## Security

Ensure your `ALMAG_TOKEN` and `ALMAG_URL` are stored in **GitHub Repository Secrets** to keep them secure.

## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).
