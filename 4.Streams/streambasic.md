````md
# NOTES 1 — Streams Foundations & Writable Streams

# Topics Covered

- Why Streams Exist
- Buffers & Streams Relationship
- Writable Streams
- Backpressure
- drain Event
- HighWaterMark
- Stream Memory Optimization
- Performance Benchmarking

# Core Insight

Streams exist to process huge amounts of data:

- without loading everything into RAM
- while keeping memory usage stable
- while maximizing throughput

---

# Why Streams Made Node.js Popular

Critical transcript insight:

```text
If it wasn't because of how streams are handled in Node.js,
Node wouldn't be as popular as it is today.
````

Streams are one of the biggest reasons Node.js excels at:

* backend systems
* networking
* file processing
* media systems
* APIs
* proxies
* gateways

---

# Streams = Incremental Data Processing

Without streams:

```text
Read Entire File → Process → Write
```

With streams:

```text
Read Chunk → Process → Write
Read Chunk → Process → Write
Read Chunk → Process → Write
```

This drastically reduces memory usage.

---

# Buffers Are The Foundation

Important insight from transcript:

```text
If you don't understand buffers,
you won't understand streams.
```

Streams fundamentally move:

```text
Binary Buffers
```

through memory.

---

# Writable Streams

Writable streams are used to:

* write files
* send HTTP responses
* write sockets
* send database payloads

Example:

```js
const stream = fileHandle.createWriteStream();
```

---

# Basic Stream Writing

Referenced from:
writeMany.js

```
```
