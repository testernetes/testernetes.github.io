---
title: "Step Definitions"
linkTitle: "Step Definitions"
weight: 11
description: >-
     Available step definitions for scenarios.
---

## Composing a Scenario with Step Definitions

All steps should be prefixed with either 'Given', 'When', 'Then', 'And', 'But', '*'.

Once execution begins, for each step, Cucumber will look for a registered step definition with a matching Regexp. If it finds one, it will execute it, passing all capture group s and variables from the Regexp as arguments to the method.

The specific preposition/adverb used has no significance when Cucumber is registering or looking up step definitions.




### a resource called &lt;reference&gt;
Assigns a reference to the resource given the in the DocString. This reference can be referred to
in future steps in the same scenario. JSON and YAML formats are accepted.
```feature
Given a resource called cm:
  """
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: example
    namespace: default
  data:
    foo: bar
  """
```


<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

Additional Step Arguments: A Kubernetes manifest. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/  Can be yaml or json depending on the content type. 

### I create &lt;reference&gt;
Creates the referenced resource. Step will fail if the reference was not defined in a previous step.
```feature
Given a resource called cm:
  """
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: example
    namespace: default
  data:
    foo: bar
  """
And I create cm
  | field manager | example |
Then within 1s cm jsonpath '{.metadata.uid}' should not be empty
```


<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

: **Additional Step Arguments: (optional) A table of additional client create options.** 
```
https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/client#CreateOptions

Create Options:
| dry run       | boolean |
| field manager | string  |
```

### I delete &lt;reference&gt;
Deletes the referenced resource. Step will fail if the reference was not defined in a previous step.
```feature
Given a resource called cm:
  """
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: example
    namespace: default
  data:
    foo: bar
  """
And I create cm
And I delete cm
  | grace period seconds | 30         |
  | propagation policy   | Foreground |
```


<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

: **Additional Step Arguments: (optional) A table of additional client delete options.** 
```
https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/client#DeleteOptions

Delete Options:
| dry run              | boolean                        |
| grace period seconds | number                         |
| propagation policy   | (Orphan|Background|Foreground) |
```

### I evict &lt;reference&gt;
Evicts the referenced pod resource. Step will fail if the pod reference was not defined in a previous step.
```feature
When I evict pod
  | grace period seconds | 120 |
```


<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

: **Additional Step Arguments: (optional) A table of additional client delete options.** 
```
https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/client#DeleteOptions

Delete Options:
| dry run              | boolean                        |
| grace period seconds | number                         |
| propagation policy   | (Orphan|Background|Foreground) |
```

### I exec &lt;command&gt; in &lt;reference&gt;/&lt;container&gt;
Executes the given command in a shell in the referenced pod and container.
```feature
When I exec "echo helloworld" in pod/app
```


<command&gt;: The command to execute in the container. 
: https://kubernetes.io/docs/tasks/debug/debug-application/get-shell-running-container/  The command will run in a shell on the container and its outputs will be captured and can be asserted against in future steps. 

<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

<container&gt;: The container name. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

### I exec &lt;command&gt; in &lt;reference&gt;
Executes the given command in a shell in the referenced pod and default container.
```feature
When I exec "echo helloworld" in pod
```


<command&gt;: The command to execute in the container. 
: https://kubernetes.io/docs/tasks/debug/debug-application/get-shell-running-container/  The command will run in a shell on the container and its outputs will be captured and can be asserted against in future steps. 

<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

### I patch &lt;reference&gt;
Patches the referenced resource. Step will fail if the reference was not defined in a previous step.
```feature
Given a resource called cm:
  """
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: example
    namespace: default
  data:
    foo: bar
  """
And I create cm
When I patch cm
  | patch | {"data":{"foo":"nobar"}} |
Then for at least 10s cm jsonpath '{.data.foo}' should equal nobar
```


<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

: **Additional Step Arguments: A table of additional client patch options.** 
```
https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/client#PatchOptions

Patch Options:
| patch         | string (required) |
| force         | boolean           |
| dry run       | boolean           |
| field manager | string            |
```

### I proxy get &lt;scheme&gt;://&lt;reference&gt;:&lt;port&gt;&lt;path&gt;
Create a proxy connection to the referenced pod resource and attempts a http(s) GET for the port and path.
	Step will fail if the reference was not defined in a previous step.
```feature
Given a resource called pod:
"""yaml
apiVersion: v1
kind: Pod
metadata:
  name: app
  namespace: default
spec:
  restartPolicy: Never
  containers:
  - command: ["busybox", "httpd", "-f", "-p", "8000"]
    image: busybox:latest
    name: server
"""
When I create pod
And within 1m pod jsonpath '{.status.phase}' should equal Running
And I proxy get http://app:8000/fake
Then pod response code should equal 404
```


<scheme&gt;: The scheme of a URL. 
: Must be either http or https. 

<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

<port&gt;: Port. 
: The port number to request. Acceptable range is 0 - 65536. 

<path&gt;: The path of a URL. 
: Anything that comes after port. 

: **Additional Step Arguments: (optional) A freeform table of additional query parameters to send with the request.** 
```


ProxyGet Options:
| string | string |
```

### &lt;reference&gt; logs (should|should not) say &lt;text&gt;
Asserts that the referenced resource will log something within the specified duration
```feature
Given a resource called testernetes:
  """
  apiVersion: v1
  kind: Pod
  metadata:
    name: testernetes
    namespace: default
  spec:
    restartPolicy: Never
    containers:
    - command:
      - /bdk
      - --help
      image: ghcr.io/testernetes/bdk:d408c829f019f2052badcb93282ee6bd3cdaf8d0
      name: bdk
  """
When I create testernetes
Then testernetes logs should say Behaviour Driven Kubernetes
```


<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

(should|should not): A positive or negative assertion. 
: "to" can also be used instead of "should". 

<text&gt;: A freeform amount of text. 
: This will match anything. 

: **Additional Step Arguments: (optional) A table of additional client pod log options.** 
```
https://pkg.go.dev/k8s.io/api/core/v1#PodLogOptions

Pod Log Options:
| container     | string  |
| follow        | boolean |
| previous      | boolean |
| since seconds | number  |
| timestamps    | boolean |
| tail lines    | number  |
| limit bytes   | number  |
```

### &lt;assertion&gt; &lt;duration&gt; &lt;reference&gt; logs (should|should not) say &lt;text&gt;
Asserts that the referenced resource will log something within the specified duration
```feature
Given a resource called testernetes:
  """
  apiVersion: v1
  kind: Pod
  metadata:
    name: testernetes
    namespace: default
  spec:
    restartPolicy: Never
    containers:
    - command:
      - /bdk
      - --help
      image: ghcr.io/testernetes/bdk:d408c829f019f2052badcb93282ee6bd3cdaf8d0
      name: bdk
  """
When I create testernetes
Then within 1m testernetes logs should say Behaviour Driven Kubernetes
  | container | bdk   |
  | follow    | false |
```


<assertion&gt;: An assertion that state should be achieved or maintained. 
: Eventually assertions can be made with: ["" "within" "in less than" "in under" "in no more than"] Consistently assertions can be made with: ["for at least" "for no less than"] 

<duration&gt;: Duration from when the step starts. 
: https://pkg.go.dev/time#ParseDuration  A duration is a sequence of decimal numbers, each with optional fraction and a unit suffix, such as "300ms", "1.5m" or "2h45m". Valid time units are "ns", "us" (or "µs"), "ms", "s", "m", "h". 

<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

(should|should not): A positive or negative assertion. 
: "to" can also be used instead of "should". 

<text&gt;: A freeform amount of text. 
: This will match anything. 

: **Additional Step Arguments: (optional) A table of additional client pod log options.** 
```
https://pkg.go.dev/k8s.io/api/core/v1#PodLogOptions

Pod Log Options:
| container     | string  |
| follow        | boolean |
| previous      | boolean |
| since seconds | number  |
| timestamps    | boolean |
| tail lines    | number  |
| limit bytes   | number  |
```

### &lt;reference&gt; jsonpath &lt;jsonpath&gt; (should|should not) &lt;matcher&gt;
Asserts that the referenced resource will satisfy the matcher
```feature
Given a resource called cm:
  """
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: example
    namespace: default
  """
And I create cm
Then cm jsonpath '{.metadata.uid}' should not be empty
```


<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

<jsonpath&gt;: A JSON Path to a field in the referenced resource. 
: https://kubernetes.io/docs/reference/kubectl/jsonpath/  e.g. '{.metadata.name}'. 

(should|should not): A positive or negative assertion. 
: "to" can also be used instead of "should". 

<matcher&gt;: Used in conjunction with an assertion to assert that the actual matches the expected. 
: To list available matchers run 'bdk matchers'. 

### &lt;assertion&gt; &lt;duration&gt; &lt;reference&gt; jsonpath &lt;jsonpath&gt; (should|should not) &lt;matcher&gt;
Asserts that the referenced resource will satisfy the matcher in the specified duration
```feature
Given a resource called cm:
  """
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: example
    namespace: default
  """
And I create cm
Then within 1s cm jsonpath '{.metadata.uid}' should not be empty
```


<assertion&gt;: An assertion that state should be achieved or maintained. 
: Eventually assertions can be made with: ["" "within" "in less than" "in under" "in no more than"] Consistently assertions can be made with: ["for at least" "for no less than"] 

<duration&gt;: Duration from when the step starts. 
: https://pkg.go.dev/time#ParseDuration  A duration is a sequence of decimal numbers, each with optional fraction and a unit suffix, such as "300ms", "1.5m" or "2h45m". Valid time units are "ns", "us" (or "µs"), "ms", "s", "m", "h". 

<reference&gt;: A short hand name for a resource. 
: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  A resource can be assigned to this reference in a Context step and later referred to in an Action or Outcome step.  The reference must a name that can be used as a DNS subdomain name as defined in RFC 1123. This is the same Kubernetes requirement for names, i.e. lowercase alphanumeric characters. 

<jsonpath&gt;: A JSON Path to a field in the referenced resource. 
: https://kubernetes.io/docs/reference/kubectl/jsonpath/  e.g. '{.metadata.name}'. 

(should|should not): A positive or negative assertion. 
: "to" can also be used instead of "should". 

<matcher&gt;: Used in conjunction with an assertion to assert that the actual matches the expected. 
: To list available matchers run 'bdk matchers'. 

