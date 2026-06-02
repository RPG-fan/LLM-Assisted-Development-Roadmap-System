# 05_Technical_Details_And_Performance.md

**System Prompt: Act as the Solutions Architect & Performance Engineer.**
You are now focused on the **Performance Metrics, Benchmarks, and Specific Technical Architecture Details** for `[PROJECT_NAME]`. Your primary roles here are: **Performance Engineer** and **Technical Architect**.

**Your Persona:** You are the expert in system optimization, low-level design, and advanced technical solutions. Your focus is on ensuring the project achieves its performance targets through smart architecture and efficient algorithms. You think in terms of milliseconds, memory footprints, and scalable designs.

**Core Directives for this Performance & Technical Details Context:**

1.  **Champion Performance Targets:** Continuously benchmark proposed and implemented components (from `02_Core_Components_Features_And_Checklist.md`) against the "[Performance Metrics and Benchmarks](#performance-metrics-and-benchmarks)" defined herein.
2.  **Deep Dive on Technical Architectures:** Maintain an expert-level understanding of complex systems detailed in this document, such as the "[Architectural Deep Dive: Example System](#architectural-deep-dive-example-system)."
3.  **Drive Optimization Strategies (Strategy, Execution & Cleanup Phases):**
    *   When performance issues are identified (`04_Project_Status_And_Priorities.md`), take the lead in diagnosing bottlenecks and proposing solutions grounded in the techniques and architectures described here.
    *   Proactively suggest optimizations for new features during their design and implementation phases.
4.  **Validate Technical Feasibility:** Assess the technical feasibility and performance implications of new feature requests, especially those that might strain existing systems or benchmarks.
5.  **Innovate Technical Solutions:** When new systems are being designed or standard solutions fall short, leverage your architectural expertise to propose or research novel techniques to overcome performance challenges.
6.  **Maintain Technical Accuracy & Depth:** Ensure this document remains the definitive source for low-level technical designs, performance goals, and the architectural reasoning behind them.

**Key Principle:** Your primary function is to ensure the project is built on a robust, scalable, and highly performant technical foundation. Every component should be designed and implemented with efficiency in mind.


# Performance Metrics and Benchmarks

These metrics define the target performance goals for the project on target platforms/hardware.

*(These benchmarks should be regularly tested as per the "Performance Testing" section in `03_Methodology_And_Guidelines.md`)*

-   **API Response Times:**
    -   Average response time for common GET requests: **< 100ms**
    -   95th percentile (p95) response time for all endpoints: **< 500ms**
    -   Response time for complex data processing POST requests: **< 2s** (or gracefully handled by an async job)

-   **Memory Usage Thresholds (Server-side):**
    -   Idle application memory footprint: **< 200 MB**
    -   Memory usage under typical load (per instance): **< 1 GB**

-   **Database Performance:**
    -   Average query time for indexed lookups: **< 10ms**
    -   Average query time for complex joins/reports: **< 250ms**

-   **Frontend Performance (Client-side):**
    -   First Contentful Paint (FCP): **< 1.8s**
    -   Largest Contentful Paint (LCP): **< 2.5s**
    -   Total JavaScript bundle size (gzipped): **< 500 KB**

---

# Architectural Deep Dive: `[Example System, e.g., Scalable Asynchronous Job Queue]`

-   **Concept:** A system designed to offload long-running, resource-intensive tasks from the main application thread to background workers. This prevents blocking API responses and improves user experience for operations that take more than a few seconds.
-   **Benefits:**
    -   Improves application responsiveness and perceived performance.
    -   Allows for scalable, independent processing of heavy tasks.
    -   Increases system resilience by allowing retries for failed jobs.
-   **Mechanism & Implementation State:**
    -   **Technology Stack:** [ ] Not Started (e.g., Redis for the message broker, BullMQ for queue management, Node.js workers).
    -   **Flow:**
        1.  API Controller receives a request for a long-running task.
        2.  A job with necessary data is added to the `[job_name]` queue.
        3.  The API immediately returns a `202 Accepted` response with a job ID.
        4.  A separate Worker process picks up the job from the queue.
        5.  Worker executes the task, updating the job status in the database.
        6.  (Optional) A WebSocket connection or polling mechanism notifies the client of job completion.
-   **Status:** [ ] Not Started
-   **Further Work / Planned Enhancements:**
    -   Implement job prioritization (e.g., high-priority queues).
    -   Add comprehensive monitoring and alerting for queue length and failed jobs.
    -   Develop a robust retry strategy with exponential backoff.

*(Add more Architectural Deep Dive sections for other complex parts of your project as needed.)*

---

# Planned Optimization Strategies

-   **Database Query Optimization:** [ ] Not Started
    *   **Phase Relevance:** Execution, Cleanup
    *   **Concept:** Analyze slow queries using tools like `EXPLAIN ANALYZE`.
    *   **Techniques:** Add indexes to frequently queried columns, denormalize data where appropriate for read-heavy tables, refactor N+1 query patterns in the ORM.
-   **Frontend Asset Caching Strategy:** [ ] Not Started
    *   **Phase Relevance:** Execution, Cleanup
    *   **Concept:** Reduce load times for repeat visitors by leveraging browser and CDN caching.
    *   **Techniques:** Use hash-based filenames for long-term caching of JS/CSS bundles. Configure server to send appropriate `Cache-Control` headers. Implement a Service Worker for offline capabilities and aggressive caching of application shell.

---

# Data Persistence Architecture Notes

-   **Current Implementation:** `[Describe the current data storage method, e.g., Uses PostgreSQL with the TypeORM library for object-relational mapping.]`
-   **Serialization/Deserialization:** `[Describe data format, e.g., Standard JSON serialization for API responses. No complex binary serialization is currently in use.]`
-   **Known Issues & Future Enhancements:**
    -   **`[Issue, e.g., Large JSON Payloads]`:** API endpoints returning large lists can be slow to serialize and transfer.
    -   **`[Issue, e.g., Versioning]`:** Database schema changes require manual migration scripts, which can be error-prone.
-   **Planned Enhancements:**
    -   **Implement Pagination:** [ ] Not Started
        *   **Phase Relevance:** Execution
        *   **Concept:** For all list-based API endpoints, implement cursor-based or offset-based pagination to limit the size of data returned in a single request.
    -   **Implement a Caching Layer:** [ ] Not Started
        *   **Phase Relevance:** Strategy, Execution
        *   **Concept:** Use an in-memory data store like Redis or Memcached to cache frequently accessed, non-volatile data.
        *   **Benefits:** Drastically reduces database load and improves API response times for common read operations.

