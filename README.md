# Eino

A fork of [cloudwego/eino](https://github.com/cloudwego/eino) — a powerful Go framework for building LLM-based applications with composable components and type-safe graph execution.

## Overview

Eino provides a clean, extensible architecture for constructing AI pipelines. It enables developers to:

- Build composable LLM chains and graphs
- Define type-safe data flows between components
- Integrate multiple LLM providers with a unified interface
- Stream responses with backpressure support
- Handle errors and retries gracefully

## Features

- **Graph-based execution**: Define complex workflows as directed graphs with typed edges
- **Streaming support**: First-class support for streaming LLM responses
- **Component model**: Reusable, testable components (ChatModel, Retriever, Tool, etc.)
- **Type safety**: Generics-based design ensures compile-time type checking
- **Observability**: Built-in callbacks for tracing, logging, and metrics

## Requirements

- Go 1.21 or higher

## Installation

```bash
go get github.com/your-org/eino
```

## Quick Start

```go
package main

import (
    "context"
    "fmt"
    "log"

    "github.com/your-org/eino/compose"
    "github.com/your-org/eino/schema"
)

func main() {
    ctx := context.Background()

    // Build a simple chain
    chain, err := compose.NewChain[*schema.Message, *schema.Message]().
        AppendChatModel(yourChatModel).
        Compile(ctx)
    if err != nil {
        log.Fatal(err)
    }

    output, err := chain.Invoke(ctx, schema.UserMessage("Hello, world!"))
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println(output.Content)
}
```

## Project Structure

```
eino/
├── compose/        # Graph and chain composition primitives
├── schema/         # Core data types (Message, Document, etc.)
├── components/     # Component interfaces (ChatModel, Retriever, Tool, etc.)
├── callbacks/      # Callback/middleware system for observability
├── experiments/    # Personal experiments (custom Retrievers, cycle handling)
└── utils/          # Shared utilities
```

## Personal Notes

> I'm using this fork primarily to experiment with custom `Retriever` implementations and to explore how the graph execution engine handles cycles. See the `experiments/` branch for work-in-progress explorations.
>
> **Current focus**: Building a `Retriever` backed by a local SQLite vector store using the `sqlite-vec` extension. Early results are promising for offline/edge use cases.

## Contributing

Please read our [Pull Request Template](.github/PULL_REQUEST_TEMPLATE.md) before submitting changes.

When reporting issues, use the appropriate template:
- [Bug Report](.github/ISSUE_TEMPLATE/bug_report.md)
- [Feature Request](.github/ISSUE_TEMPLATE/feature_request.md)

## License

This project is licensed under the Apache License 2.0 — see the [LICENSE](LICENSE) file for details.

## Acknowledgements

This project is a fork of [cloudwego/eino](https://github.com/cloudwego/eino), originally developed by the CloudWeGo team at ByteDance.
