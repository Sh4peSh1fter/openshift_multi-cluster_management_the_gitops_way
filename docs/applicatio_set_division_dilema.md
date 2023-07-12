#### the application set division / partition dilema
there are 3 ways to divide the application sets of the cluster-config configuration:
```
lets assume that we have 2 environments: test, and prod.
in each environment we have 3 configurations: node-conf, network-conf, and storage-conf.
```
1. app per env - application for each environment. under this application will be all the confiurations of that environment.
```
application sets:
1. test (contains node-conf, network-conf, storage-conf)
2. prod (contains node-conf, network-conf, storage-conf)
```
2. app per conf - application for each configuration. under this application will be the set of configuration of each environment.
```
application sets:
1. node-conf (contains node-conf-test, and node-conf-prod)
2. node-storage (contains storage-conf-test, storage-conf-prod)
3. node-network (contains network-conf-test, network-conf-prod)
```
3. app per conf-app - application for each configuration per environment. under this application will be the specific configuration for the specific environment.
```
application sets:
1. node-conf-test (contains node-conf-test)
2. node-conf-prod (contains node-conf-prod)
3. node-storage-test (contains storage-conf-test)
4. node-storage-prod (contains storage-conf-prod)
5. node-network-test (contains network-conf-test)
6. node-network-prod (contains network-conf-prod)
```

##### guiding questions:
1. does a change in the application set effects other environments \ configurations?

##### conclusion
we can disqualify the 2nd option (app per conf), becuase in that case, a change that is intended for the test env for example, if failed - can cause problems / effect the prod env configuration that is connected to the same application set.

in the 1st option (app per env), a change in one of the iner configs of the environment can effect the application set (for example if it slows down or stucks the argo from running other changes related to the other configs under the current application set), thus can effect the other configs there. but maybe its not that big of a deal and the idea of app of apps in argo gives benifits and solutions for exactly problems like this.

the 3rd option (app per conf-env) feels very "micro-managment"-ish in a bad way, as the number of application sets we will see in our overview will be the number of configs multiplied by the number of environments. not ideal. although maybe by labeling everything or grouping application sets in one way or another with argocd can solve the overwhelming effect of seeing and managing all those application sets. 
is it worth it over the 1st option? 