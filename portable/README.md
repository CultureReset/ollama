# portable/ — standalone ai-stack installer for `ollama`

Packaging only; no application source in this folder was changed. Repo license applies unchanged.

```
portable/install.sh --preflight    # check prerequisites: ollama
portable/install.sh --dry-run      # print every command, change nothing (default)
portable/install.sh --prepare      # run setup, render config/units (NOT enabled)
portable/install.sh --verify       # ExecStart/WorkingDirectory/required files exist, loopback only, secrets provisioned
AI_STACK_ACTIVATE=1 portable/install.sh --activate --yes   # explicit, future fresh target only
```

- Setup: `v=$(ollama --version 2>&1 | grep -o "[0-9]*\.[0-9]*\.[0-9]*" | tail -1); [ "$v" = 0.35.0 ] || { echo "ollama $v != pinned 0.35.0; install with: curl -fsSL https://ollama.com/install.sh | OLLAMA_VERSION=0.35.0 sh" >&2; exit 3; }; ln -sf "$(command -v ollama)" "$2/ollama"`
- Daemon: ollama.service (systemd user unit)
- Port/health: 11434 http://127.0.0.1:11434/api/version
- Secrets the owner must set (never committed): none
- Prefix: $AI_STACK_PREFIX (default ~/.local/share/ai-stack); config: $AI_STACK_CONFIG/ollama (default ~/.config/ai-stack/ollama)

Version provenance: see `component.env`. Upstream/fork HEAD is NOT claimed to be the live installed version unless stated.
Not verified: real runtime start (the build host had no Docker; tests use mocked tools).
