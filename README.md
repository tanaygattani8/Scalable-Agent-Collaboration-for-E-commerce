## Multi-Agent Fulfillment System

A robust, modular order fulfillment ecosystem powered by the **Google Agent Development Kit (ADK)** and **Gemini 2.5 Flash Lite**. This project moves beyond monolithic automation to create a team of specialized AI agents that collaborate to manage inventory, shipping, database persistence, and real-time customer tracking.


## 🧐 The Problem

Manual order fulfillment in e-commerce is notoriously labor-intensive and error-prone. It typically demands significant human oversight for inventory checks, shipping logistics, and customer tracking.

As order volumes grow, businesses face a critical choice:

1. **Compromise service quality** (delays, lack of updates).
2. **Incur escalating labor costs.**
3. **Invest in rigid monolithic systems** that are difficult to adapt.

 **The Core Challenge:** Coordinating disparate functions—inventory, shipping, and customer communication—seamlessly and intelligently without massive overhead.


## 💡 The Solution

We have designed a **Multi-Agent Fulfillment System** that leverages the Google ADK to automate the entire lifecycle. Instead of a single complex algorithm, we utilize an ecosystem of specialized agents.

**Key Benefits:**

* **Reduced Operational Overhead:** Automates repetitive tasks (inventory checks, data entry), freeing human resources for strategic initiatives.
* **Enhanced Customer Satisfaction:** Provides proactive, real-time tracking updates to build trust.
* **Scalability:** The modular nature allows for easy adaptation to increasing volume or new services without a complete system overhaul.
* **Innovation:** Demonstrates robust **Agent-to-Agent (A2A)** collaboration, laying the groundwork for complex AI-driven business processes.


## ⚙️ Technical Architecture

The system is built on a **Hub-and-Spoke** model centered around a Manager Agent. It utilizes the **A2A Protocol**, where a central orchestrator delegates complex reasoning and tool execution to specialized sub-agents.

### The Manager Agent (`manager_agent`)

* **Model:** Gemini 2.5 Flash Lite
* **Role:** The project manager and central orchestrator.
* **Function:** It utilizes the `LlmAgent` class to interpret user requests and strictly governs the sequential workflow. It does not access the database directly; instead, it uses `FunctionTools` to delegate tasks to specific specialist agents.

### The Sub-Agent Ecosystem

The system delegates tasks to a team of domain-specific agents, each acting as an expert in its domain:

* **Inventory Agent (`agent-inventory`)**
* **Role:** Responsible for checking product availability.
* **Core Tool:** `check_inventory_func` (Checks the MOCK_DB to determine if a specific `item_id` is in stock).


* **Shipping Agent (`agent-shipping`)**
* **Role:** Handles the dispatch of ordered items.
* **Core Tool:** `ship_item_func` (Simulates shipping, generates a unique tracking number, and updates tracking statuses).


* **Database Agent (`agent-db`)**
* **Role:** Persists order details for record-keeping.
* **Core Tool:** `save_order_func` (Stores the `item_id` and associated `tracking_number` in the database).


* **Tracking Agent (`agent-tracking`)**
* **Role:** Provides real-time status updates for shipped orders.
* **Core Tool:** `get_tracking_status_func` (Retrieves the current status from the database using a provided `tracking_number`).

## 🛠️ Implementation Details

### Sequential Workflow

The `manager_agent` is instructed to follow a strict logical sequence to ensure data integrity:

1. **Check Inventory** (via `agent-inventory`)
2. **Ship Item** (via `agent-shipping`)
3. **Save Order** (via `agent-db`)
4. **Get Tracking Status** (via `agent-tracking`)

### Tools & Wrappers

The system uses a unique wrapper pattern. The Manager does not call Python functions directly.

* **Custom Tools:** Each sub-agent owns a `FunctionTool` wrapping a Python function (e.g., `ship_item_func`).
* **Intermediary Tools:** The Manager uses tools like `ship_item_via_agent`. These tools trigger `tool_runner.run_debug` to spin up the specific sub-agent within the session.

### Sessions & Memory

We utilize the `InMemorySessionService` to manage state. This ensures that conversational context—such as the generated `session_id`, `item_id`, and `tracking_number`—is maintained across the multi-step fulfillment process, enabling coherent agent interactions.


## 🚀 Getting Started


### Prerequisites

* Python 3.9+
* Google Cloud Project with Gemini API enabled
* Google Agent Development Kit installed

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/multi-agent-fulfillment.git
cd multi-agent-fulfillment

```

2. **Install dependencies**
```bash
pip install -r requirements.txt
```


3. **Set up Environment Variables**
Create a `.env` file and add your Google API Key:
```bash
GOOGLE_API_KEY=your_api_key_here
```


4. **Run the System**
```bash
python main.py
```

## 📝 Conclusion

The beauty of this system lies in its iterative and collaborative workflow. The `manager_agent` acts as a project manager, coordinating the efforts of its specialized team. It delegates tasks, captures intermediate results (like tracking numbers), and ensures that each stage of the order fulfillment process is completed successfully.

This multi-agent coordination, powered by the **Google ADK**, results in a system that is **modular, reusable, and scalable**.

