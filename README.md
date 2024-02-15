# ansible-quick-change

## K8s to inventory by role
```bash
echo "" >> inventory && \
echo "[masters]" >> inventory && \
kubectl get nodes -o=jsonpath="{range .items[?(@.metadata.labels.node-role\.kubernetes\.io/master==\"\")]}{.status.addresses[?(@.type==\"InternalIP\")].address}{\"\n\"}{end}" >> inventory && \
echo "" >> inventory && \
echo "[workers]" >> inventory && \
kubectl get nodes -o=jsonpath="{range .items[?(@.metadata.labels.node-role\.kubernetes\.io/worker==\"\")]}{.status.addresses[?(@.type==\"InternalIP\")].address}{\"\n\"}{end}" >> inventory
```


## Tests
```bash
ansible-playbook -i inventory quick_change.yaml --syntax-check
```
```bash
ansible-playbook -i inventory quick_change.yaml --check --diff
```

## Run
```bash
ansible-playbook -i inventory quick_change.yaml
```