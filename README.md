# GoLint SpechtLabs Action

GitHub Action to run [golint-sl](https://github.com/SpechtLabs/golint-sl) - SpechtLabs best practices for writing good Go code.

## Features

- 🚀 Fast - Uses Go install, no Docker overhead
- 📊 32 analyzers for code quality, safety, and architecture
- 📝 Job summaries with issue counts
- ⚙️ Configurable - run all or specific analyzers
- ✅ Fail or warn on issues

## Usage

### Basic

```yaml
name: Lint
on: [push, pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: SpechtLabs/golint-sl-action@v1
```

### With Options

```yaml
- uses: SpechtLabs/golint-sl-action@v1
  with:
    # Run specific analyzers only
    analyzers: 'nilcheck,errorwrap,wideevents'
    
    # Don't fail on errors (just warn)
    fail-on-error: 'false'
    
    # Specific directory
    working-directory: './src'
    
    # Additional args
    args: './pkg/... ./internal/...'
```

### Specific Version

```yaml
- uses: SpechtLabs/golint-sl-action@v1
  with:
    version: 'v1.0.0'
```

### Full Example with Caching

```yaml
name: Go CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  lint:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-go@v5
        with:
          go-version: '1.23'
          cache: true

      - name: Run GoLint SpechtLabs
        uses: SpechtLabs/golint-sl-action@v1
        with:
          fail-on-error: 'true'

  test:
    name: Test
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version: '1.23'
          cache: true
      - run: go test -v ./...
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `version` | Version of golint-sl to use | `latest` |
| `working-directory` | Directory to run linter in | `.` |
| `args` | Arguments passed to golint-sl | `./...` |
| `analyzers` | Specific analyzers (comma-separated) | `` (all) |
| `fail-on-error` | Fail if issues found | `true` |
| `github-token` | GitHub token for API | `${{ github.token }}` |

## Outputs

| Output | Description |
|--------|-------------|
| `exit-code` | Exit code from golint-sl |
| `issues-count` | Number of issues found |

## Available Analyzers

### Error Handling
- `humaneerror` - Enforce humane-errors-go
- `errorwrap` - Detect bare error returns
- `sentinelerrors` - Prefer sentinel errors

### Observability
- `wideevents` - Wide events over scattered logs
- `contextlogger` - Context-based logging
- `contextpropagation` - Context propagation

### Kubernetes
- `reconciler` - Reconciler best practices
- `statusupdate` - Status updates
- `sideeffects` - Side effect detection

### Safety
- `nilcheck` - Nil checks on pointers
- `nopanic` - No panics in libraries
- `goroutineleak` - Goroutine leak detection
- `syncaccess` - Data race detection

### Clean Code
- `varscope` - Variable scope
- `nestingdepth` - Shallow nesting
- `functionsize` - Function length
- `closurecomplexity` - Simple closures

### Architecture
- `contextfirst` - Context as first param
- `pkgnaming` - Package naming
- `exporteddoc` - Documentation
- `hardcodedcreds` - Secret detection

[See all 32 analyzers →](https://github.com/SpechtLabs/golint-sl#analyzers-32)

## License

Apache 2.0

---

**GoLint SpechtLabs** - *Write Go code the right way.*
