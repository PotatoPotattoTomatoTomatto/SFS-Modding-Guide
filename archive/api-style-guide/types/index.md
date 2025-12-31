# Type Documentation

This section provides guidelines and best practices for documenting C# types (classes, structs, enums, interfaces, and delegates) in the SFS Modding Guide API documentation.

## Frontmatter

Refer to the [Frontmatter](/internal/style-guide/api/#frontmatter) section for general frontmatter guidelines.

### Type-Specific Frontmatter Order

Fields should appear in this order:

1. **Metadata fields** (no blank line between these):

   - `type`: Always `type` for type documentation
   - `typekind`: One of `class`, `struct`, `enum`, `interface`, `delegate`
   - `enclosingType`: (Only for nested types) Name of parent type
   - `name`: Name of the type
   - `namespace`: Full namespace path, or `.global` for global namespace

2. **Blank line**

3. **Visual fields**:
   - `description`: Brief description of the type
   - `sidebar_label`: (Optional) Custom sidebar label

### Example Frontmatter

**Regular class:**

```yaml
---
type: type
typekind: class
name: Rocket
namespace: SFS.World

description: Represents an in-game rocket.
---
```

**Nested class:**

```yaml
---
type: type
typekind: class
enclosingType: StatsMenu
name: ElementData
namespace: .global

description: Stores data about individual UI elements.
sidebar_label: ElementData
---
```

## File Structure

### Folder Organization (Classes and Structs and Interfaces)

These are container types because they can have members inside of them and nested types.

```
api/
  reference/
    namespace-folder/
      TypeName/
        index.mdx              # Main type documentation
        Constructors/          # Constructor documentation
          Constructor1.mdx
          Constructor2.mdx
        static-constructor.mdx  # Static constructor documentation (if applicable)
                                # if the class is static, then omit Constructors folder, and put static-constructor.mdx here
        Methods/               # Method documentation
          MethodName.mdx       # Non-overloaded method
          OverloadedMethod.mdx # Overloaded method. For now, just one file per overload set.
          GenericMethod/
            index.mdx          # Generic method documentation, subfolders for overloads if applicable
            GenericParameters/
              1.mdx            # Type parameter 1 documentation
              2.mdx            # Type parameter 2 documentation
              index.mdx        # Generic parameters overview (if applicable)
          GenericOverloadedMethod/
            index.mdx          # Generic overloaded method documentation, subfolders for overloads if applicable
            Overloads/
              Overload1.mdx    # Overload 1 documentation
              Overload2.mdx    # Overload 2 documentation
              Overload3/
                index.mdx      # Overload 3 documentation
                GenericParameters/
                  1.mdx        # Type parameter 1 documentation
                  index.mdx    # Generic parameters overview (if applicable)
        Properties/            # Property documentation
          PropertyName.mdx
          index.mdx              # Properties overview (if applicable)
        Fields/                # Field documentation
          FieldName.mdx
          index.mdx              # Fields overview (if applicable)
        Events/                # Event documentation
          EventName.mdx
          index.mdx              # Events overview (if applicable)
        Nested-Types/
          NestedClass/
            index.mdx            # Nested class or struct documentation, subfolders are expected
          NestedInterface/ # Nested interface, subfolders are expected for members.
            index.mdx
          NestedStruct/ # Nested struct, subfolders are expected
            index.mdx
          NestedEnum/
            index.mdx
            Fields/
              index.mdx
              EnumValue1.mdx
              EnumValue2.mdx

          NestedDelegate.mdx # not complex enough for its own folder
          NestedGenericDelegate/ # it has levelled up, and is now complex enough for its own folder
            index.mdx
            GenericParameters/
              1.mdx            # Type parameter 1 documentation
              2.mdx            # Type parameter 2 documentation
          index.mdx        # Nested types overview (if applicable)
        GenericParameters/    # Generic type parameters documentation (if applicable)
          1.mdx              # Type parameter 1 documentation
          2.mdx              # Type parameter 2 documentation
          index.mdx          # Generic parameters overview (if applicable)
```

Sidecars for types with members will mirror this structure under the `sidecars/` folder, like so:

```
api/
  sidecars/
    namespace-folder/
      TypeName/
        Info.mdx                # Main type sidecar
        Constructors/
          Constructor1/
            Info.mdx            # Constructor sidecar
          Constructor2/
            Info.mdx
        static-constructor/
          Info.mdx              # Static constructor sidecar
        Methods/
          MethodName/
            Info.mdx            # Method sidecar
          OverloadedMethod/
            Info.mdx            # Overloaded method sidecar
          GenericMethod/
            Info.mdx            # Generic method sidecar
            GenericParameters/
              1.mdx            # Type parameter 1 sidecar
              2.mdx            # Type parameter 2 sidecar
              Info.mdx        # Generic parameters overview sidecar
          GenericOverloadedMethod/
            Info.mdx            # Generic overloaded method sidecar
            Overloads/
              Overload1/
                Info.mdx        # Overload 1 sidecar
              Overload2/
                Info.mdx        # Overload 2 sidecar
              Overload3/
                Info.mdx        # Overload 3 sidecar
                GenericParameters/
                  1.mdx        # Type parameter 1 sidecar
                  Info.mdx    # Generic parameters overview sidecar
        Properties/
          PropertyName/
            Info.mdx            # Property sidecar
        Fields/
          FieldName/
            Info.mdx            # Field sidecar
        Events/
          EventName/
            Info.mdx            # Event sidecar
        Nested-Types/
          NestedClass/
            Info.mdx            # Nested class or struct sidecar
          NestedInterface/
            Info.mdx            # Nested interface sidecar
          NestedStruct/
            Info.mdx            # Nested struct sidecar
          NestedEnum/
            Info.mdx            # Nested enum sidecar
          NestedDelegate/
            Info.mdx            # Nested delegate sidecar
```

### Folder Organization (Enums, Interfaces, and Delegates)

These are leaf types because they cannot have complex members inside of them or nested types.

```title="Folder Structure for Enums"
api/
  namespace-folder/
    EnumType.mdx            # Main type documentation
```

```title="Folder Structure for Enums and Delegates"
api/
  namespace-folder/
    DelegateType.mdx            # Main type documentation
```

### File Naming Conventions

- **Type files**: Use the exact type name (e.g., `Rocket.mdx`) or folder with `index.mdx`
- **Nested types**: If it is a class or struct nested within another type, create a folder with the nested type's name containing an `index.mdx` file.
- **Constructor files**: Use descriptive names including parameter types (e.g., `Constructor-int-string.mdx`)
- **Method files**: Use method name, or create folder for overloads
  - Only create overload folders if there are overloads.
- **Generic types**: Use the type name without angle brackets (e.g., `List.mdx` not `List<T>.mdx`)
- **Static constructors**: Create a separate `static-constructor.mdx` file if applicable. This should be in the main type folder, not in the Constructors subfolder, which should not exist for static classes.

## Content Structure

Type documentation should follow this structure:

### 1. Title (H1)

Format: `# TypeName [TypeKind]`

Examples:

- `# Rocket Class`
- `# StatsMenu Class`
- `# [StatsMenu](/api/global-namespace/StatsMenu).ElementData Class` (for nested types)

For nested types, link the enclosing type name to its documentation.
Note that because of this, you'll have

### 2. Definition (H2)

Contains:

**Metadata list:**

- **Namespace**: Full namespace (display "Global Namespace" for `.global`)
- **Assembly**: Assembly name (typically `Assembly-CSharp.dll` or `SFS.dll` for SFS types, depending on what version you're documenting)
- **Version**: When the type was added (e.g., "Added in SFS 1.5.10" for all versions and before or "Added in SFS X.Y.Z")

**Description paragraph:**

- Detailed explanation of the type's purpose and functionality
- Should expand on the frontmatter description, at the very least repeating it verbatim
- Explicitly mention if it's a singleton or has special usage patterns
- Keep both descriptions in sync

**Type declaration:**

```csharp
public class TypeName : BaseClass, IInterface
```

Declaration guidelines:

- Include access modifiers
- Include base class and interfaces (if any)
- No extra indentation
- No trailing braces or method bodies
- For nested types, show full name: `public class OuterClass.NestedClass`

**Inheritance information** (classes and structs only):

Format: `[Object](https://learn.microsoft.com/en-us/dotnet/api/system.object?view=netframework-4.8) → BaseClass → DerivedClass`

- Use arrows (→) to separate types
- Link each type to its documentation
- Framework types link to Microsoft docs (.NET Framework 4.8)
- Always start with Object for classes

**Implemented Interfaces** (classes and structs only):

Bulleted list of interfaces, each linked to its documentation, in no particular order. For example:

- [`IDisposable`](link)
- [`ISerializable`](link)

### 3. Constructors (H2)

**Not applicable for:** Static classes, delegates, enums, interfaces

Table with columns:

- **Parameter Signature**: `TypeName()`, `TypeName(int x, string y)`
- **Description**: Brief description or "TBD"

Include links to constructor detail pages if they exist.

For static classes with static constructors, create a separate `static-constructor.mdx` file and link to it as H2 here, right after Definition.

```md
## [Static Constructor](/api/path/to/Type/static-constructor)
```

### 4. Members (H2)

#### 1. H3 Methods (H2)

**For classes, structs, and interfaces with methods**

Table with columns:

- **Return Type**: Return type (inline code, use `void` if applicable)
- **Name**: Method signature (inline code, e.g., `DoSomething`)
  - If overloaded, use the format `MethodName (+N Overloads)` where N is the number of overloads
- **Description**: Description or "TBD"

Include links to method detail pages if they exist.

#### 2. H3 Properties (H2)

**For classes and structs with properties**

Table with columns:

- **Type**: Return type (inline code)
- **Name**: Property name (inline code)
- **Description**: Description or "TBD"
- **Accessors**: `get; set;` or `get; private set;`, including access modifiers.

Include links to property detail pages if they exist.

#### 3. H3 Fields

##### Classes and Structs

Table with columns:

- **Type**: Field type (inline code)
- **Name**: Field name (inline code)
- **Description**: Description or "TBD"

##### Enums

Table with columns:

- **Name**: Enum value name (inline code)
- **Value**: Integer value
- **Description**: Description or "TBD"

#### 4. H3 Events

**For types with events**

Table with columns:

- **Type**: Event delegate type (inline code)
- **Name**: Event name (inline code)
- **Description**: Description or "TBD"

### 5. Examples (H2)

**Optional section, but highly recommended for complex types**

Provide code examples demonstrating how to use the type. Include:

- Instantiation examples
- Common usage patterns
- Edge cases or gotchas

If no examples are available, omit this section entirely.

### 6. Remarks (H2)

**Optional section**

Additional information about the type:

- Performance considerations
- Thread safety
- Best practices
- Common pitfalls
- Version-specific notes

If no additional information is available, omit this section.

### 7. See Also (H2)

**Optional section**

Links to related types or members:

- Base classes
- Implemented interfaces
- Related types
- Types that use this type

Format as a bulleted list with inline code and links.

## Member Documentation

For detailed guidance on documenting individual members (constructors, methods, properties, fields, events), see the [Member Documentation](members) guide.

### Quick Overview

Members are documented in subfolders within the type's folder:

- `Constructors/` - Constructor documentation
- `Methods/` - Method documentation
- `Properties/` - Property documentation
- `Fields/` - Field documentation
- `Events/` - Event documentation

Each member gets its own `.mdx` file with `type: member` and the appropriate `memberkind` in frontmatter.

## Special Cases

### Generic Types

For generic types like `MyClass<T>`:

- **File name**: `MyClass.mdx` (no angle brackets)
- **Title**: `# MyClass<T> Class`
- **Declaration**: Include type parameters and constraints

```csharp
public class MyClass<T> where T : BaseClass
```

Document type parameters in the Definition section.

### Extension Methods

Extension methods should be documented as static methods in their containing static class. Note in the description that it's an extension method and what type it extends.

### Operator Overloads

Document operator overloads as methods in the Methods section, using the operator keyword in the method name (e.g., `operator +`).

### Indexers

Document indexers in a separate "Indexers" H2 section, following the same table format as properties but showing the parameter types.

### Abstract Classes

Mark the class as abstract in the declaration and note any abstract members that must be implemented by derived classes.

### Sealed Classes

Mark the class as sealed in the declaration and note that it cannot be inherited.

## Quality Standards

- **Descriptions**: Should be clear, concise, and action-oriented
- **TBD placeholders**: Use sparingly; aim to fill them in within 7 days of creating the skeleton
- **Examples**: Provide at least one example for complex types
- **Links**: Verify all internal links work correctly
- **Consistency**: Use the same terminology as the actual code
- **Accuracy**: Ensure technical details match the actual implementation
- **Proofreading**: Check for grammar, spelling, and punctuation errors
