<div align="center">

<img src="https://raw.githubusercontent.com/x-laravel/.github/master/assets/x-laravel-logo.png" width="360" alt="x-laravel" />


**Focused, tested, well-maintained Laravel packages.**
Solving real-world problems — simply, cleanly, for the long run.

[![PHP](https://img.shields.io/badge/PHP-8.2%2B-777BB4?style=flat-square&logo=php&logoColor=white)](https://www.php.net)
[![Laravel](https://img.shields.io/badge/Laravel-12_%7C_13-FF2D20?style=flat-square&logo=laravel&logoColor=white)](https://laravel.com)
[![License](https://img.shields.io/badge/License-MIT-22c55e?style=flat-square)](https://opensource.org/licenses/MIT)
[![Packagist](https://img.shields.io/badge/Packagist-x--laravel-F28D1A?style=flat-square&logo=packagist&logoColor=white)](https://packagist.org/packages/x-laravel/)

</div>

---

<table align="center"><tr><td align="center" width="33%">
<h3>🎯</h3><b>Focused</b><br/><sub>One package, one job</sub>
</td><td align="center" width="33%">
<h3>🧪</h3><b>Tested</b><br/><sub>Across all supported versions</sub>
</td><td align="center" width="33%">
<h3>🔄</h3><b>Maintained</b><br/><sub>Tracks Laravel's release cycle</sub>
</td></tr></table>

---

## ⚡ Flagship — Vector embeddings for Eloquent

Mark a model as embeddable, pick a database driver, get semantic search.

```php
class Post extends Model implements HasEmbeddings {
    use Embeddable;
    protected array $embeddable = ['title', 'body'];
}

Post::similarToText('php frameworks', limit: 10);
```

<table>
<tr><td><b>Core</b></td><td>

[`embedding`](https://github.com/x-laravel/embedding) [![v](https://img.shields.io/packagist/v/x-laravel/embedding?style=flat-square&label=)](https://packagist.org/packages/x-laravel/embedding)

</td></tr>
<tr><td><b>Drivers</b></td><td>

[`pgsql`](https://github.com/x-laravel/embedding-pgsql-driver) · [`mysql`](https://github.com/x-laravel/embedding-mysql-driver) · [`mariadb`](https://github.com/x-laravel/embedding-mariadb-driver) · [`sqlsrv`](https://github.com/x-laravel/embedding-sqlsrv-driver) · [`oracle`](https://github.com/x-laravel/embedding-oracle-driver) · [`qdrant`](https://github.com/x-laravel/embedding-qdrant-driver)

</td></tr>
</table>

> Built on `laravel/ai`. Requires **PHP 8.3+** and **Laravel 12 / 13**.

---

## 🧩 Eloquent helpers

<table>
<tr>
<td width="33%" valign="top">

### 💬 [commentable](https://github.com/x-laravel/commentable)
[![v](https://img.shields.io/packagist/v/x-laravel/commentable?style=flat-square&label=)](https://packagist.org/packages/x-laravel/commentable)

Polymorphic comments with **nested replies** and anonymous-commenter support.

</td>
<td width="33%" valign="top">

### ✅ [eloquent-approval](https://github.com/x-laravel/eloquent-approval)
[![v](https://img.shields.io/packagist/v/x-laravel/eloquent-approval?style=flat-square&label=)](https://packagist.org/packages/x-laravel/eloquent-approval)

Pending / approved / rejected workflow with **auto re-suspension** on edits.

</td>
<td width="33%" valign="top">

### ⚙️ [eloquent-settings](https://github.com/x-laravel/eloquent-settings)
[![v](https://img.shields.io/packagist/v/x-laravel/eloquent-settings?style=flat-square&label=)](https://packagist.org/packages/x-laravel/eloquent-settings)

JSON-backed per-model settings with **dot notation** and groups.

</td>
</tr>
</table>

---

## 🔌 Integrations

| | Package | What it does |
|---|---|---|
| 🐳 | [**pulse-agent**](https://github.com/x-laravel/pulse-agent) | Docker container that writes server metrics directly to Laravel Pulse — no app changes |
| 🔧 | [**pulse-oci-mysql**](https://github.com/x-laravel/pulse-oci-mysql) | Pulse compatibility for Oracle Cloud MySQL (fixes `md5()` generated columns) |
| 📁 | [**spatie-media-library-uuid-path**](https://github.com/x-laravel/spatie-media-library-uuid-path) | UUID-based file paths for spatie/laravel-medialibrary — scalable & non-enumerable |
| 📬 | [**listmonk**](https://github.com/x-laravel/listmonk) | Listmonk newsletter client with auto-sync from Eloquent models |

---

## 🚀 Install any package

```bash
composer require x-laravel/<package-name>
```

> Each repo's README has setup, config, migrations, and usage examples.

---

<details>
<summary><b>🇹🇷 Turkish-specific packages (legacy but functional)</b></summary>

<br/>

| Package | Adds |
|---|---|
| [`validation-tr-citizen-number-extend`](https://github.com/x-laravel/validation-tr-citizen-number-extend) | `tr_citizen_number` rule (TC kimlik doğrulama) |
| [`validation-tr-tax-number-extend`](https://github.com/x-laravel/validation-tr-tax-number-extend) | `tr_tax_number` rule (vergi numarası) |
| [`str-tr-extend`](https://github.com/x-laravel/str-tr-extend) | `Str::trUpper / trLower / trUcFirst / trLcFirst / trUcWords` — Turkish-aware case conversion |

Older Laravel compatibility (5.5+). Still works, no new features planned.

</details>


---

<div align="center">

### 🤝 Contributing

Issues and PRs welcome on every active repo. Bug reports go on the relevant package, not here.

<br/>

[![GitHub](https://img.shields.io/badge/All_repos-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/orgs/x-laravel/repositories)
[![Packagist](https://img.shields.io/badge/Packagist-F28D1A?style=for-the-badge&logo=packagist&logoColor=white)](https://packagist.org/packages/x-laravel/)

<sub>MIT licensed · Made for Laravel developers who like clean code</sub>

</div>
