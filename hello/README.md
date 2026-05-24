# Hello Action

Simple GitHub Action to print a message multiple times.

---

## Inputs

| Name | Required | Default | Description |
|---|---|---|---|
| MSG | Yes | - | Message to print |
| COUNT | No | 1 | Number of repetitions |

---

## Example

```yaml
name: TEST

on:
  workflow_dispatch:

jobs:
  demo:
    runs-on: ubuntu-latest

    steps:

      - name: hello
        uses: hani86400/actions/hello@main
        with:
          MSG: "Hani"
          COUNT: 3
```

---

## Output

```text
3 : Hani
3 : Hani
3 : Hani
```

---

## Repository Structure

```text
hello/
└── action.yml
```
