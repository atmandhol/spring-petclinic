SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='gcr.io/adhol-playground/apps')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'petc-lr',
    apply_cmd="tanzu apps workload apply petc-lr --type web --live-update" +
        " --local-path " + LOCAL_PATH +
        " --source-image " + SOURCE_IMAGE +
        " --namespace " + NAMESPACE +
        " --yes >/dev/null" +
        " && kubectl get workload petc-lr --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete petc-lr --namespace " + NAMESPACE + " --yes" ,
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
        sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('petc-lr', port_forwards=["8080:8080"],
    extra_pod_selectors=[{'serving.knative.dev/service': 'petc-lr'}])

allow_k8s_contexts('gke_adhol-playground_us-east4_tap-iterate')