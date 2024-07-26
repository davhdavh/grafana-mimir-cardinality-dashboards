# grafana-mimir-cardinality-dashboards
Grafana Mimir dashboards used for cardinality exploration

## Requirements
- Grafana v8.5+
- JSON API datasource: https://grafana.com/grafana/plugins/yesoreyeram-infinity-datasource/
- TreeMap panel: https://grafana.com/grafana/plugins/marcusolsson-treemap-panel/

## Setup via grafana and mimir helm:

```yaml
grafana:
  datasources:
    datasources.yaml:
      apiVersion: 1
      datasources:
        - name: Cardinality
          uid: cardinality
          type: yesoreyeram-infinity-datasource
          isDefault: true
          url: http://mimir.monitoring.svc.cluster.local:8080/prometheus/api/v1/cardinality
          jsonData:
            allowedHosts:
            - http://mimir.monitoring.svc.cluster.local:8080
  dashboardProviders:
    dashboardproviders.yaml:
      apiVersion: 1
      providers:
      - name: 'grafana-mimir-cardinality-dashboards'
        orgId: 1
        folder: 'Cardinality'
        type: file
        disableDeletion: true
        editable: false
        options:
          path: /var/lib/grafana/dashboards/grafana-mimir-cardinality-dashboards
  dashboards:
    grafana-mimir-cardinality-dashboards:
      cardinality-management-1-overview:
        url: https://raw.githubusercontent.com/davhdavh/grafana-mimir-cardinality-dashboards/main/dashboards/cardinality-management-1-overview.json
        token: ''
      cardinality-management-2-metrics:
        url: https://raw.githubusercontent.com/davhdavh/grafana-mimir-cardinality-dashboards/main/dashboards/cardinality-management-2-metrics.json
        token: ''
      cardinality-management-3-labels:
        url: https://raw.githubusercontent.com/davhdavh/grafana-mimir-cardinality-dashboards/main/dashboards/cardinality-management-3-labels.json
        token: ''
  plugins:
  - yesoreyeram-infinity-datasource
  - marcusolsson-treemap-panel

mimir:
  mimir:
    structuredConfig:
      limits:
        cardinality_analysis_enabled: true

```

## Usage
Dashboards are configured to set the X-Scope-OrgId  from a "tenant" variable that needs to be configured in the dashboard so it can be selected dynamically.
