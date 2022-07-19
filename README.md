## What is the image for?
The intended purpose of this image is for it to be used as a Jenkins agent. By using the installed features the user is able to create Jenkins pipelines that can compile and publish Go applications. An example of using this image as a Jenkins agent via [Kubernetes](https://plugins.jenkins.io/kubernetes/) can be seen below. 

First, an example of configuring the pod template in yaml to create the agent.

```yaml
jenkins:
  clouds:
    - kubernetes:
        name: "kubernetes"
        templates:
          - name: "image-builder-goreleaser"
            label: "image-builder-goreleaser"
            nodeUsageMode: NORMAL
            containers:
              - name: "image-goreleaser"
                image: "ghcr.io/liatrio/image-builder-goreleaser:${builder_images_version}"
```
And then specifying the agent in the Jenkinsfile for an example step.

```jenkins
stage('Build') {
  agent {
    label "image-builder-goreleaser"
  }
  steps {
    container('image-goreleaser') {
      sh "go mod init man"
      sh "goreleaser init"
      sh "goreleaser release"
    }
  }
}
```

## What is installed on this image?
- Version [1.18](https://go.dev/dl/) of the Go programming language.
- Version [1.9.1](https://goreleaser.com/install/) of GoReleaser.

