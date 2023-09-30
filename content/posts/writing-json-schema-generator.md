Title: Writing a JSON Schema generator for OpenTelemetry Collector
Date: 2023-08-19
Category: posts
Tags: jsonschema, lsp, yaml, opentelemetry, collector

Have you ever wondered how text editing tools like VSCode, IntelliJ, and Vim know the JSON or YAML file structure you are editing and how it provides auto-completion, field descriptions, and (basic) validation? The answer is Language Server Protocol (LSP). LSP is a protocol that allows the editor to communicate with the language server to get information about the file you are editing. In this poast, I will describe how I wrote a tiny JSON Schema generator for OpenTelemetry Collector.

## Need

As a part of my $job, (and outside of it), I spend a lot of time working with Collector config YAML files. There are tens of components, each with a defined set of configuration options. It's hard to remember all the options and their types. I looked around to see if a tool could help me with this. A (talented and) curious soul created a gist that contains the JSON Schema for the Collector config. However, it is a few years old, and the code generated is lost, as mentioned by the author (I mailed them to collaborate and revive). I decided to write one myself as a fun exercise and side project.

## The beginning

Having no idea how any of this worked, the first thing I looked up was how YAML auto-complete works in VSCode. There is an [extension](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) built by RedHat that provides YAML support, eh? The "Associating schemas" section says _YAML Language support uses JSON Schemas to understand the shape of a YAML file, including its value sets, defaults and descriptions._ The [JSON Schema](https://json-schema.org/) sounded familiar, yet not sure what it was. A quick example [https://json-schema.org/learn/miscellaneous-examples.html#basic](https://json-schema.org/learn/miscellaneous-examples.html#basic) made it easier to follow. It's a JSON file that describes the structure of the JSON (or YAML) file.

## Generating the schema

The handwritten schema is not an option, given the number of components and their evolving config options. I needed a way to generate the schema from the code. I discovered a [library](https://github.com/invopop/jsonschema) that generates JSON Schemas from Go types through reflection. My first attempt was to use this library to generate the schema from the config structs. It worked well for simple types like string, bool, int, etc. However, it failed for complex types like nested structs. I decided to fork the library. After several hours of learning and hacking, I could generate the schema. Here is the demo of it in action: [https://github.com/srikanthccv/otelcol-jsonschema#demo](https://github.com/srikanthccv/otelcol-jsonschema#demo)

This was a fun exercise, and I learned JSON Schema, LSP, and Go reflection.