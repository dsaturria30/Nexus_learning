# Nexus-learning-repo
- This repo is maintained by [Devops with Mike](https://www.youtube.com/@DevOpsWithMike0/videos/)
- For interview preparation, use this platform [Wandaprep](http://www.wandaprep.com/)
- Visit my website for more inquiries and support [DevOpswithMike](https://devopswithmike.tech/).


Apache Maven integrates with Nexus Repository Manager to host and manage artifacts, allowing teams to share and access project dependencies efficiently. Using Maven’s distribution Management configuration, artifacts (e.g., JAR/WAR files) can be deployed to a Nexus repository and made accessible to other projects or team members. This setup is particularly useful for managing versioned builds and for facilitating dependency sharing in CI/CD workflows.

## Configuring Nexus Deployment
To configure Maven for deployment to Nexus, specify the repository in your `pom.xml` as follows:

```
<distributionManagement>
    <repository>
        <id>nexus</id>
        <url>http://localhost:8081/repository/maven-releases/</url>
    </repository>
    <snapshotRepository>
        <id>nexus-snapshots</id>
        <url>http://localhost:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

*Example settings.xml Configuration for Nexus Authentication*

To enable secure deployment to Nexus, configure authentication details in your Maven settings.xml file, typically located in `${MAVEN_HOME}/conf` or `${USER_HOME}/.m2`:

```
<settings>
    <servers>
        <server>
            <id>nexus</id>
            <username>your-nexus-username</username>
            <password>your-nexus-password</password>
        </server>
        <server>
            <id>nexus-snapshots</id>
            <username>your-nexus-username</username>
            <password>your-nexus-password</password>
        </server>
    </servers>
</settings>
```

## Deploying Artifacts to Nexus
With this setup, you can deploy your artifacts to Nexus by running the following Maven command:

```
mvn deploy
```

This command will upload the built artifacts to the Nexus repository specified in the distributionManagement section of your pom.xml. Using Nexus Repository Manager alongside Maven simplifies dependency management and enhances the scalability of your CI/CD pipelines.

# Development Environment Project
![maven-learning](maven-nexus-learning.drawio.png)

###### Project ToolBox 🧰
- [Git](https://git-scm.com/) Git will be used to manage our application source code.
- [Github](https://github.com/) Github is a free and open source distributed VCS designed to handle everything from small to very large projects with speed and efficiency
- [Maven](https://maven.apache.org/) Maven will be used for the application packaging and building including running unit test cases
- [Nexus](https://www.sonatype.com/) Nexus Manage Binaries and build artifacts across your software supply chain
- [EC2](https://aws.amazon.com/ec2/) EC2 allows users to rent virtual computers (EC2) to run their own workloads and applications.

## Configure Environments
1) **Create a GitHub Repository**
    - Navigate to https://github.com
    - Click on Repositories
    - Click on `Create` to Create a Repository
     - Repository Name: maven-learning-project
     - Click on `Create`
     - Download the Project Zip from https://github.com/devopsmike-01/maven-learning-repo.git
     - Unzip and Push the code to the Repository you just provisioned

2) **Setting up Maven Server**
    - Create an Amazon Linux 2 VM instance and call it "maven-server"
    - Instance type: t2.micro
    - Security Group (Open): 22 to 0.0.0.0/0 or Your-IP
    - Key pair: Select or create a new keypair
    - Follow the installation steps in this repo: https://github.com/devopsmike-01/nexus-learning-repo/blob/main/Installations/maven-install.md
    - Launch Instance

3) **Nexus**
    - Create an Amazon Linux 2 VM instance and call it "Nexus-server"
    - Instance type: t2.medium
    - Security Group (Open): 8081 and 22 to 0.0.0.0/0
    - Key pair: Select or create a new keypair
    - User data (Copy the following user data): https://github.com/devopsmike-01/nexus-learning-repo/blob/main/Installations/nexus-installation.sh
    - Launch Instance

## Configure Nexus Repository
Series of tutorial code snippets for use
#Maven publish tutorial steps
Publishing artifact to Nexus snapshot and release repo using maven.

1. Create a snapshot repo using nexus, or use default coming in out of the box. DEFAULT 
2. Create a release repo using nexus, or use default coming out of the box. DEFAULT
3. Create a group repo having both release, snapshot and other third party repos. or use default coming out of the box.
4. Download spring initializer project
5. Go settings.xml under <MAVEN_INSTALL_LOCATION>\apache-maven-3.6.0\conf or C:\Users\<USER_NAME>\.m2  or mkdir ~/.m2
6. Create/Move profiles named snapshot and release in settings.xml in `~/.m2` (can be done in pom.xml as well)
7. Add server user name and pwd in setting.xml (Encrypted recommended).
8. Edit pom.xml and add repository and snapshot repository in distribution management tag DEFAULT/DONE
9. Mark id should match in step 7 with server id of settings.xml, UPDATE NEXUS IP
10. Run the following `maven`/`mvn` commands to validate/package/deploy your app artifacts remotely
   - `mvn validate`   (validate the project is correct and all necessary information is available.)
   - `mvn compile`    (compile the source code of the project)
   - `mvn test`       (run tests using a suitable unit testing framework. These tests should not require the code be packaged or deployed.)
   - `mvn package`    (take the compiled code and package it in its distributable format, such as a WAR/JAR/EAR.)
   - `mvn verify`     (run any checks to verify the package is valid and meets quality criteria.)
   - `mvn install`    (install the package into the local repository, for use as a dependency in other projects locally.)
   - `mvn deploy`     (done in an integration or release environment, copies the final package to the remote/SNAPSHOT repository 
                      for sharing with other developers and projects.)

11. Change the version from 1.0-Snapshot to 1.0
12. Run `mvn deploy` to deploy to Snapshot Repo or `mvn clean deploy -P release`, to deploy it to Release Repo
