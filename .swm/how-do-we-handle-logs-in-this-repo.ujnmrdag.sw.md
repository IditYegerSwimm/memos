---
title: How do we handle logs in this repo?
---
The process of handling logs in the Memos repo involves using the zap logging library, initializing a global logger, and syncing logs. However, the specifics of how logs are processed or stored are not clear.

<SwmSnippet path="/internal\log\logger.go" line="1" repo-id="Z2l0aHViJTNBJTNBbWVtb3MlM0ElM0FJZGl0WWVnZXJTd2ltbQ==">

---

The zap logging library is imported for log handling.

```go
package log

import (
	"go.uber.org/zap"
	"go.uber.org/zap/zapcore"
```

---

</SwmSnippet>

<SwmSnippet path="/internal\log\logger.go" line="16" repo-id="Z2l0aHViJTNBJTNBbWVtb3MlM0ElM0FJZGl0WWVnZXJTd2ltbQ==">

---

A global logger is initialized in the <SwmToken path="internal\log\logger.go" pos="17:2:2" line-data="func init() {" repo-id="Z2l0aHViJTNBJTNBbWVtb3MlM0ElM0FJZGl0WWVnZXJTd2ltbQ==" repo-name="memos">`init`</SwmToken> function. It's set to the <SwmToken path="internal\log\logger.go" pos="18:11:11" line-data="	gLevel = zap.NewAtomicLevelAt(zap.InfoLevel)" repo-id="Z2l0aHViJTNBJTNBbWVtb3MlM0ElM0FJZGl0WWVnZXJTd2ltbQ==" repo-name="memos">`InfoLevel`</SwmToken> and outputs to stderr.

```go
// Initializes the global console logger.
func init() {
	gLevel = zap.NewAtomicLevelAt(zap.InfoLevel)
	gl, _ = zap.Config{
		Level:       gLevel,
		Development: true,
		// Use "console" to print readable stacktrace.
		Encoding:         "console",
		EncoderConfig:    zap.NewDevelopmentEncoderConfig(),
		OutputPaths:      []string{"stderr"},
		ErrorOutputPaths: []string{"stderr"},
	}.Build(
		// Skip one caller stack to locate the correct caller.
		zap.AddCallerSkip(1),
	)
}
```

---

</SwmSnippet>

<SwmSnippet path="/internal\log\logger.go" line="63" repo-id="Z2l0aHViJTNBJTNBbWVtb3MlM0ElM0FJZGl0WWVnZXJTd2ltbQ==">

---

The <SwmToken path="internal\log\logger.go" pos="63:2:2" line-data="// Sync wraps the zap Logger&#39;s Sync method." repo-id="Z2l0aHViJTNBJTNBbWVtb3MlM0ElM0FJZGl0WWVnZXJTd2ltbQ==" repo-name="memos">`Sync`</SwmToken> function wraps the zap Logger's Sync method, which is used to flush any buffered log entries.

```go
// Sync wraps the zap Logger's Sync method.
func Sync() {
	_ = gl.Sync()
}
```

---

</SwmSnippet>

<SwmSnippet path="/bin\memos\main.go" line="106" repo-id="Z2l0aHViJTNBJTNBbWVtb3MlM0ElM0FJZGl0WWVnZXJTd2ltbQ==">

---

<SwmToken path="bin\memos\main.go" pos="107:5:5" line-data="	defer log.Sync()" repo-id="Z2l0aHViJTNBJTNBbWVtb3MlM0ElM0FJZGl0WWVnZXJTd2ltbQ==" repo-name="memos">`Sync`</SwmToken> is called in the <SwmToken path="bin\memos\main.go" pos="106:2:2" line-data="func Execute() error {" repo-id="Z2l0aHViJTNBJTNBbWVtb3MlM0ElM0FJZGl0WWVnZXJTd2ltbQ==" repo-name="memos">`Execute`</SwmToken> function to ensure all logs are flushed before the program exits.

```go
func Execute() error {
	defer log.Sync()
	return rootCmd.Execute()
```

---

</SwmSnippet>

<SwmMeta version="3.0.0"><sup>Powered by [Swimm](/)</sup></SwmMeta>
