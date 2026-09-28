# plugin-addr

Outside-in TCP reachability probing for OpenCharly — the `addr:` check verb.

The verb probes a `host:port` from outside: an in-container `nc -z` under
`charly check box`, a host-side `net.DialTimeout` under `charly check live`. It
is a host-coupled check verb whose `RunVerb` runs against the live check engine
(`sdk/kit.CheckContext`), so it is **compiled-in only**.

## What it provides

| Capability | Surface |
|---|---|
| `verb:addr` | the `addr:` check verb — probe a `host:port` for reachability (`addr:`, optional `reachable:` defaulting to `true`) |

## How to use it

Compose the plugin candy in a box or check bed's `candy:` list:

```yaml
- '@github.com/opencharly/plugin-addr/candy/plugin-addr:<tag>'
```

Then author the verb in a plan:

```yaml
- check: the ssh port is reachable
  id: addr-ssh
  addr: {addr: "127.0.0.1:22", reachable: true}
  context: [runtime]
```

## Layout

- `candy/plugin-addr/` — the plugin module: `plugin.go` (the `verb` +
  `NewCheckVerb()`/`NewMeta()`), `schema/addr.cue` (the self-contained
  `#AddrInput`), `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-check:check` — the check verb catalog. This candy
  carries no `skill:` entity of its own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-internals:plugin` — the plugin/provider model.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
