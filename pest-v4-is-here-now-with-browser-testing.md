---
title: Pest v4 Is Here — Now with Browser Testing
description: Today, we’re thrilled to announce Pest v4 — our biggest release yet, featuring powerful new browser testing with parallel support and full Laravel integration.
---

> Note: This is a placeholder announcement for early testers only. Pest v4 is not yet publicly released. Details may change before the final launch. Please do not share this publicly.
> To get started with Pest v4's new features including browser testing, please refer to the upgrade guide: [Upgrade Guide →](/docs/upgrade-guide).


# Pest v4 Is Here — Now with Browser Testing

Today, we’re thrilled to announce the release of **Pest v4**, bringing the biggest testing upgrade yet: powerful **[Browser Testing](/docs/browser-testing]**. Pest’s new browser testing features let you run elegant, maintainable browser tests — with first-class support for Laravel’s testing API and the ability to run tests in parallel. For the first time, this is browser testing that feels as good as writing unit tests.

Here is an example using [Laravel](https://laravel.com):

```php
it('may reset the password', function () {
    // access any laravel testing helpers...
    Notification::fake();

    // access to the database — using the RefreshDatabase trait (even sqlite in memory...)
    $this->actingAs(User::factory()->create());

    $page = visit('/sign-in') // visit on a real browser...
        ->on()->mobile() // or ->desktop(), ->tablet(), etc...
        ->firefox() // or ->chrome(), ->safari(), etc...

    $page->assertSee('Sign In')
         ->assertNoJavascriptErrors() // or ->assertNoConsoleLogs()
         ->click('Forgot Password?')
         ->fill('email', 'nuno@laravel.com')
         ->click('Send Reset Link')
         ->assertSee('We have emailed your password reset link!')

    Notification::assertSent(ResetPassword::class);
});
```

With Pest v4’s browser testing, you can:
- Test on **multiple browsers** (Chrome, Firefox, Safari)
- Test on **different devices** and viewports (like iPhone 14 Pro, tablets, or custom breakpoints)
- Switch **color schemes** (light/dark mode)
- Interact with the page (click, type, scroll, select, submit)
- Run **parallel browser tests** for dramatically faster suites
- Seamlessly use **Laravel features** like `Event::fake()`, `assertAuthenticated()`, and model factories
- Take **screenshots** or pause tests for debugging
- …all with the elegance of Pest syntax
- **Playwright-based** — modern, fast, and reliable

To get started with browser testing in Pest, you need to install the Pest Browser Plugin:

```bash
composer require pestphp/pest-plugin-browser --dev

npx playwright install
```

After, you may use the `visit()` function anywhere. Finally, running this test is as simple as executing `./vendor/bin/pest` in your terminal. Pest will handle the rest, launching a browser, navigating to the page, and performing the actions you specified.

## Smoke Testing

Smoke testing your application in real browsers has never been easier. With Pest v4, you can literally visit all your application pages, and ensure they don't throw any JavaScript errors, and they don't log any console errors.

```php
$pages = visit(['/', '/about', '/contact']);

$pages->assertNoJavascriptErrors()
      ->assertNoConsoleLogs();
```

## Visual Regression Testing

Want to ensure your pages look exactly as expected? Pest v4 introduces visual regression testing with the `assertScreenshotsMatches()` assertion. This allows you to take screenshots of your pages and compare them against baseline images, ensuring that your UI remains consistent across changes.

```php
$pages = visit(['/', '/about', '/contact']);

$pages->assertScreenshotsMatches();
```

[image here missing]


This is just a glimpse of what Browser Testing in Pest v4 can do. Find out more about the new features below, and check out the [Browser Testing documentation](/docs/browser-testing) for a complete guide on how to get started.

## Test Sharding

Pest v4 introduces **Test Sharding**, allowing you to split your test suite into smaller, manageable chunks. This is particularly useful for large applications (or when running browser tests) where running all tests at once can be time-consuming.

This feature is particularly useful on CI platforms, where on things like GitHub actions you can no longer scale vertical, but rather horizontally. This means you can run your tests in parallel across multiple machines, significantly speeding up your test suite execution.

To get started with Test Sharding, you can use the `--shard` option when running Pest:

```bash
# GitHub Workflow One
./vendor/bin/pest --shard=1/4

# GitHub Workflow Two
./vendor/bin/pest --shard=2/4

# GitHub Workflow Three
./vendor/bin/pest --shard=3/4

# GitHub Workflow Four
./vendor/bin/pest --shard=4/4
```

You may combine this with the `--parallel` option to run your tests in parallel, and this way trully maximize your test suite execution speed:

```bash
./vendor/bin/pest --shard=1/4 --parallel
```

## Type Coverage Is Much Faster

Remember the days when you had to wait for your type coverage to run? Not anymore! Pest v4 introduces a new type coverage engine that is significantly faster than previous versions.

Type coverage is now up to 10x faster, on a 10 Core machine, and pretty much instant after the first run. This means you can get immediate feedback on your type coverage without waiting for long periods of time.

### On Top of PHPUnit 12

Pest v4 is built on top of PHPUnit 12, which means you get all the latest features and improvements from PHPUnit. As such, be sure to check out the [PHPUnit 12 release announcement](https://phpunit.de/announcements/phpunit-12.html).

### Thanks To You, Pest v4 Is Here!

There's never been a better time to dive into testing and start using Pest. If you're ready to get started with Pest v4 right away, check out our [installation guide](/docs/installation) for step-by-step instructions. And if you're currently using an earlier version of Pest, we've got you covered with detailed upgrade instructions in our [upgrade guide](/docs/upgrade-guide).

Thank you for your continued support and feedback. We can't wait to see what you build with Pest v4!

---

Thank you for reading about Pest v4's new features! Want to get started with Pest? You can find the installation guide in the next section of the documentation: [Installation →](/docs/installation)

