# The 3 Commands That Turn Chaos into Clarity in DevOps

![banner](./text-processing-banner.png)

> “You don’t need fancy monitoring tools to troubleshoot production issues.  
> Sometimes, all you need is a terminal., and three magical commands: `grep`, `awk`, and `sed`.”

These three commands can turn gigabytes of messy logs into clear insights within seconds.  
Once you master them, you’ll handle on-call like a pro.

---

## 🧠 Why Text Processing Matters in DevOps

In DevOps and SRE, every log line tells a story. But when you’re knee-deep in 2GB of logs, you need quick, powerful tools to extract meaning.  
This is where `grep`, `awk`, and `sed` shine.

Imagine this: you’re SSH’d into a production server. The application is down. Logs are huge. Instead of downloading them, you filter and extract what you need _right there_.

---

## 🔍 grep - The Search Engine of Your Terminal

**Purpose:** Quickly find patterns in massive text files.

**Syntax:**

```bash
grep [OPTIONS] PATTERN [FILE...]
```

### 🔹 Common Examples

```bash
# Find all error lines in logs
grep "ERROR" /var/log/app.log

# Search recursively in directory
grep -R "timeout" /etc/nginx/

# Case-insensitive + show line numbers
grep -in "failed" auth.log

# Highlight matches
grep --color=auto "warning" server.log
```

#### 💡 Pro Tip:

Combine tail -f and grep to monitor logs live:

```bash
tail -f app.log | grep "ERROR"
```

### 🧰 Real-World Use Cases

- Extracting failed pod names from Kubernetes logs:
  ```bash
  kubectl get pods | grep -i "error"
  ```
- Searching for error traces in CI/CD pipelines:
  ```bash
  cat build.log | grep "Exception"
  ```
- Monitoring real-time logs for failures:
  ```bash
  tail -f app.log | grep "500"
  ```

**Alternatives:**

- `ripgrep (rg)` - faster alternative, written in Rust.  
  👉 But `grep` remains the universal standard.

---

## ⚙️ awk - The Swiss Army Knife for Structured Data

**Purpose:** Process and analyze structured text line by line.

**Syntax:**

```bash
awk 'pattern {action}' file
```

### 🔹 Practical Examples

```bash
# Print specific columns
awk '{print $1, $4}' access.log

# Filter based on condition
awk '$3 == "ERROR" {print $0}' app.log

# Calculate average CPU usage
awk '{sum += $2} END {print "Avg:", sum/NR}' cpu.txt

# Show top 5 largest files
ls -lh | awk '{print $5, $9}' | sort -rh | head -5
```

### 🧰 Real-World Use Cases

- Extracting pod memory usage:
  ```bash
  kubectl top pods | awk '{print $1, $3}'
  ```
- Summarizing response times:
  ```bash
  awk '{sum += $5} END {print "Avg Response:", sum/NR}' access.log
  ```
- Counting unique IPs from access logs:
  ```bash
  awk '{print $1}' access.log | sort | uniq | wc -l
  ```

**Alternatives:**

- `cut` (simpler but limited)
- `jq` (great for JSON)
- `csvkit` (for CSVs)  
  👉 `awk` remains the most versatile for text streams.

---

## ✂️ sed - The Stream Editor

**Purpose:** Edit, replace, and transform text on the fly.

**Syntax:**

```bash
sed [OPTIONS] 'command' file
```

### 🔹 Common Examples

```bash
# Replace all “foo” with “bar”
sed 's/foo/bar/g' input.txt

# Delete blank lines
sed '/^$/d' file.txt

# Replace in place (useful for config changes)
sed -i 's/localhost/127.0.0.1/g' config.yml

# Prints lines starting from 5-10
sed -n '5,10p' file.txt
```

### 🧰 Real-World Use Cases

- Update IPs or URLs in config files:
  ```bash
  sed -i 's/dev.example.com/prod.example.com/g' nginx.conf
  ```
- Remove comments before deployment:
  ```bash
  sed '/^#/d' .env
  ```
- Bulk change environment variables in CI/CD:
  ```bash
  sed -i 's/DEBUG=True/DEBUG=False/' settings.py
  ```

**Alternatives:**

- `perl -pe` (more powerful but heavier)
- `tr` (for simple character replacement)  
  👉 `sed` is lightweight and available by default everywhere.

---

## 🧩 Combining Them for Power One-Liners

**Extract failed requests with response time > 500ms:**

```bash
grep "FAILED" app.log | awk '$5 > 500' | sed 's/FAILED/ERROR/g'
```

**Monitor errors in real time:**

```bash
tail -f app.log | grep "ERROR"
```

**Find all 404s and count them:**

```bash
grep "404" access.log | wc -l
```

**Extract usernames from access log and sort by frequency:**

```bash
awk '{print $3}' access.log | sort | uniq -c | sort -nr | head
```

**Replace all IPs with placeholders (for redaction):**

```bash
sed -E 's/[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+/[REDACTED]/g' access.log
```

---

## 💬 Final Thoughts

Once you start combining these three, you’ll realize you can transform any text stream into usable insights without ever leaving the terminal.

Whether you're debugging a failed deployment, analyzing performance logs, or cleaning config files, these commands will make your life easier.

Learn them once. Use them forever.

---

**👋 I'm Bharath Aaleti**, DevOps Engineer - learning DevOps and sharing everything I know/learn.
