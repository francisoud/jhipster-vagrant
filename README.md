# jhipster-vagrant
Vagrant configuration to setup a http://jhipster.github.io/ projet using ubuntu/trusty64 box.

Based on instructions at: http://jhipster.github.io/installation.html

## Content
```standalone/``` contains a vagrant conf with jhispter directly usable in ubuntu

```docker/``` contains a vagrant conf with https://github.com/jhipster/jhipster-docker embedded in it

## Under the hood

Basically it just scripting that does this:

### Docker installation (for advanced users)

<h3>Information</h3>

<p>
JHipster has a specific <a href="https://github.com/jhipster/jhipster-docker" target="_blank">jhipster-docker project</a>, which provides a <a href="https://www.docker.io/" target="_blank">Docker</a> container.
</p>
<p>
This project makes a Docker "Trusted build" that is available on:
</p>
<p>
<a href="https://registry.hub.docker.com/u/jdubois/jhipster-docker/" target="_blank">https://registry.hub.docker.com/u/jdubois/jhipster-docker/</a>
</p>
<p>
This image will allow you to run JHipster inside Docker.
</p>
<h3>Prerequisites Linux</h3>
<p>
  Linux supports out-of-box. You just need to follow the tutorial on the <a href="https://docs.docker.com/installation/#installation" target="_blank">Docker</a> website.
</p>
<h3>Prerequisites MAC or Windows</h3>
<p>
  Docker uses Linux-specific kernel features so it doesn't run direcly on MAC or Windows.
  Different solutions are available for MAC and Windows users.
  <ol>
    <li>
      <b>Boot2docker</b> - This is an official docker application to use speed up Docker on non-linux platform.
    </li>
    <li>
      <b>Virtualbox</b> - A Virtual Machine with Docker must be installed
    </li>
    <li>
      <b>Vagrant</b> - Use Virtualbox as provider and docker as Plugin
    </li>
  </ol>
</p>
<h3>Boot2docker tutorial on MAC</h3>
<ol>
  <li>
    <p>
      Install <a href="https://docs.docker.com/installation/mac/" target="_blank">Boot2docker</a>
    </p>
  </li>
  <li>
    <p>
      Initialize boot2docker - a boot2docker ISO image (tiny VM) will be downloaded and installed in the ~/.boot2docker folder.
    </p>
    <p>
      <code>
        boot2docker init
      </code>
    </p>
  </li>
  <li>
    <p>
      <a href="http://stackoverflow.com/questions/23014684/how-to-get-ssh-connection-with-docker-container-on-osxboot2docker/23021012#23021012">Add a portmap to the VM to expose the port to your host</a>
    </p>
    <p>
      <code>
        VBoxManage modifyvm "boot2docker-vm" --natpf1 "containerssh,tcp,,4022,,4022"<br/>
        VBoxManage modifyvm "boot2docker-vm" --natpf1 "containertomcat,tcp,,8080,,8080"<br/>
        VBoxManage modifyvm "boot2docker-vm" --natpf1 "containergruntserver,tcp,,9000,,9000"<br/>
        VBoxManage modifyvm "boot2docker-vm" --natpf1 "containergruntreload,tcp,,35729,,35729"<br/>
      </code>
    </p>
  </li>
  <li>
    <p>
      Start boot2docker
    </p>
    <p>
      <code>
        boot2docker up
      </code>
    </p>
  </li>
</ol>

<h3>Usage on Linux</h3>
<p>
Pull the JHipster Docker image:
</p>
<p>
<code>
sudo docker pull jdubois/jhipster-docker
</code>
</p>
<p>
Create a "jhipster" folder in your home directory:
</p>
<p>
<code>
mkdir ~/jhipster
</code>
</p>
<p>
Run The docker image, with the following options:
</p>
<p>
  <ul>
    <li>The Docker "/jhipster" folder is shared to the local "~/jhipster" folder</li>
    <li>Forward all ports exposed by docker (8080 for Tomcat, 9000 for the "grunt serve" task, 35729 for the live reload with grunt-contrib-watch and 22 for SSHD). In the following example we forward the container 22 port to the host 4022 port, to prevent some port conflicts:</li>
  </ul>
</p>
<p>
<code>
sudo docker run -v ~/jhipster:/jhipster -p 8080:8080 -p 9000:9000 -p 35729:35729 -p 4022:22 -t jdubois/jhipster-docker
</code>
</p>

<h3>Usage on MAC</h3>

<p>
Pull the JHipster Docker image:
</p>
<p>
  <code>
    docker pull jdubois/jhipster-docker
  </code>
</p>
<p>
  Create a "jhipster" folder in your home directory:
</p>
<p>
  <code>
    mkdir ~/jhipster
  </code>
</p>
<p>
Run The docker image, with the following options:
</p>
<p>
  <ul>
    <li>
      Forward all ports exposed by docker (8080 for Tomcat, 9000 for the "grunt serve" task, 35729 for the live reload with grunt-contrib-watch and 22 for SSHD). In the following example we forward the container 22 port to the host 4022 port, to prevent some port conflicts
    </li>
  </ul>
</p>
<p>
  <code>
    docker run -d -p 8080:8080 -p 9000:9000 -p 35729:35729 -p 4022:22 -t jdubois/jhipster-docker
  </code>
</p>
<p>
  <br/>Make a volume container
</p>
<p>
  The Docker <b>-v</b> option should be used to share the Docker "/jhipster" folder to the local "~/jhipster" folder.
  However, this option doesn't work on MAC (issue <a href="https://github.com/docker/docker/issues/4023">4023 on docker project</a>).
  The workaround is to use the <a href="https://github.com/SvenDowideit/dockerfiles/tree/master/samba">svendowideit/samba</a> project.
  More information can be found on the <a href="Folder sharing">https://github.com/boot2docker/boot2docker#folder-sharing</a> section on the boot2docker website.
</p>
<ol>
  <li>
    <p>
      Retrieve the name of the container
    </p>
    <p>
      Here is the resultat of the command `docker ps`. Get the name at the column NAMES.
      <code>
      CONTAINER ID        IMAGE                            COMMAND                CREATED             STATUS              PORTS                                                                                            NAMES<br/>
      418448ff3371        jdubois/jhipster-docker:latest   "/bin/sh -c '/usr/sb   59 minutes ago      Up 59 minutes       0.0.0.0:4022->22/tcp, 0.0.0.0:8080->8080/tcp, 0.0.0.0:9000->9000/tcp, 0.0.0.0:35729->35729/tcp   <b>naughty_bartik</b>
      </code>
    </p>
  </li>
  <li>
    <p>
      Add the samba support.
    </p>
    <p>
      <code>
        docker run --rm -v /usr/local/bin/docker:/docker -v /var/run/docker.sock:/docker.sock svendowideit/samba <b>naughty_bartik</b>
      </code>
    </p>
  </li>
  <li>
    <p>
      Folder sharing
    </p>
    <p>
      <code>
        goto Go|Connect to Server in Finder <br/>
        enter 'cifs://192.168.59.103 <br/>
        hit the 'Connect' button <br/>
        select the volumes you want to mount <br/>
        choose the 'Guest' radiobox and connect <br/>
      </code>
    </p>
  </li>
  <li>
    <p>
      Once mounted, will appear as /Volumes/jhipster. Create a symlink to make the volume accessible in the local "~/jhipster" folder.
    </p>
    <p>
      <code>
        ln -s /Volumes/jhipster ~/jhipster
      </code>
    </p>
  </li>
</ol>
</p>
<h3>SSH configuration</h3>
<p>
You can now connect to your docker container with SSH. You can connect as "root/jhipster" or as "jhipster/jhipster", and we recommand you use the "jhipster" user as some of the tool used are not meant to be run by the root user.
</p>
<p>
Start by adding your SSH public key to the Docker container:
</p>
<p>
<code>
cat ~/.ssh/id_rsa.pub | ssh -p 4022 jhipster@localhost 'mkdir ~/.ssh && cat >> ~/.ssh/authorized_keys'
</code>
</p>
<p>
You can now connect to the Docker container:
</p>
<p>
<code>
ssh -p 4022 jhipster@localhost
</code>
</p>
<h3>Your first project</h3>
<p>
You can then go to the /jhipster directory in your container, and start building your app inside Docker:
</p>
<p>
<code>
cd /jhipster<br/>
yo jhipster
</code>
</p>
<p>
  If the following exception occurs, you need to change the owner of the /jhipster directory. See next step.
</p>
<p>
  <code>
    ...<br/>
    ? (13/13) Would you like to use the Compass CSS Authoring Framework?: No<br/>
    <br/>
    /usr/lib/node_modules/generator-jhipster/node_modules/yeoman-generator/node_modules/mkdirp/index.js:89<br/>
                        throw err0;<br/>
                              ^<br/>
    Error: EACCES, permission denied '/jhipster/src'<br/>
        at Object.fs.mkdirSync (fs.js:653:18)<br/>
        at sync (/usr/lib/node_modules/generator-jhipster/node_modules/yeoman-generator/node_modules/mkdirp/index.js:70:13)<br/>
        at sync (/usr/lib/node_modules/generator-jhipster/node_modules/yeoman-generator/node_modules/mkdirp/index.js:76:24)<br/>
        at Function.sync (/usr/lib/node_modules/generator-jhipster/node_modules/yeoman-generator/node_modules/mkdirp/index.js:76:24)<br/>
        at JhipsterGenerator.app (/usr/lib/node_modules/generator-jhipster/app/index.js:419:10)<br/>
        at /usr/lib/node_modules/generator-jhipster/node_modules/yeoman-generator/lib/base.js:387:14<br/>
        at processImmediate [as _immediateCallback] (timers.js:345:15)<br/>
  </code>
</p>
<p>
You need to fix the jhipster folder to give the "jhipster" user ownership of the directory:
</p>
<p>
  <code>
    ssh -p 4022 jhipster@localhost<br/>
    sudo chown jhipster /jhipster
  </code>
</p>

<p>
Once your application is created, you can run all the normal grunt/bower/maven commands, for example:
</p>
<p>
<code>
mvn spring-boot:run
</code>
</p>
<p>
<b>Congratulations! You've launched your JHipster app inside Docker!</b>
</p>
<p>
On your host machine, you should be able to :
</p>
<p>
  <ul>
    <li>Access the running application at <a href="http://localhost:8080" target="_blank">http://localhost:8080</a></li>
    <li>Get all the generated files inside your shared folder</li>
  </ul>
</p>
<p>
As the generated files are in your shared folder, they will not be deleted if you stop your Docker container. However, if you don't want Docker to keep downloading all the Maven and NPM dependencies every time you start the container, you should commit its state.
</p>
