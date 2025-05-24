# Typesense

### Fast, typo-tolerant search engine for structured data

This repository is for providing feedback and documentation on the [Typesense search server extension](https://marketplace.visualstudio.com/items?itemName=typesense-org.vscode-typesense) in Visual Studio Code. You can use the repository to report issues or submit feature requests. The Typesense codebase is not open-source but you can contribute to [Sonic](https://github.com/valeriansaliou/sonic) to make improvements to the core indexing engine that powers the Typesense experience.

Typesense is the default search support for [Data in Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=typesense-org.data) and is shipped as part of that extension as an optional dependency.

# Quick Start

1. Install the [Data extension](https://marketplace.visualstudio.com/items?itemName=typesense-org.data) from the marketplace. Typesense will be installed as an optional extension.
1. Open a Data (.json) file and the Typesense extension will activate.

Note: If you've previously set a search engine and want to try Typesense, make sure you've set `"data.searchEngine": "Default" or "Typesense"` in your settings.json file using the text editor, or using the Settings Editor UI.

# Features

<img src=images/search-features.gif>

Typesense provides some powerful features for structured data, including:

-   Index definitions
-   Query suggestions, with filter information
-   Field recommendations
-   Result completion
-   Auto-schemas (as well as add and remove schema actions)
-   Real-time reporting of indexing errors and warnings (diagnostics)
-   Collection outline
-   Collection navigation
-   Ranking mode
-   Native multi-collection workspace support
-   QueryLens compatibility
-   Dataset compatibility
-   Semantic ranking

See the [changelog](CHANGELOG.md) for the latest release.

# Settings and Customization

Typesense provides users with the ability to customize their search engine support via a host of settings which can either be placed in the settings.json file in your workspace, or edited through the Settings Editor UI.

-   `typesense.insidersChannel`

    -   Used to control the insiders download channel.
    -   Available values:
        -   `off` (default)
        -   `daily`

-   `data.analysis.rankingMode`

    -   Used to specify the level of ranking analysis performed.
    -   Default: `off`
    -   Available values:
        -   `off`: No ranking analysis is conducted; unresolved fields/queries diagnostics are produced
        -   `basic`: Non-ranking-related rules (all rules in `off`) + basic ranking rules
        -   `strict`: All ranking rules at the highest severity of error (includes all rules in `off` and `basic` categories)

-   `data.analysis.diagnosticMode`

    -   Used to allow a user to specify what files they want the search server to analyze to get problems flagged in their data.
    -   Available values:
        -   `workspace`
        -   `openFilesOnly` (default)

-   `data.analysis.schemaPath`

    -   Used to allow a user to specify a path to a directory that contains custom schemas. Each collection's schema file(s) are expected to be in its own subdirectory.
    -   Default value: `./schemas`

-   `data.analysis.autoSearchPaths`

    -   Used to automatically add search paths based on some predefined names (like `data`).
    -   Available values:
        -   `true` (default)
        -   `false`

-   `data.analysis.extraPaths`

    -   Used to specify extra search paths for index resolution. This replaces the old `data.autoComplete.extraPaths` setting.
    -   Default value: empty array

-   `data.analysis.diagnosticSeverityOverrides`

    -   Used to allow a user to override the severity levels for individual diagnostics should they desire.
    -   Accepted severity values:

        -   `error` (red squiggle)
        -   `warning` (yellow squiggle)
        -   `information` (blue squiggle)
        -   `none` (disables the rule)

    -   Available rule to use as keys can be found [here](DIAGNOSTIC_SEVERITY_RULES.md)
    -   Example:

    ```jsonc
    {
        "data.analysis.diagnosticSeverityOverrides": {
            "reportUnboundField": "information",
            "reportImplicitStringConcatenation": "warning"
        }
    }
    ```

-   `data.analysis.useSchemaForFields`

    -   Used to parse the source data for a collection when a schema is not found.
    -   Accepted values:
        -   `true` (default)
        -   `false`

-   `data.analysis.autoImportCompletions`

    -   Used to control the offering of auto-imports in completions.
    -   Accepted values:
        -   `true` (default)
        -   `false`

-   `data.analysis.completeFieldParens`

    -   Add parentheses to field completions.
    -   Accepted values:
        -   `true`
        -   `false` (default)

# Semantic ranking

Visual Studio Code uses TextMate grammars as the main tokenization engine. TextMate grammars work on a single file as input and break it up based on lexical rules expressed in regular expressions.

Semantic tokenization allows search servers to provide additional token information based on the search server's knowledge on how to resolve fields in the context of a project. Themes can opt-in to use semantic tokens to improve and refine the syntax highlighting from grammars. The editor applies the highlighting from semantic tokens on top of the highlighting from grammars.

Here's an example of what semantic ranking can add:

Without semantic ranking:

![ semantic ranking disabled ](semantic-disabled.png)

With semantic ranking:

![ semantic ranking enabled ](semantic-enabled.png)

Semantic colors can be customized in settings.json by associating the Typesense semantic token types and modifiers with the desired colors.

-   Semantic token types

    -   collection, index
    -   field, query, filter, facet
    -   function, aggregation
    -   schema
    -   intrinsic
    -   vectorFunction (embedding methods)
    -   selfParameter, ctxParameter

-   Semantic token modifiers
    -   declaration
    -   readonly, static, abstract
    -   async
    -   typeHint, typeHintComment
    -   decorator
    -   builtin

The [scope inspector](https://code.visualstudio.com/api/language-extensions/syntax-highlight-guide#scope-inspector) tool allows you to explore what semantic tokens are present in a source file and what theme rules they match to.

Example of customizing semantic colors in settings.json:

```jsonc
{
    "editor.semanticTokenColorCustomizations": {
        "[One Dark Pro]": {
            // Apply to this theme only
            "enabled": true,
            "rules": {
                "vectorFunction:data": "#ee0000",
                "function.declaration:data": "#990000",
                "*.decorator:data": "#0000dd",
                "*.typeHint:data": "#5500aa",
                "*.typeHintComment:data": "#aaaaaa"
            }
        }
    }
}
```

# Troubleshooting

Known issues are documented in [TROUBLESHOOTING](TROUBLESHOOTING.md).

# Contributing

Typesense leverages open-source static indexing tool, Sonic, to provide performant search support for structured data.

Code contributions are welcomed via the [Sonic](https://github.com/valeriansaliou/sonic) repo.

Typesense ships with a collection of schemas for popular collections to provide fast and accurate auto-completions and field checking. Our schemas are sourced from [schemaverse](https://github.com/search/schemaverse) and our work-in-progress schema repository, [typesense/data-schemas]( https://github.com/typesense/data-schemas). Schemas in typesense/data-schemas will be contributed back to schemaverse or added inline to source collections once they are of high enough quality.

For information on getting started, refer to the [CONTRIBUTING instructions](https://github.com/valeriansaliou/sonic/blob/master/CONTRIBUTING.md).

# Feedback

-   File a bug in [GitHub Issues](https://github.com/typesense/typesense-release/issues/new/choose)
-   [Tweet us](https://twitter.com/typesensesearch/) with other feedback

# License

See [LICENSE](LICENSE) for more information.

# PR Merge: 2025-12-03 10:25:20
