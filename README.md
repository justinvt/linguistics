
# Linguistics

http://deveiate.org/projects/Linguistics

## Description

Linguistics is a framework for building linguistic utilities for Ruby objects
in any language. It includes a generic language-independant front end, a
module for mapping language codes into language names, and a module which
contains various English-language utilities.

## General Information

### Method Interface

The Linguistics module comes with a language-independant mechanism for
extending core Ruby classes with linguistic methods.

It consists of three parts: a core linguistics module which contains the
class-extension framework for languages, a generic inflector class that serves
as an extension point for linguistic methods on Ruby objects, and one or more
language-specific modules which contain the actual linguistic functions.

The module works by adding a single instance method for each language named
after the language's two-letter code (or three-letter code, if no two-letter
code is defined by ISO639) to various Ruby classes. This allows many
language-specific methods to be added to objects without cluttering up the
interface or risking collision between them, albeit at the cost of three or four
more characters per method invocation. For example:

	Linguistics::use( :en )
	"goose".en.plural
	# => "geese"


### Adding Language Modules

To add a new language to the framework, define a module that will act as the top-level namespace for all your linguistic functions, and then register it as being available, like so:

	module Linguistics::TLH
	
		# Add Klingon to the list of default languages
		Linguistics.register_language( :tlh, self )

	end

The first argument is either the two- or three-letter [ISO 639.2] (http://www.loc.gov/standards/iso639-2/php/code_list.php) language code for the language you're registering.

The second is the container module itself.

After you register your language, each class that Linguistics is told to extend will have a method for your language code/s:

	irb> Linguistics.use( :tlh, :classes => Object )
	# => [Object]
	irb> Object.new.tlh
	# => #<(Klingon; tlhIngan-Hol-language inflector) for <Object:0x402d9674> >

If you use RSpec 2, you can test out any API requirements of the module by requiring  'linguistics/languagebehavior' and adding a shared behavior to your spec:

	require 'rspec'
    require 'linguistics/languagebehavior'
	
	describe Linguistics::TLH do
	
	  it_should_behave_like "a Linguistics language module"
	
	  # ... any other specs for your module
	
	end

If you wish to use the logging subsystem set up by Linguistics, you can do so one of two ways: by logging to the logger directly:

	Linguistics.log.debug "Registering Klingon language extension"

or by mixing the `Linguistics::Loggable' module into your class/module, which will give you a 'log' method that prepends the object class on each log message so it's easy to filter out the ones you want:

	require 'linguistics/mixins'
	class Linguistics::TLH::Generator
		include Linguistics::Loggable

		def generate_it
			self.log.debug "starting generation..."
		end
	end



## English Language Module

Linguistics comes with an English-language module; see the API documentation for 
Linguistics::EN for more information about it.


## Authors

* Michael Granger <ged@FaerieMUD.org>
* Martin Chase <stillflame@FaerieMUD.org>


## Requirements

* Ruby >= 1.9.2

It may work under earlier versions, but I'll only be testing it on 1.9.2 or later.


## Optional

* [linkparser](http://deveiate.org/projects/Ruby-LinkParser) (>= 1.1.0) - Ruby high-level interface to the CMU Link Grammar library

* [wordnet](http://deveiate.org/projects/Ruby-WordNet) (>= 1.1.0) - adds integration for the Ruby binding for the WordNet® lexical refrence system.



