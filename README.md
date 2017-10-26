# Gemnasium Maven Plugin

The Gemnasium maven plugin helps you manage your projects dependencies with Gemnasium. Gemnasium keeps track of projects dependencies and send notifications when new versions are released or security advisories are published.

# DRAFT NOTICE

This plugin is not yet available. Coming soon.

## How to install it?

The Gemnasium Maven Plugin is published on the Maven Central Reposoritory so you just need to add the plugin to your project's `pom.xml`:

```xml
<build>
  <plugins>
    <plugin>
      <groupId>com.gemnasium</groupId>
      <artifactId>gemnasium-maven-plugin</artifactId>
      <version>1.0</version>
    </plugin>
  </plugins>
</build>
```

## Requirements?

The Gemnasium Maven Plugin is compatible with Gemnasium API V2, which is only available on https://beta.gemnasium.com for now. If you want to apply for the beta program, please contact us at http://support.gemnasium.com/

## How to use it?

Before using the plugin, you'll need to configure at least your Gemnasium API KEY (see [Configuration](#configuration) section below). 

### Ping

You can ping the Gemnasium API to ensure everything works fine.

    mvn gemnasium:ping

### Create a new project

To create a new project on Gemnasium, you need to run

    mvn gemnasium:create-project -DteamSlug="my-team" -DprojectName="My new project" -DprojectDescription="My new project description"

After a successful creation, the projectSlug will be saved in the `src/main/resources/gemnasium.properties` file.

### Configure an existing project

If your project is already on Gemnasium, you just need to provide the `projectSlug` configuration option (see [Configuration](#configuration) section below). Your project's Slug is available in your project settings on Gemnasium.

### Update your project's dependencies

Once you've setup your project, you're ready to send its dependencies to Gemnasium so that they can be monitored for security issues and updates.

    mvn gemnasium:update-dependencies

The Gemnasium Maven Plugin analyses the dependency graph used by Maven to install the dependencies of your project and send these informations to the Gemnasium API.

## Show project

To show your project details you need to run:

    mvn gemnasium:show-project


## Configuration

There are 3 ways to configure the Gemnasium Maven Plugin, in ascending priority order:

### Properties file

The configuration can be saved in `src/main/resources/gemnasium.properties` file.

```properties
apiKey=secret
projectSlug=foo/bar/baz
```

NB: Options set in `gemnasium.properties` are overriden by `pom.xml` configuration and env vars.

### Plugin configuration within pom.xml

The configuration can also be provided directly within `pom.xml`.

```xml
<build>
  <plugins>
    <plugin>
      <groupId>com.gemnasium</groupId>
      <artifactId>gemnasium-maven-plugin</artifactId>
      <version>1.0</version>
      <configuration>
        <apiKey>secret</apiKey>
        <projectSlug>foo/bar/baz</projectSlug>
      </configuration>
    </plugin>
  </plugins>
</build>
```
NB: Options set in `pom.xml` are overriden by env vars.

### Environment variables

The configuration can finally be provided via environment variables. Env vars take precedence over all other configuration sources.

    GEMNASIUM_API_KEY=secret GEMNASIUM_PROJECT_SLUG=foo/bar/baz gemnasium:show

See options details below to kow about available env vars.

### Options details

Key name | Env Var name | Description
---------- | ------- | -----------
apiBaseUrl    | GEMNASIUM_API_BASE_URL | **(Optional)** The base URL of the Gemnasium API (for Gemnasium Enteprise usage).
apiKey        | GEMNASIUM_API_KEY | **(Required)** Your Gemanisum API key (available in your Gemnasium account settings.
projectSlug | GEMNASIUM_PROJECT_SLUG | **(Required)** The project identifier on Gemnasium.
projectBranch | GEMNASIUM_PROJECT_BRANCH | **(Optional)** Current branch can be specified with this option, if the git command fails to run (git rev-parse --abbrev-ref HEAD).
projectRevision | GEMNASIUM_PROJECT_REVISION | **(Optional)** Current revision can be specified with this option, if the git command fails to run (git rev-parse --abbrev-ref HEAD).
