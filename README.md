![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)
![Docker](https://badgen.net/badge/icon/docker?icon=docker&label)
![PHP](https://badgen.net/badge/php/8.3/green?icon=php)
![MySQL](https://badgen.net/badge/mysql/8.0/green?icon=php)
![MySQL](https://badgen.net/badge/laminas/3.7/orange)

# PCR

1. [Synopsis](#Synopsis)
2. [Required Software](#Required_Software)
3. [Installation](#Installation)
4. [Development](#Development)
   1. [Code Standards](#Code_Standards)
   2. [Pushing Changes](#Pushing_Changes)
5. [Troubleshooting](#Troubleshooting)
6. [License](#License)

## Synopsis <a name="Synopsis"></a>

Simplified cloud-based patient care record system.
The expected user base would include small first-aid companies, school nurses, first responders who are based on-site for companies etc.

## Required Software <a name="Required_Software"></a>

* [Docker Desktop](https://www.docker.com/products/docker-desktop/)
* [Docker Composer CLI utility](https://docs.docker.com/compose/install/)

## Installation <a name="Installation"></a>

1. If you are using Windows:
   1. Run `appwiz.cpl` and select "Turn Windows features on or off".  Select the following:
      * Hyper-V
      * Virtual Machine Platform
      * Windows Hypervisor Platform
      * Windows Subsystem for Linux
   2. Set the default version to Debian: `wsl.exe --set-default-version debian`
2. Clone the code base.
3. Find and fix any "FIXME" items within the source code.
4. Create an env.txt file with the following settings that Docker will need.
**DO NOT ADD THIS FILE TO THE REPO!!!!!**
```
APPLICATION_ENV=local
DEVELOPER_ENV=your_user_name_here
MYSQL_ROOT_PASSWORD=your_local_dev_mysql_root_password
```
5. Build and spool the Docker containers:
```bash
cd project_folder
docker-compose --env-file <PATH TO YOUR .ENV FILE CREATED IN STEP 3> build --progress=plain
docker-compose --env-file <PATH TO YOUR .ENV FILE CREATED IN STEP 3> up -d
```
6. Open an ssh terminal connection to the new CLI and execute the following:
```bash
cd /var/www
composer install
```
7. Run Doctrine database migrations to verify that the development schema being used is the current one.

## Development <a name="Development"></a>

### Code Standards <a name="Code_Standards"></a>

* [PHP PSR Standards](https://www.php-fig.org/psr/)
* MySQL
  * Use lower case names with underscores for tables and fields (I.E: `customer_id`) 
  * Use singular forms for tables (I.E:  `invoice`, **not** `invoices`).
  * Auto-increment fields should be unsigned integers.
  * Indexes - use the following prefixes.

    | Prefix | Description     | Example |
    |--------|-----------------|---------|
    | `idx`  | Regular indexes | `idx_customer_name` |
    | `pk` | Compound primary keys | `pk_customer` |
    | `fk`    | Foreign keys    | `fk_customer` |

### Pushing Changes <a name="Pushing_Changes"></a>

1. Use the composer scripts to scan for code issues before pushing any changes.
Fix any PSR12 issues, issues found from PHPStan or from PHPUnit before pushing.
```bash
cd /var/www
composer cs-check
composer stan
composer test
```

## Troubleshooting <a name="Troubleshooting"></a>

TBD

## License <a name="License"></a>

MIT License - see the [LICENSE](LICENSE.md) file for details
