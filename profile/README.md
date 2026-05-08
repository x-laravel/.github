# x-laravel

A curated collection of open-source Laravel packages, built to solve real-world problems simply, cleanly, and with long-term maintainability in mind.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PHP](https://img.shields.io/badge/PHP-8.2%2B-777BB4?logo=php&logoColor=white)](https://www.php.net/)
[![Laravel](https://img.shields.io/badge/Laravel-12_%7C_13-FF2D20?logo=laravel&logoColor=white)](https://laravel.com/)

---

## What this is

This organization is a small set of Laravel packages, written and maintained over the past few years to handle the kind of recurring needs you run into every Laravel project — comment systems, model-level settings, approval workflows, vector search, validation rules — without dragging a kitchen-sink framework into your app.

Every package follows the same four principles:

- **Focused.** Each package does one thing well — no kitchen-sink libraries.
- **Tested.** Comprehensive test coverage across the supported PHP and Laravel versions.
- **Maintained.** Active packages track Laravel's release cycle; deprecated versions are dropped on schedule.
- **Simple to adopt.** Sensible defaults, minimal configuration, conventional Laravel APIs. If you've used Laravel, you already know how these feel.

Active packages target **PHP 8.2+** and **Laravel 12 or 13**. A few legacy packages are still around for older projects — see the bottom of this page for those.

---

## The flagship: vector embeddings

The biggest piece of work in this org is the **`embedding` ecosystem** — a full vector embedding and similarity-search system for Laravel Eloquent models.

The idea is simple: you mark a model as embeddable, tell it which fields to embed, and the package handles the rest — generating embeddings via `laravel/ai`, storing them, keeping them in sync when the source fields change, and giving you a `similarTo*()` query API to find semantically similar records.

```php
class Post extends Model implements HasEmbeddings {
    use Embeddable;

    protected array $embeddable = ['title', 'body'];

    public function toEmbeddingText(): string {
        return $this->title . ' ' . $this->body;
    }
}

$post->embed();                          // dispatches a queued job
Post::similarToText('php frameworks', limit: 10);
```

The clever part is the **driver model.** Similarity search runs natively on whichever database you happen to be using — and there's a separate driver package for each one.

| Driver | What it targets |
| --- | --- |
| [`embedding`](https://github.com/x-laravel/embedding) | The core. Includes a built-in PHP fallback that works on any database. |
| [`embedding-pgsql-plugin`](https://github.com/x-laravel/embedding-pgsql-plugin) | PostgreSQL with the **pgvector** extension. |
| [`embedding-mysql-plugin`](https://github.com/x-laravel/embedding-mysql-plugin) | MySQL 9's native **`VECTOR`** type (HeatWave). |
| [`embedding-mariadb-plugin`](https://github.com/x-laravel/embedding-mariadb-plugin) | MariaDB 11.7's native vector support. |
| [`embedding-sqlsrv-plugin`](https://github.com/x-laravel/embedding-sqlsrv-plugin) | SQL Server 2025's native vector similarity. |
| [`embedding-oracle-plugin`](https://github.com/x-laravel/embedding-oracle-plugin) | Oracle 26ai's vector similarity. |
| [`embedding-qdrant-plugin`](https://github.com/x-laravel/embedding-qdrant-plugin) | The Qdrant vector database. |

Pick whichever your stack already uses. If you're not on any of these, the core package's PHP fallback still works — it's slower for large datasets but identical in behavior.

> Embedding requires **PHP 8.3+** and **Laravel 12 or 13**, and depends on `laravel/ai`.

---

## Eloquent helpers

The kind of features you end up rebuilding in every project. Each one is a small, single-purpose package.

### [`commentable`](https://github.com/x-laravel/commentable) — Polymorphic, nested comments

Drop a `Commentable` trait on any Eloquent model and it accepts comments. Drop a `Commenter` trait on your `User` (or any other model) and it can post them. Replies nest to any depth, with lazy loading. Soft-delete with cascade options is built in. Anonymous commenting is supported when you don't have a logged-in commenter.

### [`eloquent-approval`](https://github.com/x-laravel/eloquent-approval) — Approval workflow

A three-state workflow (`pending`, `approved`, `rejected`) for any Eloquent model. New records default to `pending` and stay out of standard queries until they're approved. If a record is later edited in a way that changes a flagged attribute, it automatically goes back to `pending` for re-review. Includes events, query scopes, factory states, and a controller trait for handling approval requests over HTTP.

### [`eloquent-settings`](https://github.com/x-laravel/eloquent-settings) — Per-model settings

A flexible JSON-backed settings store for any model. Use dot notation (`$user->settings()->get('theme.colors.primary')`), define defaults via `$defaultSettings`, restrict to a whitelist via `$allowedSettings`, or split settings into multiple groups via related models. Useful any time you want "configurable per-row" without a custom table.

---

## Integrations & infrastructure

### [`pulse-agent`](https://github.com/x-laravel/pulse-agent) — Standalone metrics collector

This one is **not** a Composer package — it's a Docker container. It runs separately from your Laravel app, connects to your existing Pulse database (and optionally other servers over SSH), and writes server metrics directly to the Pulse tables every 15 seconds. Your application code stays untouched. Useful when you want Pulse dashboards but don't want the agent running inside your web/worker containers.

### [`pulse-oci-mysql`](https://github.com/x-laravel/pulse-oci-mysql) — Pulse on Oracle Cloud MySQL

Oracle Cloud MySQL doesn't allow `md5()` inside generated columns, which breaks Pulse's default schema. This package swaps the generated `key_hash` column for a plain `char(32)` and computes the hash in PHP instead. Drop-in fix — install, run the migration, Pulse works.

### [`spatie-media-library-uuid-path`](https://github.com/x-laravel/spatie-media-library-uuid-path) — UUID-distributed file paths

`spatie/laravel-medialibrary` stores files under directories named by their numeric ID — `1/`, `2/`, `3/`, and so on. That works fine until you have a few hundred thousand items in one parent folder and the filesystem starts getting unhappy, or until someone realizes your media URLs are sequentially enumerable. This generator distributes files across a four-level hierarchy derived from a UUID, so each directory holds at most 256 subdirectories and URLs aren't guessable.

### [`listmonk`](https://github.com/x-laravel/listmonk) — Listmonk client for Laravel

A clean wrapper around the [Listmonk](https://listmonk.app/) self-hosted newsletter platform's REST API. The interesting bit is automatic syncing: hook a model with the right trait and its create/update/delete/restore events become subscriber updates in Listmonk, queued by default. There's also direct API access for subscribers and lists, lifecycle events, Artisan commands for health checks and bulk syncs, and testing utilities for mocking calls.

---

## Made for Turkish projects

Three small packages that do one thing each, originally written in 2020. They still work and are still useful for any Laravel project dealing with Turkish data — just don't expect new features.

| Package | What it does |
| --- | --- |
| [`validation-tr-citizen-number-extend`](https://github.com/x-laravel/validation-tr-citizen-number-extend) | Adds a `tr_citizen_number` validation rule that validates **TC kimlik numarası** (Turkish citizen ID). |
| [`validation-tr-tax-number-extend`](https://github.com/x-laravel/validation-tr-tax-number-extend) | Adds a `tr_tax_number` validation rule for **vergi numarası** (Turkish tax number). |
| [`str-tr-extend`](https://github.com/x-laravel/str-tr-extend) | Extends the `Str` facade with `trUpper`, `trLower`, `trUcFirst`, `trLcFirst`, `trUcWords` — Turkish-aware case conversion that handles `ç`, `ğ`, `ı`, `ö`, `ş`, `ü` correctly (PHP's built-ins don't). |

Older Laravel compatibility (5.5+ for the validators), MIT-licensed.

---

## Installing

Every active package is published on Packagist and follows the same install pattern:

```bash
composer require x-laravel/<package-name>
```

Each repository has its own README with setup, configuration, migration steps, and usage examples.

---

## Conventions

- **One concern per package.** No mega-bundles. If two things end up bundled, they get split.
- **Convention over configuration.** Service providers register automatically via package discovery. Migrations and config can be published when you need to customize.
- **Tests run on every supported version.** When Laravel drops a version from its release schedule, we drop it from CI on the next minor release.
- **Breaking changes go in major versions.** Semver is taken seriously.

---

## A note on archived packages

A few packages in this org are **archived** — the predecessor `model-settings-bag`, plus older `validation-extend`, `validation-color-extend`, and `str-extend`. They're public for historical reference but no longer maintained. If you're starting a new project, prefer their replacements (`eloquent-settings`, `str-tr-extend`, etc.) or look outside this org.

---

## Contributing

Issues and pull requests are welcome on every active repository. Before opening a PR:

1. Read the package's `CONTRIBUTING.md` if present.
2. Make sure tests pass: `composer test`.
3. Keep changes focused — one concern per PR.

Bug reports and feature requests go on the relevant package's issue tracker, not here.

---

## License

Unless stated otherwise on a specific repository, all packages are released under the [MIT License](https://opensource.org/licenses/MIT).
