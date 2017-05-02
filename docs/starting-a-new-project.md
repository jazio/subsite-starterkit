## Starting a new project

Your new project will be based on the Subsite Starterkit but will live in its
own repository on GitHub. We are in fact using two repositories: 'dev' and
'reference':

- _'dev'_ repository: This is the repository where the development is done. On
  request we can give access to other developers (within the EC or external
  contractors) to this repository so that code can be shared effectively. This
  repository has 'master' and 'develop' branches as well as feature branches
  where the actual development is taking place. The 'master' branch will contain
  the latest code that is deployed on production.
- _'reference'_ repository: This is the 'official' repository which is used as a
  source for building the sites that are deployed on production. This is guarded
  by the internal QA team who will validate all pull requests and only allow the
  code to be merged in if it meets our quality guidelines. The official
  reference repositories are hosted on the internal git repository of the
  European Commission at https://webgate.ec.europa.eu/CITnet/stash/projects and
  these are mirrored on GitHub for convenience.

### 1. Create a new repository on GitHub

There are two situations:

A. If you are a ec-europa team member an need to register a new myproject-reference repository 

1. Log in to GitHub

_Note: You need to decide whether you repository will be private or public. 
We encourage developers to make their code public, but you will need to consult your
client and ask if they agree with it. If the code is public, take care not
to commit any sensitive data such as e-mail addresses, API keys or
passwords. Don't add a README or any other files, just leave it empty._

2. Go to https://github.com/ec-europa and click on "New repository" to create
   your repository. If you do not have access to this then please ask your
   project manager to create a team for you.

```
git clone https://github.com/ec-europa/myproject-reference.git
cd myproject-reference
```

3. Add starterkit and push back remotely
```
git remote add starterkit https://github.com/ec-europa/subsite-starterkit.git
git fetch starterkit
git merge starterkit/master
git push origin master
```


B. Or, if you are a developer and need to start a new project, 


1. Fork my-project-reference repository into your own. Go to the https://github.com/ec-europa/myproject-reference
and click on the Fork button. Fork it under your account.

2. Choose a repository name for your project, making sure it ends in '-dev': e.g. growth-dev
   You can rename it from your project Settings section.

```
git clone https://github.com/ec-europa/myproject-dev.git
cd myproject-dev
```

Optionally add starterkit:

```
git remote add starterkit https://github.com/ec-europa/subsite-starterkit.git
```
3. Check the URL of your repository, which will look
   similar to this: `https://github.com/ec-europa/myproject-dev.git`. 

From now on everytime you need to submit your work create from the github interface a pull request 
from your _'dev'_ repository to the _'reference'_ repository.


### 3. Create a build.properties file

Create a new file called `build.properties` in the root folder and put
properties in it that are unique to your project. You can copy any of the
properties of the `build.properties.dist` file to override them and then commit
the file. The settings will then take effect for all developers that work on the
project.

Some typical project specific settings are the site name, the install profile,
the modules to enable after installation, paths to ignore during coding
standards checks, the version of the platform to use etc.

Example file:

```
# The site name.
subsite.name = My Project

# The install profile to use.
platform.profile.name = multisite_drupal_standard

# The branch, tag or commit to use, eg. 'master', 'develop', '1.7', '7df0d254b'.
# It is possible to use MySQL style wildcards here.
platform.package.reference = master

# Modules to enable after installation. Separate multiple modules with spaces.
# This will typically be the 'mother feature' which will contain all your other
# features and modules as dependencies.
subsite.install.modules = myproject_core
```

### 4. Install Composer dependencies

The starterkit uses Composer to manage its dependencies, but it does not include
a `composer.lock` file to leave developers free to manage the exact versions of
the dependencies themselves.

It is highly recommended to install the composer dependencies at the start of
the project and then "lock" them by committing the `composer.lock` file. This
will ensure that all members of the team will always use the same versions of
the software used to build the website. From time to time you can update the
dependencies to benefit from improvements in new releases.

```
# Install composer dependencies.
$ composer install

# Commit the composer.lock file.
$ git add composer.lock
$ git commit -m "Lock composer dependencies."
```

### 5. Set up PHP CodeSniffer

When you use the starterkit, your code will automatically be scanned for coding
standard violations by PHP CodeSniffer on every push. We need to configure this
now so that we can push to the repository. This is done with a single command:

```
$ ./bin/phing setup-php-codesniffer
```

If this is omitted PHP CodeSniffer will scan your `composer.lock` and
`build.properties` files and throw a false positive error: "No PHP code was
found in this file".

### 6. Commit and push

Now finally commit your configuration file and push it to your repository. Your
project is now ready to use.

```
$ git add build.properties
$ git commit -m "Add project specific build properties."
$ git push
```
