oc create configmap ca-config-map --from-file=ca.crt=/home/blatt/certs/bundle.crt -n openshift-config
oc create secret generic ldap-secret --from-literal=bindPassword=<secret> -n openshift-config
oc adm groups sync --type=openshift --sync-config=/home/blatt/yaml-files/ldap/ldap-sync-latest.yaml --confirm  --whitelist=/home/blatt/yaml-files/ldap/whitelist.txt
