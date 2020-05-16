Welcome to [Riverpod]!

This project can be considered as an **experimental** [provider] rewrite.

Long story short:

- Declare your providers as global variables:

  ```dart
  final myNotifierProvider = ChangeNotifierProvider((_) {
    return MyNotifier();
  });

  class MyNotifier extends ChangeNotifier {
    int count;
    // TODO: typical ChangeNotifier logic
  }
  ```

- Use them inside your widgets in a compile-time safe way. No runtime exceptions!

  ```dart
  class Example extends HookWidget {
    @override
    Widget build(BuildContext context) {
      final count = useSelector(myNotifierProvider.select((m) => m.count));
      return Text(count.toString());
    }
  }
  ```

See the [FAQ](#FAQ) if you have questions around what this means for [provider].

# Index

- [Motivation](#motivation)
- [Usage](#usage)
- [FAQ](#faq)
  - [Why another project when provider already exists?](#why-another-project-when-provider-already-exists)
  - [Is it safe to use in production?](#is-it-safe-to-use-in-production)
  - [Will this get merged with provider at some point?](#will-this-get-merged-with-provider-at-some-point)
  - [Will provider be deprecated/stop being supported?](#will-provider-be-deprecatedstop-being-supported)

# Motivation

If [provider] is a simplification of [InheritedWidget]s, then [Riverpod] is
a reimplementation of [InheritedWidget]s from scratch.

It is very similar to [provider] in principle, but also has major differences
as an attempt to fix the common problems that [provider] face.

[Riverpod] has multiple goals. First, it inherits the goals of [provider]:

- Being able to safely create, observe and dispose states without having to worry about
  losing the state on widget rebuild.
- Making our objects visible in Flutter's devtool by default.
- Testable and composable
- Improve the readability of [InheritedWidget]s when we have multiple of them
  (which would naturally lead to a deeply nested widget tree).
- Make apps more scalable with a uni-directional data-flow.

From there, [Riverpod] goes a few steps beyond:

- Reading objects is now **compile-safe**. No more runtime exception.
- Makes the [provider] pattern more flexible, which allows supporting commonly
  requested features like:
  - being able to have multiple providers of the same type.
  - disposing the state of a provider when it is no longer used.
  - make a provider private.
- Simplifying complex object graphs.
- Makes the pattern independent from Flutter

These are achieved by no-longer using [InheritedWidget]s. Instead, [Riverpod]
implements its own mechanism that works in a similar fashion.

# Usage

The way [Riverpod] is used depends on the application you are making.

You can refer to the following table to help you decide which package to use:

| app type                  | package name                                                                       | description                                                                                                                                       |
| ------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Flutter + [flutter_hooks] | [hooks_riverpod]                                                                   | An improved syntax with less boilerplate for listening providers inside widgets.                                                                  |
| Flutter only              | [flutter_riverpod]                                                                 | A slightly more verbose syntax (comparable to `Theme.of` vs `StreamBuilder`).<br>But feature-wise, it is otherwise identical to [hooks_riverpod]. |
| Dart only (No Flutter)    | [riverpod](https://github.com/rrousselgit/river_pod/tree/master/packages/riverpod) | A version of [Riverpod] striped out of all the classes related to Flutter                                                                         |

# FAQ

## Why another project when [provider] already exists?

While [provider] is largely used and well accepted by the community,
it is not perfect either.

People regularly file issues or ask questions about some problems they face, such as:

- Why do I have a `ProviderNotFoundException`?
- How can I make that my state is automatically disposed of when not used anymore?
- How to make a provider that depends on other (potentially complex) providers?

These are legitimate problems, and I believe that something can be improved to fix
those.

The issue is, these problems are deeply rooted in how [provider] works, and
fixing those problems is likely impossible without drastic changes to the
mechanism of [provider].

This _could_ be merged inside [provider] directly, but would be betraying the
[provider] community to do so.\
See [Will this get merged with provider at some point?](#will-this-get-merged-with-provider-at-some-point)
for a deeper explanation.

## Is it safe to use in production?

The project is still experimental, so use it at your own risk.

In theory, it should scale well to large projects. But it is possible that
there are some bugs or missing features.

But I would expect this project to work for most people in its current state

## Will this get merged with [provider] at some point?

No. At least not until it is proven that the community likes [Riverpod]
and that it doesn't cause more problems than it solves.

While [provider] and this project have a lot in common, they do have some
major differences. Differences big enough that it would be a large breaking
change for users of [provider] to migrate [Riverpod].

Considering that, separating both projects initially sounds like a better
compromise.

## Will [provider] be deprecated/stop being supported?

Not in the short term, no.

This project is still experimental and unpopular. While it is, in a way,
a [provider] 2.0, its worth has yet to be proven.

Until it is certain that [Riverpod] is a better way of doing things
and that the community likes it, [provider] will still be maintained.

# Roadmap

- FutureChangeNotifier
- FutureStateNotifier
- AutoDispose
- overrideForSubtree AutoDispose
- Dartdoc for all public APIs
- review all public APIs
- README for all packages
- debugFillProperties
- Selector
- CI
- Packages description/homepage

Marvel example:

- Write tests for everything
- publish it on the web
- handle errors (out of calls/day, no configurations)
- Added to doc

riverpod.dev

- Combining providers
- Filtering rebuilds
- Testing (without flutter, mocking FutureProvider)
- How it works

[provider]: https://github.com/rrousselGit/provider
[riverpod]: https://github.com/rrousselGit/river_pod
[flutter_hooks]: https://github.com/rrousselGit/flutter_hooks
[inheritedwidget]: https://api.flutter.dev/flutter/widgets/InheritedWidget-class.html
[hooks_riverpod]: https://pub.dev/packages/hooks_riverpod
[flutter_riverpod]: https://pub.dev/packages/flutter_riverpod
