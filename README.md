# OWASP CRS - WordPress Rule Exclusions Plugin
![Integration tests](https://github.com/coreruleset/wordpress-rule-exclusions-plugin/actions/workflows/integration.yml/badge.svg)

## Description

This plugin contains rule exclusions for [WordPress](https://wordpress.org/), a content management system (CMS), so it can be run flawlessly together with
OWASP CRS (CRS).

This plugin only supports functionality provided with vanilla WordPress (without plugins installed). False positives that are due to WordPress plugins must be resolved with [custom rule exclusions](https://coreruleset.org/docs/concepts/false_positives_tuning/).

3rd Party themes are supported for both block based themes and classic site editor themes, however 3rd party theme plugins and builders are not supported, those must be tuned manually with a [custom rule exclusions](https://coreruleset.org/docs/concepts/false_positives_tuning/).

## Installation

For full and up to date instructions for the different available plugin
installation methods, refer to [How to Install a Plugin](https://coreruleset.org/docs/concepts/plugins/#how-to-install-a-plugin)
in the official CRS documentation.

### Conditionally enable plugins for multi-application environments

For full and up to date instructions on how to conditionally enable/disable this plugin on a multisite environment, please refer to [Conditionally enable plugins for multi-application environments](https://coreruleset.org/docs/concepts/plugins/#conditionally-enable-plugins-for-multi-application-environments) in the official CRS documentation.

### Restricting the plugin to a sub-directory

If WordPress is installed in a sub-directory (for example `/blog/`) and the
same host also serves other applications, the plugin can be limited to the
WordPress path without writing your own chained rules. Set
`tx.wordpress-rule-exclusions-plugin_path` to the path prefix in
`wordpress-rule-exclusions-config.conf`:

```apache
SecAction \
    "id:9507011,\
    phase:1,\
    pass,\
    nolog,\
    setvar:'tx.wordpress-rule-exclusions-plugin_path=/blog/'"
```

Requests whose path does not begin with the prefix are then left untouched
by the plugin (rule 9507090 sets `tx.wordpress-rule-exclusions-plugin_enabled=0`
for them). If the variable is not set, the plugin applies to every request,
as before. The config file contains examples that set the prefix per host and
that cover several WordPress installations on the same host.

The prefix should start and end with `/`. It is compared with
`REQUEST_FILENAME` using `@beginsWith`, which supports macro expansion on both
ModSecurity v2 and libmodsecurity v3.

## Testing

After the plugin is enabled, your WordPress instance should work without any
problems possibly caused by CRS (for example, false positives while blocking
requests). If you are still having any problems, please file a new issue on
[github](https://github.com/coreruleset/wordpress-rule-exclusions-plugin).

## License

Copyright (c) 2022-2026 OWASP CRS project. All rights reserved.

The OWASP CRS and its official plugins are distributed
under Apache Software License (ASL) version 2. Please see the enclosed LICENSE
file for full details.
