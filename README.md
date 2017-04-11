# Gradle Project Example

## PreRequirements

* First of ALL, save [`init.gradle`](http://git.euler.one/snippets/8) to you ~/.gradle/init.gradle

* It will use your `.m2/settings.xml` to identify the deployer's credentials.

## Usage & Features

* `gradle publish`

Automatically publish your artifact's to nexus repository, according to the **SUFFIX** of the artifact.

* `gradle cV` or `gradle currentVersion`

Powered by [Axion Release](https://github.com/allegro/axion-release-plugin)