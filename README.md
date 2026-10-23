# Cogenity

**Scale your thinking.** Run Claude Code and Codex across your accounts and
machines, from your laptop to Cogenity Cloud.

Cogenity is a command-line tool for people with several Claude Code or Codex
accounts. Pick an account, and Cogenity starts the agent with that login.
This repository holds the installer and the signed release executables, not
Cogenity's TypeScript source or development history. Compiled executables can
still be inspected. [cogenity.sh](https://cogenity.sh) has the product pages,
pricing and the [changelog](https://cogenity.sh/en/changelog).

## Why Cogenity

Claude Code and Codex use one account at a time. Cogenity lets you choose from
several.

- **Your accounts stay signed in.** Pick one when you start a session.
- **Choose an account for each project.** Keep client work and personal
  projects on separate accounts.
- **Check usage across all your accounts.** One command shows usage and reset
  times for every account.

Cogenity starts Claude Code or Codex on your machine without reading your
prompts or relaying the session. It checks account usage and available models
with the provider. Your provider credentials stay on your device.

## Install

Cogenity requires Claude Code 2.1.144 or newer, Codex, or both.

```sh
curl -fsSL https://cogenity.sh/install.sh | sh
```

With Homebrew on macOS:

```sh
brew install --cask cogenityhq/tap/cogenity
```

With mise:

```sh
mise use -g github:cogenityhq/cogenity@latest
```

| Platform | Support |
| --- | --- |
| macOS | 13 or newer. Apple silicon and Intel. Executables are signed and notarized. |
| Linux | Experimental. arm64 or x86-64 with glibc, `libstdc++` and `libgcc`; kernel 3.10 or newer (5.6 recommended); SSE4.2 on x86-64. |
| Windows | Planned. |

The installer checks the release checksum and writes `~/.local/bin/cogenity`.
Set `COGENITY_INSTALL_DIR` to change the destination. On a fresh installation
it adds that directory to your zsh or bash startup file when it is not on
`PATH`; set `COGENITY_NO_MODIFY_PATH=1` to leave the file unchanged. Pipe the
script to `sh -s -- -q` to print only errors, or to `sh -s -- -h` to list the
options. Cogenity needs no separate Bun installation.

## Get started

The install script signs you in and sets up your subscription as it runs. A
quiet install, an install without a terminal, and a Homebrew or mise install
skip that step: run `cogenity upgrade`, or plain `cogenity` in a terminal,
before you add accounts. Ctrl-C keeps the installed binary, and
`cogenity upgrade` resumes setup. Then add each account you want to use, with
your real login addresses. You sign in to each account yourself.

```sh
cogenity enroll claude dev@customer-a.ai
cogenity enroll claude me@personal.ai
cogenity enroll codex dev@customer-a.ai
```

Pick the account for each session:

```sh
cogenity                                   # pick the provider and account
cogenity --claude --account dev@customer-a.ai
cogenity --codex --account dev@customer-a.ai -- exec "summarize this repo"
cogenity status                            # usage and reset times for every account
```

Each launch opens Claude Code or Codex as usual with the selected login. You
can also run `cogenity` and select **Add account** instead of `cogenity enroll`.

## Commands

| Command | What it does |
| --- | --- |
| `cogenity` | Pick a provider and account, or add an account |
| `cogenity --claude`, `cogenity --codex` | Pick an account for one provider |
| `cogenity --account <email>` | Start one enrolled account without the picker |
| `cogenity --model <name>` | Start with this model and skip the model picker |
| `cogenity --yolo` | Start in the provider's unsafe permission mode |
| `cogenity -- <args>` | Pass everything after `--` to the provider |
| `cogenity enroll <tool> <email>` | Add a provider account and run its own login |
| `cogenity remove <tool> <email>` | Log an account out and remove it |
| `cogenity status` | Show enrolled accounts, usage and reset times |
| `cogenity ls` | List the models each account can launch |
| `cogenity doctor` | Check the installation, binaries and credentials |
| `cogenity update` | Install the newest release |
| `cogenity telemetry off` | Stop usage reporting |
| `cogenity skill` | Print the guidance an agent reads before using Cogenity |

Provider permission checks stay on by default. `cogenity --yolo` or `YOLO=1`
passes Claude Code `--dangerously-skip-permissions` or Codex `--yolo` exactly
once. Management commands accept `--json` for agents.

## Where Cogenity is going

The account pool is available now. The rest is planned, with no date yet;
check this repository or the changelog for updates.

| Feature | Status | What it means |
| --- | --- | --- |
| Account pool | Available | Add your Claude Code and Codex accounts once. Your provider credentials stay on your device. |
| Shared MCP servers | Planned | Set up MCP servers once, then use them with your agents locally or in the cloud. |
| Orchestration | Planned | Let agents hand tasks to other agents or models, with the accounts and machines you choose. |
| Cogenity Cloud | Planned | Run agents locally, on remote servers you manage, or in the cloud with Cogenity Cloud. |

## One plan. All your accounts.

**Pro, US$5 per month.** Unlimited Claude Code and Codex accounts. An active
subscription is required to add accounts and run agents. Start a one-month free trial with
code **SINGULARITY** at checkout. Then US$5/month.

- Run `cogenity upgrade` to subscribe.
- Sign in on any machine with `cogenity login`.
- Pick the account for each session.
- Check usage for every account with `cogenity status`.
- Billing and machines at [app.cogenity.sh](https://app.cogenity.sh).
- Provider logins stay on your machine.

```sh
cogenity login      # sign in to your Cogenity account in the browser
cogenity upgrade    # subscribe, or confirm you already have Pro
cogenity dashboard  # open your dashboard
cogenity billing    # manage your subscription
cogenity logout     # sign this machine out
```

An interactive `cogenity` launch signs in when needed, then completes
subscription setup before it starts an agent. Launches without a terminal
never prompt; they require a saved session and an active subscription.
`cogenity upgrade` reports an existing subscription and exits; otherwise it
opens your dashboard and waits for the payment. Ctrl-C stops the wait and
keeps a completed sign-in. Your subscription belongs to your account, not to
one machine: the dashboard lists every machine you signed in from and can sign
one out, and `cogenity logout` signs out the machine you run it on. A machine that loses access locks every
provider account at the next check; help, status, account removal, sign-in and
subscription management stay available.

## Updates

Cogenity checks for a new release once a day without delaying a launch and
only shows a notice. Run `cogenity update` when you are ready. A Homebrew Cask
installation updates through Homebrew after Cogenity verifies its install
receipt, and a mise installation updates through mise; one recorded under
another tool name is refused with the one command to run,
`mise use -g github:cogenityhq/cogenity@latest`. An older Homebrew
Formula installation moves to `~/.local/bin` and removes the Formula only
after the replacement and the shell `PATH` pass their checks. A failed check
waits one day before it retries; `cogenity doctor` shows the saved failure.
Set `COGENITY_UPDATE_CHECK=0` to disable update checks.

## What Cogenity stores

Profiles live under `~/.config/cogenity`. On macOS, Claude Code credentials
stay in the system Keychain; on Linux they stay inside each profile directory.

- `account.json` holds your Cogenity user ID, the address you signed in with
  and the current signed entitlement. It is readable only by your user and
  holds no credential. An older `pro.json` is ignored and can be deleted.
- Your Cogenity refresh credential lives in the macOS Keychain in an item named
  `Cogenity-account-…`; on other systems it is a `.credentials.json` file
  readable only by your user. `cogenity logout` removes it.
- `telemetry.json` holds a random machine ID and your telemetry choice.
  Telemetry reports which command ran, how long it took, whether it succeeded,
  and your OS, CPU architecture, Bun version and Cogenity version. It never
  sends arguments, paths, account names, emails, prompts or tokens. The relay
  gives your IP address to the analytics service, which turns it into an
  approximate location; the address itself never becomes part of the event.
  `COGENITY_TELEMETRY=0`, `DO_NOT_TRACK=1` and a set `CI` also turn it off.
- `update.json` holds release check times, release versions, notice times and
  a safe error category, with no account or telemetry ID.
- The Cogenity service stores your email, plan, paid-through date, signed-in
  machines and Creem customer, checkout, subscription and transaction IDs. It
  stores keyed digests of your credentials, not the raw secrets. Creem stores
  the customer, payment and subscription records.

Cogenity never sends Claude Code or Codex credentials to the Cogenity service,
and no sign-in token from the identity provider ever reaches the CLI. See the
[privacy policy](https://cogenity.sh/en/privacy),
[GDPR information](https://cogenity.sh/en/gdpr-compliance) and
[terms of service](https://cogenity.sh/en/terms).

## Legal

Cogenity by Kenneth Lynne AS. It is not affiliated with, endorsed by, or
supported by Anthropic or OpenAI. You can use several Claude or Codex accounts
if the accounts are yours and you follow each provider's terms. You are
responsible for complying with provider terms, controlling account access,
paying usage charges and for actions taken in unsafe mode. The software is
provided as-is, without warranty. Copyright Kenneth Lynne AS. All rights
reserved. Kenneth Lynne AS permits you to download and run this installer and
the official Cogenity executables; it grants no right to modify or
redistribute the Cogenity code.
Each executable embeds Bun under Bun's own license; see
[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for bundled software terms.

Questions and installation problems: [hello@cogenity.sh](mailto:hello@cogenity.sh).
