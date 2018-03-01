apache_vhost
============

This role creates virtual hosts for an Apache webserver.

Requirements
------------

* Apache is installed.

Role Variables
--------------

The following role variables should be provided:

| Variable            | Required | Description                                                    |
|:--------------------|:--------:|:---------------------------------------------------------------|
| vhost_index         | false    | The index number for this vhost's conf file.  Defaults to 20.  |
| vhost_ip            | false    | The IP address for this vhost.  Defaults to "*".               |
| vhost_port          | false    | The port for this vhost.  Defaults to 80.                      |
| vhost_admin         | true     | The primary admin's email address for the server.              |
| vhost_name          | true     | The primary domain name of the server.                         |
| vhost_aliases       | false    | A list of alternate server names.                              |
| vhost_type          | true     | Selection of: static, wsgi, redirect                           |
| vhost_docroot       | true     | Base document root.  Required for all types except 'redirect'. |
| vhost_ssl           | false    | A boolean to determine if the server is SSL or not.            |
| vhost_sslcert       | false    | The location of the SSL certificate for this vhost.            |
| vhost_sslcertkey    | false    | The location of the private key for this vhost's certificate.  |
| vhost_extra_configs | false    | Extra config settings.  List of strings.                       |
| vhost_modules       | false    | Extra Apache modules.  List of dicts described below.          |
| vhost_urlaliases    | false    | Exposed URL aliases.  List of dics described below.            |
| vhost_locations     | false    | Apache Location settings.  Dictionary of dicts described below.|
| vhost_directories   | false    | Apache Directory settings.  Dictionary of dicts desc. below.   |

Additional variables used when 'vhost_type' is 'redirect':

| Variable            | Required | Description                                                    |
|:--------------------|:--------:|:---------------------------------------------------------------|
| vhost_destination   | true     | The destination for the redirection.                           |

Additional notes:
* This type should be used when the desire is a full redirect of a website from one to another.  (E.g. http -> https, onesite.com -> anothersite.com)
* This is the only type that doesn't require 'vhost_docroot' to be defined.

Additional variables used when 'vhost_type' is 'wsgi':

| Variable                 | Required | Description                                                    |
|:-------------------------|:--------:|:---------------------------------------------------------------|
| vhost_wsgi_name          | false    | The name for the daemon process group.  Default: vhost_name    |
| vhost_wsgi_procs         | false    | Number of WSGI processes.  Default: wsgi_proc_count (below)    |
| vhost_wsgi_threads       | false    | Number of WSGI threads.  Must be a number.                     |
| vhost_wsgi_home          | false    | The default dir of each WSGI process.  Default: user home      |
| vhost_wsgi_user          | false    | The user that will run WSGI processes.  Default: Apache User   |
| vhost_wsgi_group         | false    | The group that will run WSGI processes.  Default: Apache Group |
| vhost_wsgi_lang          | false    | The WSGI language locale.  Default: wsgi_language (below)      |
| vhost_wsgi_pythonhome    | false    | The Python virtualenv to use.  Must be same version as module. |
| vhost_wsgi_scriptaliases | true     | The WSGI scripts to configure.  List of dicts desc. below.     |

Additional notes:
* The mod_wsgi module will be automatically added to the 'vhost_modules' list.

The following configuration variables may be provided:

| Variable         | Required | Description                                                    |
|:-----------------|:--------:|:---------------------------------------------------------------|
| apache_log_dir   | false    | Location of Apache logs.  Default: /var/log/httpd              |
| apache_confd_dir | false    | Location of Apache sub-conf files.  Default: /etc/httpd/conf.d |
| wsgi_proc_count  | false    | Number of procs for WSGI.  Default: ansible_processor_count    |
| wsgi_language    | false    | The WSGI language locale.  Default: "en_US.UTF-8"              |

Structures
----------

'vhost_modules' is a list of dictionaries.  Each dictionary requires two keys:
* module - the Apache module name.
* filename - the path to the Apache module.

'vhost_urlaliases' is a list of dictionaries.  Each dictionary requires two keys:
* urlpath - the exposed URL alias.
* path - the real path in the filesystem.  Must be a file or a directory.

'vhost_locations' is a dictionary of dictionaries.  The internal dictionary is more free-form:
* key - is a location.
* value - is a dictionary of setting-values.

'vhost_directories' is a dictionary of dictionaries.  The internal dictionary is more free-form:
* key - is a local directory or filename.
* value - is a dictionary of setting-values, where 'require' is a required setting.
* NOTE: 'vhost_docroot' will be added automatically with 'require all granted', if the key doesn't exist.

'vhost_wsgi_scriptaliases' is a list of dictionaries.  Each dictionary requires two keys:
* urlpath - the exposed URL alias.
* path - the real path in the filesystem.  Must be a file.
* NOTE: Each script alias will be added to 'vhost_directories' with 'require all granted', if the the key 
doesn't exist.


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

MIT License

Copyright (c) 2018 Ken Treadway

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
