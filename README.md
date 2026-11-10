### <img width="35" height="32" alt="PCI Monitor Icon" src="https://github.com/user-attachments/assets/9df66119-ce5b-4eb7-a46a-8618df504759" align="absmiddle" style="vertical-align: middle; margin-right: 8px;" /> PCI Monitor

> High-performance, cross-platform hardware diagnostic and telemetry suite built for enterprise Linux and mobile environments.

---

## 🚀 Visuals & Demo

| C++ Native Engine (`.so` Terminal Output) | Kotlin Multiplatform Desktop App UI |
| :---------------------------------------: | :--------------------------------: |
| ![Native Terminal Output](docs/images/_so_output.png) | ![KMP Desktop UI](docs/images/kmp_desktopApp_ubuntu.png) |

> *The C++ binary executes low-level direct system calls to read raw PCIe telemetry, while the Kotlin Multiplatform desktop app instantly streams, parses, and renders those exact hardware metrics into a reactive UI.*

---

## 🏗️ Architectural Overview

`PCI_Monitor` is engineered with a clean, decoupled architecture separating low-level hardware diagnostics from the modern multiplatform user interface.

```text
                 ┌─────────────────────────────────────────┐
                 │           PCI_Monitor UI Layer          │
                 │   (Jetpack Compose & Compose Desktop)   │
                 └────────────────────┬────────────────────┘
                                      │
                                      ▼
                 ┌─────────────────────────────────────────┐
                 │           Shared KMP Module             │
                 │   (Business Logic & State Management)   │
                 └─────────┬─────────────────────┬─────────┘
                           │                     │
      (Desktop / Linux)    │                     │    (Android / NDK)
                           ▼                     ▼
              ┌─────────────────────────┐ ┌─────────────────────────┐
              │        JNA Bridge       │ │       Android JNI       │
              └──────────┬──────────────┘ └──────────┬──────────────┘
                         │                           │
                         └─────────────┬─────────────┘
                                       │
                                       ▼
                 ┌─────────────────────────────────────────┐
                 │         C++ Backend Core Engine         │
                 │   (libpci_diag.so - PCI Diagnostics)    │
                 └─────────────────────────────────────────┘
