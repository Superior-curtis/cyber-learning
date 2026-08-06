# XXE & Insecure Deserialization

> 📅 2026-08-05 · Deep Dive
> Two classic 'parsing what should not be parsed' flaws: XXE makes an XML parser read local files, and insecure deserialization turns untrusted data into objects. This article unpacks both conceptually and gives defense directions.

---

Business-logic flaws need a human; these next two are technical flaws a machine should defend against: **XXE** and **insecure deserialization.** They share one sentence: **parsing what should not be parsed.**

## XXE: the XML parser overreaches

**XXE (XML External Entity)** happens when a program parses **user-supplied XML** and allows "external entities." This XML feature can make the parser **read local files or fire requests for you**:

```
<!-- Conceptual: make the parser read a local file -->
<!DOCTYPE x [ <!ENTITY f SYSTEM "file:///etc/passwd"> ]>
<x>&f;</x>
```

If the parser obeys, `&f;` expands to the file content — the attacker is effectively **borrowing the server's file-read permission.** At its core this is a type of **SSRF** (`web-04-ssrf-csrf-upload`): making the server reach things only it has access to.

Defense directions are clear:

* **Disable external entities**: turn off DTD / external-entity parsing on the XML parser.
* **Avoid parsing untrusted XML when you can**: use JSON instead.
* **Least privilege**: even if parsed, the server account should not read sensitive files.

## Insecure deserialization: turning data into objects

**Serialization** turns an object into data that can travel or be stored; **deserialization** is the reverse, turning data back into objects. The problem: **if you hand "untrusted data" to a deserializer, it can be designed to "execute code" or "build malicious objects."**

```
# Conceptual: deserialize from an untrusted source
obj = unserialize(untrustedInput)   # input may be crafted to load arbitrary classes
```

Severity varies by language, but the core risk is the same: **the deserializer executes the actions "described in the data" — and you control the data.** It is like treating a "program" as "data," a variant of the `web-02-injection` line "treating input as code."

Defense directions:

* **Never deserialize untrusted input.**
* When unavoidable, **whitelist** the types that may be loaded.
* Replace native object deserialization with safer formats (JSON + explicit validation).

> Both are defense lessons — we only describe the flaws and defenses, not usable payloads. Understanding "where parsing happens" is the first step of defense; applying these techniques to real systems is attack.

## The shared mindset

Put XXE and deserialization together and one line emerges: **wherever "input gets parsed/executed" is the attack surface.**

| Flaw | What gets parsed | Defense slogan |
|---|---|---|
| XXE | User-supplied XML | Disable external entities; do not parse untrusted XML |
| Deserialization | Untrusted serialized data | Never deserialize untrusted input; whitelist types |

The top principle from `web-05-securing-web-apps` holds again: **assume input is untrusted by default.** Any function that "takes data in and turns it into a structure/object" first asks "can this input be trusted?"

## Next

The `websec` series now spans injection, identity, SSRF/CSRF, IDOR, business logic, and XXE/deserialization. Next, flip to the tool side: `kali-13-wireless-tools` introduces wireless-testing tools, and their use in authorized settings.

#### Q: What is the shared root of XXE and insecure deserialization?

* Weak passwords

* The program parses input it should not — XML external entities or untrusted serialized data

* Firewall misconfiguration

* Missing backups

> 💡 Both are 'parsing/executing untrusted input': XXE makes a parser read files, deserialization turns data into objects; assuming input is untrusted is the common defense.
