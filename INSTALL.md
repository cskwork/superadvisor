# Install superadvisor

<details>
<summary><strong>Claude Code</strong></summary>

### Install

```bash
claude plugin marketplace add cskwork/superadvisor
claude plugin install superadvisor@superadvisor
```

Type `/superadvisor`.

### Verify

```bash
claude plugin list
```

### Update

```bash
claude plugin marketplace update superadvisor
```

### Uninstall

```bash
claude plugin uninstall superadvisor
claude plugin marketplace remove superadvisor
```

</details>

<details>
<summary><strong>Codex</strong></summary>

### Install

```bash
codex plugin marketplace add cskwork/superadvisor --ref main
codex plugin add superadvisor@superadvisor
```

Type `$superadvisor`.

### Verify

```bash
codex plugin list
```

### Uninstall

```bash
codex plugin remove superadvisor
codex plugin marketplace remove superadvisor
```

</details>

<details>
<summary><strong>Gemini CLI</strong></summary>

### Install (extension, always-on)

```bash
gemini extensions install https://github.com/cskwork/superadvisor
```

### Install (command, opt-in)

```bash
mkdir -p ~/.gemini/commands
curl -fsSL https://raw.githubusercontent.com/cskwork/superadvisor/main/skills/superadvisor/agents/gemini.toml \
  -o ~/.gemini/commands/superadvisor.toml
```

Type `/superadvisor` in a new session.

### Verify

```bash
gemini extensions list
```

### Uninstall

```bash
gemini extensions uninstall superadvisor
```

</details>

<details>
<summary><strong>Cursor, OpenCode, Amp, and other agent-skills harnesses</strong></summary>

### Install

```bash
npx skills add cskwork/superadvisor
npx skills add cskwork/superadvisor -g
```

Type `/superadvisor` in a new agent chat.

### Verify

```bash
npx skills list
```

### Update

```bash
npx skills update superadvisor
```

### Uninstall

```bash
npx skills remove superadvisor
```

</details>

<details>
<summary><strong>Antigravity (agy)</strong></summary>

### Install

```bash
agy plugin install https://github.com/cskwork/superadvisor
```

### Verify

```bash
agy plugin list
```

### Uninstall

```bash
agy plugin uninstall superadvisor
```

</details>
