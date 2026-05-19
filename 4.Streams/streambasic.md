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



````md
# NOTES 2 — Readable Streams, Backpressure & Pipeline

# Topics Covered

- Readable Streams
- Chunk-Based Reading
- Stream States
- pause/resume
- Backpressure
- pipeline()
- Gigabyte File Processing
- Memory Efficiency

# Readable Streams

Readable streams read data incrementally.

Example:

```js
const stream = fileHandle.createReadStream();
````

Instead of loading entire files into memory.

---

# Data Event

```js
stream.on("data", (chunk) => {
   console.log(chunk);
});
```

Each chunk is:

```text
A Buffer
```

---

# Chunk-Based Reading

Instead of:

```text
Load 10 GB file into RAM
```

Streams do:

```text
64 KB
64 KB
64 KB
64 KB
```

This keeps memory usage low.

---

# Default HighWaterMark

For file readable streams:

```text
64 KB
```

Unlike normal streams:

```text
16 KB
```

Referenced from transcript. 

---

# Backpressure

Most important streams concept.

Definition:

```text
When producer is faster than consumer.
```

Example:

```text
Disk Read Speed > Disk Write Speed
```

Without backpressure handling:

* memory explodes
* process crashes
* system freezes

---

# Dangerous Code

```js
streamRead.on("data", (chunk) => {
   streamWrite.write(chunk);
});
```

This ignores backpressure.

---

# Proper Backpressure Handling

```js
if (!streamWrite.write(chunk)) {
   streamRead.pause();
}

streamWrite.on("drain", () => {
   streamRead.resume();
});
```

This is production-grade flow control.

---

# pause()

Temporarily stops readable stream.

---

# resume()

Continues reading again.

---

# drain Event

Triggered when writable buffer becomes available again.

Critical interview insight:

```text
Never continue writing after write() returns false.
Wait for drain.
```

---

# pipeline()

Best production solution.

```js
pipeline(readStream, writeStream, callback);
```

Advantages:

* automatic cleanup
* automatic error handling
* safer than pipe()

Referenced from:
copy.js

```
```
````md
# NOTES 3 — Custom Streams, Duplex & Transform Streams

# Topics Covered

- Implementing Custom Streams
- _construct
- _read
- _write
- _final
- _destroy
- Duplex Streams
- Transform Streams
- Encryption/Decryption Streams

# Custom Readable Stream

Referenced from:
customReadable.js

```js
class FileReadStream extends Readable
````

Readable streams must implement:

```js
_read(size)
```

---

# _read(size)

Purpose:

```text
Push data into internal stream buffer.
```

Example:

```js
this.push(buffer);
```

---

# End of Stream

```js
this.push(null);
```

Null signals:

```text
End Of Stream
```

---

# Custom Writable Stream

Referenced from:
customWritable.js

Writable streams must implement:

```js
_write(chunk, encoding, callback)
```

---

# callback()

Critical rule:

```text
Always call callback().
```

Otherwise:

* stream hangs
* backpressure breaks
* pipeline freezes

---

# _construct()

Runs:

```text
After constructor
Before stream starts
```

Best place for:

* opening files
* DB connections
* sockets
* async setup

---

# _final()

Runs before stream finishes.

Best for:

* flushing remaining buffers
* cleanup writes

---

# _destroy()

Responsible for:

* cleanup
* closing descriptors
* releasing resources

---

# Duplex Streams

Referenced from:
customDuplex.js

Duplex streams are:

```text
Readable + Writable
```

in the same stream object.

Examples:

* TCP sockets
* WebSockets
* SSH
* HTTP/2 streams

---

# Duplex Architecture

```text
Input  → Writable Side
Output → Readable Side
```

---

# Transform Streams

Transform streams:

```text
Read → Modify → Output
```

Examples:

* compression
* encryption
* parsing
* transcoding

---

# Encryption Stream

Referenced from:
encryption.js

```js
class Encrypt extends Transform
```

---

# _transform()

Main transform method.

```js
_transform(chunk, encoding, callback)
```

Responsibilities:

* receive chunk
* modify chunk
* return transformed chunk

---

# Encryption Logic

```js
chunk[i] = chunk[i] + 1;
```

Simple byte-shifting encryption.

---

# Decryption Logic

Referenced from:
decryption.js

```js
chunk[i] = chunk[i] - 1;
```

Restores original bytes.

---

# Most Important Streams Insight

Backend engineering is fundamentally:

```text
Efficient movement & transformation of bytes.
```

Streams are the abstraction enabling this at scale.

---

# FAANG-Level Interview Questions

## Q1. Why are streams memory efficient?

```text
Because they process data incrementally in chunks instead of loading entire datasets into memory.
```

---

## Q2. What is backpressure?

```text
A flow-control mechanism that prevents writable consumers from being overwhelmed by faster producers.
```

---

## Q3. Difference Between pipe() and pipeline()

| pipe                  | pipeline              |
| --------------------- | --------------------- |
| manual cleanup        | automatic cleanup     |
| weaker error handling | strong error handling |
| lower safety          | production-grade      |

---

## Q4. Why does write() return false?

```text
Because internal writable buffer exceeded highWaterMark.
```

---

# Ultimate FAANG Insight

Streams are one of the most important abstractions in backend engineering because they allow scalable systems to process terabytes of data while maintaining stable memory usage.

```
```
