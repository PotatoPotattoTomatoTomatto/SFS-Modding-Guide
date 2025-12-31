# Namespaces

Namespaces are used to organise code into logical groups and to prevent name collisions.
When documenting namespaces in the SFS Modding Guide API documentation, follow these guidelines:

## File Structure

- Each namespace should have its own folder within the `api` directory. The folder structure should reflect the namespace hierarchy, and nested namespaces should be represented as subfolders.
- Each namespace folder and category should be named using their exact, full name as it appears in the codebase.
  - For example, the namespace `SFS.World` should be located in the `api/SFS/SFS.World` folder.
- Each namespace should have an `index.mdx` file that provides an overview of the namespace and links to its classes, structs, enums, interfaces, and delegates.

## Frontmatter

Refer to the [Frontmatter](/internal/style-guide/api/#frontmatter) section for general frontmatter guidelines.

## Contents

The contents of the `index.mdx` file should include:

- The title with the namespace's name, aka H1.
- A H2 "Overview" section that provides a brief description of the namespace's purpose and functionality.
- The `NamespaceList` component, which automatically generates:
  - A H2 "Namespace Hierarchy" section showing the full hierarchy of parent namespaces (only shown for nested namespaces with 2+ levels, e.g., `SFS.World` shows hierarchy, but `SFS` does not)
  - A H2 "Nested Namespaces" section with a table listing all nested namespaces with their descriptions (omitted if none exist)
  - A H2 "Types" section with tables organized by type kind (Classes, Structs, Enums, Interfaces, Delegates), showing type names and descriptions
- H2 "Remarks" section for any additional information about the namespace that may be relevant to users (optional).
- H2 "See Also" section for links to related namespaces or types (optional).

## Boilerplate Example

Here is a boilerplate example of a namespace `index.mdx` file. Text in brackets, `()`, should be replaced with the appropriate values.

```mdx title="index.mdx"
---
type: namespace
name: (Full.Namespace.Name)
description: (A brief description of the namespace's purpose and functionality.)
---

import NamespaceList from "@site/src/components/NamespaceList";

# (Full.Namespace.Name)

## Overview

(A brief description of the namespace's purpose and functionality.)
(Should preferably be the same, if not more detailed, than the `description` frontmatter field.)

<NamespaceList namespaceKey="(namespace-key)" />

## Remarks

(Any additional information about the namespace that may be relevant to users.)

## See Also

- [`Related.Namespace1`](path/to/related/namespace1)
```

## Notes

- The `namespaceKey` prop should match the top-level key in the manifest JSON file (e.g., `"global-namespace"` or `"SFS"`).
- Nested types are automatically displayed with their full name (e.g., `OuterClass.NestedClass`).
- The component handles the display of namespace names, converting `.global` to "Global Namespace" where appropriate.
