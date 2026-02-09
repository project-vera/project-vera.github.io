
# The quest for AI Agents as DevOps.

<div style="margin-top: -1.2rem; margin-bottom: 1rem; font-size: 0.85rem; color: #6b7280;">
  February 6, 2026
</div>

---

Cloud infrastructure is the cornerstone of the modern IT industry, yet managing it remains a surprisingly manual and tedious endeavor. From provisioning resources to debugging failures, DevOps teams are constantly battling complexity.

In our recent paper, **"Cloud Infrastructure Management in the Age of AI Agents,"** my colleagues and I explore a provocative question: *Is it time for AI agents to take the reins?*

We didn't just want to build a demo; we wanted to understand the fundamental friction between AI models and cloud interfaces. We envision a future where Large Language Models (LLMs) empower autonomous agents to handle these complex workflows. But to get there, we first had to break things.

### The Setup: A Battle of Four Agents

To test the current capabilities of AI, we conducted a preliminary study using four different "modalities"—the standard interfaces humans use to manage the cloud. We built four distinct AI agent prototypes to perform tasks on Microsoft Azure:

1.  **SDK Agent:** Uses Python code (imperative programming).
2.  **CLI Agent:** Uses command-line shell scripts (terminal-based).
3.  **Infrastructure-as-Code (IaC) Agent:** Uses Terraform (declarative configuration).
4.  **ClickOps Agent:** Navigates the web portal visually (just like a human clicking buttons).

<p align="center">
  <img src="../pics/four_modals.png-1.png" width="90%">
  <br>
  <em>Figure 1: Four cloud user interaction modalities with simplified code snippets.</em>
</p>

### Insight 1: Speed vs. Visibility (The "Blindness" of Code)

We pitted these agents against each other in three rounds of tasks: Provisioning, Updates, and Monitoring. The results revealed a massive trade-off between execution speed and state awareness.

The **CLI agent** was the speed demon of the group. For provisioning tasks, it was highly efficient, completing tasks in an average of 1.6 steps. Because it operates via text commands, it could often do in one line what took the **ClickOps agent** 30 times as many steps to achieve. The ClickOps agent was excruciatingly slow because it had to wait for page loads and navigate complex menus, just like a human user would.

 However, speed isn't everything. When we moved to **Update tasks** (like changing a VM's disk), the "blindness" of the coding agents became a liability. The CLI and SDK agents struggled because they couldn't easily "see" the current configuration of the cloud. They often failed because they needed to query the state before modifying it, introducing extra steps where errors could creep in.

In contrast, the ClickOps agent actually performed better here (67% success rate vs. 33% for IaC). Why? Because the web portal *visually presented* the existing configuration. The agent could "see" the current state on the screen, reducing hallucinations and errors during modification.

### Insight 2: The "Wrong Tool" Fallacy

One of our most critical findings was that some industry-standard tools are fundamentally ill-suited for AI agents in certain contexts.

Take **Infrastructure-as-Code (IaC)**. It is the gold standard for human DevOps because it is declarative—you describe *what* you want, not *how* to get there. But for an AI agent trying to **Monitor** a system, IaC was a disaster.

When we asked the IaC agent to check the health of a system, it failed significantly (40% success rate). IaC is designed to define infrastructure, not to query real-time telemetry. The agent hallucinated non-existent commands because it was trying to force a square peg into a round hole. Meanwhile, the Web/ClickOps agent excelled at monitoring because it could simply look at the graphs and dashboards already built into the Azure portal.

<p align="center">
  <img src="../pics/radarV3.png-1.png" width="50%">
  <br>
  <em>Figure 2: The radar chart summarizing agent performance across different modalities.</em>
</p>

### The Path Forward: A New Architecture

Our experiments highlight that simply connecting an LLM to a cloud shell isn't enough. We need a more sophisticated architecture that embraces these trade-offs.

We propose a new roadmap for **Agentic Cloud Management** involving three key pillars:

1.  **Multi-Agent Orchestration:** We shouldn't force one agent to do everything. We envision a system that routes tasks to the best "expert." It would dispatch a CLI agent for fast provisioning but switch to a ClickOps agent for visual debugging or monitoring.
2.  **Sandboxed Exploration ("Cloud Gyms"):** Agents need a safe space to fail. We propose separating workflows into "Exploration" and "Exploitation" phases. Agents should test their strategies in a sandboxed "gym"—a simulated or non-production environment—before touching live infrastructure.
3.  **Workflow Memory & Guardrails:** Once an agent figures out a complex task in the sandbox, it shouldn't have to relearn it. It should "cache" that workflow into a memory bank for future use. Furthermore, we need strict "human-in-the-loop" guardrails to prevent costly or dangerous mistakes before they happen.

<p align="center">
  <img src="../pics/future_figv5.png-1.png" width="60%">
  <br>
  <em>Figure 3: Envisioned agentic system architecture and workflow.</em>
</p>

### Conclusion

Cloud management is ripe for automation, but our research shows that the interface matters just as much as the model. By combining the speed of CLI, the stability of IaC, and the visibility of ClickOps, we can build the next generation of autonomous cloud engineers.


*For more details, check out our full paper in the ACM SIGOPS Operating Systems Review.*

---
<div style="margin: 0.6rem 0; font-size: 0.8rem; line-height: 1.3;">
  <div>
    <strong>Authors:</strong>
    Zhenning Yang, Archit Bhatnagar, Yiming Qiu, Tongyuan Miao, Patrick Tser Jern Kon, Yunming Xiao, Yibo Huang, Martin Casado, Ang Chen
  </div>
  <div>
    <strong>Affiliations:</strong>
    University of Michigan, UC Berkeley, Andreessen Horowitz
  </div>
  <div>
    <strong>Paper link:</strong>
    <a href="https://dl.acm.org/doi/abs/10.1145/3759441.3759443" target="_blank" rel="noopener noreferrer">
      https://dl.acm.org/doi/abs/10.1145/3759441.3759443
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
<code>@article{yang2025cloudagent,
    author = {Yang, Zhenning and Bhatnagar, Archit and Qiu, Yiming and Miao, Tongyuan and Tser Jern Kon, Patrick and Xiao, Yunming and Huang, Yibo and Casado, Martin and Chen, Ang},
    title = {Cloud Infrastructure Management in the Age of AI Agents},
    journal = {SIGOPS Oper. Syst. Rev.},
    year = {2025}
}</code></pre>
</div>
