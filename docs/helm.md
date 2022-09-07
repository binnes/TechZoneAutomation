# Helm

Helm is the Kubernetes packaging solution.  It packages kubernetes deployment artifacts into a single, parameterised bundle that can deliver consistent deployments.

## Testing

The workflow around Helm doesn't appear very well established.  There is a testing capability [build into helm, using hooks](https://helm.sh/docs/topics/chart_tests/){target=:blank}, but this doesn't appear to be in mainstream use.

There are a number of different opinions and strategies around CI/CD helm development workflows:

- [Kubernetes Helm Charts Testing](https://faun.pub/helm-charts-testing-2091a63a83af)
- [13 Best Practices for using Helm](https://codersociety.com/blog/articles/helm-best-practices#5-test-your-charts) - this is weaker on testing
- [How To Continuously Test and Deploy Your Helm Charts on Kubernetes Clusters Using Kind](https://betterprogramming.pub/how-to-continuously-test-and-deploy-your-helm-charts-on-kubernetes-clusters-using-kind-d71e3585d2dc)
- [Testing strategy of Helm Chart
](https://github.com/apache/superset/discussions/18551?sort=top) - discussion on helm testing in Apache discussion forum

## Pre-install Validation

It is possible to add a schema to validate input variables before a Helm install is run.  This ensures that all required values are provided and in the correct format.

The values validation uses a [JSON Schema](https://json-schema.org)