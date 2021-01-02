---
title: "Packaging custom Keycloak extensions as an EAR in Kotlin"
date: "2021-01-01T15:30:00-08:00"
author: "Duncan Hill"
authorTwitter: "duncanahill"
cover: "keycloak-kotlin-ear.jpg"
tags:
  - keycloak
  - kotlin
keywords:
  - keycloak
  - kotlin
  - ear
  - spi
  - packaging
description: |
  Keycloak allows for adding custom code to the application in a variety of formats.
  This post walks through the process of packaging it as an EAR with an example
  given using code written with Kotlin.
showFullContent: false
---

_The completed source for this post can be found [here](https://github.com/duncanhill/keycloak-ear-example)._

# Background
The process that lead me to this started with a desire to extend Keycloak.
I'd been wanting a robust authentication solution where I control the data.
Services like [Auth0](https://auth0.com) and [Okta](https://www.okta.com)
are robust and offer excellent support, but the price tag and loss of purview
of the account data meant they weren't quite what I was looking for.

So, after some research I settled on Keycloak. I've been very impressed
with it so far and have felt a degree of comfort from Red Hat's involvement.

The key feature that I wanted to add was an association between one-or-more
email domains and an identity provider so that I could support automatic redirects
for enterprise single sign-on situations.

So, I set out to do just that. While I won't be including
the UI portion in this example I will be using the domain/identity provider
assocation as the focal point.

# Project Structure 
We're not going for anything particular crazy here. For the sake of the example
assume that we're operating with the following:

| Property           | Value    |
| ------------------ | -------- |
| Build System       | Gradle   |
| Group ID           | fracture |
| Artifact ID        | utopia   |
| JDK Version        | 1.8      |

_(For those curious what I was listening to while writing this: [Utopia](https://youtu.be/giB7TPjjjss))_

If you're creating a fresh project the resulting directory structure should look something like:

{{< code language="bash" >}}
$ tree ./utopia
./utopia
├── build.gradle.kts
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradle.properties
├── gradlew
├── gradlew.bat
├── settings.gradle.kts
└── src
    ├── main
    │   ├── kotlin
    │   └── resources
    └── test
        ├── kotlin
        └── resources
{{< /code >}}
