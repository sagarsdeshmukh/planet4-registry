# Greenpeace Planet 4 Registry.
This repository contains the Planet 4 composer registry. 
All plugins and themes that are available inside the Planet 4 system will be
published here. 

It is based on [Satis](https://github.com/composer/satis).

## Proof of concept
Be advised that this registry is a work in progress and the current 
implementation of combining different sources is just a proof of concept. 

## How does it work
This registry is powered by two independent systems that are combined together
with the `composer run build` script. 

### VCS Support
Inside the `satis.json` file the `repositories` section is used to define 
repositories that support Composer. 
Each repository is required to contain a `composer.json` file. 

More information about how to use this file is available inside the 
[Satis documentation](https://getcomposer.org/doc/articles/handling-private-packages-with-satis.md#satis).

### Static
The Wordpress Core, many plugins and themes don't support Composer out of the
box. To be able to use them with a satis registry we need to enhance them 
manually. The `packages` folder of this repository contains all such 
dependencies.

The folder name is the name of the dependency and it contains multiple JSON 
files that reference the published version we did test and support.

As [Composer cannot load repositories recursively](https://getcomposer.org/doc/faqs/why-can%27t-composer-load-repositories-recursively.md)
we need to manage the source code and the download manually. Each file should
reference only one repository of the `package` type. 

### Continuous Integration
Once a continuous integration server is available, the management of this static
package list should be handled automatically. As soon as a new repository is 
created and push, the CI server should collect this `composer.json` files from 
them and store them in the `packages` directory. This will make sure all new
versions of Wordpress, the plugins and themes will appear in our registry, 
without each of them having direct Composer support.

## Installation
The installation if this registry can be done in three simple steps:

1. Install the dependencies: `composer install`
2. Build the static Satis registry `composer run build`
3. Add the web address to a `composer.json` file to be used

Internally this will combine the `satis.json` with the packages from the 
`packages` folder into a `satis.extended.json`. This file will be used by 
Satis to generate the static registry.
