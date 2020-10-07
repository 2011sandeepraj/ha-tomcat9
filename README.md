# ha-tomcat9
Highly Available Tomcat 9 Image

## ./docker
    Contains the ha tomcat 9 configurations required to run Java App within a cluster
    To update the configuration files such as web.xml or server.xml, update the files under ./docker/contrib/patched and run ./docker/create-patch.sh 

## ./openshift
    Contains the default openshift build and deployment templates to run the vanilla tomcat deployment with no application
    Modify your application build and deployment templates accordingly to make use of this image, for example,
       -> Customize and build the tomcat 9 image in your workspace
       -> Update your application build.yaml to use this image for hosting your java application 
       -> Update your application deployment.yaml to add the rbac authorizations and roles required to run the tomcat in a cluster and don't forget to add the service account required to run your container under the spec.

## Build & Deploy: Tag Convention
    We build in perrsi-tools. Per the Red Hat standard, we will use the time since the epoch as a tag suffix to the Tomcat version:
        > oc process -f ./openshift/build.yaml -p TAG_SUFFIX=$(date +%s) -p SOURCE_GIT_REF=release/0.0.1 | oc create -n perrsi-tools -f -
        Use oc replace instead of create after the first run
    When tagging over to perrsi-prod, use the same tag; update -latest; update -stable if necessary.
        > oc tag perrsi-tools/ha-tomcat9:9.0.27-1654321 perrsi-prod/ha-tomcat9:9.0.27-1654321 --reference-policy=local
        > oc tag perrsi-tools/ha-tomcat9:9.0.27-1654321 perrsi-prod/ha-tomcat9:9.0.27-latest --reference-policy=source

#### Note: To run with existing docker configuration, you will not be needed to make any changes to the docker folder