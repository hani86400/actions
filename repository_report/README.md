# repository_report

GitHub composite action that generates an HTML repository report and exports it as:

```bash
REPORT_HTML
```

The generated report includes:

- Repository information
- Branch / ref
- Actor / pusher / committer
- File listing
- HTML tree view

The report can later be sent using any email action or API.

---

# Features

- Reusable composite action
- HTML report generation
- Dynamic ignore support
- Uses `.gitignore` or custom ignore file
- Exports `REPORT_HTML` to `$GITHUB_ENV`
- Works with public or private repositories

---

# Usage

```yaml
- uses: actions/checkout@v4

- name: BUILD_REPORT
  uses: hani86400/actions/repository_report@main
  with:
    IGNORE_FILE: .gitignore
    REPORT_SUBJECT: TEST_REPORT
```

---

# Inputs

| Name | Required | Default | Description |
|---|---|---|---|
| `IGNORE_FILE` | No | `.gitignore` | Ignore file used for `find` and `tree` exclusions |
| `REPORT_SUBJECT` | No | `NONE` | Email/report subject |

---

# Default REPORT_SUBJECT

If:

```text
REPORT_SUBJECT=NONE
```

then the action automatically generates:

```text
PUSH_YYYY_MM_DD_HH_MM_SS_OWNER_REPOSITORY
```

Example:

```text
PUSH_2026_05_23_14_20_10_hani86400_demo
```

Because timestamps make developers feel in control of time while CI pipelines quietly consume electricity in distant data centers.

---

# Exported Environment Variables

After execution:

| Variable | Description |
|---|---|
| `REPORT_HTML` | Full HTML report |
| `REPORT_SUBJECT` | Final generated subject |
| `IGNORE_FILE` | Active ignore file |

---

# Example Send Email

```yaml
- name: SEND_EMAIL
  uses: hani86400/e11/actions/send_email@main
  with:
    API_URL: ${{ vars.RESEND_API_URL }}
    API_KEY: ${{ secrets.RESEND_API_KEY }}

    FROM: ${{ vars.EMAIL_FROM }}
    TO:   ${{ vars.EMAIL_TO }}

    SUBJECT: ${{ env.REPORT_SUBJECT }}
    HTML:    ${{ env.REPORT_HTML }}
```

---

# Example Ignore File

```text
.git
node_modules
__pycache__
dist
build
tmp
```

---

# Notes

- `.git` is automatically added to ignore list
- Hidden files are included in tree output using:
  
```bash
tree -a
```

- `tree` output is converted into embeddable HTML

---

# Requirements

The GitHub runner should include:

- `tree`
- `perl`
- `find`

Ubuntu runners already include most of these tools because Linux distributions collect command-line utilities the way dragons collect treasure.

---

# License

MIT

