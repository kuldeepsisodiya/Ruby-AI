<p align="center">
  <img src="assets/banners/ruby_hero_banner.png" alt="Ruby 1.6.2 Hero Banner" width="100%" />
</p>

<p align="center">
  <a href="https://github.com/kuldeepsisodiya/Ruby-AI/releases/download/v1.6.2/Ruby.dmg"><img src="https://img.shields.io/badge/Download-Ruby%201.6.2%20DMG-E02444?style=for-the-badge&logo=apple&logoColor=white" alt="Download DMG"></a>
  <a href="https://github.com/kuldeepsisodiya/Ruby-AI/releases/tag/v1.6.2"><img src="https://img.shields.io/badge/Release-v1.6.2-blue?style=for-the-badge&logo=github&logoColor=white" alt="v1.6.2"></a>
  <img src="https://img.shields.io/badge/macOS-14.0%2B%20Sonoma%20%7C%20Sequoia-000000?style=for-the-badge&logo=apple&logoColor=white" alt="macOS 14+">
  <img src="https://img.shields.io/badge/Architecture-Apple%20Silicon%20(arm64)-0A84FF?style=for-the-badge" alt="Architecture">
  <img src="https://img.shields.io/badge/Security-Encrypted%20Keychain-34C759?style=for-the-badge&logo=apple&logoColor=white" alt="Security">
</p>

---

## 💎 Overview

**Ruby 1.6.2** is a native macOS desktop application engineered for real-time visual question solving and unobtrusive AI-assisted reasoning. Built from the ground up in **Swift & SwiftUI**, Ruby pairs Apple Vision OCR technology with a customizable multi-provider AI engine, hardware-level feedback systems, and an automated answer accuracy validator.

Whether you're running local offline inference via **Apple Silicon Fundamental Models (`fm serve`)**, local LLMs with **Ollama / LM Studio**, or querying high-parameter cloud models (**OpenAI GPT-4o, Claude, Mistral**), Ruby operates invisibly under your fingertips with zero telemetry, zero monthly subscriptions, and end-to-end local security.

---

## ✨ What's New in Ruby 1.6.2

### 🎯 Answer Accuracy, Consistency & Response Quality
* **Single-Source-of-Truth Answer Validation**: Added `AnswerValidator` to cross-check selected multiple-choice option letters against their extracted option text and explanation, automatically reconciling letter-vs-explanation contradictions (`Answer: <Letter> — <Option Text>`) and flagging uncertain or numerically inconsistent results.
* **Parser & Prompt Fixes**:
  * Fixed a regex bug in `ParsedAnswer` where ordinary prose lines (e.g., `"Because..."`, `"Answer: C..."`) were falsely parsed as option letters `B`/`A`.
  * Upgraded `APIRequestFormatter` with Ruby's Master System Prompt across OpenAI Chat Completions, Ollama Generate, and Custom JSON providers, including negative-qualifier awareness (`NOT`, `EXCEPT`, `FALSE`, `INCORRECT`, `LEAST`, `MOST`, `ALL/NONE OF THE ABOVE`).
* **OCR Reliability Improvements**: Added top-to-bottom, left-to-right reading-order sorting, overlapping-line deduplication, structured option extraction, and incomplete-OCR detection in `OCRManager`.
* **Short Answer & Explained Answer Modes**: Added `ResponseDepth` (`Auto` | `Short` | `Explain`) persisted across **General** settings and the **Remote Exam** floating overlay footer.
* **Session & Concurrency Isolation**: Added request/session generation guards in `AnswerService` and `OverlayController` so cancelled, superseded, or cross-preset responses cannot overwrite newer state.

### 🔄 Built-in GitHub Update Checker
* Added `UpdateChecker` and `UpdateSheetView` to check `kuldeepsisodiya/Ruby-AI` for new releases on launch, from **General → Software Update**, or via **Check for Updates...** in the menu bar.
* Displays release version, full **Changelog**, **Never Remind** opt-out, and one-click **Download** of `Ruby.dmg` directly to `~/Downloads` with live progress.

### 🪟 macOS Dock & Window Lifecycle Fixes
* **Fixed Duplicate Dock Icons**: Applied `NSApp.setActivationPolicy` once at launch and coalesced **Hide from Dock** toggles with a clean app relaunch so `Dock.app` never creates duplicate Ruby tiles.
* **Fixed Reopening Closed Window from Dock**: Implemented `applicationShouldHandleReopen(_:hasVisibleWindows:)` in `AppDelegate` and preserved the window instance on close (`orderOut(nil)`), so clicking the Ruby Dock icon reliably reopens the window.

### 🧪 Comprehensive Testing
* Expanded automated suite to **37/37 passing Swift unit tests**, including 14 new accuracy/consistency/regression tests in `AnswerAccuracyAndConsistencyTests` and update-checker parsing/semver tests.

---

## 🚀 Quick Download & Installation

### 1. Download the Installer
Download the verified disk image directly from the [v1.6.2 GitHub Release](https://github.com/kuldeepsisodiya/Ruby-AI/releases/tag/v1.6.2):

| Asset | Version | Architecture | Size | SHA-256 Checksum |
| :--- | :--- | :--- | :--- | :--- |
| [**Ruby.dmg**](https://github.com/kuldeepsisodiya/Ruby-AI/releases/download/v1.6.2/Ruby.dmg) | `1.6.2` | Apple Silicon (`arm64`) | `18.7 MB` | `7b4417642d1454754e202ad8603d1ca6da019a5cb3911dc93cb0225ec08e1916` |

> [!TIP]
> You can verify your download integrity anytime via the terminal:
> ```bash
> shasum -a 256 Ruby.dmg
> ```

---

### 2. Install to Applications
1. Double-click **`Ruby.dmg`** to mount the volume.
2. Drag **`Ruby.app`** into your **`Applications`** directory inside the custom installer platter:

<p align="center">
  <img src="assets/screenshots/dmg_installer_preview.png" alt="Ruby DMG Installer Layout" width="85%" style="border-radius: 12px; box-shadow: 0 12px 36px rgba(0,0,0,0.4);" />
</p>

3. Unmount the disk image when complete.

---

### 3. macOS Gatekeeper (First-Time Launch)
Because Ruby is distributed directly outside the Mac App Store, macOS may show an *"Unidentified Developer"* or *"App is damaged"* prompt on your first launch.

To resolve this instantly, open **Terminal.app** and clear the quarantine flag:
```bash
xattr -cr /Applications/Ruby.app
```
*Alternatively: Right-click (Control-click) `Ruby.app` in Finder and select **Open**.*

---

## ⚡ Global Keyboard Shortcut

| Trigger Shortcut | Action |
| :--- | :--- |
| <kbd>Control</kbd> + <kbd>Option</kbd> + <kbd>Command</kbd> (`⌃⌥⌘`) | **Solve question under cursor** |
| <kbd>Control</kbd> + <kbd>Option</kbd> + <kbd>Command</kbd> + <kbd>Space</kbd> (`⌃⌥⌘Space`) | **Alternative solver trigger** |

Hover your mouse over any question, multiple-choice prompt, or on-screen diagram, tap the shortcut, and Ruby instantly processes the text and communicates the result via your configured mode. Built-in **0.8s debounce protection** prevents accidental duplicate calls.

---

## 🌟 Architectural Highlights

<p align="center">
  <img src="assets/banners/ruby_features_banner.png" alt="Ruby 1.6.2 Architecture and Modes" width="100%" />
</p>

---

## 🖥️ The 5 Solving Modes

<p align="center">
  <img src="assets/screenshots/interface_modes.png" alt="Ruby Modes Interface" width="85%" style="border-radius: 12px; box-shadow: 0 12px 36px rgba(0,0,0,0.4);" />
</p>

Ruby adapts to any test environment through 5 specialized output modes:

### 1. 🕶️ Remote Exam Mode (Stealth HUD)
* **Screen Sharing Immunity**: Uses native macOS `.sharingType = .none`.
* **Zero Recording Footprint**: The floating HUD window is **entirely omitted from screen capture and video calls** (Zoom, Google Meet, Microsoft Teams, Cisco Webex, ScreenCaptureKit, Loom, and OBS).
* **Interactive Follow-up & Pin**: Includes dedicated Pin/Unpin position locking and an inline Search / Ask bar for interactive conversation.

### 2. 🔤 In-Person (CBT) Menu Bar Disguise
* Answers appear seamlessly within the macOS menu bar status area.
* Disguisable as ordinary system icons:
  * 📶 Wi-Fi Status
  * 🔋 Battery Level
  * ☀️ Display Brightness
  * 🛡️ Security Shield
  * 🔢 Discrete Minimalist Numbers / Letters

### 3. 💡 Caps Lock LED Pulse Mode *(Beta)*
* Uses low-level macOS IOKit hardware drivers to blink your physical keyboard's **Caps Lock LED**.
* Pulses the exact count of the correct answer (e.g., 3 pulses = Option C).

### 4. ⌨️ Keyboard Backlight Breathing Mode *(Beta)*
* Modulates the built-in keyboard illumination intensity in subtle rhythmic pulses to indicate the answer index without requiring visual focus.

### 5. ☀️ Screen Brightness Pulse Mode *(Beta)*
* Employs microscopic screen gamma adjustments to silently signal the answer option.

---

## ⚙️ Multi-Provider AI Architecture (BYOK)

<p align="center">
  <img src="assets/screenshots/interface_api_settings.png" alt="Ruby API Settings Interface" width="85%" style="border-radius: 12px; box-shadow: 0 12px 36px rgba(0,0,0,0.4);" />
</p>

Ruby features a multi-preset provider engine with zero lock-in:

* **Preconfigured Presets**:
  * `Fundamental Models (system)` (`http://127.0.0.1:1976/v1` via `fm serve`) — **Default with auto-launcher**
  * `OpenAI GPT-4o Mini`
  * `OpenAI GPT-4o Vision`
  * `Ollama Local (qwen2.5vl:7b)` (`http://localhost:11434/v1`)
  * `Ollama Local (llama3.2-vision)` (`http://localhost:11434/v1`)
  * `LM Studio Local` (`http://localhost:1234/v1`)
  * `Mistral / Ministral-3b`
  * `Custom Proxies / Anthropic Claude / DeepSeek`
* **Real-time Connectivity Testing**: Test endpoints, latency (ms), and response parsing live from the interface before saving.
* **Granular Controls**: Configure custom base URLs, endpoints, authentication schemes (`Bearer`, `X-API-Key`, `Custom Headers`), temperature, and custom JSON key paths.
* **Direct `fm serve` Terminal Launcher**: Automatically prompts and launches `fm serve` in Terminal if the local port is offline.
* **Portable Presets**: Export and import non-sensitive configurations via JSON.

---

## 👤 Dynamic User Account & Profile

<p align="center">
  <img src="assets/screenshots/interface_account.png" alt="Ruby Dynamic User Account and Avatar" width="85%" style="border-radius: 12px; box-shadow: 0 12px 36px rgba(0,0,0,0.4);" />
</p>

* **Native Identity Discovery**: Automatically retrieves the logged-in macOS account's real full name and short username.
* **Apple Account Profile Avatar**: Resolves the user's authentic Apple profile picture and Memoji avatar dynamically across all Mac devices without hardcoding.
* **Unlimited Tokens Status**: Real-time connection feedback showcasing active offline inference status and zero third-party dependencies.

---

## 🔒 Security & Privacy by Design

<p align="center">
  <img src="assets/screenshots/interface_security.png" alt="Ruby Security and Permissions" width="85%" style="border-radius: 12px; box-shadow: 0 12px 36px rgba(0,0,0,0.4);" />
</p>

* **Encrypted macOS Keychain**: Sensitive API tokens and bearer keys are stored directly inside Apple's hardware-backed Keychain (`Security.framework`). Zero plaintext storage.
* **Redacted Logs**: In-app diagnostic logging automatically filters and masks bearer tokens, headers, and secret strings.
* **Sparkle & Trackers Stripped**: All telemetry, tracking scripts, and background update hooks have been entirely eliminated.
* **Direct Permission Verification**: In-app shortcuts to grant and verify macOS **Screen Recording** (for Vision OCR) and **Accessibility** (for global hotkeys).

---

## 🎛️ General Settings & Customization

<p align="center">
  <img src="assets/screenshots/interface_general.png" alt="Ruby General Settings" width="85%" style="border-radius: 12px; box-shadow: 0 12px 36px rgba(0,0,0,0.4);" />
</p>

* **Dual Language Localization**: Full dynamic switching between **English** and **简体中文 (Simplified Chinese)**.
* **Dock Visibility**: Option to hide Ruby completely from the macOS Dock and run quietly from the menu bar.
* **Active Model Status**: Real-time status cards reflecting local Fundamental Models (`system`) readiness and profile state.
* **Custom Hotkeys**: Rebindable trigger sequences with conflict detection.

---

## ❓ Frequently Asked Questions (FAQ)

<details>
<summary><b>1. Why does macOS say the app is damaged upon opening?</b></summary>
<br>
This is standard macOS Gatekeeper behavior for downloaded unsigned applications. Simply execute:

```bash
xattr -cr /Applications/Ruby.app
```
Then launch Ruby normally.
</details>

<details>
<summary><b>2. Can I use Ruby 100% offline without spending API credits?</b></summary>
<br>
Yes! Ruby 1.6.2 defaults to Apple Silicon Foundation Models (<code>model: system</code>) on <code>127.0.0.1:1976</code> via <code>fm serve</code> with unlimited tokens. You can also run <a href="https://ollama.com">Ollama</a> or <a href="https://lmstudio.ai">LM Studio</a> completely offline without an internet connection or external API keys.
</details>

<details>
<summary><b>3. Is Remote Exam Mode detectable on Zoom or Google Meet?</b></summary>
<br>
No. Ruby constructs its floating overlay using macOS AppKit <code>NSWindow.SharingType.none</code>. macOS explicitly prevents the window buffer from being captured by ScreenCaptureKit, Zoom, Teams, Webex, Google Meet, or recording utilities.
</details>

<details>
<summary><b>4. Which macOS versions are supported?</b></summary>
<br>
Ruby 1.6.2 is optimized for <b>macOS 14.0 Sonoma</b> and <b>macOS 15.0+ Sequoia</b> on Apple Silicon (M1, M2, M3, M4 series).
</details>

---

## 👨‍💻 Developer & Contact

<p align="center">
  <img src="assets/screenshots/interface_contact.png" alt="Ruby Contact and Support" width="85%" style="border-radius: 12px; box-shadow: 0 12px 36px rgba(0,0,0,0.4);" />
</p>

* **Developer**: Kuldeep Sisodiya
* **Portfolio & Website**: [https://kuldeep.is-a.dev/](https://kuldeep.is-a.dev/)
* **Contact Email**: [kuldeepsisodiya901@gmail.com](mailto:kuldeepsisodiya901@gmail.com)

---

<p align="center">
  <b>Ruby 1.6.2</b> • Native macOS Engineering • Designed with ❤️ by Kuldeep Sisodiya
</p>
