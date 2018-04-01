1. Deploy Prometheus
   A. Create ServiceAccount
      In Case using managed Kubernetes by GCP use the following command in order to grant permissions to your user:
      ` kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user $(gcloud config get-value account)`
      Use kubernetes/serviceAccount.yml to create Prometheus service account , Cluster role and binding:
      ` kubectl apply -f kuberenetes/serviceAccount.yml `
   B. Create configMap
      Use prometheus/prometheus.yml to create configmap the contains the configuration of prometheus:
      ` kubectl create configmap prometheus-server-conf --from-file=prometheus.yml=prometheus/prometheus.yml `
   C. Use kuberenetes/prometheus-deployment.yml to create Prometheus deployment with the ServiceAccount and ConfigMap:
      ` kubectl apply -f kuberenetes/prometheus-deployment.yml ` 
   D. Expose Prometheus with ClusterIP
      ` kubectl expose deployment prometheus-deployment `
2. Deploy Grafana
   A. Use kuberenetes/grafana-deployment.yml to create Grafana Deployment 
      ` kubectl apply -f kuberenetes/grafana-deployment.yml ` 
   B. Expose Grafana out of the Cluser using NodePort or LoadBalancer:
      ` kubectl expose deployment grafana --port=80 --target-port=3000 --name=grafana-service --type=LoadBalancer `
3. Configure Prometheus as a Data source 
   A. Log in to kibana using default user and password (admin/admin)
   B. Go to Configuration > Data sources > Add new Data source
   C. fill the following fields:
      Name: Prometheus
      Type: Prometheus
      URL:  http://prometheus-deployment:9090
   D. Leave everyting else as is and click 'Save & Test'
4. Create Kubernetes dashboard in Grafana
   For that we will use this Dashboard: https://grafana.com/dashboards/315
   A. On Grafana Dashboard click on Create > Import > type 315 (The ID of the Dashboard) and Load.
   B. Choose prometheus as our DataSource and click import 
