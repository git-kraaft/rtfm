# Liquid template language

## Date and Time

Add UTC offset to a timestamp "2026-06-11T19:00:00Z"
```html
{% assign utc = "2026-06-11T19:00:00Z" | date: "%s" %}
{% assign local = utc | plus: 7200 %}
{{ local | date: "%Y-%m-%d %H:%M" }}
```
Calculate number of days between now (timestamp) and a future date
```html
{% assign days_difference = difference_seconds | plus: 86399 | divided_by: 86400 %}
```

## String operations
upper case
replace
```html
{{ "hello world" | upcase | replace: " ", "_" }}
``