# Python Logging Module Reference

## Overview

The `logging` module provides a flexible system for recording events that occur during program execution.

Logging is preferred over `print()` in real applications because it allows:

- Different severity levels
- Output to files
- Structured messages
- Configurable behavior

---

# Logging Levels

Logging messages have severity levels that indicate importance.

| Level | Purpose |
|------|------|
| DEBUG | Detailed debugging information |
| INFO | General program events |
| WARNING | Something unexpected happened |
| ERROR | A failure occurred |
| CRITICAL | Serious error, program may stop |

Example:

```python
import logging

logging.debug("Debug message")
logging.info("Program started")
logging.warning("Low disk space")
logging.error("File not found")
logging.critical("System failure")
```

Default logging level:

```
WARNING
```

---

# Basic Configuration

The simplest way to configure logging is with `logging.basicConfig()`.

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    filename="app.log",
    format="%(asctime)s - %(levelname)s - %(message)s"
)
```

## Common Parameters

| Parameter | Description |
|---|---|
| level | Minimum severity level to record |
| filename | File where logs are written |
| filemode | `"a"` append (default) or `"w"` overwrite |
| format | Format of the log message |
| datefmt | Date/time format |
| encoding | File encoding |

Example with additional options:

```python
logging.basicConfig(
    level=logging.DEBUG,
    filename="pipeline.log",
    filemode="a",
    format="%(asctime)s - %(levelname)s - %(message)s"
)
```

---

# Creating a Logger

Instead of using the root logger, most applications create a named logger.

```python
logger = logging.getLogger(__name__)
```

## Why use named loggers?

- Organizes logs by module
- Makes debugging easier in larger programs
- Standard practice in Python projects

Example usage:

```python
logger.info("Fetching weather data")
```

---

# Logger Methods

Logger objects provide the following methods:

| Method | Description |
|---|---|
| logger.debug() | Debug message |
| logger.info() | Informational message |
| logger.warning() | Warning message |
| logger.error() | Error message |
| logger.critical() | Critical failure |

Example:

```python
logger.error("Database connection failed")
```

---

# Logging Exceptions

Use `logging.exception()` inside an `except` block to record a full traceback.

```python
import logging

try:
    x = 1 / 0
except Exception:
    logging.exception("An error occurred")
```

Equivalent to:

```python
logging.error("An error occurred", exc_info=True)
```

---

# Log Message Formatting

Logging supports placeholder formatting.

Recommended:

```python
logger.info("Fetching weather for %s", zipcode)
```

Avoid:

```python
logger.info(f"Fetching weather for {zipcode}")
```

Reason: logging only formats the message **if the message will actually be logged**, which improves performance.

---

# Format Variables

Common formatting variables used in log messages:

| Variable | Meaning |
|---|---|
| %(asctime)s | Timestamp |
| %(levelname)s | Log level |
| %(message)s | Log message |
| %(name)s | Logger name |
| %(filename)s | File name |
| %(lineno)d | Line number |

Example format string:

```python
"%(asctime)s - %(name)s - %(levelname)s - %(message)s"
```

Example output:

```
2026-03-24 12:30:15 - api - INFO - Fetching weather data
```

---

# Writing Logs to a File

```python
import logging

logging.basicConfig(
    filename="application.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)
```

Logs will be written to `application.log`.

---

# Important Rule About basicConfig()

`logging.basicConfig()` only runs **once per program**.

If logging has already been configured, additional calls will be ignored.

Example:

```python
logging.basicConfig(level=logging.INFO)
logging.basicConfig(level=logging.DEBUG)
```

The second call does nothing.

To force reconfiguration (Python 3.8+):

```python
logging.basicConfig(level=logging.DEBUG, force=True)
```

---

# Recommended Minimal Setup

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

logger = logging.getLogger(__name__)
```

Usage:

```python
logger.info("Pipeline started")
logger.warning("Incomplete API response")
logger.error("Database insert failed")
```

---

# Best Practices

- Configure logging **once at program start**
- Use **`logger = logging.getLogger(__name__)`**
- Prefer **logging over `print()`**
- Use appropriate **log levels**
- Log exceptions using **`logging.exception()`**