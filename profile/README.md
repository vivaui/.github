<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/vivaui/viva/main/website/src/assets/viva-lockup-dark.svg">
  <img src="https://raw.githubusercontent.com/vivaui/viva/main/website/src/assets/viva-lockup.svg" alt="Viva" width="280">
</picture>

### Native desktop applications, written in Go.

Cocoa + Metal on macOS · Win32 + Direct3D 11 on Windows · X11 + Vulkan on Linux.  
No WebView, no browser engine, no bundled runtime.

[![Latest release](https://img.shields.io/github/v/release/vivaui/viva?style=flat-square&label=release&color=D7263D)](https://github.com/vivaui/viva/releases) [![Go version](https://img.shields.io/github/go-mod/go-version/vivaui/viva?style=flat-square&label=Go&logo=go&logoColor=white&color=00ADD8)](https://vivaui.dev/en/docs/getting-started/) [![License](https://img.shields.io/github/license/vivaui/viva?style=flat-square&label=license&color=D7263D)](https://github.com/vivaui/viva/blob/main/LICENSE) [![Platforms](https://img.shields.io/badge/platforms-macOS%20%7C%20Windows%20%7C%20Linux-57606a?style=flat-square)](#where-it-runs)

[Website](https://vivaui.dev/en/) · [Documentation](https://vivaui.dev/en/docs/getting-started/) · [Components](https://vivaui.dev/en/components/) · [Showcase](https://vivaui.dev/en/showcase/) · [中文](https://vivaui.dev/zh/)

</div>

---

Viva is a Go framework for native desktop applications. It draws windows, text and components straight through each operating system's own APIs and the GPU, so an application links the system frameworks and the Go runtime — nothing else. An idle application does no work at all.

<p align="center">
  <b>1.6 MiB</b> stripped window &nbsp;·&nbsp; <b>0</b> GPU submissions while idle &nbsp;·&nbsp; <b>0.0%</b> idle CPU<br>
  <sub>Measured on an M0 Mac. Every number comes from a report in the repository — <a href="https://vivaui.dev/en/docs/performance/">rerun them yourself</a>.</sub>
</p>

## What it is built around

<table>
<tr>
<td width="50%" valign="top">
<b>Native, not a browser</b><br>
<sub>macOS calls Cocoa and Metal, Windows calls Win32 and Direct3D 11, Linux calls XCB and Vulkan from the system's own libraries. No WebView, no bundled renderer.</sub>
</td>
<td width="50%" valign="top">
<b>Idle means idle</b><br>
<sub>An unchanged frame does no layout, rebuilds no scene and submits nothing to the GPU. No background timers, no polling loops.</sub>
</td>
</tr>
<tr>
<td width="50%" valign="top">
<b>Components you own</b><br>
<sub><code>viva add</code> copies a component's Go source into your project. No component registry and no stylesheet parser at runtime — just Go you can edit.</sub>
</td>
<td width="50%" valign="top">
<b>Testable without a window</b><br>
<sub>The headless driver runs the real painter, input router and focus rules from explicit calls, so tests need no desktop session.</sub>
</td>
</tr>
<tr>
<td width="50%" valign="top">
<b>One API, three native backends</b><br>
<sub>The same components, input, text and accessibility semantics on macOS, Windows and Linux.</sub>
</td>
<td width="50%" valign="top">
<b>Measured, reproducibly</b><br>
<sub>Binary size, idle cost and scene timings are quoted from benchmark reports on named machines and builds.</sub>
</td>
</tr>
</table>

## Quick start

```sh
# macOS / Linux
brew install vivaui/tap/viva

# Windows
scoop bucket add vivaui https://github.com/vivaui/homebrew-tap
scoop install viva

viva doctor       # is this machine ready?
viva init hello   # a runnable project, with a window and tests
cd hello && viva dev .   # hot reload: save a file, the window keeps its state
```

<sub>Prebuilt archives with <code>SHA256SUMS</code> are on the <a href="https://github.com/vivaui/viva/releases">releases page</a>. Go 1.26 or newer; <code>viva</code> downloads the toolchain once if you have none.</sub>

## A window in Go

```go
package main

import (
	"fmt"
	"log"

	"github.com/vivaui/viva/app"
	"github.com/vivaui/viva/ui"
)

type model struct {
	clicks int
}

// page declares the window's content. It runs on every repaint and reads the
// model; a click changes the model, and the repaint that follows shows it.
func (m *model) page(c ui.C) {
	ui.Column(c).Fill().Pad(24).Gap(12).Children(func() {
		if ui.Button(c, fmt.Sprintf("Clicked %d times", m.clicks)).Clicked() {
			m.clicks++
		}
	})
}

func main() {
	m := &model{}
	if err := ui.Run(app.Config{Title: "Hello", Width: 480, Height: 240}, m.page); err != nil {
		log.Fatal(err)
	}
}
```

The state is yours, the layout is a function of it, and there are no coordinates to keep in sync. When nothing changes, nothing runs. [Read the guide →](https://vivaui.dev/en/docs/getting-started/)

## Built with Viva

<table>
<tr>
<td width="50%">
<a href="https://motty.page"><img src="https://raw.githubusercontent.com/vivaui/viva/main/website/src/assets/showcase/motty-hero.webp" alt="Motty: a file panel, an SSH terminal and an AI assistant side by side" width="100%"></a>
</td>
<td width="50%">
<a href="https://github.com/talongao/tinglan"><img src="https://raw.githubusercontent.com/vivaui/viva/main/website/src/assets/showcase/tinglan-hero.webp" alt="Tinglan: album art beside Cantonese lyrics with transliteration" width="100%"></a>
</td>
</tr>
<tr>
<td align="center" valign="top">
<b>Motty</b> — a native SSH workspace<br>
<sub>Terminal, SFTP, port forwarding, remote desktops and an AI assistant in one window.<br>macOS · Windows · Linux</sub>
</td>
<td align="center" valign="top">
<b>Tinglan</b> — an unofficial Huawei Music client<br>
<sub>Library, playlists and lyrics in a native window, with twelve themes.<br>macOS</sub>
</td>
</tr>
</table>

## Repositories

| Repository | What it is |
| :--- | :--- |
| [**viva**](https://github.com/vivaui/viva) | The framework, its components and the `viva` command. |
| [**awesome-viva**](https://github.com/vivaui/awesome-viva) | A curated list of applications, tools and resources. |
| [**homebrew-tap**](https://github.com/vivaui/homebrew-tap) | The Homebrew formula and Scoop manifest for `viva`. |

## Where it runs

macOS (Cocoa · Metal · CoreText) · Windows (Win32 · Direct3D 11 · DirectWrite) · Linux (X11 · Vulkan · Pango/Cairo) — and an optional WebAssembly target that previews the same components in a browser without touching the desktop runtime.

[Platform details](https://vivaui.dev/en/docs/platforms/) · [Performance](https://vivaui.dev/en/docs/performance/)

---

<p align="center">
  <sub>Viva is Apache-2.0 licensed. Screenshots belong to their respective applications.</sub>
</p>
