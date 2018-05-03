#	How to deploy the Labelme on Linux
*There are four ways to install Labelme, Please check your machine and choose which way suited to you.*
<img src="./DOC/ChooseWay.png"/>
---
##	1. Check version.  
Please confirm your Apache version 2.0+ 、PHP version 5.0+ and Perl version 5.0+ .
```sh
apachectl -version
php -version
perl -version
```
###	Install new version of application.
```sh
sudo apt-get install apache2
sudo apt-get install php
sudo apt-get install perl
```
###	Apache process management
```sh
/etc/init.d/apache2 (start|restart|stop|status)
```
Then, you can see Apache introduction page on http://localhost:80 or http://yourIPaddress:80 .
##	2. Install perl module.
###	Install Make on your machine:  
```sh
sudo apt-get install make
```
###	Download mod_perl:  
Download : [mod_perl](http://perl.apache.org/download/index.html)
###	Install mod_perl :  
*	Unzip download file : `sudo tar -xvf mod_perl-2.0.10.tar.gz`.
*	Search 'Apxs' to install mod_perl:
```sh
locate apxs
cd mod_perl-2.0.10
perl Makefile.PL MP_APXS=/usr/local/apache/bin/apxs
make && make install
```
<img src="./DOC/apxs_path.png"/>  

*	Error in process of Make : execute `sudo apt-get install libperl-dev` and try agagin  

##	3.Load Module.
Now, we need load mod_perl, mod_cgi, mod_rewite, mod_include on Apache.
```sh
cd /usr/local/apache/conf
vim httpd.conf
```
*	Add mod_perl:
```vim
LoadModule perl_module modules/mod_perl.so
```
<img src="./DOC/load_perl.png"/>  

***  

*	Delete note of mod_cgi:
```vim
<IfModule mpm_prefork_module>
        #LoadModule cgi_module modules/mod_cgi.so		//Delete '#'
</IfModule>
//
<IfModule mpm_prefork_module>
        LoadModule cgi_module modules/mod_cgi.so
</IfModule>
```
<img src="./DOC/load_cgi.png"/>  

***  

* Delete note of mod_rewite:
```vim
#LoadModule rewrite_module modules/mod_rewrite.so    //Delete '#'
```
<img src="./DOC/load_rewrite.png"/>  

***  

* Delete note of mod_include:
```vim
#LoadModule include_module modules/mod_include.so  //Delete '#'
```
<img src="./DOC/load_include.png"/>

##  4.Create a new virtual host
Create virtual host, and modify the configuration files of virtual host:
```vim
<VirtualHost *:80>
    ServerName LabelmeTool
    ServerAlias LabelmeTool
    DocumentRoot "REPLACE_WITH_YOUR_LOCATION"
    <Directory "REPLACE_WITH_YOUR_LOCATION">
        Options Indexes FollowSymLinks MultiViews Includes ExecCGI
        AddHandler cgi-script .cgi
        AllowOverride All
        Require all granted
        AddType text/html .shtml
        AddOutputFilter INCLUDES .shtml
        DirectoryIndex index.shtml
    </Directory>
    ErrorLog "REPLACE_WITH_YOUR_LOCATION"/error.log
    CustomLog "REPLACE_WITH_YOUR_LOCATION"/access.log combined
</VirtualHost>
```  
Then, restart Apache server : `/etc/init.d/apache2 restart`

## Tips:
Test CGI:
```vim
test.cgi

#! usr/bin/perl            //"REPLACE_WITH_YOUR_PERL_LOCATION"
print "Content-type: text/html/n/n";
print "Hello,World.";
```  
---
* Email: <taoceng@hp.com>
