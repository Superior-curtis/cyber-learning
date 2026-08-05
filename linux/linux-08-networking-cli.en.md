# Networking on the Command Line (ip / ss / ports)

> 📅 2026-08-05 · Getting Started
> Answering three network questions from the command line: what interfaces exist, which ports are listening, and what this machine is talking to. Practical scenes for ip, ss, curl, dig, and nc.

---

## Three questions, one toolbox

When the network misbehaves, you want answers to exactly three questions: **what does this machine look like? who is at the door waiting to serve? and who is it talking to right now?** The good news is that the Linux command line has already turned the answers to all three into a handful of commands.

The bad news is there are a lot of tool names, split across old and new generations. This chapter is not a dictionary — it is a battle map of "which command to type when." We will walk through `ip`, `ss`, `curl`, `dig`, and `nc`, with each tool mapped to a real scenario.

## ip: the first stop for network cards

The classic `ifconfig` is being phased out; modern distros use the `ip` command. It answers "what does this machine look like": which interfaces exist, their IPs, and whether they are up.

```bash
# List all network interfaces and IPs
ip addr

# A compact one-line-per-interface status (UP or DOWN)
ip -br addr

# The routing table: which way the default gateway goes
ip route
```

`ip addr` is where troubleshooting starts — first confirm the interface is `UP` or `DOWN`, then confirm the IP is configured correctly. When the network is broken, these three commands tell you in ten seconds whether the problem is the interface, the IP, or the route.

## ss: which port is listening?

"Which service opened which port, and who is connected right now?" is the daily question of both ops and security. The modern answer is `ss`, which replaces the older `netstat`. **LISTEN** means some program is waiting for connections on that port — which is exactly the answer to "who is opening the door."

```bash
# Every listening TCP port, with the owning program
sudo ss -tlnp

# Listening UDP ports
sudo ss -ulnp

# All established connections (including the remote IP)
ss -tn
```

Adding `-p` reveals the process — the blue team's favorite "whose door is this port" query. The older `netstat -tlnp` still works in the same scenario, though some distros install it separately.

**The most common beginner mistake**: remembering the port number but not asking which interface it is bound to. A service bound to `0.0.0.0:22` is reachable on port 22 of every interface; one bound to `127.0.0.1:5432` is only reachable from the local machine. The former is often why databases get scanned and hit — a point we return to in `linux-09-hardening-basics`.

## A quick reference of common ports

No need to memorize — just know they exist:

| Port | Protocol | Service | Security relevance |
|------|----------|---------|---------------------|
| 22 | TCP | SSH | Remote login; the most brute-forced target there is |
| 53 | TCP/UDP | DNS | The lifeline of resolution; often abused in amplification attacks |
| 80 / 443 | TCP | HTTP / HTTPS | Web services; 443 is the encrypted one |
| 3306 | TCP | MySQL | Database; the number one "should not be public" candidate |
| 5432 | TCP | PostgreSQL | Database; same story |
| 27017 | TCP | MongoDB | Mass unauthenticated data dumps happened here |
| 3389 | TCP | RDP | Windows remote desktop; a brute-force hotspot |
| 8080 | TCP | Common alternate HTTP port | Note: any program can "occupy" this port |

When you see an unexpected listening port, run `ss -tlnp` and ask "who," then decide whether it should be there — this is how you build the reflex of "port → service → should it be open."

## curl: the universal HTTP Swiss army knife

`curl` is arguably the most versatile network tool on the command line, especially for talking to web services: fetching pages, testing APIs, carrying cookies, inspecting response headers — it does it all.

```bash
# Fetch a page
curl https://example.com

# Headers only (great for checking server configuration)
curl -I https://example.com

# POST to an API
curl -X POST https://api.example.com/login \\
-d "user=alice&pass=secret"

# Inspect TLS certificate details
echo | openssl s_client -connect example.com:443 \\
-servername example.com 2>/dev/null | openssl x509 -noout -dates
```

> curl -k skips certificate verification — do not use it casually. It bypasses the check that protects you from man-in-the-middle attacks. Occasional debugging aside, a "certificate error" is itself a warning sign that deserves investigation, not a bypass.

## dig: a dedicated DNS probe

A website being down is not always the server's fault — DNS may simply fail to find the way. `dig` is the tool built for talking to DNS, querying the resolver configured on your system directly:

```bash
# Look up the A record for a domain
dig example.com

# Ask a specific DNS server
dig @8.8.8.8 example.com

# Short answer only
dig +short example.com

# Reverse lookup: which name maps to this IP
dig -x 93.184.216.34
```

`dig +short` is the daily quick-check mode. Beyond "does this name resolve," it lets you cross-compare answers from multiple DNS servers — when they disagree, that is often a warning sign of DNS poisoning or a misconfiguration.

## nc: the last word in connectivity tests

`nc` (netcat) bills itself as the Swiss army knife of TCP/IP. Its two basic uses answer the last of our three questions: **can this machine talk to that machine?**

```bash
# Test whether a host's port is reachable
nc -zv example.com 443

# Listen on a local port (for your own testing)
nc -l 12345

# Connect and send a line of text
echo "hello" | nc example.com 80
```

With `nc -zv`, the `-z` means "send no data, only test reachability" and `-v` gives verbose output — the standard technique for checking firewalls and connectivity. `nc -l` opens a test port so you can verify "can the outside actually reach in"; close it the moment you are done, and do not leave an open door on the system.

## Putting it together: what is this machine talking to?

Chain the tools from this chapter into one reconnaissance flow and you can answer "who is this machine talking to right now" in about a minute:

```bash
# 1. What does this machine's network look like?
ip addr show

# 2. Which program opened which port?
sudo ss -tlnp

# 3. What connections are active right now, and to where?
ss -tnp

# 4. What is that remote IP's name on the internet?
dig -x <that-IP>
```

> All of these commands are standard operational checks on your own machine. Pointing the same tools at someone else's servers, or scanning targets without authorization, is illegal in many jurisdictions — the boundary for testing is always systems you own or are explicitly authorized to test.

## Next: network fundamentals, then hardening

By now you can answer "who opened the door and what is this machine talking to" from the command line. But the *why* behind the design — how IP addressing works, what the TCP three-way handshake does, how DNS really operates — is the territory of `net-01-network-fundamentals`. And "how to lock the doors properly" belongs to `linux-09-hardening-basics`. Tools first, then principles, then hardening — that rounds out the networking thread.

#### Q: You need to check which program is listening on port 8080. Which command do you run?

* sudo ss -tlnp

* ip addr

* dig example.com

* curl -I example.com

> 💡 ss -tlnp lists every listening TCP port and shows the owning process; sudo is needed to see processes belonging to other users. ip addr shows interface IPs, dig queries DNS, and curl tests HTTP — none of them answer 'what is listening on this port.'
