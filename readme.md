# Theme Assistant for Movable Type 6

If you are familiar with the Melody/Endevver approach to theming with Movable Type 4, you are also likely familar with the [Theme Manager plugin](https://github.com/openmelody/mt-plugin-theme-manager). Theme Manager did a lot of great things to make themes more manageable on Movable Type 4, however Movable Type 5 introduced a more comprehensive theming system, largely negating the need for Theme Manager's capabilities.

But, there are still some things Theme Manager does that we missed in Movable Type 6 -- most notably, setting Template Module and Widget caching options automatically and the ability to update Custom Fields. (Theming in Movable Type 5 and 6 supports *installing* Custom Fields, but not updating them.)

## Updates to Theme Assistant Made by After6 Services

After6 Services forked Theme Assistant in October 2018 because:

* One of our clients needs the Custom Field management functionality of Theme Manager.

* The schema attributes we use to specify our theme, `elements` and `custom_fields`, are the Movable Type 5/6 way of theming, not the Movable Type 4 way of theming that Endevver's version of Theme Assistant supported.

* The [Endevver version of Theme Assistant](https://github.com/endevver/mt-plugin-theme-assistant) is not really being maintained anymore.

No hard feelings to Dan Wolfgang or Endevver. We just need a version of Theme Assistant to work effectively with our theme.

# Prerequisites

* Movable Type 6.0 or later.

# Installation

To install this plugin follow the instructions found here:

http://tinyurl.com/easy-plugin-install

# Use

Specify your Custom Fields and caching options in YAML. Note that Theme Assistant currently only looks at `elements` (the Movable Type 5/6 way of theming) to find these values. Some abbreviated example YAML:

elements:
    my_awesome_theme:
        label: 'My Awesome Theme'
        templates:
            module:
                my_helper_module:
                  label: 'Cached Helper Module'
                  cache:
                    expire_type: 1
                    expire_interval: 60
                    include_with_ssi: 1
        custom_fields:
            entry_extra_text_field:
                label: 'Extra Text Field'
                description: 'This is a text custom field.'
                default: 'Replace this default text with any value.'
                required: 1
                obj_type: entry
                type: text
                tag: EntryExtraTextField

## Set Caching and Include Options

Caching options can also be specified for Template Modules and Widgets with
the following keys (if you've used the UI to set caching, these options should
all be familiar). Module Caching must be enabled, though Theme Assistant will automatically enable caching if it's not already set.

* `cache` - the parent key to the below options
* `expire_type` -
    * 0: No caching (the default method)
    * 1: time-based expiration ("Expire after *x* minutes")
    * 2: event-based expiration ("Expire upon creation or modification of
      object")
* `expire_interval` - This key is used only if `expire_type: 1` is used.
  Specify a time to expire in minutes.
* `expire_event` - This key is used only if `expire_type: 2` is used. Specify
  a valid object to cause expiration. Valid objects are as follows:
    * asset
    * author
    * category
    * comment
    * entry
    * folder
    * page
    * tbping

Another import aspect to caching is using "includes." The key
`include_with_ssi` allows the specified module or widget to be included as an
external file, saving server resources and making it easy to keep content
updated site-wide. Possible values are `1` and `0` (the default). Within the
UI, this option corresponds to the "Process as [SSI method] include" option
found when editing Template Modules and Widgets.

Server Side Includes must be enabled at the blog level (enable this in
Settings > General).

## Install and Update Custom Fields

Many sites require the use of the Movable Type Commercial Pack's Custom Fields
(part of MT Pro). If custom fields are specified in your theme's `config.yaml` they
can be automatically created when you apply your theme.

The following example shows how to add a text custom field for Entries to the
theme we're building.

    elements:
        my_awesome_theme:
            label: 'My Awesome Theme'
            custom_fields:
                entry_extra_text_field:
                    label: 'Extra Text Field'
                    description: 'This is a text custom field.'
                    default: 'Replace this default text with any value.'
                    required: 1
                    obj_type: entry
                    type: text
                    tag: EntryExtraTextField
                    scope: blog

Custom Field definitions appear beneath the key `custom_fields`.

The key `entry_extra_text_field` is the basename of this field.

The `description`, `required`, `default`, and `scope` keys are optional.

The key `obj_type` is the type of object this field targets; `entry`, `page`,
`category`, `folder`, and `author` are valid. These correlate to the System
Object field in the GUI, of course.

The key `type` is the type of field to be created. Note that this is the key
name of the field, not the public-facing name you see in the GUI. The
Commercial Pack defines the following types of fields with these keys:

* Text: `text`
* Multi-Line Text: `textarea`
* Checkbox: `checkbox`
* URL: `url`
* Date and Time: `datetime`
* Drop Down Menu: `select`
* Radio Buttons: `radio`
* Embed Object: `embed`
* Post Type: `post_type`
* Asset: `asset`
* Audio: `asset.audio`
* Image: `asset.image`
* Video: `asset.video`

If you have other custom fields available they may also be specified in your
theme's `config.yaml`; you just need to specify the key correctly.

To create a system-level custom field (necessary is you use the `author`
object type), include the `scope` key:

    elements:
        my_awesome_theme:
            label: 'My Awesome Theme'
            custom_fields:
                author_bio:
                    label: 'Author Bio'
                    obj_type: author
                    type: textarea
                    tag: AuthorBio
                    scope: system

# License

This plugin is licensed under the same terms as Perl itself.

# Authorship

Theme Assistant was originally written by Dan Wolfgang at Endevver.

Movable Type 5/6-style theme modifications written by Dave Aiello at After6 Services.

#Copyright

Copyright 2013, Endevver LLC. All rights reserved.
Copyright &copy; 2018, After6 Services LLC. All rights reserved.
