Version=0.0.1
=============

What do you need?
-----------------

- Latest Vagrant version from their [download](https://www.vagrantup.com/downloads.html) page, the one in the debian/ubuntu repos does not work properly with amazon
- Vagrant [aws-plugin](https://github.com/mitchellh/vagrant-aws), you can install install it:

    $ vagrant plugin install vagrant-aws

    $ vagrant box add dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box

Testing it
----------

In the same folder where you vagrant file is execute:

    $ vagrant up

Be aware that the ports 8080, 9000 and 12900 are maped to your host, so if you want to change them modify the following lines in the vagrant file:

```ruby

    api_port = 12900

    # Graylog2 json endpoint
    override.vm.network "forwarded_port", guest: 12900, host: api_port
    # Nginx: To test it as in prod evironment
    override.vm.network "forwarded_port", guest: 8080, host: 8080
    # Graylog2 web interface: To access direct to the web interface
    override.vm.network "forwarded_port", guest: 9000, host: 9000
```

And change the values to fit your needs. The *api_port* must be changed in the variable because it is passed to the ansible playbook too, so the graylog client web app can be able to connect to the public api, the others can be modified directly in the corresponding line.

Deploying
---------

If you want to deploy this app to amazon you must be sure that you have the proper credentials exported ad env vars. For more informacion about authentication check the aws [quick start](https://github.com/mitchellh/vagrant-aws#quick-start)



