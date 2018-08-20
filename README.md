# logstash-amazone_es



Installing Logstash

Logstash requires Java 8. Java 9 is not supported. Use the or an open-source distribution such as OpenJDK.

To check your Java version, run the following command:

java -version

On systems with Java installed, this command produces output similar to the following:

java version "1.8.0_65"
Java(TM) SE Runtime Environment (build 1.8.0_65-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.65-b01, mixed mode)
On some Linux systems, you may also need to have the JAVA_HOME environment exported before attempting the install, particularly if you installed Java from a tarball. This is because Logstash uses Java during installation to automatically detect your environment and install the correct startup method (SysV init scripts, Upstart, or systemd). If Logstash is unable to find the JAVA_HOME environment variable during package installation time, you may get an error message, and Logstash will be unable to start properly.

Installing from a Downloaded Binaryedit
Download the Logstash installation file that matches your host environment. Unpack the file. Do not install Logstash into a directory path that contains colon (:) characters.

Note
These packages are free to use under the Elastic license. They contain open source and free commercial features and access to paid commercial features. Start a 30-day trialto try out all of the paid commercial features. See the Subscriptions page for information about Elastic license levels.

Alternatively, you can download an oss package, which contains only features that are available under the Apache 2.0 license.

On supported Linux operating systems, you can use a package manager to install Logstash.

Installing from Package Repositoriesedit
We also have repositories available for APT and YUM based distributions. Note that we only provide binary packages, but no source packages, as the packages are created as part of the Logstash build.

We have split the Logstash package repositories by version into separate urls to avoid accidental upgrades across major versions. For all 6.x.y releases use 6.x as version number.

We use the PGP key D88E42B4, Elastic’s Signing Key, with fingerprint

4609 5ACC 8548 582C 1A26 99A9 D27D 666C D88E 42B4
to sign all our packages. It is available from https://pgp.mit.edu.

APTedit
Download and install the Public Signing Key:

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
You may need to install the apt-transport-https package on Debian before proceeding:

sudo apt-get install apt-transport-https
Save the repository definition to /etc/apt/sources.list.d/elastic-6.x.list:

echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
Warning
Use the echo method described above to add the Logstash repository. Do not use add-apt-repository as it will add a deb-src entry as well, but we do not provide a source package. If you have added the deb-src entry, you will see an error like the following:

Unable to find expected entry 'main/source/Sources' in Release file (Wrong sources.list entry or malformed file)
Just delete the deb-src entry from the /etc/apt/sources.list file and the installation should work as expected.

Run sudo apt-get update and the repository is ready for use. You can install it with:

sudo apt-get update && sudo apt-get install logstash
See Running Logstash for details about managing Logstash as a system service.

YUMedit
Download and install the public signing key:

rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
Add the following in your /etc/yum.repos.d/ directory in a file with a .repo suffix, for example logstash.repo

[logstash-6.x]
name=Elastic repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
And your repository is ready for use. You can install it with:

sudo yum install logstash


I'm pleased to report that I installed the logstash-output-amazon-es plugin successfully. Here are the commands that worked:

cd /usr/share/logstash
sudo bin/logstash-plugin install logstash-output-amazon_es
It's worth noting that these commands are not documented in the README for this plugin.


I see the plugin listed as installed.

$ /usr/share/logstash/bin/logstash-plugin list logstash-output
logstash-output-amazon_es

$ tail /var/log/logstash/logstash-plain.log





Configuration for Amazon Elasticsearch Output plugin
To run the Logstash output Amazon Elasticsearch plugin simply add a configuration following the below documentation.

An example configuration:





input {
        file {
              path => "/var/log/nginx/access.log"
        }
}

output {
       amazon_es {
              hosts => ["search-analytics-development-v4-s73xhf3idca6b3bo7iks3lg7ey.us-east-1.es.amazonaws.com"]
              region => "us-east-1"
              aws_access_key_id => 'AKIAJ7EUEKB6QACSGA3A'
              aws_secret_access_key => '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'
              index => "actv8-nginx-logs-%{+YYYY.MM.dd}"
       }
}


To run Logstash from the command line, use the following command:


sudo /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/logstash.conf 

