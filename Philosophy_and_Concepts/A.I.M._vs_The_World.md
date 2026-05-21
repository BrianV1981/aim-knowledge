# A.I.M. vs. The World

## The Failure of "General Purpose" OS Agents
Most current agent frameworks attempt to turn an LLM into a "General Purpose OS". This inevitably fails due to context decay, hallucinatory state management, and the fragility of unstructured agent loops.

## The A.I.M. Philosophy
A.I.M. rejects this. Instead of a loose agent, it is an **Engineering Exoskeleton**.
* **GitOps over Autonomy:** Autonomous file writes lead to repository rot. A.I.M. forces every change through isolated branches and pull requests.
* **LanceDB over SQLite:** Relational databases create scaling bottlenecks in high-density RAG. A.I.M. treats memory as an immutable Parquet-based ROM/RAM layer.
* **Exoskeleton over OS:** LLMs are incapable of long-term state maintenance. A.I.M. enforces stability by wrapping the model in a strict state-machine that ensures repeatability and auditability.
