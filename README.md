# SS Monitor

SS Monitor is a client-side Fabric mod for consent-based Minecraft integrity and screenshare telemetry. It is built for Minecraft **1.21.11** and only runs while a compatible server explicitly starts a monitoring session.

Installing the mod by itself does not start monitoring or send reports.

## Requirements

- Minecraft 1.21.11
- Fabric Loader 0.19.5 or newer
- Fabric API 0.141.6+1.21.11 or newer
- Java 21

## Installation

1. Install Fabric Loader and Fabric API for Minecraft 1.21.11.
2. Download the SS Monitor JAR.
3. Place the JAR in your Minecraft instance's `mods` directory.
4. Launch the game with the Fabric profile.

The mod is client-only. A compatible server must initiate a session before SS Monitor collects or transmits telemetry.

## Using the mod

When a server enables monitoring, SS Monitor displays a chat message confirming that the session was received. The session ends automatically when you disconnect or when the server disables it.

Two client commands are available:

| Command | Description |
| --- | --- |
| `/ssmonitor status` | Shows whether monitoring is active, delivery queue and failure counts, session state, and endpoint status. |
| `/ssmonitor report` | Queues a full report for the active monitoring session. |

## What SS Monitor reports

During an active session, the mod can report the following information to the session endpoint supplied by the server:

- Installed mod identifiers, names, versions, origins, and nested-mod relationships.
- Hashes and basic metadata for mod files, plus changes detected between integrity scans.
- The SS Monitor JAR's SHA-256 hash and, if configured, whether it matches an expected hash.
- Java/runtime metadata, JVM agent types, and hashes of runtime values such as the classpath and JVM arguments.
- Non-chat key-binding press and release transitions. Chat, command, and social-interaction bindings are excluded.
- In-game left-click timing, screen changes, and session health/delivery information.

Reports are sent over HTTP or HTTPS with the session token provided by the server. The client discards its queued telemetry when the session ends. If delivery fails, it uses a bounded queue and retry backoff.

## Configuration

On first launch, SS Monitor creates:

```text
config/ssmonitor.properties
```

The default configuration is:

```properties
heartbeat_seconds=30
scan_seconds=30
server_url=
expected_self_sha256=
report_mods=true
report_file_hashes=true
file_hash_refresh_seconds=300
report_jvm=true
report_input=true
report_screen=true
max_queue=100
input_sample_ticks=1
input_report_interval_seconds=1
max_input_events_per_report=64
```

`server_url` is an optional fallback endpoint. A server-provided session endpoint takes precedence. Numeric settings are constrained by the mod to safe ranges; invalid values fall back to their defaults.

## Building from source

With Java 21 and Gradle available, run:

```powershell
gradle build
```

The built mod JAR is written to `build/libs/`.

# This plugin will be rewritten
I'll hire real devs to add more features that the all mighty Ethical AI's would never do

## License

SS Monitor is licensed under the [MIT License](LICENSE.txt).
