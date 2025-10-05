---
# 🧠 Mastering cURL: From Beginner to Pro

> A complete deep dive into `curl` — from the basics of HTTP communication to advanced API testing, scripting, and automation.  
> Written and compiled by **Al Hasib** (Web Developer & IoT Expert).

---

## 📘 Chapter 1: Understanding How curl Works

Before diving into commands, understand **what curl really is**.

### What is cURL?

**curl** stands for **Client URL**.  
It’s a command-line tool used to **send and receive data** from URLs using various protocols — mainly HTTP and HTTPS.

You can think of it as:
> “A programmable browser for the terminal.”

---

### 🧭 Basic Flow

1. You → make a request using `curl`.
2. `curl` → sends it to a web server (like an API or website).
3. The server → sends back a response.
4. You → see that response in your terminal.

Example:
```bash
curl https://example.com
````

This fetches the HTML source code of the page.

---

### 🧩 Example of What’s Happening

```bash
curl https://api.github.com
```

**curl** → sends an HTTP GET request to GitHub’s public API.
GitHub → replies with JSON data (server info, endpoints, etc.).

---

### 🎯 Key Takeaway

* `curl` = client
* API/server = responder
* You control what you send (method, headers, data) and what you want to see in return.

---

## 🧱 Chapter 2: HTTP Methods in curl

HTTP isn’t just about getting data — it’s a communication protocol that supports various **methods**.
Let’s learn how to use each with `curl`.

---

### 🟢 1. GET (default)

Retrieve data from a server.

```bash
curl https://api.github.com
```

Add headers:

```bash
curl -H "Accept: application/json" https://api.github.com
```

Add query parameters:

```bash
curl "https://api.example.com/users?id=5&active=true"
```

---

### 🟡 2. POST — Send Data

```bash
curl -X POST -d "name=Al&age=24" https://example.com/api/users
```

Automatically adds:

```
Content-Type: application/x-www-form-urlencoded
```

#### Sending JSON data

```bash
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Al Hasib","role":"developer"}'
```

---

### 🟠 3. PUT — Replace a Resource

```bash
curl -X PUT https://api.example.com/users/10 \
  -H "Content-Type: application/json" \
  -d '{"name":"Updated Name","role":"IoT expert"}'
```

---

### 🔵 4. PATCH — Partial Update

```bash
curl -X PATCH https://api.example.com/users/10 \
  -H "Content-Type: application/json" \
  -d '{"role":"teacher"}'
```

---

### 🔴 5. DELETE — Remove a Resource

```bash
curl -X DELETE https://api.example.com/users/10
```

---

### 🧠 Useful Flags Recap

| Flag | Meaning               |
| ---- | --------------------- |
| `-X` | Specify HTTP method   |
| `-d` | Send data             |
| `-H` | Add header            |
| `-i` | Show response headers |
| `-v` | Verbose mode          |

---

## 🧩 Chapter 3: Headers, Cookies & Authentication

### 🧾 1. Custom Headers

```bash
curl -H "X-API-Key: myapikey" -H "Accept: application/json" https://api.example.com
```

### 🍪 2. Cookies

Save cookies:

```bash
curl -c cookies.txt https://example.com/login
```

Send cookies:

```bash
curl -b cookies.txt https://example.com/profile
```

---

### 🔐 3. Authentication

#### Basic Auth

```bash
curl -u "username:password" https://api.example.com/login
```

#### Bearer Token

```bash
curl -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..." https://api.example.com/me
```

Store token in an environment variable:

```bash
curl -H "Authorization: Bearer $TOKEN" $URL
```

---

### 📦 4. Combine Headers + Data + Auth

```bash
curl -X POST https://api.example.com/users \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"Al Hasib","role":"Developer"}'
```

---

## 🌐 Chapter 4: Inspecting & Debugging Requests

When something goes wrong, `curl` helps you **see everything happening under the hood.**

---

### 🔍 1. View Response Headers

```bash
curl -i https://example.com
```

### 📜 2. Verbose Mode (Debug)

```bash
curl -v https://example.com
```

Shows DNS lookup, SSL handshake, and data transfer info.

### 🧮 3. Measure Time

```bash
curl -w "Time: %{time_total}s\n" -o /dev/null -s https://example.com
```

### 📊 4. Full Response with Headers & Body

```bash
curl -i -w "\nTime: %{time_total}s\n" https://example.com
```

---

## 📁 Chapter 5: File Uploads, Downloads & Transfers

---

### 💾 1. Download Files

```bash
curl -O https://example.com/file.zip
```

Change name:

```bash
curl -o myfile.zip https://example.com/file.zip
```

---

### 📤 2. Upload Files

```bash
curl -X POST -F "file=@image.png" https://api.example.com/upload
```

Multiple files:

```bash
curl -F "file1=@a.txt" -F "file2=@b.txt" https://example.com/upload
```

---

### 🔄 3. Resume Interrupted Download

```bash
curl -C - -O https://example.com/largefile.iso
```

---

### 🧰 4. Follow Redirects

```bash
curl -L https://short.url/link
```

---

## 🤖 Chapter 6: Automation, Scripting & Real-World API Testing

This is where `curl` becomes a developer’s **API robot** 🤖.

---

### ⚙️ 1. Automate API Health Checks

```bash
#!/bin/bash
URL="https://api.example.com/health"

while true; do
  curl -s -o /dev/null -w "HTTP Code: %{http_code}\nTime: %{time_total}s\n" $URL
  sleep 10
done
```

---

### 💥 2. Automate POST Requests

From file:

```bash
curl -X POST -H "Content-Type: application/json" -d @"data.json" https://api.example.com/users
```

Dynamic data:

```bash
curl -X POST -H "Content-Type: application/json" \
  -d "{\"name\":\"Al Hasib\",\"timestamp\":\"$(date +%s)\"}" https://api.example.com/users
```

---

### 🔁 3. Batch Requests

```bash
for id in {1..5}; do
  curl -s "https://api.example.com/users/$id" -o "user_$id.json"
done
```

---

### 🔐 4. Auth in Scripts

```bash
TOKEN="eyJhbGciOiJIUzI1NiIs..."
curl -H "Authorization: Bearer $TOKEN" https://api.example.com/me
```

---

### 🧪 5. Real-World API Testing

Login test:

```bash
curl -X POST https://api.example.com/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"123456"}' \
  -w "\nStatus: %{http_code}\nTime: %{time_total}s\n"
```

Login → Token → Use:

```bash
TOKEN=$(curl -s -X POST https://api.example.com/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"123456"}' | jq -r '.token')

curl -H "Authorization: Bearer $TOKEN" https://api.example.com/me
```

---

### ⚡ 6. Basic Load Testing

```bash
for i in {1..20}; do
  curl -s -o /dev/null -w "%{http_code}\n" https://api.example.com/users &
done
wait
```

---

### 🧾 7. Save Responses Automatically

```bash
curl -s https://api.example.com/data -o "log_$(date +%s).json"
```

---

### 🔧 8. Use curl in CI/CD Pipelines

```yaml
- name: Check API health
  run: |
    curl -s -o /dev/null -w "Status: %{http_code}\n" https://api.example.com/health
```

---

### 🧙 9. Combine curl + jq + Bash

```bash
#!/bin/bash
API="https://jsonplaceholder.typicode.com/posts"
DATA=$(curl -s $API | jq '[.[] | {id, title}]')

echo "Fetched $(echo $DATA | jq length) posts"
echo $DATA | jq '.[0:5]'
```

---

## 🏁 Final Summary

| Skill          | You Learned                            |
| -------------- | -------------------------------------- |
| HTTP Basics    | Client-server communication            |
| Methods        | GET, POST, PUT, PATCH, DELETE          |
| Headers & Auth | Custom headers, cookies, tokens        |
| Debugging      | Response headers, timing, verbose mode |
| File Handling  | Upload, download, resume               |
| Automation     | Scripts, loops, health checks          |
| API Testing    | Auth flows, chaining, CI/CD            |

---

## 💬 Author’s Note

> This guide was built step-by-step through real practice sessions.
> I, **Al Hasib**, created this to document my deep dive into cURL — so others can learn from it too.
> Keep exploring, experiment with real APIs, and make your terminal your playground.

---

### ⭐ Recommended Practice APIs

* [https://httpbin.org](https://httpbin.org)
* [https://reqres.in](https://reqres.in)
* [https://jsonplaceholder.typicode.com](https://jsonplaceholder.typicode.com)

---

**© 2025 — Al Hasib**
*Web Developer | IoT Expert | Educator*

