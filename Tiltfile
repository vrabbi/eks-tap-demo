SOURCE_IMAGE = "your-registry.io/project/repo"
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'tanzu-java-web-app',
    apply_cmd="tanzu apps workload apply -f tap/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload tanzu-java-web-app --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f tap/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)
allow_k8s_contexts('eduk8s')
k8s_resource('tanzu-java-web-app', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'tanzu-java-web-app'}])
