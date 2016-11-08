#Introduction to CF

##CF - Session @ SUSECON16

Fetch work container


    $ docker pull abonilla/cf-cli

    $ docker run -ti abonilla/cf-cli


Login to CF:

    $ cf login -a http://api.cf.cloud.geeko.land -u studentXX --skip-ssl-validation

params (-o susecon -s spaceXX) are optional
    
    ### Password is suseconXX

    $ cf target -o "susecon" -s "spaceXX"

Basics of the CF CLI: (reference - not to be executed next)

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

    $ cf push doraXX -m 128M

Visit the URL for your App by opening the host Firefox

Clone Sinatra App
    
    $ git clone https://github.com/abonillasuse/cf-example-sinatra

    $ cd cf-example-sinatra

    $ cf push frank-XX

Visit URL for your App, Notice the DEA INDEX

    $ cf statistics frank-XX

    $ cf scale frank-XX -i 3

    $ cf statistics frank-XX


Add CF Community Rep and install 2 Plugins:

    $ cf add-plugin-repo CF-Community http://plugins.cloudfoundry.org/


Show App statistics

    $ cf install-plugin Statistics -r CF-Community

    $ cf statistics APP_NAME



Firehose, to Debug apps

    $ cf install-plugin "Firehose Plugin" -r CF-Community

    $ cf nozzle --debug

Select 5 - visit deployed app.


Diego


Where is my App running?

    $ cf curl /v2/apps/$(cf app dora --guid)/stats


Is it running on warden or diego?

    $ cf curl /v2/apps/$(cf app dora --guid)| grep diego
