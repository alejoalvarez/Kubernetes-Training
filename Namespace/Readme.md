# Namespace

Allows to define isolation for microservices (logical partitions), they can be created through YAML or command line

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: namespace1
```

**List namespaces**
```sh
kubectl get namespace
```

**Create a Namespace**
```
kubectl apply -f <File.yaml>
```

Example 
```sh
kubectl apply -f namespace.yaml
```


