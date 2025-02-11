# File: prometheus/adding-prometheus-to-grafana.md

# Adding Prometheus to Grafana

This document explains how to add Prometheus as a data source in Grafana for visualization.

## Prerequisites

- Ensure that you have Grafana installed and running.
- Prometheus should be up and running, collecting metrics.

## Steps to Add Prometheus to Grafana

1. **Log in to Grafana:**
   Open your web browser and navigate to your Grafana instance. Log in with your credentials.

2. **Add a Data Source:**
   - Click on the gear icon (⚙️) in the left sidebar to open the Configuration menu.
   - Select "Data Sources."

3. **Select Prometheus:**
   - Click on the "Add data source" button.
   - Choose "Prometheus" from the list of available data sources.

4. **Configure the Data Source:**
   - In the HTTP section, set the URL to your Prometheus server (e.g., `http://localhost:9090`).
   - Adjust any other settings as necessary, such as access mode (Server or Browser).

5. **Save & Test:**
   - Click the "Save & Test" button to verify that Grafana can connect to your Prometheus instance.
   - You should see a message indicating that the data source is working.

6. **Create a Dashboard:**
   - Navigate to the "+" icon in the left sidebar and select "Dashboard."
   - Click on "Add new panel" to start visualizing your Prometheus metrics.

7. **Select Metrics:**
   - In the new panel, select your Prometheus data source.
   - Use the query editor to select the metrics you want to visualize.

8. **Customize Your Panel:**
   - Adjust the visualization type, panel title, and other settings as needed.
   - Click "Apply" to save your panel.

## Conclusion

You have successfully added Prometheus to Grafana. You can now create dashboards and visualize your metrics effectively. For further customization and advanced features, refer to the Grafana documentation.