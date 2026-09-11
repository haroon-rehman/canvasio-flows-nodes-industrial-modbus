# Development notes — Modbus (`@sevio-hq/node-red-contrib-modbus`)

## Why a special install is needed

Full `npm install` currently attempts to build native dependency `sockopt`
(via `node-gyp`). On Windows this fails when Python / Visual Studio build tools
are not available.

## Recommended local sequence

```powershell
cd C:\GitHub\canvasio\cio-wfs-nodes\cio-wfs-node-industrial-modbus
npm install --ignore-scripts
npx gulp
```

- `--ignore-scripts` skips the native rebuild of `sockopt`.
- `npx gulp` builds the Node-RED-loadable output into `modbus/` from `src/`.
- `modbus/` is gitignored; it must be built locally before the package can load.

## What this affects

| Concern | With `--ignore-scripts` + gulp |
|---------|--------------------------------|
| Editor / palette load | Works (HTML/JS under `modbus/` present) |
| Canvas IO category | Applied in `src/*.html`, copied by gulp |
| Full Modbus TCP/Serial protocol with native socket options | May be limited if `sockopt` is not built |
| Serialport optional features | Depend on optional native modules |

## Fully reproducible environment (future)

To remove the workaround, a developer machine needs:

- Python available to `node-gyp`
- Windows build tools (VS C++ toolchain)
- Successful native build of `sockopt` (and optional `serialport`)

Do not remove the native dependency solely to avoid Python.

## Harness

After building `modbus/`, the package is linked from `cio-wfs-dev` via:

```text
file:../cio-wfs-nodes/cio-wfs-node-industrial-modbus
```
