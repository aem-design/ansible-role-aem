# Ansible Role: AEM

[![Build Status](https://travis-ci.org/aem-design/ansible-role-aem.svg?branch=master)](https://travis-ci.org/aem-design/ansible-role-aem)

Create AEM Containers in your stack.
> This role was developed as part of
> [AEM.Design](http://aem.design/)

## Requirements

None.

## Role Variables

| Name                   	| Required 	| Default                                                                                                                                              	| Notes                                	|
|------------------------	|----------	|------------------------------------------------------------------------------------------------------------------------------------------------------	|--------------------------------------	|
| docker_image_user      	|          	| aemdesign                                                                                                                                            	|                                      	|
| docker_image_name      	|          	| aem                                                                                                                                                  	|                                      	|
| docker_image           	|          	| {{ docker_image_user }}/{{ docker_image_name }}                                                                                                      	|                                      	|
| docker_image_tag       	|          	| 6.5.0                                                                                                                                                	|                                      	|
| docker_container_name  	|          	| author                                                                                                                                               	|                                      	|
| docker_timezone        	|          	| Australia/Melbourne                                                                                                                                  	|                                      	|
|                        	|          	|                                                                                                                                                      	|                                      	|
|                        	|          	|                                                                                                                                                      	|                                      	|
| adobeaemcloud_username 	|          	|                                                                                                                                                      	| will be used to login to adobe cloud 	|
| adobeaemcloud_password 	|          	|                                                                                                                                                      	| will be used to login to adobe cloud 	|
| aem_port               	|          	| 4502                                                                                                                                                 	|                                      	|
| aem_debug_port         	|          	| 58242                                                                                                                                                	|                                      	|
| aem_imageserver_port   	|          	| 57345                                                                                                                                                	|                                      	|
| aem_debug              	|          	| false                                                                                                                                                	|                                      	|
| aem_debug_opts         	|          	| -Dcom.sun.management.jmxremote.port={{ aem_debug_port }} -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false  	|                                      	|
| aem_host               	|          	| localhost                                                                                                                                            	|                                      	|
| aem_username           	|          	| admin                                                                                                                                                	|                                      	|
| aem_password           	|          	| admin                                                                                                                                                	|                                      	|
| aem_runmode            	|          	| -Dsling.run.modes=author,crx3,crx3tar,nosamplecontent                                                                                                	|                                      	|
| aem_jvm_opts           	|          	| -server -Xms1024m -Xmx1024m -XX:MaxDirectMemorySize=256M -XX:+CMSClassUnloadingEnabled -Djava.awt.headless=true -Dorg.apache.felix.http.host=0.0.0.0 	|                                      	|
| aem_start_opts         	|          	| start -c /aem/crx-quickstart -i launchpad -p 8080 -a 0.0.0.0 -Dsling.properties=conf/sling.properties                                                	|                                      	|
|                        	|          	|                                                                                                                                                      	|                                      	|
|                        	|          	|                                                                                                                                                      	|                                      	|
| docker_published_ports 	|          	|                                                                                                                                                      	|                                      	|
|                        	|          	| "0.0.0.0:{{ aem_port }}:8080/tcp",                                                                                                                   	|                                      	|
|                        	|          	| "0.0.0.0:{{ aem_debug_port }}:58242/tcp",                                                                                                            	|                                      	|
|                        	|          	| "0.0.0.0:{{ aem_imageserver_port }}:57345/tcp"                                                                                                       	|                                      	|
|                        	|          	|                                                                                                                                                      	|                                      	|
| docker_volumes         	|          	|                                                                                                                                                      	|                                      	|
|                        	|          	| "author-repository:/aem/crx-quickstart/repository:z",                                                                                                	|                                      	|
|                        	|          	| "author-logs:/aem/crx-quickstart/logs:z",                                                                                                            	|                                      	|
|                        	|          	| "author-backup:/aem/backup:z"                                                                                                                        	|                                      	|
|                        	|          	|                                                                                                                                                      	|                                      	|
| package_files_skip     	|          	| false                                                                                                                                                	|                                      	|
| package_files          	|          	| []                                                                                                                                                   	|                                      	|
|                        	|          	|                                                                                                                                                      	|                                      	|
| iptable_rules          	|          	|                                                                                                                                                      	|                                      	|
|                        	|          	| - port: "{{ aem_port | default('4502') }}"                                                                                                           	|                                      	|
|                        	|          	| comment: "service_{{ docker_container_name }}_port"                                                                                                  	|                                      	|
|                        	|          	| - port: "{{ aem_debug_port | default('58242') }}"                                                                                                    	|                                      	|
|                        	|          	| comment: "service_{{ docker_container_name }}_debug_port"                                                                                            	|                                      	|


## Dependencies

This role depends on roles:
 
- `aem_design.docker_container`
- `aem_design.aem_license`
- `aem_design.aem_package`
- `aem_design.aem_verify`

## Example Playbook

```yaml
- hosts: all
  roles:
    - {
        role: aem,
        aem_port: "4502",
        aem_runmode: "-Dsling.run.modes=author,crx3,crx3tar,nosamplecontent",
        aem_jvm_opts: "-server -Xms1024m -Xmx1024m -XX:MaxDirectMemorySize=256M -XX:+CMSClassUnloadingEnabled -Djava.awt.headless=true -Dorg.apache.felix.http.host=0.0.0.0",
        aem_start_opts: "start -c /aem/crx-quickstart -i launchpad -p 8080 -a 0.0.0.0 -Dsling.properties=conf/sling.properties",
        docker_container_name: "author",
        docker_volumes: [
          "author-repository:/aem/crx-quickstart/repository:z",
          "author-logs:/aem/crx-quickstart/logs:z",
          "author-backup:/aem/backup:z"
          ],
        docker_published_ports: [
          "0.0.0.0:4502:8080/tcp",
          "0.0.0.0:58242:58242/tcp",
          "0.0.0.0:57345:57345/tcp"
          ]
      }
```

with variables if package installation is required will need to specify `adobeaemcloud_username` and `adobeaemcloud_password` to enable package download and `maven_repository` to store packages after download for best performance.

```yaml
package_files:
  ## SERVICE PACKS

  - {
    package_source: "adobecloud",
    simple_name: "adobe servicepack 1",
    file_name: 'aem-service-pkg-6.5.1.zip',
    version: '6.5.1',
    group_name: 'adobe/cq650/servicepack',
    package_name: 'aem-service-pkg',
    requires_restart: false,
    requires_admin: true,
    package_url: "https://www.adobeaemcloud.com/content/companies/public/adobe/packages/cq650/servicepack/AEM-6.5.1.0/jcr%3acontent/package/file.res/AEM-6.5.1.0-6.5.1.zip"
  }

```

## License

Apache 2.0

## Author Information

This role was created by [Max Barrass](https://aem.design/).