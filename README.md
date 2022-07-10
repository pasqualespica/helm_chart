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
## Template Functions And Pipelines
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