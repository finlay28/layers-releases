# Layers for macOS

### One app. Multiple identities.

Layers helps you use supported Mac applications with separate local profiles, so personal, work and client accounts can stay apart and run side by side.

**[Download Layers 0.1.4](https://github.com/finlay28/layers-releases/releases/download/v0.1.4-5/Layers-0.1.4-5.dmg)** · [All releases](https://github.com/finlay28/layers-releases/releases) · **[Deutsch](README.de.md)**

**macOS 14.0 or later · Apple Silicon and Intel · Developer ID signed and Apple notarized**

This is the official download and documentation repository. Layers is proprietary software; its application source code is not published here.

## Contents

- [What is Layers?](#what-is-layers)
- [Who is it for?](#who-is-it-for)
- [What a Layer actually separates](#what-a-layer-actually-separates)
- [Supported applications](#supported-applications)
- [System requirements](#system-requirements)
- [Installation and your first Layer](#installation-and-your-first-layer)
- [Everyday use](#everyday-use)
- [Privacy, security and local storage](#privacy-security-and-local-storage)
- [Updates and troubleshooting](#updates-and-troubleshooting)
- [Frequently asked questions](#frequently-asked-questions)

## What is Layers?

Many desktop apps open a single default workspace or account. Switching identities can mean signing out, signing back in, reopening the correct context and hoping you have not mixed personal and client work.

Layers adds a local profile manager around applications that offer a compatible way to select their data directory. You create a named profile, give it an icon and accent, and open that identity when you need it. For supported configurations, multiple profiles can run at the same time.

For example:

| Profile | Intended use |
|---|---|
| Claude Personal | Your personal account and work |
| Claude Work | Your professional account |
| Client A Browser | A client's web accounts in a separate browser profile |
| Client B Browser | Another client's independent browser context |
| Editor Project A | Editor settings and extensions dedicated to a project |

Those names are examples. Layers does not create accounts or assign subscriptions: you sign into each application's own account inside the appropriate profile.

## Who is it for?

- **Freelancers and agencies:** keep client identities separate and name launchers clearly.
- **People with personal and work accounts:** open the intended identity without repeatedly signing out.
- **Developers and testers:** maintain separate editor or browser configurations for different projects.
- **Teams using several organisations:** reduce account switching when the relevant desktop application supports separate profiles.

Layers organises the application's local context. It does not bypass account limits, subscription requirements, authentication, two-factor checks or a service provider's terms.

## What a Layer actually separates

Each Layer receives a stable UUID and its own directory under:

```text
~/Library/Application Support/Layers/Profiles/<profile-UUID>/
```

An app-specific adapter tells the vendor application to use that profile's data path. The mechanism depends on the application: it may be a supported environment variable, a user-data-directory argument, or separate editor configuration directories.

The original application retains its normal data location. Existing logins are not automatically imported or copied into a new Layer. A new Layer usually starts with a fresh local context and requires its own login.

### The profiles outlive the manager

Closing Layers leaves the profile folders in place and does not automatically terminate the applications running in them. Current named launchers contain their own permanent start configuration. They can open the corresponding profile directly from Finder, Dock or Spotlight while the Layers manager is closed.

Layers is the place to create, inspect and manage those profiles. It is not a background service that must keep each data directory alive.

### Application profiles, under your macOS account

These are separate application profiles under the same macOS user. Layers does not create virtual machines, containers with a separate operating system, or a security boundary against other software running as that user. It does not promise separate IP addresses, device identities or complete separation of every vendor-managed system resource. Applications may still use macOS Keychain or other operating-system services.

## Supported applications

Compatibility requires a real application-specific adapter. Finding an installed app does not mean it can safely become a Layer.

| Application or family | Profile mechanism | Scope |
|---|---|---|
| Claude Desktop | `CLAUDE_USER_DATA_DIR` and a separately identified profile app | Separate application data path; vendor/Keychain approvals may be required |
| Compatible ChatGPT desktop installations | Dedicated user-data directory and `CODEX_HOME` | Only desktop builds recognised by the shipped adapter |
| Google Chrome, Chromium, Brave | Dedicated `--user-data-dir` | Independent browser profile data |
| Visual Studio Code, Cursor | Dedicated user-data and extensions directories | Separate editor settings and extension storage |
| Compatible Codex installations | `CODEX_HOME` | Depends on the installed vendor build honouring that environment variable |

The current ChatGPT-specific adapter recognises bundle ID `com.openai.codex` with executable `ChatGPT`. This does **not** mean every application distributed under the ChatGPT name is supported. The installed bundle and executable matter.

Applications without a compatible adapter are shown as unsupported. Native WhatsApp, for example, is not currently offered as a supported Layer. Layers does not silently copy a shared account context and describe it as separate.

Third-party updates can change profile behaviour. The app's compatibility indication and the installed vendor version take precedence over an assumption that all releases in a product family behave identically.

## System requirements

| Requirement | Current release |
|---|---|
| macOS | **14.0 or later**; macOS 14 is Sonoma |
| Processor | Universal app for Apple Silicon and Intel Macs |
| Distribution | Developer ID signed and Apple notarized |
| Third-party app | Must be installed in a compatible version and meet its own system requirements |
| Account | Your existing account and any subscription required by the third-party service |

Layers' minimum version does not lower another application's minimum version. A vendor may require a newer operating system or a particular processor even when Layers itself can run on your Mac.

The macOS 14 target is checked by compilation and availability checking for both architectures. Automated runtime tests run on the development Mac; this is not an exhaustive test of every older macOS release and vendor combination. macOS 13 and earlier are not currently supported. Supporting them would require changes to the observation and interface implementation, rather than just changing a version label.

Newer visual effects are enabled only on compatible macOS versions. Older supported systems use the existing material-based appearance.

## Installation and your first Layer

1. Download the current **DMG** from the link above or the [Releases page](https://github.com/finlay28/layers-releases/releases).
2. Open the installer and drag **Layers** into **Applications**.
3. Start Layers from Applications. macOS can still display its normal first-open confirmation for an app downloaded from the internet.
4. Choose whether Layers should live in the Dock or in the menu bar.
5. Open **Apps** and choose a compatible installed application.
6. Select **Add layer**. Choose a name, icon and accent, and optionally create a separately named Dock app.
7. Create the Layer, open it and sign into the intended account inside the vendor application.

For a second account, create a second Layer with a different name. Do not sign out of the original app just to initialise the new Layer.

## Everyday use

### Home, Layers and Apps

- **Home** shows active managed profiles and recent activity.
- **Layers** groups your saved profiles by application, including an explicit Original/Main entry.
- **Apps** discovers installed applications and shows whether the shipped adapters support them.
- **Settings** controls presentation, login startup, update checks, discovery and launcher preferences.

An open window in an ordinary app is not automatically a running generated Layer. For example, Claude Main can be active while Claude Work is closed. The manager distinguishes the original application from the named profiles instead of counting every window as a separate identity.

### Named Dock apps

Named launchers live in `~/Applications/Layers`. You can add one to the Dock or find it through Spotlight. Its embedded start configuration selects the same data folder whether you open it from Layers or directly.

Claude and the supported ChatGPT variant use derived local profile app bundles. Other adapters can use a lightweight launcher that requires the original vendor app to remain installed. Generated third-party profile apps are locally re-signed; notarizing the Layers manager does not automatically notarize every modified vendor bundle.

### Running, quitting and rebuilding

Layers reconciles actual macOS processes and persistent launcher records, including apps started outside the manager. A manager restart should not make a running profile disappear from the overview.

Quitting a profile asks the application to terminate; unsaved work may still require your attention. Closing Layers itself leaves vendor applications running. Rebuilding a launcher replaces the launcher, while retaining its profile data. Running legacy launchers are deferred until they stop, and a failed rebuild keeps the previous launcher available.

### Deletion and backups

Deleting a Layer can permanently remove its local sessions, settings and files. Quit it first and review the confirmation carefully. The original app cannot be deleted through the Layer deletion action.

Keep a backup of important profile data. Removing only the Layers application is different from deleting profile folders, but deleting `~/Library/Application Support/Layers` removes the stored metadata and managed profile data. Do not remove that directory as a routine update step.

## Privacy, security and local storage

| Location | Purpose |
|---|---|
| `~/Library/Application Support/Layers/profiles.json` | Names, IDs, source apps, appearance and launcher references |
| `~/Library/Application Support/Layers/profiles.backup.json` | Recovery copy of profile metadata |
| `~/Library/Application Support/Layers/Profiles/` | Persistent per-profile application data |
| `~/Applications/Layers/` | Generated profile apps and named launchers |

Layers does not inspect or migrate passwords, account cookies, authentication tokens or Keychain contents. Vendor applications can store their own account information inside their configured profile directories and use their own Keychain integration. Their network activity remains governed by their providers.

Layers has no built-in analytics, advertising SDK or Layers cloud account. Update checks and downloads contact GitHub using ordinary network requests, which expose normal connection metadata to the hosting infrastructure. Automatic update checks can be disabled in Settings. A manual update check still makes a network request.

The official Layers app and installer are signed and notarized. Update archives also carry a Sparkle EdDSA signature. Notarization is Apple's malware check, not a guarantee that all software is error-free or that every generated third-party app inherits the manager's approval.

## Updates and troubleshooting

### Updating

Use **Settings → Updates → Check for updates**. The signed update channel and public downloads are hosted on GitHub. Updates replace the manager application while retaining the profile data folders.

Version 0.1.0 was shipped without a configured update channel and requires one manual installation of a current release. Builds with the configured channel can discover later compatible releases. The appcast carries the minimum system version so an older Mac is not deliberately offered an incompatible build.

### Common issues

| Situation | What to check |
|---|---|
| An app is listed but Add layer is unavailable | Discovery and compatibility are separate; there may be no adapter for that app/version |
| A Layer asks you to log in | Fresh profiles do not copy the original app's login |
| macOS asks about Keychain access | Review the vendor app's request; modified profile apps can require approval |
| Nothing is active in a named Layer | Confirm that you opened that Layer rather than the original application |
| A launcher cannot open after the vendor app moved | Restore the compatible source app or rebuild the launcher from Layers |
| A profile app needs rebuilding | Quit it first; keep its persistent data directory |
| An update is not immediately visible | Check your macOS version and allow time for GitHub's public feed cache to refresh |
| Behaviour changed after a vendor update | Include both app versions and the exact steps in a bug report |

## Frequently asked questions

**Can I use different accounts at the same time?**  
That is the purpose of compatible Layer profiles: separate local contexts that can run concurrently. Each service still controls login, subscription rules and account restrictions. Not every third-party release has been tested with two authenticated accounts.

**Do my profiles disappear when Layers is closed?**  
No. The data folders persist, and current named launchers contain their own start configuration. The manager can be reopened later to manage them.

**Does Layers copy my current logged-in account into every new profile?**  
No. It does not migrate existing account credentials. Sign into the account you want inside the new profile.

**Is this a virtual machine or a separate macOS user?**  
No. It is application profile separation within your current macOS account.

**Can I add any application?**  
Only applications with a compatible profile mechanism can become supported Layers. Installed apps without a shipped adapter remain unsupported.

**Can I use Layers offline?**  
The manager stores its profile configuration locally. Whether an opened application works offline depends on that vendor. Update checks and downloads require a connection.

**Does this grant another subscription or bypass service limits?**  
No. You provide the relevant accounts and subscriptions and remain responsible for complying with the service's terms.

**Is the code open source because the downloads are on GitHub?**  
No. This repository distributes releases and documentation. Layers remains proprietary; see [LICENSE](LICENSE).

## Reporting a problem

Use this repository's [Issues page](https://github.com/finlay28/layers-releases/issues). Include your macOS version, Mac processor, Layers version, vendor application/version, steps to reproduce and expected versus actual behaviour.

Do not attach profile data directories, cookies, passwords, tokens or private account screenshots. Report process/launcher behaviour without exposing an account's contents.

## Product and licensing

Layers is a product of **Falah Agency, Finlay Sippel**. Third-party applications and names remain the property of their respective owners; compatibility does not imply endorsement.

Current release: **0.1.4 (5)** · [Release notes](https://github.com/finlay28/layers-releases/releases/tag/v0.1.4-5) · [Update feed](https://raw.githubusercontent.com/finlay28/layers-releases/main/releases/appcast.xml)

© 2026 Layers. All rights reserved.
