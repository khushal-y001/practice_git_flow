# **Introducing Dart 3**

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*2XwxNKHrKb3SGaWEyqg2nA.png)

# What's new in Dart 3?  
Dart 3 is a new major release. Partly to signify the large step forward in new functionality, and partly because it’s a breaking release in terms of semantic versioning: We’re changing the type system to only support **sound null safety** (in Dart 2.12 and later this was opt-in), and have made the corresponding breaking changes in Dart’s core libraries. Let’s dive into the details!

## Dart 3 productivity enhancements  
- In Dart 3 we expect to add two new major features, records and patterns, with the goal of making working with structured data more productive.

- Records allow you to efficiently and concisely create anonymous composite values from any existing data, without the conceptual overhead of needing to declare a class to hold the values. With records, you can easily build new data structures that combine existing data. For example, to return a pair of values:

 ``` dart
 (double x, double y) geoLocation() {
    return (-1.2921, 36.8219);
}
 ```
 ``` dart
 void main(){
    final (lat,long) = geoLocation();
    print('Current location: $lat, $long');
 }
 ```
- Patterns are fully type safe, and checked during development.  
- You can also pattern match on the type of values, for example from a hierarchy of classes. A `switch` can use patterns that match on the type, and the individual fields of each type, as in the body of `calculateArea` here:  
``` dart
sealed class Shape {
}

class Square implements Shape {
  final double length;
  Square(this.length);
  
}

class Circle implements Shape {
  final double radius;
  Circle(this.radius);
}

double calculateArea(Shape shape) => switch (shape) {
  Square(length: var l) => l * l,
  Circle(radius: var r) => math.pi * r * r
};
```

Overall, we’re adding a large selection of patterns that, when combined, make Dart much more expressive for structured data.
In conjunction with patterns, we’re also adding capability controls to classes, via several new modifiers:

- **interface class**: Cannot be extended.
- **base class**: Disables the implicit interface, so cannot be implemented.
- **final class**: Cannot extend, implement, or mix in the class (outside the current library).
- **sealed class**: Same as abstract + final + the type is considered the root of a sealed type family for exhaustiveness checking. As an example, take the Shape class hierarchy above. In switch statements on the Shape type (like the calculateArea function), the analyzer will trigger errors if the switch statement does not handle all possible subtypes of the sealed type.
- **mixin class**: A class which may be used as a mixin.
Every new feature adds complexity to the language. To ensure Dart remains approachable, classes default to be fully permissive just like today, with the small exception that classes intended to be used as mixins must now use the mixin keyword.
