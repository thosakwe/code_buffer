# code_buffer
[![Pub](https://img.shields.io/pub/v/code_buffer.svg)](https://pub.dartlang.org/packages/code_buffer)
[![build status](https://travis-ci.org/thosakwe/code_buffer.svg)](https://travis-ci.org/thosakwe/code_buffer)

An advanced StringBuffer geared toward generating code, and source maps.

# Installation
In your `pubspec.yaml`:

```yaml
dependencies:
  code_buffer: ^1.0.0
```

# Usage
Use a `CodeBuffer` just like any regular `StringBuffer`:

```dart
String someFunc() {
    var buf = new CodeBuffer();
    buf
      ..write('hello ')
      ..writeln('world!');
    return buf.toString();
}
```

However, a `CodeBuffer` supports indentation.

```dart
void someOtherFunc() {
  var buf = new CodeBuffer();
  // Custom options...
  var buf = new CodeBuffer(newline: '\r\n', space: '\t', trailingNewline: true);
  
  // Any following lines will have an incremented indentation level...
  buf.indent();
  
  // And vice-versa:
  buf.outdent();
}
```

`CodeBuffer` instances keep track of every `SourceSpan` they create.
This makes them useful for codegen tools, or to-JS compilers.

```dart
void someFunc(CodeBuffer buf) {
  buf.write('hello');
  expect(buf.lastLine.text, 'hello');
  
  buf.writeln('world');
  expect(buf.lastLine.lastSpan.start.column, 5);
}
```

You can copy a `CodeBuffer` into another, heeding indentation rules:

```dart
void yetAnotherFunc(CodeBuffer a, CodeBuffer b) {
  b.copyInto(a);
}
```