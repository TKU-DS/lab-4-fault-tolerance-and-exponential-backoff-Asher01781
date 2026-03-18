**🧠Reflection Questions**

- **The Thundering Herd**: Why is it strictly necessary to add random.uniform(0, 0.5) to our sleep time? Imagine a factory power outage where 1,000 Edge devices all reboot at exactly 12:00:00. What would happen if they all retried their failed uploads at the exact same mathematical intervals?

Adding random.uniform(0, 0.5) is necessary to avoid all devices retrying at the same time, preventing congestion or repeated failures.

If 1,000 edge devices reboot simultaneously and retry uploads at identical intervals, they will create a huge spike of requests, overwhelming the server or network (thundering herd problem).

- **DLQ Lifecycle**: Once a payload is safely written to dead_letter_queue.json, what should the system architecture do next? How would you design a background service to eventually deliver this stranded data once the network is stable?

After data is written to dead_letter_queue.json, the system should periodically check the DLQ and retry sending the failed payloads when the network becomes stable.

This service can use retry logic with backoff and jitter to avoid overload. Once a payload is successfully delivered, it should be removed from the DLQ. This ensures no data is permanently lost and the system remains reliable.