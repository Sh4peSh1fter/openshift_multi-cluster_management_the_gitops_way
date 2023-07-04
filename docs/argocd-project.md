The AppProject CRD is the Kubernetes resource object representing a logical grouping of applications. It is defined by the following key pieces of information:

* "sourceRepos" reference to the repositories that applications within the project can pull manifests from.
* "destinations" reference to clusters and namespaces that applications within the project can deploy into (don't use the name field, only the server field is matched).
* "roles" list of entities with definitions of their access to resources within the project.

