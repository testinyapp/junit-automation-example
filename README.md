# Importing JUnit into Testiny

JUnit XML files are the _de facto_ standard format for exchanging test results, despite lacking a formal specification. Because of its wide adoption, most test frameworks and tools—including Testiny—support JUnit as a primary import format.

Testiny is designed to be compatible with various JUnit dialects, providing flexible options for interpreting test structure, results, and metadata.

## Supported Features

### Test Suite & Class Name Mapping

Suite and class names like `com.example.tests.MyTest` can be interpreted as:

- **Nested folders** (split by `.`)
- **Flat folder names**, if preferred

### Test Case Naming

- The `classname` prefix can optionally be **stripped** from the test case `name` to avoid duplication:

```xml
<testcase classname="com.example.tests.MyTest" name="MyTest.testLogin"/>
```

The following CLI options can be used to control naming:
`--junit-strip-classname-prefix <prefix>` allows removing a prefix from the classname (e.g. redundant namespaces)
`--junit-ignore-classname` excludes the "classname" attribute from the test case title
`--junit-classname-as-folder <single|nested>` controls if folders are created in a nested hierarchy from namespaces or as a flat list

### Test Status

- Supports `passed`, `failed`, `skipped`, `untested`, and `blocked` results
- These can appear as child tags or attributes:

```xml
<testcase name="test1">
    <failure message="Assertion failed">...</failure>
</testcase>
<testcase name="test2" status="skipped">
</testcase>
```

### Test Duration

The `time` attribute is used to determine duration. Due to a lack of standardization, time is sometimes supplied as either seconds or milliseconds. By convention, if `time` includes a `"."` decimal separator it is assumed that the duration is in seconds and milliseconds otherwise.

The time unit for the duration can also be set using the CLI:

```bash
 --junit-duration-unit sec
 --junit-duration-unit ms
```

### Output Messages and Attachments

- `<system-out>` and `<system-err>` are automatically processed:
    - If small, content is added to the **result or error message**
    - If large, content is added as an **attachment**

### Attachments

Testiny supports embedded attachment markers in system output/error (Jenkins-style):

``[[ATTACHMENT|./screenshots/tc-4-xyz.jpg|Title]]``

### Custom Properties

Custom fields under `<properties>` are supported:

```xml
<property name="testid" value="T-1234"/>
<property name="testkey" value="APP_LOGIN_TEST_1234"/>
```

`testid` or `testkey` are optional properties that can be used to **uniquely identify** a test across runs. If not specified, a combination of title and folder path will be used to identify a test.

---
Additional properties can be included as fields via the CLI:

```xml
<property name="priority" value="high"/>
<property name="severity" value="critical"/>
```

```bash
 --custom-result-fields priority,severity
```


## Example File

You can find a detailed example with explanations for supported JUnit tags and attributes in the [JUnit Example File](./junit-example.xml).

>If the files you are trying to import are not imported correctly, let us know!
