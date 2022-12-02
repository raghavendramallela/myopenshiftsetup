# myopenshiftsetup


## Instructions on installing `openshift local` (tested on rhel 9.1)

- Login with [redhat developer account](https://developers.redhat.com/)
- Goto [redhat console](https://console.redhat.com/) > [openshift](https://console.redhat.com/openshift/) > [create](https://console.redhat.com/openshift/create) > [local](https://console.redhat.com/openshift/create/local)
- Download and extract the OpenShift Local archive  and place the binary `crc` in your `$PATH` :
    ```
    $ tar xvf ~/Downloads/crc-linux-amd64.tar.xz
    crc-linux-2.10.1-amd64/
    crc-linux-2.10.1-amd64/LICENSE
    crc-linux-2.10.1-amd64/crc

    $ sudo mv ~/Downloads/crc-linux-2.10.1-amd64/crc /usr/local/bin/ 
    ```
- Download the `pull-secret.txt` from the [redhat openshift local console](https://console.redhat.com/openshift/create/local)
- Check for crc version: 
    ``` 
    $ crc version
    CRC version: 2.10.1+426703d
    OpenShift version: 4.11.7
    Podman version: 4.2.0
    ```
- Run `crc setup` for checking dependencies like `libvirt`, `KVM`, `NetworkManager` & download `crcbundle` :
    ```
    $ crc setup

    INFO Using bundle path /home/raghu/.crc/cache/crc_libvirt_4.11.7_amd64.crcbundle 
    INFO Checking if running as non-root              
    INFO Checking if running inside WSL2              
    INFO Checking if crc-admin-helper executable is cached 
    INFO Checking for obsolete admin-helper executable 
    INFO Checking if running on a supported CPU architecture 
    INFO Checking minimum RAM requirements            
    INFO Checking if crc executable symlink exists    
    INFO Checking if Virtualization is enabled        
    INFO Checking if KVM is enabled                   
    INFO Checking if libvirt is installed             
    INFO Checking if user is part of libvirt group    
    INFO Checking if active user/process is currently part of the libvirt group 
    INFO Checking if libvirt daemon is running        
    INFO Checking if a supported libvirt version is installed 
    INFO Checking if crc-driver-libvirt is installed  
    INFO Checking crc daemon systemd service          
    INFO Checking crc daemon systemd socket units     
    INFO Checking if systemd-networkd is running      
    INFO Checking if NetworkManager is installed      
    INFO Checking if NetworkManager service is running 
    INFO Checking if /etc/NetworkManager/conf.d/crc-nm-dnsmasq.conf exists 
    INFO Checking if /etc/NetworkManager/dnsmasq.d/crc.conf exists 
    INFO Checking if libvirt 'crc' network is available 
    INFO Checking if libvirt 'crc' network is active  
    INFO Checking if CRC bundle is extracted in '$HOME/.crc' 
    INFO Checking if /home/raghu/.crc/cache/crc_libvirt_4.11.7_amd64.crcbundle exists 
    INFO Getting bundle for the CRC executable        
    3.15 GiB / 3.15 GiB [-------------------------------------] 100.00% 3.92 MiB p/s
    INFO Uncompressing /home/raghu/.crc/cache/crc_libvirt_4.11.7_amd64.crcbundle 
    crc.qcow2: 12.01 GiB / 12.01 GiB -------------------------] 100.00%
    oc: 118.14 MiB / 118.14 MiB ------------------------------] 100.00%
    Your system is correctly setup for using CRC. Use 'crc start' to start the instance
    ```
- Run `crc start` pointed towards the downloaded `pull-secret.txt` to start openshift local container:

    (can also use `--memory int` or `--cpu int` flags to limit host-machine's resources, check `crc start --help` ): 
    ```
    $ crc start -p ~/Downloads/pull-secret.txt

    INFO Checking if running as non-root              
    INFO Checking if running inside WSL2              
    INFO Checking if crc-admin-helper executable is cached 
    INFO Checking for obsolete admin-helper executable 
    INFO Checking if running on a supported CPU architecture 
    INFO Checking minimum RAM requirements            
    INFO Checking if crc executable symlink exists    
    INFO Checking if Virtualization is enabled        
    INFO Checking if KVM is enabled                   
    INFO Checking if libvirt is installed             
    INFO Checking if user is part of libvirt group    
    INFO Checking if active user/process is currently part of the libvirt group 
    INFO Checking if libvirt daemon is running        
    INFO Checking if a supported libvirt version is installed 
    INFO Checking if crc-driver-libvirt is installed  
    INFO Checking crc daemon systemd socket units     
    INFO Checking if systemd-networkd is running      
    INFO Checking if NetworkManager is installed      
    INFO Checking if NetworkManager service is running 
    INFO Checking if /etc/NetworkManager/conf.d/crc-nm-dnsmasq.conf exists 
    INFO Checking if /etc/NetworkManager/dnsmasq.d/crc.conf exists 
    INFO Checking if libvirt 'crc' network is available 
    INFO Checking if libvirt 'crc' network is active  
    INFO Loading bundle: crc_libvirt_4.11.7_amd64...  
    INFO Creating CRC VM for openshift 4.11.7...      
    INFO Generating new SSH key pair...               
    INFO Generating new password for the kubeadmin user 
    INFO Starting CRC VM for openshift 4.11.7...      
    INFO CRC instance is running with IP 192.168.130.11 
    INFO CRC VM is running                            
    INFO Updating authorized keys...                  
    INFO Configuring shared directories               
    INFO Check internal and public DNS query...       
    INFO Check DNS query from host...                 
    INFO Verifying validity of the kubelet certificates... 
    INFO Starting kubelet service                     
    INFO Waiting for kube-apiserver availability... [takes around 2min] 
    INFO Adding user's pull secret to the cluster...  
    INFO Updating SSH key to machine config resource... 
    INFO Waiting for user's pull secret part of instance disk... 
    INFO Changing the password for the kubeadmin user 
    INFO Updating cluster ID...                       
    INFO Updating root CA cert to admin-kubeconfig-client-ca configmap... 
    INFO Starting openshift instance... [waiting for the cluster to stabilize] 
    INFO 3 operators are progressing: image-registry, openshift-controller-manager, service-ca 
    INFO 3 operators are progressing: image-registry, openshift-controller-manager, service-ca 
    INFO 3 operators are progressing: image-registry, openshift-controller-manager, service-ca 
    INFO 3 operators are progressing: image-registry, openshift-controller-manager, service-ca 
    INFO 3 operators are progressing: image-registry, openshift-controller-manager, service-ca 
    INFO 2 operators are progressing: image-registry, openshift-controller-manager 
    INFO Operator openshift-controller-manager is progressing 
    INFO Operator openshift-controller-manager is progressing 
    INFO Operator openshift-controller-manager is progressing 
    INFO Operator openshift-controller-manager is progressing 
    INFO Operator openshift-controller-manager is progressing 
    INFO All operators are available. Ensuring stability... 
    INFO Operators are stable (2/3)...                
    INFO Operators are stable (3/3)...                
    INFO Adding crc-admin and crc-developer contexts to kubeconfig... 
    Started the OpenShift cluster.

    The server is accessible via web console at:
    https://console-openshift-console.apps-crc.testing

    Log in as administrator:
    Username: kubeadmin
    Password: GHE8B-uSRJn-5u8Sg-T7J7a

    Log in as user:
    Username: developer
    Password: developer

    Use the 'oc' command line interface:
    $ eval $(crc oc-env)
    $ oc login -u developer https://api.crc.testing:6443
    ```
- Check the openshift local container status:
    ```
    $ crc status

    CRC VM:          Running
    OpenShift:       Running (v4.11.7)
    Podman:          
    Disk Usage:      16.21GB of 32.74GB (Inside the CRC VM)
    Cache Usage:     16.43GB
    Cache Directory: /home/raghu/.crc/cache

    $ crc ip
    192.168.130.11
    ```
- Set up environment variables & start using `oc` (OpenShift-cli) commands:
    ```
    $ crc oc-env
    $ eval $(crc oc-env)

    $ crc console --credentials
    To login as a regular user, run 'oc login -u developer -p developer https://api.crc.testing:6443'.
    To login as an admin, run 'oc login -u kubeadmin -p GHE8B-uSRJn-5u8Sg-T7J7a https://api.crc.testing:6443
    
    $ oc version
    Client Version: 4.11.7
    Kustomize Version: v4.5.4
    Server Version: 4.11.7
    Kubernetes Version: v1.24.0+3882f8f

    $ oc login -u kubeadmin https://api.crc.testing:6443
    Logged into "https://api.crc.testing:6443" as "kubeadmin" using existing credentials.
    You have access to 66 projects, the list has been suppressed. You can list all projects with 'oc projects'

    $ oc get nodes -o wide
    NAME                 STATUS   ROLES           AGE   VERSION           INTERNAL-IP      EXTERNAL-IP   OS-IMAGE                                                        KERNEL-VERSION                 CONTAINER-RUNTIME
    crc-lgph7-master-0   Ready    master,worker   54d   v1.24.0+3882f8f   192.168.126.11   <none>        Red Hat Enterprise Linux CoreOS 411.86.202209211811-0 (Ootpa)   4.18.0-372.26.1.el8_6.x86_64   cri-o://1.24.2-7.rhaos4.11.gitca400e0.el8
    ```
- Set up `oc completion` (Output shell completion):
    ```
    $ oc completion -h
    ```
- Set up `oc completion` for a non-root user (raghu) for zshell oc completion:
    ```
    # login to root user:

    $ sudo -i
    $ mkdir -p /usr/local/share/zsh/site-functions
    $ touch /usr/local/share/zsh/site-functions/_oc
    $ chmod +rx /usr/local/share/zsh/site-functions/_oc
    $ chown raghu /usr/local/share/zsh/site-functions/_oc
    $ exit
    
    # logged out of root user back in to non root user:
    
    $ source <(oc completion zsh)
    $ oc completion zsh > "${fpath[1]}/_oc"

## Instructions on installing `OKD local` (tested on rhel 9.1)

### (doesnt require redhat developer account)

- Download `crc` archive from [redhat](https://developers.redhat.com/content-gateway/rest/mirror/pub/openshift-v4/clients/crc/latest/) or from [crc-git-releases-page](https://github.com/crc-org/crc/releases)

- Download and extract the crc Local archive  and place the binary `crc` in your `$PATH` :
    ```
    $ cd ~/Downloads

    $ tar xvf ~/Downloads/crc-linux-amd64.tar.xz
    crc-linux-2.11.0-amd64/
    crc-linux-2.11.0-amd64/LICENSE
    crc-linux-2.11.0-amd64/crc

    $ sudo mv ~/Downloads/crc-linux-2.11.0-amd64/crc /usr/local/bin/ 
    ```
-  Check `crc` version:
    ```
    $ crc version
    version: 2.11.0+823e40d
    OpenShift version: 4.11.13
    Podman version: 4.2.0
    ```

- Configure `crc`: 

    use `cpus` (in number of cpu cores), `memory` (in MiB), & `disk-size` (in GiB) according to your host machine resources

    ```
    $ crc config set enable-cluster-monitoring false
    Successfully configured enable-cluster-monitoring to false

    $ crc config set consent-telemetry no
    Successfully configured consent-telemetry to no

    $ crc config set cpus 10
    Changes to configuration property 'cpus' are only applied when the CRC instance is started.
    If you already have a running CRC instance, then for this configuration change to take effect, stop the CRC instance with 'crc stop' and restart it with 'crc start'.

    $ crc config set disable-update-check true
    Successfully configured disable-update-check to true

    $ crc config set disk-size 75
    Changes to configuration property 'disk-size' are only applied when the CRC instance is started.
    If you already have a running CRC instance, then for this configuration change to take effect, stop the CRC instance with 'crc stop' and restart it with 'crc start'.

    $ crc config set memory 15000
    Changes to configuration property 'memory' are only applied when the CRC instance is started.
    If you already have a running CRC instance, then for this configuration change to take effect, stop the CRC instance with 'crc stop' and restart it with 'crc start'.

    $ crc config set kubeadmin-password raghudevops
    Successfully configured kubeadmin-password to raghudevops

    $ crc config set preset okd
    Changes to configuration property 'preset' are only applied when the CRC instance is created.
    If you already have a running CRC instance with different preset, then for this configuration change to take effect, delete the CRC instance with 'crc delete', setup it with `crc setup` and start it with 'crc start'.
    ```
- Check `crc` configuration:
    ```
    $ crc config view
    - consent-telemetry                     : no
    - cpus                                  : 10
    - disable-update-check                  : true
    - disk-size                             : 75
    - kubeadmin-password                    : raghudevops
    - memory                                : 15000
    - preset                                : okd
    ```
- Setting up `crc`:
    ```
    $ crc setup
    INFO Using bundle path /home/raghu/.crc/cache/crc_okd_libvirt_4.11.0-0.okd-2022-11-05-030711_amd64.crcbundle 
    INFO Checking if running as non-root              
    INFO Checking if running inside WSL2              
    INFO Checking if crc-admin-helper executable is cached 
    INFO Caching crc-admin-helper executable          
    INFO Using root access: Changing ownership of /home/raghu/.crc/bin/crc-admin-helper-linux 
    INFO Using root access: Setting suid for /home/raghu/.crc/bin/crc-admin-helper-linux 
    INFO Checking for obsolete admin-helper executable 
    INFO Checking if running on a supported CPU architecture 
    INFO Checking minimum RAM requirements            
    INFO Checking if crc executable symlink exists    
    INFO Creating symlink for crc executable          
    INFO Checking if Virtualization is enabled        
    INFO Checking if KVM is enabled                   
    INFO Checking if libvirt is installed             
    INFO Checking if user is part of libvirt group    
    INFO Checking if active user/process is currently part of the libvirt group 
    INFO Checking if libvirt daemon is running        
    INFO Checking if a supported libvirt version is installed 
    INFO Checking if crc-driver-libvirt is installed  
    INFO Installing crc-driver-libvirt                
    INFO Checking crc daemon systemd service          
    INFO Checking crc daemon systemd socket units     
    INFO Checking if systemd-networkd is running      
    INFO Checking if NetworkManager is installed      
    INFO Checking if NetworkManager service is running 
    INFO Checking if /etc/NetworkManager/conf.d/crc-nm-dnsmasq.conf exists 
    INFO Checking if /etc/NetworkManager/dnsmasq.d/crc.conf exists 
    INFO Checking if libvirt 'crc' network is available 
    INFO Checking if libvirt 'crc' network is active  
    INFO Checking if CRC bundle is extracted in '$HOME/.crc' 
    INFO Checking if /home/raghu/.crc/cache/crc_okd_libvirt_4.11.0-0.okd-2022-11-05-030711_amd64.crcbundle exists 
    INFO Getting bundle for the CRC executable        
    Getting image source signatures
    Copying blob b50227bf5146 done  
    Copying config 8d9de90438 done  
    Writing manifest to image destination
    Storing signatures
    INFO Extracting the image bundle layer...         
    crc_okd_libvirt_4.11.0-0.okd-2022-11-05-030711_amd64.crcbundle: 3.80 GiB / 3.80 GiB [---------------------------------------------------------------------------------------------------------------------] 100.00%
    INFO Verifying the bundle signature...            
    INFO Uncompressing /home/raghu/.crc/cache/crc_okd_libvirt_4.11.0-0.okd-2022-11-05-030711_amd64.crcbundle 
    crc.qcow2: 13.68 GiB / 13.68 GiB [------------------------------------------------------------------------------------------------------------------------------------------------------------------------] 100.00%
    oc: 118.14 MiB / 118.14 MiB [-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------] 100.00%
    Your system is correctly setup for using CRC. Use 'crc start' to start the instance
    ```

- Start the `crc` instance:
    ```
    $ crc start
    INFO Checking if running as non-root              
    INFO Checking if running inside WSL2              
    INFO Checking if crc-admin-helper executable is cached 
    INFO Checking for obsolete admin-helper executable 
    INFO Checking if running on a supported CPU architecture 
    INFO Checking minimum RAM requirements            
    INFO Checking if crc executable symlink exists    
    INFO Checking if Virtualization is enabled        
    INFO Checking if KVM is enabled                   
    INFO Checking if libvirt is installed             
    INFO Checking if user is part of libvirt group    
    INFO Checking if active user/process is currently part of the libvirt group 
    INFO Checking if libvirt daemon is running        
    INFO Checking if a supported libvirt version is installed 
    INFO Checking if crc-driver-libvirt is installed  
    INFO Checking crc daemon systemd socket units     
    INFO Checking if systemd-networkd is running      
    INFO Checking if NetworkManager is installed      
    INFO Checking if NetworkManager service is running 
    INFO Checking if /etc/NetworkManager/conf.d/crc-nm-dnsmasq.conf exists 
    INFO Checking if /etc/NetworkManager/dnsmasq.d/crc.conf exists 
    INFO Checking if libvirt 'crc' network is available 
    INFO Checking if libvirt 'crc' network is active  
    INFO Loading bundle: crc_okd_libvirt_4.11.0-0.okd-2022-11-05-030711_amd64... 
    INFO Creating CRC VM for okd 4.11.0-0.okd-2022-11-05-030711... 
    INFO Generating new SSH key pair...               
    INFO Generating new password for the kubeadmin user 
    INFO Starting CRC VM for okd 4.11.0-0.okd-2022-11-05-030711... 
    INFO CRC instance is running with IP 192.168.130.11 
    INFO CRC VM is running                            
    INFO Updating authorized keys...                  
    INFO Resizing /dev/vda4 filesystem                
    INFO Configuring shared directories               
    INFO Check internal and public DNS query...       
    INFO Check DNS query from host...                 
    INFO Verifying validity of the kubelet certificates... 
    INFO Starting kubelet service                     
    INFO Waiting for kube-apiserver availability... [takes around 2min] 
    INFO Adding user's pull secret to the cluster...  
    INFO Updating SSH key to machine config resource... 
    INFO Waiting for user's pull secret part of instance disk... 
    INFO Overriding password for kubeadmin user       
    INFO Changing the password for the kubeadmin user 
    INFO Updating cluster ID...                       
    INFO Updating root CA cert to admin-kubeconfig-client-ca configmap... 
    INFO Starting okd instance... [waiting for the cluster to stabilize] 
    INFO 4 operators are progressing: image-registry, network, openshift-controller-manager, service-ca 
    INFO 4 operators are progressing: image-registry, network, openshift-controller-manager, service-ca 
    INFO 4 operators are progressing: image-registry, network, openshift-controller-manager, service-ca 
    INFO 4 operators are progressing: image-registry, network, openshift-controller-manager, service-ca 
    INFO 3 operators are progressing: image-registry, network, openshift-controller-manager 
    INFO 3 operators are progressing: image-registry, network, openshift-controller-manager 
    INFO Operator openshift-controller-manager is progressing 
    INFO 2 operators are progressing: authentication, openshift-controller-manager 
    INFO Operator openshift-controller-manager is progressing 
    INFO Operator authentication is not yet available 
    INFO Operator authentication is not yet available 
    INFO All operators are available. Ensuring stability... 
    INFO Operators are stable (2/3)...                
    INFO Operators are stable (3/3)...                
    INFO Adding crc-admin and crc-developer contexts to kubeconfig... 
    Started the OpenShift cluster.

    The server is accessible via web console at:
    https://console-openshift-console.apps-crc.testing

    Log in as administrator:
    Username: kubeadmin
    Password: raghudevops

    Log in as user:
    Username: developer
    Password: developer

    Use the 'oc' command line interface:
    $ eval $(crc oc-env)
    $ oc login -u developer https://api.crc.testing:6443
    ```

- Set up environment variables & start using `oc` (OpenShift-cli) commands:
    ```
    $ eval $(crc oc-env)

    $ oc login -u kubeadmin https://api.crc.testing:6443
    Logged into "https://api.crc.testing:6443" as "kubeadmin" using existing credentials.

    You have access to 67 projects, the list has been suppressed. You can list all projects with 'oc projects'

    Using project "default".


    $ oc get no -o wide
    NAME                 STATUS   ROLES           AGE   VERSION           INTERNAL-IP      EXTERNAL-IP   OS-IMAGE           KERNEL-VERSION          CONTAINER-RUNTIME
    crc-t6jgr-master-0   Ready    master,worker   15d   v1.24.6+5157800   192.168.126.11   <none>        Fedora CoreOS 36   6.0.5-200.fc36.x86_64   cri-o://1.24.3
    ```