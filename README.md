# Cronjob for sync OpenShift Groups with records from an external provider.

Follow - https://docs.openshift.com/container-platform/4.3/authentication/identity_providers/configuring-ldap-identity-provider.html

create secret for LDAP - https://docs.openshift.com/container-platform/4.3/authentication/identity_providers/configuring-ldap-identity-provider.html#identity-provider-creating-ldap-secret_configuring-ldap-identity-provider
`oc create secret generic ldap-secret --from-literal=bindPassword="$SVCALDAPBIND" -n openshift-config --dry-run -o yaml  | oc apply -f -`

Modify files for cronjob to consume
`oc create configmap ldap-sync --from-file=./active_directory_config.yaml --from-file=./whitelist.txt  -n openshift-config --dry-run -o yaml | oc apply -f -`

Create Service Account 
`oc create sa ldap-sync-sa`
`oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:openshift-config:ldap-sync-sa`
Configure cronJob
`oc apply -f ./ldap_sync_cron.yaml`