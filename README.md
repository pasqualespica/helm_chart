# helm_chart

[Install](https://helm.sh/docs/intro/install/)

👀 https://helm.sh/docs/chart_template_guide/getting_started/





## Getting Started

`helm create mychart`

`rm -rf mychart/templates/*`

`touch mychart/templates/configmap.yaml`

Let's copy
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mychart-configmap
data:
  myvalue: "Hello World"
```

`helm install full-coral ./mychart`

`helm get manifest full-coral`


Let's alter configmap.yaml accordingly.
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
```
`helm install clunky-serval ./mychart`

`helm get manifest clunky-serval`

`helm install --debug --dry-run goodly-guppy ./mychart`

## Built-In Objects

## Values Files

- The values.yaml file in the chart
- If this is a subchart, the values.yaml file of a parent chart
- A values file if passed into helm install or helm upgrade with the -f flag (helm install -f myvals.yaml ./mychart)
- Individual parameters passed with --set (such as helm install --set foo=bar ./mychart)

moving the defaults in values.yaml, we'll set just one parameter:

`favoriteDrink: coffee`

Now we can use this inside of a template:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favoriteDrink }}
```

`helm install geared-marsupi ./mychart --dry-run --debug

`helm install solid-vulture ./mychart --dry-run --debug --set favoriteDrink=slurm`

Values files can contain more structured content, too. For example, we could create a favorite section in our values.yaml file, and then add several keys there:

```yaml
favorite:
  drink: coffee
  food: pizza
```

Now we would have to modify the template slightly:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink }}
  food: {{ .Values.favorite.food }}
```

## Template Functions And Pipelines

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | quote }}
  food: {{ .Values.favorite.food | quote }}
```

> Inverting the order is a common practice in templates. You will see .val | quote more often than quote .val. Either practice is fine

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | repeat 5 | quote }}
  food: {{ .Values.favorite.food | upper | quote }}
```

One function frequently used in templates is the default function: default DEFAULT_VALUE GIVEN_VALUE. This function allows you to specify a default value inside of the template, in case the value is omitted. Let's use it to modify the drink example above:

`drink: {{ .Values.favorite.drink | default "tea" | quote }}`

Now, we will remove the favorite drink setting from values.yaml:

```yaml
favorite:
  #drink: coffee
  food: pizza
```  
Now re-running 
`helm install --dry-run --debug fair-worm ./mychart` 

The `lookup` function can be used to look up resources in a running cluster. The synopsis of the lookup function is `lookup` `apiVersion`, `kind`, `namespace`, `name` -> resource or resource list.


Both `name` and `namespace` are optional and can be passed as an empty string ("").

The following example will return the annotations present for the mynamespace object:

`(lookup "v1" "Namespace" "" "mynamespace").metadata.annotations`

When lookup returns a list of objects, it is possible to access the object list via the items field:

```yaml
{{ range $index, $service := (lookup "v1" "Service" "mynamespace" "").items }}
    {{/* do something with each service */}}
{{ end }}
```

When no object is found, an empty value is returned. This can be used to check for the existence of an object.

Keep in mind that Helm is not supposed to contact the Kubernetes API Server during a `helm template` or a `helm install|upgrade|delete|rollback --dry-run`, so the `lookup` function will return an empty list (i.e. dict) in such a case


For templates, the operators `(eq, ne, lt, gt, and, or` and so on) are all implemented as functions. In pipelines, operations can be grouped with parentheses `((, and ))`.


## Template Function List
## Flow Control
## Variables
## Named Templates
## Accessing Files Inside Templates
## Creating A NOTES.Txt File
## Subcharts And Global Values
## The .Helmignore File
## Debugging Templates
## Next Steps
## Appendix: YAML Techniques
## Appendix: Go Data Types And Templates