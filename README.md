# trollop

by William Morgan (http://masanjin.net/)

Rubyforge page: https://rubygems.org/gems/trollop

Release announcements and comments: http://masanjin.net/blog/label/trollop/

Documentation: http://www.rubydoc.info/gems/trollop/2.0/Trollop

## DESCRIPTION

Trollop is a commandline option parser for Ruby that just gets out of your way.
One line of code per option is all you need to write. For that, you get a nice
automatically-generated help page, robust option parsing, and sensible defaults
for everything you don't specify.

## FEATURES

- Dirt-simple usage.
- Single file. Throw it in lib/ if you don't want to make it a Rubygem dependency.
- Sensible defaults. No tweaking necessary, much tweaking possible.
- Support for long options, short options, subcommands, and automatic type validation and
  conversion.
- Automatic help message generation, wrapped to current screen width.
- Lots of unit tests.

## REQUIREMENTS

* A burning desire to write less code.

## INSTALL

Add this line to your application's Gemfile:
```
gem 'trollop'
```

then run:
```
bundle install
```

Or install it yourself as:
```
$ gem install trollop
```

## SYNOPSIS
```
  require 'trollop'
  opts = Trollop::options do
    opt :monkey, "Use monkey mode"                    # flag --monkey, default false
    opt :name, "Monkey name", :type => :string        # string --name <s>, default nil
    opt :num_limbs, "Number of limbs", :default => 4  # integer --num-limbs <i>, default to 4
  end

  p opts # a hash: { :monkey=>false, :name=>nil, :num_limbs=>4, :help=>false }
```

## OPTION CONFIGURATION

Options in Trollop can be configured in various ways depending on what is
specified in the options hash.

* `long: 'the-long-form'` - Specify the long form of the argument, i.e. the form with two dashes. If unspecified, will be automatically derived based on the argument name by turning the name option into a string, and replacing any \_'s by -'s.
*  `short: 's'` -  Specify the short form of the argument, i.e. the form with one dash. If unspecified, will be automatically derived from name.
*  `type: 'value'` -  Require that the argument take a parameter or parameters of the specified type. For a single parameter, the value can be any one of `:int, :integer, :string, :double, :float, :io, :date`, or a corresponding Ruby class (e.g. `Integer` for `:int`). For multiple-argument parameters, the value can be any one of `:ints, :integers, :strings, :doubles, :floats, :ios, :dates`. If unset, the default argument type is `:flag`, meaning that the argument does not take a parameter. The specification of `:type` is not necessary if a `:default` is given.
*  `default: 'value'` -  Set the default value for an argument. Without a default value, the hash returned by #parse (and thus Trollop::options) will have a `nil` value for this key unless the argument is given on the commandline. The argument type is derived automatically from the class of the default value given, so specifying a `:type` is not necessary if a `:default` is given. (But see below for an important caveat when `:multi`: is specified too.) If the argument is a flag, and the default is set to `true`, then if it is specified on the commandline the value will be `false`.
*  `required: 'value'` -  If set to `true`, the argument must be provided on the commandline.
*  `multi: 'value'` -  If set to `true`, allows multiple occurrences of the option on the commandline. Otherwise, only a single instance of the option is allowed. (Note that this is different from taking multiple parameters. See below.)

### A note on :multi arguments

Note that there are two types of argument multiplicity: an argument
can take multiple values, e.g. "--arg 1 2 3". An argument can also
be allowed to occur multiple times, e.g. "--arg 1 --arg 2".

Arguments that take multiple values should have a `:type` parameter
drawn from `:ints, :integers, :strings, :doubles, :floats, :ios, :dates`,
or a `:default` value of an array of the correct type (e.g. [String]).
The value of this argument will be an array of the parameters on the commandline.

Arguments that can occur multiple times should be marked with
`:multi` => `true`. The value of this argument will also be an array.
In contrast with regular non-multi options, if not specified on
the commandline, the default value will be [], not nil.

These two attributes can be combined (e.g. `:type` => `:strings`,
`:multi` => `true`), in which case the value of the argument will be
an array of arrays.

There's one ambiguous case to be aware of: when `:multi`: is true and a
`:default` is set to an array (of something), it's ambiguous whether this
is a multi-value argument as well as a multi-occurrence argument.
In this case, Trollop assumes that it's not a multi-value argument.
If you want a multi-value, multi-occurrence argument with a default
value, you must specify `:type` as well.

## LICENSE

Copyright (c) 2008--2012 William Morgan. Trollop is distributed under the same
terms as Ruby.
