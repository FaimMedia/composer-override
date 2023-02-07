# Composer Override

Allows to use a `composer.override.json` next to your `composer.json` file for external package management. This is useful, in example, when you need to have packages symlinked for local development, but not for production.

This tool will not use an existing `composer.lock`-file, so pulling may be slower. It will also not modify the existing lock file, if you need this, you can pass `--copy-lock` as parameter, and will copy the created lock file to the directory where the tool is run from.

## Install

Please note that `php-cli` 5.6+ should be installed on your system, as well as [https://getcomposer.org/download/](Composer).

```bash
wget https://raw.githubusercontent.com/FaimMedia/composer-override/master/composer-override -nv -O composer-override

php -r "if(hash_file('md5', 'composer-override') !== '9f27f51d17f0313f2ff8f8609f5d39f2') { print \"Invalid checksum\r\n\"; exit(1); }" \
	&& sudo mv composer-override /usr/local/bin/composer-override \
	&& sudo chmod +x /usr/local/bin/composer-override
```

## Usage

`composer.json`
```json
{
	"name": "faimmedia/my-app",

	"require": {
		"faimmedia/my-library": "^1.0"
	}
}
```

Next create a `composer.override.json` file next to the `composer.json` file:

`composer.override.json`
```json
{
	"repositories": {
		"faimmedia/my-library": {
			"type": "path",
			"url": "../faimmedia-my-library",
			"symlink": true
		}
	}
}
```

When running `composer-override` it will merge the two files together, giving precedence for the override file, and will now create a symlink to a local directory, instead of pulling the package from Packagist.

You may override or add any value, in example:

`composer.override.json`
```json
{
	"require": {
		"faimmedia/my-library": "dev-master"
	},

	"config": {
		"platform-check": false
	}
}
```
