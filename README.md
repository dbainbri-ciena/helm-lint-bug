# `helm install` Generates Correctly
[debug] Created tunnel using local port: '36020'

[debug] SERVER: "127.0.0.1:36020"

[debug] Original chart version: ""
[debug] CHART PATH: /home/ubuntu/helm-bug/mychart

NAME:   foo
REVISION: 1
RELEASED: Wed Jul 24 16:17:29 2019
CHART: mychart-1.0.0
USER-SUPPLIED VALUES:
{}

COMPUTED VALUES:
general:
  value: null
specific:
  value: '{{ .Values.general.value | default .Chart.AppVersion  }}'

HOOKS:
MANIFEST:

---
# Source: mychart/templates/mychart.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  mykey: '1.2.3'

# `helm lint` Verifies Good
==> Linting ./mychart
Lint OK

# `helm lint --strict` Fails
1 chart(s) linted, no failures
==> Linting ./mychart
[ERROR] templates/: render error in "mychart/templates/mychart.yaml": template: mychart/templates/mychart.yaml:5:13: executing "mychart/templates/mychart.yaml" at <tpl .Values.specific.value .>: error calling tpl: Error during tpl function execution for "{{ .Values.general.value | default .Chart.AppVersion  }}": render error in "mychart/templates/mychart.yaml": template: mychart/templates/mychart.yaml:1:10: executing "mychart/templates/mychart.yaml" at <.Values.general.value>: map has no entry for key "value"

