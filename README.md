
Performance Summary
Test duration: 26 nseconds under concurrent load

Syscalls observed:

read : 3
write : 1
accept: 2


Observations:

High read and write counts indicate the server is I/O bound.
accept calls reflect number of new client connections.
Mutex contention (futex) may appear under high concurrency, potentially slowing writes.
Load Scenarios
1. Read-heavy Load
State: Many clients performing GET operations simultaneously.
Observation: Minimal mutex contention; server handles multiple reads efficiently.
Fix / Optimization: No major fix needed; consider caching if read volume grows further.
2. Write-heavy Load
State: Many clients performing SET operations.
Observation: Mutex contention increases; writes may slow down.
Fix / Optimization: Use finer-grained locks or lock-free data structures to reduce contention.
3. Mixed Load
State: Reads and writes interleaved.
Observation: Some mutex contention; moderate latency.
Fix / Optimization: Optimize critical sections; minimize lock duration.
4. High Concurrency
State: Many clients connecting simultaneously.
Observation: High accept count; increased futex calls due to thread synchronization.
Fix / Optimization: Consider using a thread pool instead of one thread per client, or asynchronous I/O.
Commands Used
1. Compile the server
gcc kv-server.c -o kv-server -lpthread

2. Run the server
   ./kv-server

3. Run multiple clients manually
   ./kv-client # Terminal 

4. Profile the server with perf
sudo perf stat -e syscalls:sys_enter_read,syscalls:sys_enter_write,syscalls:sys_enter_accept ./kv-server

5. (Optional) View server PID manually
ps aux | grep kv-server

6. Capture screenshot (if in WSL, use Windows + Shift + S)
Linux GUI users:
Linux GUI: PrtScn or gnome-screenshot -a

Screenshot
Performance Screenshot

Replace this image with your actual screenshot showing server interaction and perf stats.

Notes
This report excludes server/client code.
Designed to show system behavior under different loads and bottleneck analysis.
Provides insights for optimizing thread-based key-value store performance.
