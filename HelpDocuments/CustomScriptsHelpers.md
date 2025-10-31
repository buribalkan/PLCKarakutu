# 🧩 ScriptHelpers (`H.`)

`ScriptHelpers` provides a compact set of helper functions accessible through the `H.` prefix inside scripts.  
They simplify value conversion, text matching, timing, and diagnostic output.

---

## 🔹 Reason & Control Flow

| Function | Description | Example |
|-----------|--------------|----------|
| `H.Reason("text")` | Stores a human-readable reason string (usually for later access). | `H.Reason("Temperature too high")` |
| `H.Hit("text")` | Sets a reason and returns `true`. Shortcut for `return H.Hit("...")`. | `return H.Hit("Valid temperature range")` |
| `H.Fail("text")` | Sets a reason and returns `false`. | `return H.Fail("Sensor not found")` |

---

## 🔹 Console Logging

| Function | Description | Example |
|-----------|--------------|----------|
| `H.Console(msg)` | Writes an info-level message to the script console. | `H.Console("Starting check...")` |
| `H.ConsoleWarn(msg)` | Writes a warning message. | `H.ConsoleWarn("Low battery")` |
| `H.ConsoleError(msg)` | Writes an error message. | `H.ConsoleError("Device offline")` |

---

## 🔹 Retrieve Last Sample Values

| Function | Description | Example |
|-----------|--------------|----------|
| `H.Last(tag)` | Returns the last recorded string value for a given tag. | `var s = H.Last("Temperature");` |
| `H.LastDouble(tag)` | Returns the last recorded numeric value as `double?`. | `if (H.LastDouble("Voltage") > 5.0)` |
| `H.LastBool(tag)` | Returns the last recorded boolean value as `bool?`. | `if (H.LastBool("IsActive") == true)` |

---

## 🔹 Type Conversion Helpers

### String conversion

```csharp
H.Str(obj)
```
Converts any object into a readable string.

**Examples:**
```csharp
H.Str(123.45)  // "123.45"
H.Str(null)    // null
```

---

### Boolean conversion

```csharp
H.Bool(value)
```
Converts text or numeric values into a boolean (`bool?`).  
Recognizes “yes”, “on”, “true”, “1”, “açık” (Turkish for “open”), etc.

**Examples:**
```csharp
H.Bool("yes")   // true
H.Bool("0")     // false
H.Bool(12)      // true
```

---

### Double conversion

```csharp
H.Double(value)
```
Converts strings or objects into `double?`, culture-safe.

**Examples:**
```csharp
H.Double("3,14")   // 3.14
H.Double(true)     // 1.0
```

---

### Integer conversion

```csharp
H.Int(value)
```
Converts to `int?`, rounding decimals when needed.

**Examples:**
```csharp
H.Int("3.6")  // 4
H.Int(false)  // 0
```

---

### Long conversion

```csharp
H.Long(value)
```
Converts to 64-bit integer (`long?`).  
Understands binary (`0b...`) and hex (`0x...`) strings.

**Examples:**
```csharp
H.Long("0xFF") // 255
H.Long("0b101") // 5
```

---

### Decimal conversion

```csharp
H.Decimal(value)
```
Converts text or numbers to `decimal?`, culture-safe.

**Examples:**
```csharp
H.Decimal("3,5") // 3.5
```

---

## 🔹 Time and Duration Helpers

```csharp
H.TimeSpan(value)
```
Converts strings or numbers into a `TimeSpan?`.  
Supports multiple formats:

| Format | Meaning | Example |
|--------|----------|----------|
| `"1h 30m"` | 1 hour 30 minutes | `H.TimeSpan("1h 30m")` |
| `"1500ms"` | milliseconds | `H.TimeSpan("1500ms")` |
| `"PT2H10M"` | ISO format | `H.TimeSpan("PT2H10M")` |
| `"00:05:00"` | hh:mm:ss | `H.TimeSpan("00:05:00")` |
| `2500` | milliseconds | `H.TimeSpan(2500)` |

---

## 🔹 Math Helpers

| Function | Description | Example |
|-----------|--------------|----------|
| `H.Clamp(v, min, max)` | Restrains a value within a range. | `H.Clamp(15, 0, 10)` → `10` |
| `H.Diff(a, b)` | Difference `a - b` (returns `null` if missing). | `H.Diff(10, 3)` → `7` |
| `H.IsBetween(v, min, max)` | Checks if a value is within the range. | `H.IsBetween(5, 0, 10)` → `true` |
| `H.Abs(v)` | Absolute value. | `H.Abs(-3)` → `3` |

---

## 🔹 String Pattern Helpers

| Function | Description | Example |
|-----------|--------------|----------|
| `H.Like(text, pattern)` | Wildcard match: `*` = any, `?` = single char. | `H.Like("Sensor1", "Sensor*")` → `true` |
| `H.Contains(text, fragment)` | Case-insensitive substring match. | `H.Contains("Hello World", "world")` → `true` |

---

## 🔹 Time Utilities

| Function | Description | Example |
|-----------|--------------|----------|
| `H.Now()` | Returns the current UTC time. | `H.Now()` |
| `H.SecondsSince(date)` | Seconds since a given timestamp. | `H.SecondsSince(H.Now().AddSeconds(-5))` → `5` |
| `H.Ago(date)` | Human-readable elapsed time string. | `H.Ago(DateTime.UtcNow.AddSeconds(-12))` → `"12.0s ago"` |

---

## 🔹 Formatting

| Function | Description | Example |
|-----------|--------------|----------|
| `H.Format(double?, digits=2)` | Converts number to string with given decimal digits. | `H.Format(3.14159)` → `"3.14"` |

---

## 🔹 Bit & Boolean Group Operations

| Function | Description | Example |
|-----------|--------------|----------|
| `H.IsBitSet(value, bitIndex)` | Checks if a bit is set in an integer. | `H.IsBitSet(5, 0)` → `true` (since 5 = `101b`) |
| `H.ActiveCount(bool[])` | Counts `true` values in an array. | `H.ActiveCount(true, false, true)` → `2` |
| `H.AllTrue(bool[])` | Checks if all values are `true`. | `H.AllTrue(true, true)` → `true` |
| `H.AnyTrue(bool[])` | Checks if any value is `true`. | `H.AnyTrue(false, true)` → `true` |

---

## 🔹 Logging Samples

```csharp
H.LogSample(stream, params (string key, object? val)[] cols)
```
Publishes a structured sample record for monitoring/logging.

**Example:**
```csharp
H.LogSample("Machine1", ("Temperature", 35.2), ("Status", "Running"));
```

---

✨ **Tip:**  
All conversion helpers (`Bool`, `Double`, `Int`, etc.) are *null-safe* and culture-independent.  
They return `null` instead of throwing exceptions.
