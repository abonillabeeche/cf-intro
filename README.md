#Introduction to CF

##CF - Session @ SUSECon 19

Note: Replace 'X' with your student number

Fetch work container

    $ rcdocker start

    $ docker pull abonilla/cf-cli

    $ docker run -ti abonilla/cf-cli


Login to SUSE Cloud Application Platform with the cf CLI:

    $ cf login -a http://api.scf.geeko.land -u trainingX --skip-ssl-validation

params (-o susecon -s spaceX) are optional
    
    ### Password is suseconX

    $ cf target -o "susecon" -s "spaceX"


Clone working App - Dora
    
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

Notice the URLs section or see the apps deployed with

    $ cf apps

Visit the URL for your App by opening the host Firefox

Clone Sinatra App (first, exit the dora folder)

    $ cd ..
    
    $ git clone https://github.com/abonillasuse/cf-example-sinatra

    $ cd cf-example-sinatra

    $ cf push frank-X -m 128M

Add CF Community Repo and install 2 Plugins (note - it already exists)

    $ cf add-plugin-repo CF-Community http://plugins.cloudfoundry.org/

Show App statistics

    $ cf install-plugin Statistics -r CF-Community

    $ cf statistics APP_NAME (reference)

Visit URL for your App, Notice the DEA INDEX (refresh the page several times)

    $ cf statistics frank-X

    $ cf scale frank-X -i 3

    $ cf statistics frank-X


Using Firehose, to Debug apps

    $ cf install-plugin "Firehose Plugin" -r CF-Community

    $ cf nozzle --debug

Select 5 - re-visit any deployed app URL and return to the console for output.
An error will be visible of "not-authorized" since the user doesn't have priviledges.


Diego


Where is my App running?

    $ cf curl /v2/apps/$(cf app doraX --guid)/stats


Is it running on warden or diego?

    $ cf curl /v2/apps/$(cf app doraX --guid)| grep diego
