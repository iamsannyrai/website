---
sidebar_position: 2
title: Getting started
description: Learn how to set up and use Celest in your Flutter project.
---

# Getting started

This guide will walk you through how to set up Celest on your development machine, and how to create a new project.


## Prerequisites
To use Celest in your Flutter app, you need the following prerequisites:

1. Install [Flutter](https://docs.flutter.dev/get-started/install)
2. Create a new Flutter project (or choose an existing one)
3. [Download](/download) and install the Celest CLI

That’s it! You are now ready to start building your backend - all in Dart!

## Starting a new Celest project

Start by first creating a new Flutter project. If you have an existing Flutter project, you can use that instead. To start a new Flutter project, go to your console and run the following command.

```shell
$ flutter create <flutter_app>
```

Then, navigate to your new Flutter app directory.

```shell
$ cd <flutter_app>
```

Once you are in your Flutter app directory, run the following command to initialize your Celest project.

```shell    
$ celest start
```

Once the command executes, Celest will spin up a local environment and watch for changes made to your backend, and code-generate a Dart client for you to test your changes.

The CLI will also create a folder in your project called `celest`, which will include the following files.

```shell
flutter_app/
└── celest/
    ├── functions/          # Celest Functions folder
    |   └── greeting.dart   # Example API file
    ├── lib/
    │   │── client.dart     # Generated client for your Flutter app
    │   ├── models.dart     # Custom API models
    │   └── exceptions.dart # Custom API exceptions
    └── test/               # Tests for your backend
```

To get started building your serverless cloud function, navigate to the `<flutter_app>/celest/functions/` folder and create a file named `<api_name>.dart`. You can create as many APIs as you want in this directory. Think of each file as a way to organize and group multiple Celest Functions of similar functionality into a namespace.

Celest Functions are defined as top-level functions as shown below.

```dart
Future<String> sayHello(String name) async {
  return 'Hello, $name';
}

Future<String> sayGoodbye(String name) async {
  return 'Goodbye, $name';
}
```

That's all you need to define your API! Now, you can connect your Flutter app to your cloud functions by using the Celest client as shown in the following example. Replace the contents of the `main.dart` file in your Flutter app to the following code-snippet.

```dart
import 'package:flutter/material.dart';
// Import the generated Celest client
// highlight-next-line
import 'package:celest_backend/client.dart';

void main() {
  // Initializes Celest in your Flutter app
  // highlight-next-line
  celest.init();
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Homepage'),
        ),
        body: Center(
          child: FutureBuilder(
            // Call your function using the Celest client
            // highlight-next-line
            future: celest.functions.greeting.sayHello('Celest'),
            builder: (_, snapshot) => switch (snapshot) {
              AsyncSnapshot(:final data?) => Text(data),
              AsyncSnapshot(:final error?) =>
                Text('${error.runtimeType}: $error'),
              _ => const CircularProgressIndicator(),
            },
          ),
        ),
      ),
    );
  }
}
```

You have now set up your Celest project and integrated it into your Flutter app. Follow our [guides](/docs/functions/introduction.md) to learn more about using Celest Functions.

## Deploying your backend resources

:::info

Deploying your backend is not currently available, but you can still experiment and build your Celest backend locally. We are working hard to bring backend deployment and management to you. Please join our [waitlist](/) to stay updated on our progress.

:::

