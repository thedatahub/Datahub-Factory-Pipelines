# Datahub-Factory-Pipelines

This repository contains several example pipeline configurations for the [Datahub::Factory](https://github.com/thedatahub/Datahub-Factory) application. You could use them as a boilerplate to create your own pipelines

## Pipelines

A pipeline is a connection between two system used to exchange records. A pipeline has three functions:

* Fetch data from a source
* Transform the data. This entails manipulating the structure of the data as
well as the format in which the data will be delivered to a destination.
* Push transformed data to a destination.

## Pipeline configuration

The definition of a pipeline is contained in a configuration file. These configuration files are based on the [Config::Simple](http://search.cpan.org/~sherzodr/Config-Simple-4.59/Simple.pm) syntax. A configuration file roughly defines these three 'plugin' types and their associated configuration in distinct blocks:

* Importer. Defines the source of the data and how to fetch it.
* Fixer. Defines the [Catmandu::Fix](http://search.cpan.org/~nics/Catmandu-1.0507/lib/Catmandu/Fix.pm) logic that will transform the data
* Exporter. Defines how the destination for the data and how it can be accessed it.

Note that each plugin instance needs to be referenced explicitely in a global plugin block. Each instance gets it's own dedicated plugin definition.

A very minimal configuration would look like this:

```
[General]
id_path = 'id'

[Importer]
plugin = JSON

[plugin_importer_JSON]
file_name = './data/bar.json'

[Fixer]
plugin = Fix

[plugin_fixer_Fix]
file_name = './fixes/empty.fix'

[Exporter]
plugin = YAML

[plugin_exporter_YAML]

```

## Conditions

The pipeline also accept Fixer "conditions". Since a collection can contain multiple
sets of records that require different transformation processing, "conditions"
allow you to determine which Fix will be applied to a given record based on a
conditional statement.

A condition would look like this:

```
[Fixer]
plugin = Fix

[plugin_fixer_Fix]
condition = "_metadata.institution\\.name.value"
fixers = Foo, Bar

[plugin_fixer_Foo]
condition_path = 'Museum of Foo'
file_name = '/home/foobar/fixes/foo.fix'

[plugin_fixer_Bar]
condition_path = 'Museum of Bar'
file_name = '/home/foobar/fixes/bar.fix'
```

## Technology

Datahub::Factory is an application based on [Catmandu](http://librecat.org). As
such, pipeline configurations are based on Catmandu concepts and terminology.

## Authors

* Pieter De Praetere <pieter@packed.be>
* Matthias Vandermaesen <matthias.vandermaesen@vlaamsekunstcollectie.be>

## Copyright

Copyright 2016, 2019 - PACKED vzw, Vlaamse Kunstcollectie vzw

## License

This library is free software; you can redistribute it and/or modify
it under the terms of the GPLv3.
