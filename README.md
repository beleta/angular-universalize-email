Use angular universal to generate email templates
=================================================

angular-universalize-email is a small utility that allows an angular project to easily generate email templates using angular universal.

Basically you create emails just like you create any other angular pages. Don't worry about the styles and scripts, it will be taken care
by the script.

Prerequisites
-------------

1. The project must be an angular project.
2. The project must have added angular universal support
3. You have created your email templates using angular
4. These email templates must have routing defined.

To make life easier, I'll recommend you separate your primary angular project with your email template angular project. Most of the time,
the emails are not needed by the primary angular project, so it's a good idea not to mix them together.

With angular 7+, it's as easy as executing the below to create a new sub-project.

    ng generate application <application name>

You can universalize this sub-project the same way you universalize the primary application. Check
[this guide](https://medium.com/@sohoffice/angular-universal-an-adventure-9d969d401072) should you require a reference.

Install
-------

    npm install angular-universalize-email --save-dev

A executable `angular-universalize-email` will be installed to your node_modules/.bin folder.
You may run the executable by explicitly refer to the path as `./node_modules/.bin/angular-universalize-email` or add it to your package.json as a script.

    "scripts": {
      ...
      "gen:email": "angular-universalize-email -a ./dist/foo-email -A ./dist/foo-email-server -o tmp -m EmailAppServerModule '/email/bar'"
    }

Executing
---------

    # Supposed we have a `foo-email` sub application, and universalized as `foo-email-server`.
    # The email to generate is located in the routing '/email/bar'.
    angular-universalize-email -a ./dist/foo-email -A ./dist/foo-email-server -o tmp '/email/bar'

    # The output will be generated at tmp/email-bar.html

If you have renamed your server module, use -m to specify the module name.

    angular-universalize-email -a ./dist/foo-email -A ./dist/foo-email-server -o tmp -m EmailAppServerModule '/email/bar'

Command line options
--------------------

```
usage: angular-universalize-email [-h] [-v] -a BROWSERASSET -A SERVERASSET
                                  [--bundle BUNDLE] [--index INDEX]
                                  [-o OUTPUTDIR] [-m MODULENAME] [-p PATTERN]
                                  [-P PREPEND]
                                  url

angular-universalize-email, a small utility that allows an angular project to
easily generate email templates using angular universal.

Positional arguments:
  url                   The url path to generate current email.

Optional arguments:
  -h, --help            Show this help message and exit.
  -v, --version         Show program's version number and exit.
  -a BROWSERASSET, --asset BROWSERASSET
                        The directory contains the angular browser asset.
  -A SERVERASSET, --server-asset SERVERASSET
                        The directory contains the angular server asset.
  --bundle BUNDLE       The angular universal server bundle name, default to
                        `main`
  --index INDEX         The entry html filename, default to `index.html`
  -o OUTPUTDIR, --output-dir OUTPUTDIR
                        The output directory
  -m MODULENAME, --module-name MODULENAME
                        The email server module name. default to
                        `AppServerModule`.
  -p PATTERN, --pattern PATTERN
                        The output file pattern, url can be used as a
                        substitute variable. Example: `prefix-{dashed}.html`
                        or `{camel}.scala.html`. Currently only camel and
                        dashed conversion are supported.
  --prepend PREPEND     Additional text to be added in the beginning of
                        generated html. Useful if generated for other
                        framework.
```

Advanced usage
--------------

#### Prepend text

If you are generating email template for other framework, you may want to prepend some text before the generated html.

For example, to generated a template for play framework(java/scala), something similar to the below may be required:

    @(user: io.User, link: String)

In which case, use the --prepend argument to instruct 'angular-universalize-email' to add such line in the beginning.

#### Filename pattern

The default filename is '{dashed}.html'. What does this mean exactly ?

The URL used to generate current template will be converted into dashed string. For example: /email/foo-bar will be converted
as `email-foo-bar`. This converted value is substituted into this pattern to make the result: `email-foo-bar.html`.

You may also use `camel` conversion, the same example will be converted into `emailFooBar.html`.

At the moment, dashed and camel are the only supported conversions.