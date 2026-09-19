# Arnux

[![Status](https://img.shields.io/badge/status-in%20development-orange)](#project-status)
[![Version](https://img.shields.io/badge/version-0.1.0-blue)](#project-status)
[![Tests](https://img.shields.io/badge/tests-WIP-yellow)](#testing)
[![License](https://img.shields.io/badge/license-MIT-green)](../LICENSE)
[![Browser OS](https://img.shields.io/badge/type-browser%20OS-2563eb)](#about-arnux)
[![Barebones](https://img.shields.io/badge/design-barebones-111827)](#design-goals)
[![Web](https://img.shields.io/badge/interface-web-06b6d4)](#architecture)
[![Contributions](https://img.shields.io/badge/contributions-welcome-brightgreen)](#contributing)

**Arnux is a barebones browser operating system designed to provide a simple, focused desktop-like experience inside a web browser.**

> **Project status:** Arnux is experimental and actively changing. The interface, system behavior, and supported browser features may change before the first stable release.

## About Arnux

Arnux is a lightweight browser OS built around a minimal web-based environment. It provides a small foundation for launching browser applications, organizing simple tools, and experimenting with an operating-system-style interface without requiring a traditional desktop operating system.

Arnux is intentionally barebones. It focuses on the essential experience instead of trying to reproduce every feature of a full operating system.

## Design goals

- **Minimal** — keep the interface and system behavior simple.
- **Fast** — load quickly and avoid unnecessary background work.
- **Browser-based** — run inside a modern web browser.
- **Accessible** — make the core interface easy to understand and use.
- **Extensible** — allow new browser applications and system features to be added over time.
- **Experimental** — provide a foundation for exploring browser-based operating-system concepts.

## Features

Current or planned features may include:

- A simple browser-based desktop or home screen.
- Launchable web applications.
- Basic windows, panels, menus, or navigation controls.
- Lightweight settings and preferences.
- Local browser storage for selected user configuration.
- A modular application structure.
- Keyboard and pointer interaction.
- A low-overhead interface designed for older or limited hardware.

Features may be incomplete while Arnux is in development.

## Preview

Add screenshots or a live demo here when they are available:

```md
![Arnux desktop preview](assets/arnux-preview.png)
```

## Running Arnux

Arnux is intended to run as a web project. Clone the repository and install its dependencies using the package manager defined by the project:

```bash
git clone https://github.com/<your-username>/<your-repository>.git
cd <your-repository>

# Choose the command that matches the project setup
npm install
npm run dev
```

Open the local development URL shown in the terminal, commonly:

```text
http://localhost:3000
```

Replace the placeholder repository URL and commands with the actual Arnux project setup before publishing this README.

## Production build

Create a production build with:

```bash
npm run build
```

Preview the production build locally with:

```bash
npm run preview
```

The exact commands may differ depending on the framework used by Arnux.

## Architecture

Arnux can be organized into a few small layers:

```text
Browser
  ↓
Arnux shell
  ├── Desktop or home screen
  ├── Navigation and system UI
  ├── Application launcher
  ├── Settings and preferences
  └── Browser applications
```

The browser provides the runtime environment. Arnux provides the interface and system-like behavior on top of it.

## Applications

An Arnux application is a browser-based tool that runs inside the Arnux environment. Applications should be small, focused, and designed to work well within the available screen space.

Possible applications include:

- Notes
- File or project viewer
- Calculator
- Terminal-style interface
- Settings
- Text editor
- System information
- Simple games
- Web shortcuts

Application APIs and packaging rules will be documented as the project develops.

## Browser support

Arnux is intended for modern browsers with support for standard web platform features such as:

- JavaScript modules
- CSS layout and modern styling
- Local storage or IndexedDB where required
- Pointer and keyboard events
- Service workers, if offline support is implemented

The browser support matrix should be updated after compatibility testing is added.

## Privacy and storage

Arnux should keep local preferences and application data in browser-managed storage whenever possible. It should not collect personal information without clear notice and user consent.

Do not store secrets, passwords, tokens, or sensitive personal information in client-side storage unless the project explicitly provides a secure design for doing so.

## Development

Install dependencies and start the development server:

```bash
npm install
npm run dev
```

Run formatting, linting, and tests when those scripts are configured:

```bash
npm run lint
npm run format:check
npm test
```

Keep the interface lightweight and avoid adding dependencies that do not support Arnux's core browser OS goals.

## Testing

Tests should cover both the interface and the core browser behavior. Useful test areas include:

- Application launching and closing.
- Navigation and keyboard controls.
- Settings and preference persistence.
- Responsive layout behavior.
- Offline or reload behavior, if supported.
- Unsupported browser features.
- Invalid application configuration.
- Accessibility basics, including keyboard navigation and readable contrast.

The tests badge is currently marked **WIP** until continuous integration and coverage reporting are configured.

## Project status

Arnux is currently a barebones prototype. Planned milestones include:

- [ ] Establish the core browser shell.
- [ ] Add a simple desktop or home screen.
- [ ] Create the first built-in applications.
- [ ] Add settings and local preferences.
- [ ] Define an application interface.
- [ ] Improve keyboard and accessibility support.
- [ ] Add responsive layouts for different screen sizes.
- [ ] Add automated tests and continuous integration.
- [ ] Publish a live demo or release build.

## Roadmap ideas

Future versions may explore:

- Offline-first behavior.
- Installable Progressive Web App support.
- Application permissions.
- Themes and appearance settings.
- User profiles stored locally in the browser.
- Drag-and-drop windows.
- A lightweight application marketplace or package format.
- Optional synchronization between devices.

These ideas are not promises and may change as the project develops.

## Contributing

Contributions are welcome. Before making a large change, open an issue to discuss the proposed feature or design. Keep pull requests focused, preserve Arnux's minimal design, test user-facing behavior, and update documentation when the interface changes.

Please avoid adding unnecessary complexity. A feature should support the browser OS experience without turning Arnux into a full desktop operating system.

## License

Arnux is intended to be released under the [MIT License](../LICENSE). If this README is moved into a standalone repository, update the license link to point to that repository's `LICENSE` file.

## Support

Use GitHub Issues for bug reports and feature requests. Include the browser name and version, operating system, screen size, reproduction steps, and relevant console output with private information removed.

<!--
Badge customization:
- Replace <your-username>/<your-repository> with the actual repository owner and name.
- Replace static status and test badges with live CI badges when CI is configured.
- Add screenshots or a live demo link when they become available.
-->
