# README

## Getting Started (using Docker Compose)

The easiest way to get VideoShelter...
**Download the videoshelter.zip and unzip it.**

**Git is giving hard time to push the code to repo, hence uploaded compressed file**

### System Requirements

 * Ruby 2.7.5
 * npm 8.19.3
 * psql (PostgreSQL) 15.1
    * The application is confirmed to work with version 15.1
 * Java
 * macOS Catalina
    * `bundle install` will not work for Big Sur from a clean slate
    * There are changes in Big Sur that prevent the `zxing_cpp` gem from installing when running `bundle install`
    * Versions of macOS earlier than Catalina may work
 * Dependency ffmpegthumbnailer
 	* Install ffmpegthumbnailer for MacOs
 	```sh
	brew install ffmpegthumbnailer
	```
	* Install ffmpegthumbnailer for Linux
	```sh
	sudo apt install ffmpegthumbnailer
	```
 * Database creation
 	```sh
	$bundle exec rails db:create
	$bundle exec rails db:migrate
	```
 * Database initialization
	```sh
	bundle exec rails db:seed
	```
 * How to run the test suite
	```sh
	bin/bundle exec rspec spec
	```

### Running the Application

1. Start the server in one terminal session
    * OneTime execution to load esbuild
      `./bin/rails javascript:install:esbuild`
    * Make sure to validate file Procfile.dev, it should have below content
      ```sh
      web: bin/rails server -p 3000
      js: yarn build --watch
      ```
    * Start The App Server
      `./bin/dev`
2. Navigate to [http://localhost:3000/]()
4. Login using the following credentials:
    * Email Address: `admin1@videoshelter.com`
    * Password: `123456`

#######################################################################################################
#### Installing Ruby

We will use [rbenv][rbenv] to manage and install different Ruby versions. Install the appropriate version of Ruby using the follow steps. See the [documentation][rbenv-installation] for more details:

```sh
brew install rbenv
rbenv init
```

#### Setting Ruby Version

Here are a few useful `rbenv` commands:

 * `rbenv global`
    * Show what version of Ruby you have running globally
 * `rbenv install 2.7.5`
    * Install Ruby 2.7.5
 * `rbenv global 2.7.5`
    * Set the global Ruby version to 2.7.5. You probably should not do this.

#### Install PostgreSQL

1. Install PostgreSQL
    * `brew install postgresql@15` or download from the [Postgres site][postgres-download]
2. Verify PostgreSQL is on your `PATH` and is the correct version
    * `psql --version`
3. Create a PostgreSQL superuser
    * `createuser -s postgres`
4. Verify superuser created successfully
    * `psql --list`

#### Installing & Configuring Java

```sh
brew tap adoptopenjdk/openjdk
brew update
brew install --cask adoptopenjdk/openjdk/adoptopenjdk8
```

**macOS**

Additionally, if you are using **macOS**, you may be prompted to **"install the legacy Java SE 6 runtime"**.

The reason for that prompt is that by default JDK from Oracle only enables the CommandLine capability but the RJB library (dependency for Aspose) needed JNI and possibly more capabilities because of which macOs was suggesting me to install the old version of the Java from Apple which had those capabilities enabled by default.

The solution is to modify the file at `/Library/Java/JavaVirtualMachines/<current-java-folder>/Contents/Info.plist`. You will need to add additional entries to the `JVMCapabilities` array as shown below.

```xml
<key>JVMCapabilities</key>
<array>
  <string>CommandLine</string>
  <string>JNI</string>
  <string>BundledApp</string>
  <string>WebStart</string>
  <string>Applets</string>
</array>
```

**Linux**

If you are using **Linux** for development, Java versions higher than 11 have JRE embedded into JDK and some of the application's dependencies will look for the `jre` folder under your java installation.

You will need to create a symlink by running the commands:

```sh
$ mkdir -p /PATH/TO/JVM/default-java/jre/lib/amd64
$ ln -s /PATH/TO/JVM/default-java/lib/server /PATH/TO/JVM/default-java/jre/lib/amd64/server
```

In all environments, you should set the config value `java_home_path` to the
home directory for Java (This should be the same as your `$JAVA_HOME`
environment variable)

In your config/config.yml file that will look like this:

```
java_home_path: '/usr/lib/jvm/java-8-oracle'
```
