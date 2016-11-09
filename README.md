#Introduction to CF

##CF - Session @ SUSECON16

Note: Replace 'X' with your student number

Fetch work container

    $ rcdocker start

    $ docker pull abonilla/cf-cli

    $ docker run -ti abonilla/cf-cli


Login to CF:

    $ cf login -a http://api.cf.cloud.geeko.land -u studentX --skip-ssl-validation

params (-o susecon -s spaceX) are optional
    
    ### Password is suseconX

    $ cf target -o "susecon" -s "spaceX"

Basics of the CF CLI: (for reference - not to be executed next)

    $ cf apps

    $ cf push (name)(-m 128m)

    $ cf logs %name

    $ cf


Clone working App - Dora

    $ zypper -n in git
    
    $ git clone https://github.com/abonillasuse/dora.git

    $ cd dora


Inspect the `dora_manifest.yml`

    $ cat dora_manifest.yml

Deploy the Dora App

    $ cf push doraX -m 128M

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

Visit URL for your App, Notice the DEA INDEX

    $ cf statistics frank-X

    $ cf scale frank-X -i 3

    $ cf statistics frank-X


Using Firehose, to Debug apps

    $ cf install-plugin "Firehose Plugin" -r CF-Community

    $ cf nozzle --debug

Select 5 - re-visit any deployed app URL and return to the console for output.


Diego


Where is my App running?

    $ cf curl /v2/apps/$(cf app doraX --guid)/stats


Is it running on warden or diego?

    $ cf curl /v2/apps/$(cf app doraX --guid)| grep diego
