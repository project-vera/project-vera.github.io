
# No More Manual Mocks: A Case for Learned Cloud Emulators

<div style="margin-top: -1.2rem; margin-bottom: 1rem; font-size: 0.85rem; color: #6b7280;">
  February 6, 2026
</div>

---

DevOps engineers face a constant dilemma: testing against the real cloud is expensive and slow, but testing locally requires emulators that often lack features or behave differently than the real thing.

Existing emulators (like the popular LocalStack) rely on manual engineering. We observe that this is a Sisyphean task—AWS alone has over 240 services, and human developers struggle to keep up with the constant stream of new APIs and updates. As a result, coverage is often spotty; for example, we found that an existing emulator LocalStack covers only ~11% of the AWS Network Firewall APIs.

In our paper, we propose a paradigm shift: instead of hand-coding mocks, using Large Language Models (LLMs) to **learn** the emulation logic directly from cloud documentation.

### The "Learned Emulator" Approach

Simply asking an LLM to "write a cloud emulator" results in buggy, hallucinated code. To solve this, we introduce a **neuro-symbolic workflow** that constrains the AI using formal abstractions. We view each cloud resource not just as code, but as a formal state machine (SM) and the entire cloud as a hierarchical state machine.

<p align="center">
  <img src="../pics/cloudemu_fig1.png" width="50%">
  <br>
  <em>Figure 1: The grammar for specifying an emulator.</em>
</p>


Our workflow consists of three key stages:

1.  **Documentation Wrangling:** We automatically scrape and index massive cloud documentation (like AWS PDFs), organizing it into resource-specific contexts to overcome LLM context window limits.
2.  **State Machine (SM) Extraction:** Instead of generating raw Python code immediately, we task the LLM with extracting a formal **State Machine specification** from the docs. This captures the resource's states and valid transitions (API calls like `CreateVpc`), enforcing logic and dependencies that unstructured code generation misses.
3.  **Automated Alignment:** To ensure the emulator behaves exactly like the real cloud, we run traces against both our generated emulator and the actual cloud. We detect discrepancies (e.g., an error code mismatch) and feed them back to the LLM to patch the logic automatically.

<p align="center">
  <img src="../pics/cloudemu_design.png-1.png" width="80%">
  <br>
  <em>Figure 2: Our envisioned workflow.</em>
</p>

### Preliminary Results

We built a prototype and the results highlight significant advantages over current manual methods:

*   **Better Coverage:** In our preliminary tests, we achieved complete coverage for the AWS Network Firewall service, compared to just 11% for the state-of-the-art manual emulator.
*   **Accuracy:** By modeling resources as State Machines, we prevent common logic errors. As shown below, our SM-based approach maintains high accuracy during state updates, whereas a direct-to-code (D2C) emulator generation using an LLM often fails completely (0% accuracy) because it loses track of resource dependencies.

<p align="center">
  <img src="../pics/em_accuracy.png-1.png" width="60%">
  <br>
  <em>Figure 3: Accuracy of learned emulators across scenarios.</em>
</p>

We also used our extracted state machines to quantify the complexity of different cloud services, counting the number of state transitions inherent to services like EC2 versus DynamoDB.

<p align="center">
  <img src="../pics/state_machine_cdf.png-1.png" width="60%">
  <br>
  <em>Figure 4: CDF of SM complexity across services.</em>
</p>

### The Future: A "Cloud Gym"

Beyond just testing, we believe this technology opens new doors. A high-fidelity, zero-cost emulator could serve as a **"Cloud Gym"**, a training ground for AI agents to learn how to manage cloud infrastructure and understand the intricacies, without the risk of racking up a massive bill.

By turning static documentation into executable logic, we hope to finally solve the bottleneck of cloud development velocity.

*For more details, check out our full paper in the HotNets '25 proceedings.*

---
<div style="margin: 0.6rem 0; font-size: 0.8rem; line-height: 1.3;">
  <div>
    <strong>Authors:</strong>
    Archit Bhatnagar, Yiming Qiu, Sarah McClure, Sylvia Ratnasamy, Ang Chen
  </div>
  <div>
    <strong>Affiliations:</strong>
    University of Michigan, The University of Hong Kong, UC Berkeley
  </div>
  <div>
    <strong>Paper link:</strong>
    <a href="https://dl.acm.org/doi/10.1145/3772356.3772404" target="_blank" rel="noopener noreferrer">
      https://dl.acm.org/doi/10.1145/3772356.3772404
    </a>
  </div>
  <div>
    <strong>BibTeX:</strong>
  </div>
<pre style="
/* padding: 0.4rem 0.5rem; */
font-size: 0.7rem;
line-height: 1.2;
border-radius: 3px;
overflow-x: auto;">
<code>@inproceedings{bhatnagar2025cloudemu,
  author = {Bhatnagar, Archit and Qiu, Yiming and McClure, Sarah and Ratnasamy, Sylvia and Chen, Ang},
  title = {A Case for Learned Cloud Emulators},
  booktitle = {Proceedings of the 24th ACM Workshop on Hot Topics in Networks},
  year = {2025}
}</code></pre>
</div>
