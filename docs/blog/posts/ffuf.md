---
date:
  created: 2026-04-27
authors:
  - dcyberguy
  - playroom
---

# __**FFUF**__
![ffuf](../../assets/images/Blog/ffuf_run_logo_600.png)

!!! WARNING Use Authorization Carefully
    Only fuzz systems you own or have explicit permission to test. Aggressive wordlists and recursion can generate a large number of requests very quickly.

FFUF stands for `Fuzz Faster U Fool`. It is an open-source web fuzzing tool written in Go.

<!-- more -->

## Why Use FFUF

FFUF is commonly used to:

- Discover hidden directories or files on web servers.
- Enumerate virtual hosts.
- Fuzz GET/POST parameters.
- Test for common web vulnerabilities.

### Key Strengths

- Extremely fast due to concurrency.
- Supports recursive fuzzing.
- Flexible output formats (JSON, HTML, CSV, and more).
- Customizable headers, cookies, and HTTP methods.

### Installation

Download a prebuilt binary from the FFUF [release page](https://github.com/ffuf/ffuf/releases/latest).

Or, if you have Go installed:

```bash
go install github.com/ffuf/ffuf
```

Or on Linux using apt:

```bash
sudo apt update
sudo apt install ffuf
```

Verify installation:

```bash
ffuf -h
```

### Quick Start

The simplest form of ffuf is directory discovery:

```bash
ffuf -u http://example.com/FUZZ -w /path/to/wordlist.txt
```

Flag breakdown:

- `-u`: URL with `FUZZ` as placeholder for fuzzing.
- `-w`: Wordlist for fuzzing.

### Filtering and Match Logic

You can filter results based on status codes, content length, or words:

```bash
ffuf -u http://example.com/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -mc 200,301,302
```

- `-mc`: Match status codes (200 OK, 301 Redirect, and so on).
- `-fc`: Filter status codes (for example, 404).

### Recursive Discovery

FFUF can explore discovered directories recursively:

```bash
ffuf -u http://example.com/FUZZ -w /path/to/wordlist.txt -recursion
```

This helps map nested directories automatically.

### GET Parameter Fuzzing

You can fuzz parameters to find hidden endpoints or test for vulnerabilities:

```bash
ffuf -u "http://example.com/page.php?id=FUZZ" -w /path/to/wordlist.txt
```

Useful for finding unlinked pages or input points for injection attacks.

### Subdomain and VHOST Fuzzing

A subdomain is any website underlying another domain. For example, `http://test.example.com` is the `test` subdomain of `example.com`.

```bash
ffuf -u http://FUZZ.example.com -w /path/to/wordlist.txt:FUZZ
```

### Output and Reporting

Store results for later analysis:

```bash
ffuf -u http://example.com/FUZZ -w wordlist.txt -o results.json -of json
```

- `-o`: Output file.
- `-of`: Output format (`json`, `html`, `csv`, `md`).

### Wordlists

FFUF is only as good as its wordlists. Popular sources:

- SecLists: `/usr/share/seclists/`
- DirBuster wordlists
- Custom wordlists tailored to your target

### Practical Example

Discover directories on a website:

```bash
ffuf -u http://testphp.vulnweb.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200
```

This quickly shows directories that exist (status 200).

### Common Flags

| Flag             | Purpose                                      |
| ---------------- | -------------------------------------------- |
| -u               | URL with `FUZZ` placeholder                  |
| -w               | Wordlist                                     |
| -mc              | Match status code(s) (e.g., 200, 301)        |
| -fc              | Filter status code(s) (e.g., 404)            |
| -fs              | Filter by response size                      |
| -mw              | Match by words in response                   |
| -ml              | Match by line count                          |
| -recursion       | Fuzz recursively into discovered directories |
| -t               | Number of threads (default 40)               |
| -H               | Add custom HTTP headers                      |
| -b               | Add cookies                                  |
| -X               | HTTP method (GET, POST, PUT, DELETE, etc.)   |
| -o               | Output file                                  |
| -of              | Output format (json, csv, html, md)          |
| -v               | Verbose mode (shows all requests)            |
| -recursion-depth | Maximum recursion depth                      |
| -ac              | Auto-calibration (ignore errors)             |
