# Gripmock Types

A package of custom types for [Gripmock](https://github.com/bavix/gripmock) that solves JSON serialization/deserialization problems.

## Problem

In [issue #648](https://github.com/bavix/gripmock/issues/648), a problem was identified with deserializing string time values into `time.Duration`. When trying to use JSON like:

```json
{
  "output": {
    "delay": "100ms"
  }
}
```

The following error occurred:
```
json: cannot unmarshal number { into Go struct field Output.Delay of type time.Duration
```

## Solution

The `types` package provides a custom `Duration` type that correctly handles string time values in JSON.

## Available Types

### Duration

A custom type alias for `time.Duration` that provides JSON marshaling/unmarshaling support for string values.

#### Supported formats

Only string values are supported:
```json
{
  "delay": "100ms"
}
```

Valid duration formats:
- `"100ms"` - milliseconds
- `"2s"` - seconds  
- `"1m"` - minutes
- `"1h"` - hours
- `"1h30m"` - combined formats

#### Usage

```go
type Output struct {
    Delay types.Duration `json:"delay"`
    Data  map[string]interface{} `json:"data"`
}

var output Output
json.Unmarshal([]byte(`{"delay": "100ms"}`), &output)

// Access the duration value
duration := time.Duration(output.Delay)
```

## Installation

```bash
go get github.com/gripmock/types
```

## Testing

```bash
go test ./...
```

## License

MIT License - see [LICENSE](LICENSE) file.