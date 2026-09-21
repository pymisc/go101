# go101

Hands-on **Go learning for DevOps, Platform Engineering, Kubernetes, and Infrastructure-as-Code development**.

The goal is not to learn every aspect of Go. The goal is to learn practical Go specifically for building and understanding:

* REST API clients
* Kubernetes clients
* Kubernetes Custom Resources (CRDs)
* Kubernetes controllers
* Kubernetes operators
* Terraform providers
* Infrastructure integration tools

---

## 🎯 End Goal

```text
Go Fundamentals
      │
      ▼
REST API Client
      │
      ▼
Kubernetes client-go
      │
      ▼
Custom Resource Definitions (CRDs)
      │
      ▼
Reconciliation Loop
      │
      ▼
Kubernetes Controller
      │
      ▼
Kubernetes Operator
      │
      ▼
Terraform Provider
      │
      ▼
Terraform Plugin Framework
```

The final objective:

> **Be able to understand, troubleshoot, modify, and eventually build Kubernetes controllers, operators, Terraform providers, and infrastructure integrations written in Go.**

---

# 🗺️ Learning Roadmap

## Phase 1 — Go Fundamentals

### STEP 1 — Go Toolchain

Learn:

* `go version`
* `go env`
* `go run`
* `go build`
* `go fmt`
* `go test`
* `go mod init`
* `go mod tidy`

First program:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello from Go!")
}
```

---

### STEP 2 — Variables and Data Types

Learn:

* Strings
* Integers
* Floats
* Booleans
* Constants
* Type inference
* `:=`

Use infrastructure-oriented examples:

```go
clusterName := "gpu-cluster-01"
nodeCount := 3
healthy := true
```

---

### STEP 3 — Conditions and Loops

Learn:

* `if`
* `else`
* `switch`
* `for`
* `range`

Example exercises:

* Check whether a Kubernetes node is `Ready`
* Iterate through a list of servers
* Identify unhealthy nodes

---

### STEP 4 — Arrays, Slices, and Maps

Pay particular attention to:

```go
[]string
map[string]string
```

Example:

```go
nodes := []string{
    "worker-01",
    "worker-02",
    "worker-03",
}
```

These structures appear frequently when working with APIs, Kubernetes, and configuration.

---

### STEP 5 — Functions

Learn:

* Function arguments
* Return values
* Multiple return values
* Variadic functions

Understand the common Go pattern:

```go
result, err := someFunction()

if err != nil {
    // handle error
}
```

---

### STEP 6 — Structs

Structs are extremely important for infrastructure development.

```go
type Server struct {
    Name      string
    IPAddress string
    Status    string
}
```

Understand how structs map to:

* API responses
* Configuration
* Kubernetes resources
* Terraform resource models

---

### STEP 7 — Methods and Interfaces

Learn:

* Methods
* Receivers
* Interfaces

Understand why interfaces are commonly used in infrastructure software.

---

### STEP 8 — Pointers

Learn:

```go
*Server
&server
```

Understand:

* Values vs pointers
* Passing objects efficiently
* Modifying structures through pointers
* Why Kubernetes Go libraries use pointers extensively

---

### STEP 9 — Error Handling

Understand Go's explicit error model:

```go
result, err := doSomething()

if err != nil {
    return err
}
```

Practice:

* Creating errors
* Returning errors
* Wrapping errors
* Handling failures cleanly

---

### STEP 10 — Packages and Modules

Learn:

* Packages
* Modules
* Imports
* Dependency management

Example project:

```text
go101/
├── go.mod
├── cmd/
├── internal/
├── pkg/
└── README.md
```

---

# Phase 2 — Infrastructure Programming

## STEP 11 — JSON Handling

Learn:

```go
encoding/json
```

Practice converting between JSON and Go structs.

Example JSON:

```json
{
  "name": "worker-01",
  "status": "Ready"
}
```

This is essential preparation for REST APIs and Kubernetes.

---

## STEP 12 — HTTP and REST APIs

Learn:

```go
net/http
```

Build a Go program that:

```text
Go Program
     │
     │ HTTP
     ▼
REST API
     │
     ▼
JSON Response
     │
     ▼
Go Struct
```

Practice:

* GET
* POST
* Authentication headers
* JSON parsing
* HTTP status handling
* Timeouts

---

## STEP 13 — Build a Reusable API Client

Move HTTP/API functionality out of `main.go`.

Example:

```text
cmd/
internal/
└── api/
    └── client.go
```

Architecture:

```text
CLI / Controller
       │
       ▼
   API Client
       │
       ▼
External Platform
```

---

## STEP 14 — Context

Learn:

```go
context.Context
```

Understand:

* Cancellation
* Timeouts
* Request lifecycle

This becomes particularly important with Kubernetes controllers and Terraform providers.

---

## STEP 15 — Goroutines and Channels

Learn the basics of:

```go
go function()
```

and:

```go
chan
```

The initial objective is not advanced concurrent programming.

Learn enough concurrency to comfortably understand infrastructure-oriented Go code.

---

# Phase 3 — Kubernetes Development

## STEP 16 — Kubernetes `client-go`

Use Go to communicate directly with a Kubernetes cluster.

```text
Go Program
     │
     ▼
 client-go
     │
     ▼
Kubernetes API Server
```

First goal:

> List Kubernetes Pods using Go.

Then expand to:

* Nodes
* Deployments
* Services
* Namespaces

---

## STEP 17 — Kubernetes API Concepts

Understand:

* API groups
* Versions
* Kinds
* Metadata
* Spec
* Status

Example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example
spec:
  ...
```

Learn how Kubernetes YAML objects map into Go structures.

---

## STEP 18 — Custom Resource Definitions (CRDs)

Create a simple CRD.

Example:

```yaml
apiVersion: platform.example.com/v1
kind: NetworkCheck
metadata:
  name: internet-check
spec:
  target: "8.8.8.8"
  interval: 60
```

Understand:

```text
CRD
 │
 ▼
Custom Resource
 │
 ▼
Kubernetes API
```

---

## STEP 19 — Understand Reconciliation

This is one of the most important concepts in Kubernetes controller development.

Desired state:

```text
replicas: 3
```

Actual state:

```text
replicas: 2
```

Controller:

```text
Desired State
      │
      ▼
  Reconcile()
      │
      ▼
Actual State
```

The controller continually attempts to make:

```text
Actual State == Desired State
```

---

## STEP 20 — Build the First Kubernetes Controller

Build a small controller that watches the custom resource created earlier.

```text
NetworkCheck CR
       │
       ▼
Kubernetes API
       │
       ▼
Controller
       │
       ▼
Reconcile()
       │
       ▼
Create / Update Resources
```

---

## STEP 21 — `controller-runtime`

Learn:

* Manager
* Client
* Reconciler
* Watches
* Events
* Predicates

Understand how `controller-runtime` simplifies Kubernetes controller development.

---

## STEP 22 — Kubebuilder

Use Kubebuilder to scaffold a controller project.

Important:

> Do not simply generate the project and run it.

Read and understand the generated code.

Learn:

* Project structure
* API definitions
* Controller structure
* Generated manifests
* RBAC
* CRDs

---

## STEP 23 — Operator SDK

Build an operator using Operator SDK.

Understand:

```text
Kubernetes API
      │
      ▼
Custom Resource
      │
      ▼
Controller
      │
      ▼
Reconciliation Logic
      │
      ▼
Managed Infrastructure
```

---

## STEP 24 — Production Controller Concepts

Learn how production controllers handle:

* Retries
* Idempotency
* Finalizers
* Resource deletion
* Status updates
* Conditions
* API failures
* Eventual consistency

This is where a toy controller starts becoming a real operator.

---

# Phase 4 — Terraform Provider Development

## STEP 25 — Understand Terraform Provider Architecture

Understand:

```text
Terraform Configuration
        │
        ▼
Terraform Core
        │
        ▼
Terraform Provider
        │
        ▼
REST / gRPC / SDK
        │
        ▼
Infrastructure Platform
```

A provider translates Terraform resources into operations against an external platform.

---

## STEP 26 — Terraform Plugin Framework

Study the modern Terraform provider development framework.

Learn:

* Providers
* Resources
* Data sources
* Schemas
* Models
* Diagnostics

---

## STEP 27 — Build a Minimal Terraform Provider

Create a simple provider.

Example:

```hcl
terraform {
  required_providers {
    example = {
      source = "local/example"
    }
  }
}

provider "example" {
}
```

Understand how Terraform discovers and communicates with provider plugins.

---

## STEP 28 — Build a Terraform Data Source

Create a data source that calls an API.

```hcl
data "example_server" "worker01" {
  name = "worker-01"
}
```

Flow:

```text
Terraform
    │
    ▼
Provider
    │
    ▼
REST API
    │
    ▼
Server Information
```

---

## STEP 29 — Build a Terraform Resource

Example:

```hcl
resource "example_server" "worker" {
  name = "worker-01"
}
```

Implement the resource lifecycle:

```text
Create
  │
Read
  │
Update
  │
Delete
```

Understand how the provider connects remote infrastructure with Terraform state.

---

## STEP 30 — Terraform Provider State and Lifecycle

Study:

* Terraform state
* Resource IDs
* Drift
* Import
* Updates
* Deletes
* Schema changes
* Diagnostics
* API failures

Understand why **Terraform provider development is fundamentally different from writing Terraform modules**.

---

# Phase 5 — Final Integration Project

## STEP 31 — Infrastructure Integration Platform

Build a small infrastructure service exposing a REST API.

Manage the same infrastructure through both:

* Kubernetes
* Terraform

Architecture:

```text
                 Infrastructure API
                    ▲          ▲
                    │          │
             Go Operator    Go Provider
                    ▲          ▲
                    │          │
                   CRD        HCL
                    ▲          ▲
                    │          │
              Kubernetes   Terraform
```

### Kubernetes Interface

Example:

```yaml
apiVersion: platform.example.com/v1
kind: NetworkCheck
metadata:
  name: google-dns
spec:
  target: "8.8.8.8"
```

### Terraform Interface

Example:

```hcl
resource "networkcheck_target" "google_dns" {
  target = "8.8.8.8"
}
```

Both interfaces ultimately communicate with the same backend API.

This demonstrates a common architecture used by modern infrastructure platforms.

---

# 🧠 Repository Philosophy

This repository follows one rule:

> **Learn Go by building infrastructure tooling.**

Instead of:

```text
Create a program that manages students.
```

Prefer:

```text
Create a program that manages Kubernetes nodes.
```

Instead of:

```text
Create a library management system.
```

Prefer:

```text
Create a REST client that queries infrastructure resources.
```

Instead of:

```text
Create an employee database.
```

Prefer:

```text
Parse Kubernetes or cloud API responses into Go structs.
```

The objective is to learn **Go and infrastructure software engineering simultaneously**.

---

# 📊 Progress

## Phase 1 — Go Fundamentals

* [ ] STEP 1 — Go Toolchain
* [ ] STEP 2 — Variables and Data Types
* [ ] STEP 3 — Conditions and Loops
* [ ] STEP 4 — Arrays, Slices, and Maps
* [ ] STEP 5 — Functions
* [ ] STEP 6 — Structs
* [ ] STEP 7 — Methods and Interfaces
* [ ] STEP 8 — Pointers
* [ ] STEP 9 — Error Handling
* [ ] STEP 10 — Packages and Modules

## Phase 2 — Infrastructure Programming

* [ ] STEP 11 — JSON Handling
* [ ] STEP 12 — HTTP and REST APIs
* [ ] STEP 13 — Reusable API Client
* [ ] STEP 14 — Context
* [ ] STEP 15 — Goroutines and Channels

## Phase 3 — Kubernetes Development

* [ ] STEP 16 — Kubernetes `client-go`
* [ ] STEP 17 — Kubernetes API Concepts
* [ ] STEP 18 — Custom Resource Definitions
* [ ] STEP 19 — Reconciliation
* [ ] STEP 20 — First Kubernetes Controller
* [ ] STEP 21 — `controller-runtime`
* [ ] STEP 22 — Kubebuilder
* [ ] STEP 23 — Operator SDK
* [ ] STEP 24 — Production Controller Concepts

## Phase 4 — Terraform Provider Development

* [ ] STEP 25 — Terraform Provider Architecture
* [ ] STEP 26 — Terraform Plugin Framework
* [ ] STEP 27 — Minimal Terraform Provider
* [ ] STEP 28 — Terraform Data Source
* [ ] STEP 29 — Terraform Resource
* [ ] STEP 30 — Provider State and Lifecycle

## Phase 5 — Final Project

* [ ] STEP 31 — Infrastructure Integration Platform

---

# 🚀 Why `go101` Exists

Modern Platform/DevOps engineering is increasingly moving beyond simply **using infrastructure tools** toward extending and building them.

Traditional platform engineering:

```text
Terraform
Kubernetes
Helm
Ansible
Python / Bash
```

Infrastructure software engineering adds:

```text
Go
 │
 ├── Kubernetes CRDs
 ├── Kubernetes Controllers
 ├── Kubernetes Operators
 ├── Terraform Providers
 ├── API Clients
 └── Infrastructure Integrations
```

`go101` is intended to bridge that gap.

The ultimate objective is not:

> "I know Go syntax."

The objective is:

> **"I can understand, troubleshoot, modify, and eventually build Kubernetes controllers, operators, Terraform providers, and infrastructure integrations written in Go."**
