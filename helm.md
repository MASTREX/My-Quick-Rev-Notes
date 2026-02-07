> Only has points that I may forget.

> Message to author: If you didn't found something you forgot, Add it!

# Helm
[Docs Links](https://helm.sh/docs/v3/chart_template_guide/getting_started)

## Commands
- `helm template <release-name> <chart-name>`
  - `--values values.yaml`: Render with values
- `helm lint <chart path>`
- `helm install <release-name> <chart-name>`
  - `-f myval.yaml`: user defined values
  - `--set foo=bar`: set single value
  - `--debug --dry-run goodly-guppy ./mychart`: This will render the templates without installing
  - `--dry-run=server`: this will connect to kubernetes API server for teplate loading
  - `-n \ --namespace`: namespace name
- `helm get`
  - `manifest <chart-name>`: To get the actual values that will be loaded

---
---
# Helm Charts

## Basic Structure
```
mychart/
  Chart.yaml		# Chart details
  values.yaml		# default static values
  charts/			# Subcharts
  templates/		# templates files which are sent to template rendering engine (each file)
  |  NOTES.txt			# help text displayed to user when they run `helm install`
  |  *.yaml				# kubernetes manifest filees
  |  _helper.tpl			# contains template helpers. These files are not rendered to Kubernetes object definitions, but are available everywhere within other chart templates for use.
  ...
```

## Basics
- Template directives are inclosed in `{{` and  `}}`. These will be completely replaced by rendering engine. **And leaves any remaining characters(include whitespace)**
- To handle whitespaces character use `{{- ` for left chomp and ` -}}` for right chomp. **whitespace also include newlines**. Use ` | indent n` function for fixed indentation
- In `{{ .Release.Name }}`.` is used to separate each namespace element and first dot means 'current scope'
- Comments as `{{/* comment text */}}`

<details>

<summary>

## Built-in Objects

</summary>

- **Release**: This object describes the release itself.
- **Values**: Values passed into the template from the `values.yaml` file and from user-supplied files. By default, Values is empty.
- **Chart**: The contents of the `Chart.yaml` file.
- **Subcharts**: This provides access to the scope (.Values, .Charts, .Releases etc.) of subcharts to the parent. For example `.Subcharts.mySubChart.myValue` to access the `myValue` in the `mySubChart` chart.
- **Files**: This provides access to all non-special files in a chart. While you cannot use it to access templates, you can use it to access other files in the chart.
- **Capabilities**: This provides information about what capabilities the Kubernetes cluster supports.
- **Template**: Contains information about the current template that is being executed.

</details>

## Values Files

`.Values` is loaded in this order (bottom one most priority):
- The `values.yaml` file in the chart
- If this is a subchart, the values.yaml file of a parent chart
- A values file is passed into `helm install` or `helm upgrade` with the `-f` flag (`helm install -f myvals.yaml ./mychart`)
- Individual parameters are passed with `--set` (`such as helm install --set foo=bar ./mychart`)

## Template functions

**Syntax**: `functionName arg1 arg2 ...`

**Pipeline syntax**: `expression | function args ...`
- expression value is added at the end
- Example: `.Values.favorite.drink | repeat 5 | quote` equivalent to `quote (repeat 5 .Values.favorite.drink)`
- Either practice is fine

## Some basic functions
- `quote ARGUMENT`
- `default DEFAULT_VALUE GIVEN_VALUE`: Give default value if "given value" is empty. Must be used for computed values. All static defaults should be in `values.yaml`

  The definition of "empty" depends on type:
  - Numeric: 0
  - String: ""
  - Lists: []
  - Dicts: {}
  - Boolean: false
  - And always nil (aka null)
  - For structs, there is no definition of empty, so a struct will never return the default.

- `eq` `ne` `lt` `gt` `and` `or` and so on: Are functions representing operators. In pipelines, operations can be grouped with parentheses ( and ).
  ```helm
  and .Arg1 .Arg2
  not .Arg1
  ```
- `required .Arg1`
- `coalesce .Arg1 .Arg2 ...`: Takes a list of values and returns the first non-empty one.
- `ternary <true value> <false value> <condition>` or `<condition> | ternary <true value> <false value>`

## Flow Contorl

### If/Else
```hel,
{{ if PIPELINE }}
  # Do something
{{ else if OTHER PIPELINE }}
  # Do something else
{{ else }}
  # Default case
{{ end }}
```

A pipeline is evaluated as false if the value is:
- a boolean false
- a numeric zero
- an empty string
- a nil (empty or null)
- an empty collection (map, slice, tuple, dict, array)

Under all other conditions, the condition is true.


### `with` or scoped block

Sets current scope `.` to a Variable. Use `$` to access global top namespace scope inside with block.

```helm
{{- with .Values.favorite }}
  drink: {{ .drink | default "tea" | quote }}
  food: {{ .food | upper | quote }}
  release: {{ $.Release.Name }}
{{- end }}
```

### `range`
range iterates through collection and set each item to `.`
```helm
sizes: |-
    {{- range tuple "small" "medium" "large" }}
    - {{ . }}
    {{- end }}

# results to
sizes: |-
    - small
    - medium
    - large
```

Use Variable to get indexes in list and key/val in maps

Syntax: `$name := value`
```helm
toppings: |-
    {{- range $index, $topping := tuple "mushroom" "cheese" "onions" }}
      {{ $index }}: {{ $topping }}
    {{- end }}

# result to
toppings: |-
  0: mushrooms
  1: cheese
  2: onions

# similarly for maps
{{- range $key, $val := .Values.favorite }}
  {{ $key }}: {{ $val | quote }}
{{- end }}
```

## Named Templates
- template names are **Global**.
- If you declare two templates with the same name, whichever one is loaded last will be the one used (including one in Subcharts). Popular naming convention is to prefix each defined template with the name of the chart: `{{ define "mychart.labels" }}`

`define` block
```
{{/* Generate basic labels */}}
{{- define "mychart.labels" }}
  labels:
    generator: helm
    date: {{ now | htmlDate }}
{{- end }}
```

`template` block
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
  {{- template "mychart.labels" . }}    # current scope is passed unless it is not accessible in define block
data:
  myvalue: "Hello World"
```

Prefer `include` as template is just an action but include is a function. Used for formating
```
{{ include "mychart.app" . | indent 2 }}
```
