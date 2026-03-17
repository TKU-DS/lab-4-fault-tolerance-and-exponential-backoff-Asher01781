# Lab 4: Fault Tolerance & Exponential Backoff

## 📌 Objective
The network at the edge is inherently unreliable. If an Edge device simply crashes every time a cloud API returns a `500 Internal Server Error`, valuable sensor data will be permanently lost. 

This lab introduces **Retry Logic**, **Exponential Backoff with Jitter**, and the architectural concept of a **Dead Letter Queue (DLQ)** to build a highly resilient sender module.

## 🛠️ Environment Setup
1. Launch your **GitHub Codespaces** from this repository.
2. Open the `lab4_fault_tolerance.py` file.

## 🚀 Instructions
1. Review the `cloud_api_mock()` function. Notice that it simulates a server that randomly fails 30% of the time (simulating rate limits or server crashes).
2. Locate `TODO 1` through `TODO 4` inside the `upload_with_backoff()` function.
3. Implement the **Exponential Backoff** algorithm:
   * Calculate the base backoff time: `BASE_DELAY * (2 ** (attempt - 1))`
   * Add a random **Jitter** between 0.0 and 0.5 seconds to prevent the Thundering Herd problem.
   * Pause the program using `time.sleep()`.
4. Implement the **DLQ Fallback**:
   * If the loop finishes without success (Max Retries Reached), route the payload to the `save_to_dlq()` function instead of throwing an exception or dropping the data.
5. Run the script multiple times in your terminal to observe the random failures and backoff behavior:

    ```bash
    python lab4_fault_tolerance.py
    ```

## 🧠 Reflection Questions
1. **The Thundering Herd**: Why is it strictly necessary to add `random.uniform(0, 0.5)` to our sleep time? Imagine a factory power outage where 1,000 Edge devices all reboot at exactly 12:00:00. What would happen if they all retried their failed uploads at the exact same mathematical intervals?
2. **DLQ Lifecycle**: Once a payload is safely written to `dead_letter_queue.json`, what should the system architecture do next? How would you design a background service to eventually deliver this stranded data once the network is stable?

## ✅ Submission
1. Ensure your script correctly implements the backoff delays and properly writes to the DLQ upon complete failure.
2. Commit and push your code to GitHub.
