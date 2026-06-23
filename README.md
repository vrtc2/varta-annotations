# varta-annotations

Public Protobuf **annotations** (custom options) that integrations use to describe
their messages to [Varta](https://github.com/vrtc2). They are the shared contract
between integrations, the Varta SDK and the Varta server — nothing else leaks here.

Published to the Buf Schema Registry as **`buf.build/vrtc2/varta-annotations`**.

## What's inside

| File | Package | Purpose |
|------|---------|---------|
| `varta/task/catalog/v1/task_definition.proto` | `varta.task.catalog.v1` | Annotate a message as a **task** and describe its input form (`task`, `field`, `enum_value`, `oneof_value`, `oneof_field`). |
| `varta/telemetry/v1/telemetry_definition.proto` | `varta.telemetry.v1` | Annotate a message as **telemetry** and label its fields/enum values (`telemetry`, `field`, `enum_value`). |

These files only depend on `google/protobuf/descriptor.proto`.

## Using it

### Proto (buf)

Add the dependency to your `buf.yaml`:

```yaml
deps:
  - buf.build/vrtc2/varta-annotations
```

Then `buf dep update`. The annotations are import-only — buf never generates them
locally, so there is exactly one Go registration of each annotation file.

### Go

```
go get github.com/vrtc2/varta-annotations
```

The generated code lives under `gen/go/...` with the import path baked into each
`.proto` via `option go_package`. In your `buf.gen.yaml` keep managed mode from
rewriting it:

```yaml
managed:
  enabled: true
  disable:
    - file_option: go_package
      module: buf.build/vrtc2/varta-annotations
```

## Publishing

CI (`.github/workflows/buf.yml`) runs `buf lint` + breaking-change detection on
every PR and `buf push`es to the BSR on merge to `main`. Requires a `BUF_TOKEN`
repository secret.
