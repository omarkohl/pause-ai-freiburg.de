---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
# Start (and optional end) of the event. Always include the UTC offset.
eventDate: {{ now.Format "2006-01-02" }}T18:00:00+02:00
# eventEnd: {{ now.Format "2006-01-02" }}T20:00:00+02:00
location: ""
summary: ""
# Same value in every language version, so Hugo links the translations.
translationKey: ""
---
