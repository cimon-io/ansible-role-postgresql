How to add a new PostgreSQL version
===================================

1) Download the Debian package 'postgresql-<version>_[...].deb' from
http://apt.postgresql.org/pub/repos/apt/pool/main/p/postgresql-<version>/

2) Extract the 'usr/share/postgresql/<version>/postgresql.conf.sample' file
and save it under the 'templates' role directory
    => templates/postgresql.conf.{X}.orig

3) Check the difference between another version:
    => vimdiff postgresql.conf.{X-1}.orig postgresql.conf.{X}.orig

4) Copy an existing template:
    => cp postgresql.conf.{X-1}.j2 postgresql.conf.{X}.j2

5) Update the new template following the major differences.

5) If there are new options or some of them removed, update the 'default/main.yml' file and add a "(>= X)" or "(<= X)" comment to them.

6) Update the '.travis.yml' file to test its new version.


How to check for unused variables
=================================

```bash
# find all variables and save them to a file
cat defaults/main.yml | grep -o '^postgresql_[0-9A-Za-z_-]*' > /tmp/vars

# calculate the number of times they are used, and print those that are used only once
while read line ; do q=$(grep -rl "$line" ./ | wc -l); if [ "$q" -eq 1 ]; then echo "$line"; grep -rl "$line" ./ ; echo "---" ; fi ; done < /tmp/vars

# check variables from the list
```
