# content-hub

## Install dependencies
Content Hub depends on jQuery, Django and other Python or java script packages, you must install them before running Content Hub.
Below is the setup commands with the recommended versions of dependencies.

    	dpkg -i python-setuptools_3.3-1ubuntu1_all.deb
    	easy_install --upgrade pip
    	dpkg -i libssl1.0.0_1.0.1f-1ubuntu2.15_amd64.deb
    	dpkg -i libssl-dev_1.0.1f-1ubuntu2.15_amd64.deb
    	dpkg -i libpython2.7-minimal_2.7.6-8ubuntu0.2_amd64.deb
    	dpkg -i libpython2.7-stdlib_2.7.6-8ubuntu0.2_amd64.deb
    	dpkg -i libpython2.7_2.7.6-8ubuntu0.2_amd64.deb
    	dpkg -i libpython2.7-dev_2.7.6-8ubuntu0.2_amd64.deb
    	dpkg -i libpython-dev_2.7.5-5ubuntu3_amd64.deb
    	dpkg -i python2.7-minimal_2.7.6-8ubuntu0.2_amd64.deb
    	dpkg -i python2.7_2.7.6-8ubuntu0.2_amd64.deb
    	dpkg -i python2.7-dev_2.7.6-8ubuntu0.2_amd64.deb
    	dpkg -i libpython-dev_2.7.5-5ubuntu3_amd64.deb
    	dpkg -i python-dev_2.7.5-5ubuntu3_amd64.deb
    	[sudo] pip install -r requirements.txt

## Install Content Hub
1.  Copy Content hub source code to the device which is use as Content Hub server.
2.  Remove the old version:

		$ rm -rf /srv/easyconnect/
		$ rm -rf /srv/static/

3.  Install new version:
		
        $ cd Content_Hub_2.0.1/
		$ cp -r /easyconnect/ /srv/
		$ cp -r /static/ /srv/

4.  Add auto-ingestion directory:

		$ mkdir /media/preloaded/auto
		
5.  Add database backup directory:

		$ mkdir /media/preloaded/dbbak

## lighttpd configuration
add corresponding configration in lighttpd.conf as below:

	$SERVER["socket"] == ":80" { 
		server.document-root = "/srv/easyconnect/"
		fastcgi.server = (
	                "/content.fcgi" => (
	                        "main" => (
	                                "socket" => "/tmp/content.sock",
	                                "check-local" => "disable",
	                        )
	                ),
	        )
		alias.url = (
	                "/media" => "/srv/media/",
	                "/static" => "/srv/static/",
	        )
	
	        url.rewrite-once = (
	                "^(/media.*)$" => "$1",
	                "^(/static.*)$" => "$1",
	                "^(/.*)$" => "/content.fcgi$1",
	        )
	}

