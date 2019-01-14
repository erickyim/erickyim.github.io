# Serializing JSON using code generation libraries

## Setting up json_serializable in a project

Add depdendecies to `pubspec.yaml`:

```yaml
dependencies:
  # Your other regular dependencies here
  json_annotation: ^2.0.0

dev_dependencies:
  # Your other dev_dependencies here
  build_runner: ^1.0.0
  json_serializable: ^2.0.0
```

Run `flutter packages get` inside your project root folder (or click Packages Get in your editor) to make these new dependencies available in your project.

## Creating model classes the json_serializable way

Model example:

```dart
import 'package:json_annotation/json_annotation.dart';

/// This allows the `User` class to access private members in
/// the generated file. The value for this is *.g.dart, where
/// the star denotes the source file name.
part 'user.g.dart';

/// An annotation for the code generator to know that this class needs the
/// JSON serialization logic to be generated.
@JsonSerializable()

class User {
  User(this.name, this.email);

  String name;
  String email;

  /// A necessary factory constructor for creating a new User instance
  /// from a map. Pass the map to the generated `_$UserFromJson()` constructor.
  /// The constructor is named after the source class, in this case User.
  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);

  /// `toJson` is the convention for a class to declare support for serialization
  /// to JSON. The implementation simply calls the private, generated
  /// helper method `_$UserToJson`.
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```

The file `user.g.dart` is not created yet.

By running `flutter packages pub run build_runner build in the project root`, you generate JSON serialization code for your models whenever they are needed.

Or running `flutter packages pub run build_runner watch` to start a watcher, the watcher wiill generates code continuously.

## Consuming json_serializable models

To decode a JSON string the json_serializable way, you do not have actually to make any changes to our previous code.

```dart
Map userMap = jsonDecode(jsonString);
var user = User.fromJson(userMap);
```

The same goes for encoding. The calling API is the same as before.

```dart
String json = jsonEncode(user);
```

## Reference

https://flutter.io/docs/development/data-and-backend/json#code-generation
