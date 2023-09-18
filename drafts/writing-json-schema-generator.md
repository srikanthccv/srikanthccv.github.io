Title: Writing a JSON Schema generator for OpenTelemetry Collector
Date: 2023-08-19
Category: poasts
Tags: jsonschema, lsp, yaml, opentelemetry, collector

Have you ever wondered how the text editing tools like VSCode, IntelliJ, and Vim know the structure of the JSON or YAML file you are editing and how does it provide auto-completion, field descriptions, and (basic) validation? The answer is Language Server Protocol (LSP). LSP is a protocol that allows the editor to communicate with the language server to get the information about the file you are editing. In this poast, I will describe how I wrote a tiny JSON Schema generator for OpenTelemetry Collector.

## The need

As a part of my $job, (and outside of it), I spend a lot of time working with Collector config YAML files. There are tens of components and each has a defined set of configuration options. It's hard to remember all the options and their types. I looked around to see if there is a tool that can help me with this. A (talented and) curious soul created a gist that contains the JSON Schema for the Collector config. However, it is few years old and the code that generates is lost as mentioned by the author (I mailed them to collaborate and revive). So I decided to write one myself as a fun exercise and side project.

## Where do I even start?

Having no idea how any of this worked, the first thing I looked up was how YAML auto-complete works in VSCode. Oh, there is an [extension](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) built by RedHat that provides YAML support. 