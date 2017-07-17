# PNDA Avro Logstash Plugin

The current Avro and Kafka output plugins are not compatible when transporting bytes.
This, hopefully, temporary, plugin patches the Avro Codec in order to make it compatible with the kafka output plugin

## Installation

* Clone this repository.
* Ensure JRuby installed. We recommend using [RVM](https://rvm.io/interpreters/jruby) to do this.
* In the cloned repository, build the plugin with `gem build logstash-codec-pnda-avro.gemspec`
* Move the generated gem to your Logstash installation.
* In your Logstash installation, install the plugin with `bin/logstash-plugin install logstash-codec-pnda-avro-3.1.1-java.gem`

## Configuration

Here is an example configuration. Please note that we use the Kafka `ByteArraySerializer`.

    input { stdin { codec => "json" } }
    
    output {
            kafka {
                    codec => pnda-avro { schema_uri => "/home/cloud-user/pnda.avsc" }
    
                    bootstrap_servers => "localhost:9092"
                    topic_id => "valid.messages"
                    compression_type => "none" # "none", "gzip", "snappy", "lz4"
                    value_serializer => 'org.apache.kafka.common.serialization.ByteArraySerializer'
            }
    }

You can then inject the following example input on the standard input (one line per event):

    { "timestamp": 1492173538000, "src": "netflow", "host_ip": "test_host", "rawdata": "this is raw data" }

# Logstash Plugin

[![Travis Build Status](https://travis-ci.org/logstash-plugins/logstash-codec-avro.svg)](https://travis-ci.org/logstash-plugins/logstash-codec-avro)

This is a plugin for [Logstash](https://github.com/elastic/logstash).

It is fully free and fully open source. The license is Apache 2.0, meaning you are pretty much free to use it however you want in whatever way.

## Documentation

Logstash provides infrastructure to automatically generate documentation for this plugin. We use the asciidoc format to write documentation so any comments in the source code will be first converted into asciidoc and then into html. All plugin documentation are placed under one [central location](http://www.elastic.co/guide/en/logstash/current/).

- For formatting code or config example, you can use the asciidoc `[source,ruby]` directive
- For more asciidoc formatting tips, see the excellent reference here https://github.com/elastic/docs#asciidoc-guide

## Need Help?

Need help? Try #logstash on freenode IRC or the https://discuss.elastic.co/c/logstash discussion forum.

## Developing

### 1. Plugin Developement and Testing

#### Code
- To get started, you'll need JRuby with the Bundler gem installed.

- Create a new plugin or clone and existing from the GitHub [logstash-plugins](https://github.com/logstash-plugins) organization. We also provide [example plugins](https://github.com/logstash-plugins?query=example).

- Install dependencies
```sh
bundle install
```

#### Test

- Update your dependencies

```sh
bundle install
```

- Run tests

```sh
bundle exec rspec
```

### 2. Running your unpublished Plugin in Logstash

#### 2.1 Run in a local Logstash clone

- Edit Logstash `Gemfile` and add the local plugin path, for example:
```ruby
gem "logstash-filter-awesome", :path => "/your/local/logstash-filter-awesome"
```
- Install plugin
```sh
# Logstash 2.3 and higher
bin/logstash-plugin install --no-verify

# Prior to Logstash 2.3
bin/plugin install --no-verify

```
- Run Logstash with your plugin
```sh
bin/logstash -e 'filter {awesome {}}'
```
At this point any modifications to the plugin code will be applied to this local Logstash setup. After modifying the plugin, simply rerun Logstash.

#### 2.2 Run in an installed Logstash

You can use the same **2.1** method to run your plugin in an installed Logstash by editing its `Gemfile` and pointing the `:path` to your local plugin development directory or you can build the gem and install it using:

- Build your plugin gem
```sh
gem build logstash-filter-awesome.gemspec
```
- Install the plugin from the Logstash home
```sh
# Logstash 2.3 and higher
bin/logstash-plugin install --no-verify

# Prior to Logstash 2.3
bin/plugin install --no-verify

```
- Start Logstash and proceed to test the plugin

## Contributing

All contributions are welcome: ideas, patches, documentation, bug reports, complaints, and even something you drew up on a napkin.

Programming is not a required skill. Whatever you've seen about open source and maintainers or community members  saying "send patches or die" - you will not see that here.

It is more important to the community that you are able to contribute.

For more information about contributing, see the [CONTRIBUTING](https://github.com/elastic/logstash/blob/master/CONTRIBUTING.md) file.
