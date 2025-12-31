---
description: Guidelines for documenting the C# API of the game.
sidebar_label: C# API Documentation
---

# C# API Documentation

This repository has a unique approach to documenting the C# API of the game. The documentation is generated from the source code using custom tools that extract relevant information and format it into markdown files. This allows for accurate and up-to-date documentation that reflects the current state of the codebase.

## The Manifest and the Sidecars

The core of the API documentation generation process revolves around two key components: the manifest and the sidecars.

- **Manifest**: The manifest is a JSON file that contains a list of all the types in the API, along with their UIDs (unique identifiers) and other metadata. This file serves as the primary index for the API documentation.
- **Sidecars**: Sidecars are additional JSON files that provide detailed information about each type, such as its properties, methods, events, and other members. Each sidecar corresponds to a specific type in the manifest and contains all the necessary data to generate its documentation page.

## Mirror File Structure

There are two main folders in the API documentation system: `reference` and `sidecars`.

Reference contains the generated markdown files that make up the API documentation, while sidecars contain the source markdown files written by documentation contributors.
The folder structure of the `reference` folder mirrors that of the `sidecars` folder, with the same organization based on namespaces and types. However, instead of containing index.mdx, the documents are named `Info.mdx` in the sidecars folder.

e.g.

```
/reference
  /namespace1
    /TypeA
      /Methods
        method1.mdx
        method2.mdx
      index.mdx
    /TypeB
      index.mdx
  /namespace2
    /TypeC
      index.mdx
/sidecars
  /namespace1
    /TypeA
      Info.mdx
      /Methods
        /method1
          Info.mdx
        /method2
          /TypeParameters
            1.mdx
            2.mdx
          Info.mdx
    /TypeB
      Info.mdx
  /namespace2
    /TypeC
      Info.mdx
```

## Document Types

There are 3 main document types in the API documentation system:

- Namespaces
- Types (Classes, Structs, Enums, Interfaces, Delegates)
- Members (Methods, Properties, Fields, Events, etc.)

There are two kinds of type documents:

- Container: Classes, structs and interfaces.
- Leaf: Enums and delegates.
  - Technically, enums are container types since they can have members (the enum values), but for documentation purposes, they are treated as leaf types since they do not have additional members like methods or properties.

The only kind of type to not have its own folder structure is delegates, as they are leaf types and have no members.

The UID format for each document type is as follows:

- Namespace: `ns:<FullyQualifiedNamespace>`
- Type: `type:<FullyQualifiedTypeName>`
- Member: `member:<FullyQualifiedTypeName>.<FullyQualifiedMemberName>`

These UIDs are used to uniquely identify each document in the manifest and sidecars, allowing for easy retrieval and organization of the API documentation.

## Sidecars

Sidecars are organized in a way that allows for easy retrieval of information about each type. They are typically stored in a directory structure that mirrors the namespace hierarchy of the API. Each sidecar file is named after the UID of the type it represents, making it straightforward to locate the relevant sidecar for any given type.

When generating the documentation, the tools read the manifest to identify all types and then look up their corresponding sidecars to gather detailed information. This information is then formatted into markdown files that make up the API documentation.

The manifest automates the boring and repetitive task of creating the file structure and boilerplate for each type's documentation, and is machine-generated from the source code.
On the other hand, sidecars are manually created and maintained by documentation contributors.

Sidecars must contain specific frontmatter fields to ensure proper generation of the documentation. The required frontmatter fields for sidecars are:

- `short_description`: A brief summary of the type.
  - Used for SEO, search results, overviews, lists, and is the text shown in embed previews.
  - Defaults to the following for:
    - Namespaces: "Namespace <NamespaceName>."
    - Types: "<TypeName> <TypeKind>."
    - Members: "<MemberName> <MemberKind>."
    - A flag called `auto_generate_short_description` is set to true. This indicates that the short description was auto-generated and can be replaced by a custom description.

As of now that's the only frontmatter field used by the generation tools, but more may be added in the future as needed.

Sidecars work by providing H2 headings for each section of the documentation. The generation tools look for these headings and extract the corresponding content to include in the final documentation.
The H2 headings are put in between the Definition H2 section and the Members section (if applicable) when generating the final documentation.
Note that only H2 sections are extracted and if you do not have a H2 heading for a section, that text will not be included in the final documentation.

## Pipeline

The API documentation generation process follows a specific pipeline to ensure that the final output is accurate and well-structured. The steps in the pipeline are as follows:

### Pre-Actions (manual)

1. When the game is updated, the source code is analyzed to identify any changes to the API.

- The architecture assumes the source code is only in 1 assembly. This must be selected by the user when running the tools.

2. The manifest is updated to reflect any new, modified, or removed types in the API, this is done using automated tools.
3. The documentation is versioned to match the game version through Docusaurus versioning system.

### File Structure Generation (automated)

4. The file structure for the API documentation is generated based on the updated manifest. This includes creating folders and boilerplate files for each type in the API.

- Both the `reference` and `sidecars` folder structures are generated to mirror each other.
  - In `src/config.ts`, there is a property called `ASSEMBLY_SIDECARS_FORWARD` that specifies what sidecars to copy over from the previous version to the new version. This is useful for preserving manually written documentation across versions when the types have not changed, but the assembly containing them has been renamed or moved.
- This step is only done once per version.

5. The sidecar files are created with the required frontmatter and boilerplate content.

### Documentation Generation (automated)

6. The documentation generation tools read the manifest and sidecars to gather all necessary information about each type in the API.
7. The gathered information is formatted into markdown files that make up the API documentation.

- Sidecar information is inserted into the appropriate sections of the documentation.

### Post-Actions (manual)

8. Documentation contributors review and enhance the generated documentation by adding more detailed explanations, examples, and other relevant information to the sidecars.
9. The final documentation is published and made available to users.
