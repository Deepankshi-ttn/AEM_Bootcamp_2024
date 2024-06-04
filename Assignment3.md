3. Create a project with latest aem archtype


Step 1: mvn -B archetype:generate -DarchetypeGroupId=com.adobe.aem -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=49 -DappTitle="My Site" -DappId="mysite" -DgroupId="com.mysite" -DgroupId="com.mysite" -DincludeDispatcherConfig=y

Step 2: mvn clean install -PautoInstallPackage



