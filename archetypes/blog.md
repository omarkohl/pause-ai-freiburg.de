---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
date: {{ .Date }}
summary: ""
# Optional. Remove to publish without an author. link and image are optional.
# authors:
#   - name: Max Mustermann
#     link: https://example.com
#     image: /images/authors/max.jpg
# Same value in every language version, so Hugo links the translations.
translationKey: ""
draft: true
---
