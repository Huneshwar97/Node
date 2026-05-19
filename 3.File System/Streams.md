````md
# Node.js File System (FS) — FAANG-Level Backend Engineering Notes

---

# 1. What Is a File?

A file is:

```text
A sequence of bits whose meaning is defined by interpretation.
````

Important insight:

```text
Files themselves have no meaning.
Meaning comes from decoders.
```

Examples:

| File Type | Interpretation     |
| --------- | ------------------ |
| `.txt`    | Characters         |
| `.png`    | Pixels/colors      |
| `.mp4`    | Video/audio frames |
| `.exe`    | CPU instructions   |

---

# 2. Everything in OS Is a File

Critical systems concept:

```text
Everything in Unix-like systems is represented as files.
```

Examples:

* applications
* binaries
* directories
* sockets
* pipes
* devices
* logs
* configs

Even commands like:

```bash
ls
mkdir
echo
```

are executable files.

---

# 3. Node.js and Filesystem Architecture

High-level architecture:

```text
Node.js Process
      ↓
Libuv
      ↓
System Calls
      ↓
Operating System
      ↓
Hard Drive / SSD
```

---

# 4. Node.js Never Directly Touches Hardware

Very important FAANG interview insight:

```text
Node.js does NOT directly access the hard drive.
```

Instead:

* Node uses Libuv
* Libuv performs system calls
* OS handles hardware operations

---

# 5. Important System Calls

Examples:

| System Call | Purpose     |
| ----------- | ----------- |
| open        | Open file   |
| read        | Read file   |
| write       | Write file  |
| rename      | Rename file |
| unlink      | Delete file |

---

# 6. FS Module

Core module:

```js
const fs = require("fs");
```

Purpose:

* create files
* read files
* append files
* rename files
* delete files
* watch files/directories

---

# 7. Reading a File

Example:

```js
const fs = require("fs");

const content = fs.readFileSync("text.txt");

console.log(content);
```

Output:

```text
<Buffer 48 65 6c 6c 6f>
```

Because files are:

```text
Raw binary data
```

---

# 8. Files Are Read as Buffers

Important concept:

```text
Node reads files as binary buffers first.
```

Because:

```text
Computers only understand bits.
```

---

# 9. Converting Buffer → Human Readable Text

Using decoder:

```js
content.toString("utf8");
```

UTF-8 decoder converts:

```text
Binary bytes → Characters
```

---

# 10. Decoder vs Encoder

## Decoder

```text
Bits → Meaningful data
```

Examples:

* UTF-8 decoder
* image decoder
* video decoder

---

## Encoder

```text
Meaningful data → Bits
```

---

# 11. Three FS APIs in Node.js

Node provides:

| API          | Type     |
| ------------ | -------- |
| Promise API  | Async    |
| Callback API | Async    |
| Sync API     | Blocking |

---

# 12. Promise API (Recommended)

Example:

```js
const fs = require("fs/promises");

await fs.copyFile("a.txt", "b.txt");
```

Advantages:

* clean
* modern
* readable
* async/await support

Recommended for:

```text
90%+ real-world applications
```

---

# 13. Callback API

Example:

```js
const fs = require("fs");

fs.copyFile("a.txt", "b.txt", (err) => {
   if (err) console.log(err);
});
```

Important Node.js pattern:

```text
Error-first callbacks
```

Pattern:

```js
(err, data) => {}
```

---

# 14. Synchronous API

Example:

```js
fs.copyFileSync("a.txt", "b.txt");
```

Danger:

```text
Blocks the main thread.
```

---

# 15. Why Sync APIs Are Dangerous

Suppose:

```text
User uploads 1 GB file
```

If using sync API:

```text
Entire Node.js process freezes.
```

No requests handled.

No concurrency.

Bad scalability.

---

# 16. Event Loop Blocking

Critical backend concept:

```text
Synchronous filesystem operations block the event loop.
```

Meaning:

* no incoming requests handled
* server becomes unresponsive
* terrible scalability

---

# 17. When Sync APIs Are Acceptable

Good use case:

```text
Loading configuration during app startup
```

Because:

* executed once
* before traffic begins

---

# 18. Why Async FS APIs Matter

Async APIs allow:

* concurrency
* non-blocking IO
* scalable servers
* multiple uploads/downloads simultaneously

---

# 19. File Watching

Node can monitor filesystem changes.

Example:

```js
const fs = require("fs/promises");

const watcher = fs.watch("./command.txt");
```

---

# 20. Async Iterators

Watcher returns:

```text
Async Iterator
```

Used with:

```js
for await (const event of watcher) {
   console.log(event);
}
```

---

# 21. Important Watch Events

| Event  | Meaning                      |
| ------ | ---------------------------- |
| change | File contents changed        |
| rename | File renamed/deleted/created |

---

# 22. File Handles

Opening a file:

```js
const fileHandle = await fs.open("command.txt", "r");
```

This DOES NOT read file contents.

It returns:

```text
File descriptor wrapper
```

---

# 23. File Descriptor

A file descriptor is:

```text
OS-level numeric identifier for an opened file.
```

Think of it as:

```text
OS-level file ID.
```

---

# 24. Important Production Rule

Always close files.

```js
await fileHandle.close();
```

Otherwise:

```text
Memory/resource leaks
```

---

# 25. Reading File Using File Handle

Example:

```js
await fileHandle.read(buffer, offset, length, position);
```

---

# 26. Important Read Parameters

| Parameter | Meaning              |
| --------- | -------------------- |
| buffer    | destination memory   |
| offset    | where filling starts |
| length    | bytes to read        |
| position  | where reading starts |

---

# 27. Reading Is Stateful

Critical insight:

```text
File reads advance internal cursor position.
```

Without resetting position:

```text
Future reads may return empty data.
```

---

# 28. File Stats

Using:

```js
await fileHandle.stat();
```

Returns metadata:

* size
* timestamps
* permissions
* inode info

---

# 29. Dynamic Buffer Allocation

Bad:

```js
Buffer.alloc(100000)
```

Good:

```js
const size = stats.size;
const buffer = Buffer.alloc(size);
```

---

# 30. Efficient File Reading Flow

Production-grade flow:

```text
Open file
   ↓
Get file size
   ↓
Allocate exact buffer
   ↓
Read bytes
   ↓
Decode bytes
   ↓
Close file
```

---

# 31. Decoding File Content

```js
const command = buffer.toString("utf8");
```

Now binary becomes:

```text
Human-readable string
```

---

# 32. Event-Driven Architecture

Very important backend principle:

```text
Filesystem changes trigger events.
```

This is:

```text
Event-driven programming
```

---

# 33. FileHandle Is an EventEmitter

Important Node.js insight:

```text
FileHandle inherits from EventEmitter.
```

Meaning:

```js
fileHandle.on(...)
fileHandle.emit(...)
```

can be used.

---

# 34. Absolute vs Relative Paths

## Absolute Path

Starts from root:

```text
/Users/test/file.txt
```

---

## Relative Path

Starts from working directory:

```text
./file.txt
```

---

# 35. Libuv Threadpool

Critical Node.js architecture insight:

```text
Filesystem operations use Libuv threadpool.
```

Heavy FS work can:

```text
Exhaust threadpool
```

causing:

* slow IO
* request latency
* degraded throughput

---

# 36. FAANG-Level Interview Questions

## Q1. Why Are Async FS APIs Preferred?

```text
Because synchronous filesystem operations block the event loop and prevent Node.js from handling concurrent requests efficiently.
```

---

## Q2. What Happens When Node Reads a File?

```text
Node.js delegates filesystem operations to Libuv, which performs operating system system calls like open/read and returns binary data as buffers.
```

---

## Q3. Why Does fs.readFile Return Buffer?

```text
Because files fundamentally contain binary data.
```

---

## Q4. What Is a File Descriptor?

```text
OS-level numeric identifier for an opened file.
```

---

## Q5. Why Must Files Be Closed?

To avoid:

* memory leaks
* descriptor exhaustion
* resource starvation

---

# 37. Ultimate FAANG Insight

Backend engineering is fundamentally:

```text
Efficient movement of bytes between:
RAM ↔ CPU ↔ Disk ↔ Network
```

The FS module is one of the core abstractions enabling this in Node.js.

---

# 38. One-Line Interview Summary

```text
Node.js filesystem operations are asynchronous Libuv-powered abstractions over operating system system calls that efficiently manipulate binary data using buffers and event-driven IO.
```

```
```
