Issue Summary

Duration of the Outage: June 15, 2024, 14:00 - 18:30 UTC

Impact: Our primary web application experienced significant slowdown, with page load times increasing from an average of 2 seconds to over 30 seconds. Approximately 85% of our users were affected, leading to widespread user complaints and a 40% drop in active user sessions during the outage period.

Root Cause: The root cause was an unoptimized database query that caused a deadlock situation under heavy load, leading to a cascading failure in the web application.

---

Timeline

- 14:00 - Issue detected through automated monitoring alerts indicating elevated response times.
- 14:05 - Engineering team alerted and began investigating the web server logs and performance metrics.
- 14:20 - Initial assumption was a DDoS attack due to the sudden spike in traffic.
- 14:45 - Network team checked firewall and traffic logs; no evidence of DDoS found.
- 15:10 - Issue escalated to the database team after noticing slow database query response times.
- 15:30 - Database team began analyzing slow query logs and identified a specific query causing deadlocks.
- 16:00 - Attempted to optimize the query, but performance did not significantly improve.
- 16:30 - Misleading path: Restarted database server hoping to clear any locks, but issue persisted.
- 17:00 - Escalated to senior database architect for deeper analysis.
- 17:45 - Architect identified a missing index and suboptimal query structure as the root cause.
- 18:00 - Applied necessary index and rewrote the problematic query.
- 18:15 - Performance began to normalize.
- 18:30 - Monitoring confirmed system stability and normal load times restored.

---

Root Cause and Resolution

The root cause of the outage was a poorly optimized database query that, under heavy user load, resulted in a deadlock scenario. This query was part of a recent update intended to improve data retrieval speed but was not adequately tested under high-load conditions.

Detailed Explanation:
- The specific query involved multiple JOIN operations without appropriate indexing, leading to full table scans.
- Under heavy load, these scans caused database locks, preventing other queries from executing efficiently.
- This situation spiraled into a deadlock, where multiple processes were waiting indefinitely for resources held by each other.

Resolution:
- The database architect identified the absence of a crucial index on one of the columns frequently used in the JOIN operations.
- An index was created on this column, significantly reducing the time required for the query.
- The query was also rewritten to minimize the complexity and optimize the execution plan.

---

Corrective and Preventative Measures

Improvements/Fixes:
1. Database Query Optimization: Ensure all new queries undergo thorough performance testing under simulated load conditions.
2. Index Monitoring: Implement a system to automatically suggest and monitor necessary indexes for frequently executed queries.
3. Monitoring Enhancements: Enhance monitoring to include alerts for query performance and potential deadlock scenarios.

Tasks:
1. Audit and Optimize Existing Queries: Review all existing queries for potential optimization and indexing needs.
2. Load Testing: Integrate load testing into the CI/CD pipeline to identify performance issues before deployment.
3. Monitoring Tools: Upgrade monitoring tools to include detailed query performance metrics and deadlock detection.
4. Training: Conduct a training session for developers on efficient query writing and the importance of indexing.
5. Documentation: Update documentation to include best practices for database interactions and query optimizations.

Through the  implementation of these measures, we aim to prevent similar issues in the future and ensure our web application remains robust and responsive under all conditions.

