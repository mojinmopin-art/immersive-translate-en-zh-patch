# Immersive Translate - 沉浸式翻译增强补丁

[![Official Site](https://img.shields.io/badge/Official_Site-chenni--translate.com-brightgreen)](https://chenni-translate.com)
[![Version](https://img.shields.io/badge/Version-2026.4-blue)]()

Enhanced patch for Immersive Translate browser extension. Fixes API connection issues, adds custom endpoint support, and improves translation stability for users in mainland China and East Asia.

## Problem Description

Recent versions of Immersive Translate have several known issues:

- **API connection timeout** when using DeepL or Google Translate from certain regions
- **Translation stops working** after Chrome 120+ update (Manifest V3 migration)
- **Custom API endpoint** configuration removed in official closed-source version
- **Bilingual display** formatting broken on specific websites (Twitter/X, Reddit)

## What This Patch Fixes

| Issue | Status | Details |
|-------|--------|---------|
| API timeout in China/Asia | ✅ Fixed | Adds fallback relay endpoints |
| Chrome 120+ MV3 compatibility | ✅ Fixed | Service Worker lifecycle handling |
| Custom API endpoint support | ✅ Restored | Re-enables developer settings panel |
| Bilingual layout on Twitter/X | ✅ Fixed | Updated CSS injection rules |
| PDF translation crash | ✅ Fixed | Memory management for large documents |

## Browser Compatibility

| Browser | Version | Status |
|---------|---------|--------|
| Chrome | 120-126 | ✅ Tested |
| Edge | 119+ | ✅ Tested |
| Firefox | 121+ | ✅ Tested |
| Safari | 17+ | ⚠️ Partial (no PDF support) |
| Arc | Latest | ✅ Compatible |

## Installation

### Method 1: Online Diagnostic (Recommended)
1. Visit [chenni-translate.com](https://chenni-translate.com) — the diagnostic tool will detect your browser and system
2. Follow the recommended patch version for your environment
3. Download and install the patched extension

### Method 2: Manual Installation
1. Download the latest `.crx` file from [Releases](../../releases)
2. Open `chrome://extensions/` → Enable Developer Mode
3. Drag and drop the `.crx` file to install

## Technical Background

### Why does the API timeout occur?

Immersive Translate uses direct connections to translation API providers (DeepL, Google, OpenAI). In certain network environments, these connections are unreliable due to:

1. **DNS resolution failures**: Default DNS cannot resolve API endpoints consistently
2. **TCP connection reset**: Intermediate network devices interrupt long-lived HTTPS connections
3. **TLS fingerprint mismatch**: Some network inspection tools interfere with the TLS handshake

Our patch addresses these by:
- Adding DNS-over-HTTPS (DoH) resolution for API endpoints
- Implementing automatic retry with exponential backoff
- Providing configurable relay endpoints as fallback

### Manifest V3 Migration Issues

Chrome's migration from Manifest V2 to V3 fundamentally changed how extensions work:
- Background pages → Service Workers (with limited lifecycle)
- `chrome.webRequest.onBeforeRequest` blocking → `declarativeNetRequest` rules
- Persistent storage access patterns changed

This patch ensures the translation engine works correctly under the new Service Worker lifecycle.

## Frequently Asked Questions

**Q: Will this patch conflict with the official Immersive Translate extension?**
A: Yes, you should uninstall the official version before installing the patch. They cannot run simultaneously.

**Q: Is my translation history preserved?**
A: The patch uses the same IndexedDB schema. Your history and settings will be preserved after switching.

**Q: How do I update when a new patch version is released?**
A: Check [chenni-translate.com](https://chenni-translate.com) periodically, or watch this repository for release notifications.

**Q: Can I use my own API keys?**
A: Yes. The patch restores the developer settings panel where you can configure custom DeepL, OpenAI, or Google Translate API keys.

## Contributing

Found a new compatibility issue? Please open an [Issue](../../issues) with:
- Browser and version (`chrome://version/`)
- Operating system
- Translation provider being used
- Error messages from DevTools console (F12 → Console tab)

---
*Full technical documentation, diagnostic tools, and FAQ at [chenni-translate.com](https://chenni-translate.com)*
