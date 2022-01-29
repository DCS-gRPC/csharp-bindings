# DCS-gRPC C# language bindings

## Introduction

This project is used to generate a nuget package that contains only the C#
bindings generated from the `.proto` files from the latest release of the 
[DCS-gRPC](https://github.com/DCS-gRPC/rust-server) project.

It does not contain any manually written classes that add any abstractions
on top of the gRPC bindings.

The versioning of this package will follow the versioning of the DCS-gRPC
project.

## Installation

Install using `NuGet` package manager.

```plain
nuget install RurouniJones.Dcs.Grpc
```

## Usage

Following is an example that streams units from the server.

```csharp
using Grpc.Core;
using Grpc.Net.Client;
using RurouniJones.Dcs.Grpc.V0.Mission;

// Create a channel that can be shared amongst clients
using var channel = GrpcChannel.ForAddress($"http://localhost:50051");

// Create a client that uses the above channel
var client = new MissionService.MissionServiceClient(channel);

// Send a `StreamUnits` request
var units = client.StreamUnits(new StreamUnitsRequest()
{
    PollRate = 1,
    MaxBackoff = 30
});

// Process each streamed unit
await foreach (var unitUpdate in units.ResponseStream.ReadAllAsync()) {
}
```