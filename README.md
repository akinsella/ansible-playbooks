# Ansible Playbooks

Set of playbook roles to orchestrate commute web servers, powered by Ansible.

>
    $ tree roles -L 2
    roles
    ├── ansible
    │   ├── accelerated
    │   ├── facts
    │   ├── fireball
    │   └── setup
    ├── database
    │   ├── mongodb
    │   ├── mysql
    │   └── redis
    └── web
        ├── apache2
        ├── nginx
        ├── nodejs
        ├── php5
        └── varnish


## Quick start

+ You can run the playbooks with [Vagrant](http://www.vagrantup.com/)

>
    $ vagrant up
    $ ansible-playbook -i vagrant_inventory playbook.yml --tag apt_update,mongodb,nodejs

## Authors

**Alexis Kinsella**

+ http://helyx.io
+ http://github.com/helyx-io


Based on work of _Olivier Louvignes_

+ http://github.com/mgcrea


## Copyright and license

    The MIT License

    Copyright (c) 2013-2014 Alexis Kinsella

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in
    all copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
    THE SOFTWARE.