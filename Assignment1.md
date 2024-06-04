1. Define the priority when run mode are applied using options
system properties (-D)
-r option
file name detection
Solution1 : 

Priority Order
System Properties (-D option):
Highest priority.
This is because JVM arguments are processed first and can override any other configurations set by other means.
Example: java -jar AEM-author-4502.jar -Dsling.run.nodes=author,dev,development
Run Mode Options (-r option):
Second highest priority.
These are processed after system properties and can override configurations detected by file name.
Example: java -jar AEM-author-4502.jar -r author,dev
File Name Detection:
Lowest priority.
These configurations are used if no run modes are specified by the other two higher-priority methods.
Example: cq5-author-p4502 or aem-publish-p4503



