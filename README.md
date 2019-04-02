#Introduction to CF - SUSE Cloud Application Platform

## Session @ SUSECon 19

Note: Replace 'X' with your student number

Fetch work container (ONLY necessary if using your own system)

    $ systemctl start docker

    $ docker pull abonilla/cf-cli

    $ docker run -ti abonilla/cf-cli

Login to the client System

    $ ssh trainingX@demo.geeko.land
    ### Ask for Password

Login to SUSE Cloud Application Platform with the cf CLI:

    $ cf login -a http://api.scf.geeko.land -u trainingX --skip-ssl-validation
    
    $ cf target -o "susecon" -s "spaceX" (this step is optional)


Clone the `Dora` App
    
    $ git clone https://github.com/abonillasuse/dora.git

    $ cd dora

Push the `dora` Application

    $ cf push dora -b ruby_buildpack -s sle12 -k 128m -m 36m --random-route
    
*This Fails* - find out why
    
    $ cf logs dora --recent
    
*Notice the ERR* - redeploy with:  
    
    $ cf push dora -b ruby_buildpack -s sle12 -k 512m -m 36m --random-route
    
The Application has been deployed - notice the `routes` section at the bottom of the output
access the application with a browser or with curl, ie:

    $ curl dora-surprised-possum.scf.geeko.land

Handling currently running applications

    $ cf apps
    $ cf app dora
    $ cf ssh dora (close with `exit`)
    $ cf logs dora --recent

Delete `dora`

    $ cf delete dora

Create the `manifest.yml` as we don't want to constantly add these parameters in the App

    $ vi manifest.yml
    
```    
applications:
- name: dora
  instances: 1
  memory: 36M
  disk_quota: 512M
  random-route: true
  buildpack: ruby_buildpack
  stack: sle12
```

Re-Deploy the `Dora` App

    $ cf push
    
Running multiple instances on different Container Cells is recommended for HA

    $ cf scale dora -i 2

Fetch the `ID` of the Container instance being hit by the request - use a browser or curl, ie

    $ curl dora-interested-wombat.scf.geeko.land/id
    $ curl dora-interested-wombat.scf.geeko.land/id
    $ curl dora-interested-wombat.scf.geeko.land/id

Delete the `dora` App deployment

    $ cf delete dora
    $ cf apps
    $ cd ~

Clone the `Sinatra` App `-X `is your Student Number! 

    $ cd ~
    
    $ git clone https://github.com/abonillasuse/cf-example-sinatra

    $ cd cf-example-sinatra

    $ cf push frank-X -m 64M -k 512M

Open a browser and visit the URL for the `frank` App

Visit the URL for the `frank` App, then scale-out the App

    $ cf scale frank-X -i 2
    
Back on the browser, notice the `Container` value (refresh the page several times)


Profile App basic resource usage

Add CF Community Repo to install Plug-Ins (note - it already exists)

    $ cf add-plugin-repo CF-Community http://plugins.cloudfoundry.org/

Show App statistics - Installing the plugin

    $ cf install-plugin Statistics -r CF-Community

    $ cf statistics frank-X (exit with q)


Remove the SUSECON image from the source code and re-deploy the App

    $ cf-example-sinatra> vi views/index.erb
    # Delete Line 23
    
    $ cf push frank-X -m 64M -k 512M
    
Additionally, the App does not require 64M of RAM, let's decrease the sizing

    $ cf scale frank-x -m 32M
    
    $ cf statistics frank-X



Deploying Pre-Build (docker) Apps

    $ cd ~
    
    $ mkdir dockerapp ; cd dockerapp
    
    $ vi manifest.yml
```
applications:
  - name: dockerapp
    random-route: true
    memory: 128M
    instances: 1
    docker:
      image: gruntwork/docker-test-webapp
      health-check-type: http
 ```     
    $ cf push 

    $ cf app dockerapp
    
    $ curl dockerapp-random-name.scf.geeko.land
    
    $ cf delete dockerapp


Writing an App in Go

    $ cd ~
    
    $ mkdir goapp ; cd goapp
    
    $ vi hello.go
```
package main

import (
        "fmt"
        "net/http"
        "os"
        )
func main () {
        http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request){
                fmt.Fprintf(w, "Hello, you've requested: %s\n", r.URL.Path)
                })
                http.ListenAndServe(":"+os.Getenv("PORT"), nil)
                }
  ```           
 
    $ vi manifest.yml
```
applications:
  - name: goapp
    random-route: true
    memory: 64M
    instances: 1
    buildpack: go_buildpack
    env:
      GOPACKAGE: main
```
    $ cf push
    
    # Identify why the app deployment failed
    
    
    
    
    
    
    # Change the env variable to GOPACKAGENAME and cf push the App again
    
    $ cf apps

    $ curl goapp-random-name.scf.geeko.land/more-suse

    $ cf delete goapp
    
    
    
Let's deploy an App from the WebUI

    # open https://stratos.scf.geeko.land:8443/login
    
    # At the Top Right - Click the Arrow to deploy an App
    
    # Leave Cloud Foundry / trainingX / spaceX and click Next
    
    # Select Public Github / under Project type: abonillasuse/cf-example-sinatra / Branch: stratos / Click Next
    
    # Choose the first Commit / Click Next
    
    # Name: frankX / Disk Quota: 512 / Mem Quota: 32 / Select `Create a random route` / Click Deploy
    
    # Follow the output as the application builds and deploys - Click "Go To App Summary"
    
    # Look around the provided information in the UI - visit the App by clicking the square/arrow button on the top Right Corner "Visit"



