## Utiliser les ReplicaSets

Un ReplicaSet est un superviseur pour les pods qui doivent absolument fonctionner. Il lancera autant de pods que spécifié.

Si un pod survient au niveau d'un pod, il sera automatiquement relancé.

Copier dans un fichier **rs.yaml**


```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  replicas: 1
  selector:
    matchLabels:
      tier: frontend
    matchExpressions:
      - {key: tier, operator: In, values: [frontend]}
  template:
    metadata:
      labels:
        app: guestbook
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: gcr.io/google_samples/gb-frontend:v3
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: dns
          # If your cluster config does not include a dns service, then to
          # instead access environment variables to find service host
          # info, comment out the 'value: dns' line above, and uncomment the
          # line below.
          # value: env
        ports:
        - containerPort: 80
```

Lancer le RS via `kubectl create -f rs.yaml`

Désormais vous pouvez afficher tous les objets :

`kubectl get all`



