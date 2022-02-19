# Full Stack Architectural Specification
There are hundreds of languages and frameworks for building web and mobile applications, but no set of simple standards to describe the problems they commonly solve. Crafted by the open source community, this specification aims to establish a set of language and framework agnostic instructions for building web and mobile apps. 

## Core Concepts
* **Entity** - Most commonly a business object, such as a product or service. 
* **Relationship** - Represents a connection between two (or more?) Entities. Standard relationships are 1-1, 1-N, N-N. 
* **Event** - An action in the system, triggered by a human or machine actor. Anything from a button click to a background job. 
* **Attribute** - Attributes and their values describe an Entity or Event, such as a product that has a price (attribute) of $10 (value) or a scheduled job that runs with a frequency (attribute) of daily (value). 

Core terminology represents ubiquitous concepts that apply to all levels of the stack. An event or relationship may have different implications in the UI versus the data layer, but they are fundamentally necessary and fill the same function across the stack.

## Examples
### Core Specification (Draft)
```yaml
# YAML
entities:
  post:
    attributes:
      id: integer!
      title: string!
      body: text
    relationships:
      comment: 1-N
      author: N-N
  comment:
    attributes:
      id: integer!
      body: text
    relationships:
      post: N-1
  author:
    attributes:
      id: integer!
      first_name: string
      last_name: string
    relationships:
      post: N-N
configuration:
  stack:
    layers:
      data: true
      api: true
      ui: false # headless
  deployment:
    environments: [development, staging, production]
```
This represents a core specification, anticipating a need to overlay UI, API, and/or data layer specifications. Events would likely be represented in those specs, as it's hard to conceptualize an event that would take place across the entire stack. 

### Event Specification (Draft)
```yaml
events:
  create_post:
    input: post
    output: post
    subscribable: true # enables access/subscription/notification
    authorization:
      users: null # no specific users (in general) are authorized to create a post
      roles: [admin] # the admin role can create a post
      scoped: [author] # authors are allowed to create a post associated with them
    relationships:
      entity: post
      triggering: author
```

## Research/Influences

### Syntax
YAML and JSON were chosen as formats for the initial prototype due to their ubiquitous usage in software systems, simple syntax, and for being language/framework agnostic.

We borrowed concepts from the following open-source models/specifications:
* `!` - required field (GraphQL)

### Parts of Speech
In order for architectures and workflows to be commonly understood by humans, they must conform to basic linguistic patterns. This specification parallels the well-known parts of speech:
| | Noun | Adjective | Verb | Adverb |
| --- | --- | --- | --- | --- |
| Specification | Element | (Element) Attribute | Event | (Event) Attribute |
| Example | System | admin | creates (record) | manually |

TODO: Refine example (adverbs are awkward)

## Experimental Constructs
1. Graph Theory - Entities and Events are both nodes (of different types) with Attributes on those nodes, with Relationships as links. Data can obviously be represented as a graph, but so also can UI components and their structures (Page -->(embeds) Parts).
