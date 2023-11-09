# Deploy IAP

This repo provide a default structure to create a GCE Ingress with IAP directly from your ArgoCD Application.

You will need to use an envsubst plugin like:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
[...]
spec:
    source:
        plugin:
        name: envsubst
        env:
            - name: ARGOCD_ENV_INGRESS_IP_NAME
              value: "my-load-balancer-ip"

            - name: ARGOCD_ENV_INGRESS_NAME
              value: "my-ingress-name"

            - name: ARGOCD_ENV_INGRESS_NAMESPACE
              value: "my-ingress-namespace"

            - name: ARGOCD_ENV_INGRESS_DNS
              value: "my-dns.my-company.com"

            - name: ARGOCD_ENV_SERVICE_NAME
              value: "my-service"
```

And annotate your service with:

```yaml
metadata:
    annotations:
        cloud.google.com/neg: '{"ingress": true}'
        cloud.google.com/backend-config: '{"default": "backend-config-my-ingress-name"}'
```

You also need to have ExternalSecret operator installed and a ExternalSecret named `oauth-internal-creds-secrets` containing your iap creds.
