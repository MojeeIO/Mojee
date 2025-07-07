---
{{ include "data/emojis" }}
icon: ":grinning:"
order: 100
---
# Emojis

Below you’ll find all available emojis, their shortcodes, aliases, descriptions, and related tags.

Emoji count: **{{ emojis.size + emojis2.size }}**

{.compact}
| Emoji | Shortcode | Description | Tags |
| :---: | --- | --- | --- |
{{~ for $i in emojis ~}}
| :{{ $i.shortcode }}: | `:{{ $i.shortcode }}:` {{ if $i.aliases && $i.aliases.size > 0 }}{{ for $alias in $i.aliases }}<br>`:{{ $alias }}:`{{ if for.last == false }}, {{ end }}{{ end }}{{ end }} | {{ $i.description }} | {{ $i.tags | array.join ", " }}
{{~ end ~}}
{{~ for $i in emojis2 ~}}
| :{{ $i.shortcode }}: | `:{{ $i.shortcode }}:` {{ if $i.aliases && $i.aliases.size > 0 }}{{ for $alias in $i.aliases }}<br>`:{{ $alias }}:`{{ if for.last == false }}, {{ end }}{{ end }}{{ end }} | {{ $i.description }} | {{ $i.tags | array.join ", " }}
{{~ end ~}}