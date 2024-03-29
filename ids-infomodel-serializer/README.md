# International Data Spaces Serializer

This repository contains the IDS Serializer.
It handles serializing Java objects created by [the CodeGen tool](https://jira.iais.fraunhofer.de/stash/projects/ICTSL/repos/ids-infomodel-codegen/browse) to JSON-LD and vice versa.
For both directions, Jackson Annotations on the classes, fields and functions are of great importance.
While serializing, these annotations are used to determine the name of objects and arrays, while during parsing (deserialization) the annotations are used to find appropriate implementing classes of interfaces, determine mandatory fields, and more.
An illustration of the parsing workflow (not illustrating foreign namespace objects) is shown below.

![Parser Workflow](documentation/workflow/Parser_Workflow.png)
[source](https://jira.iais.fraunhofer.de/wiki/display/EIS/Parsing+IDS+Messages)

Note that during the parsing stage, there are some implicit dependencies on certain naming conventions.
1) We parse many literals to the class TypedLiteral, which is defined in the CodeGen tool. At one point, we are forced to reference this class by name. Renaming this class requires appropriate adaptions in the parser class.
2) For each core IDS class/interface, we expect that the CodeGen tool created an implementable subclass with either identical name as the RDF class, or with an appended "Impl".
3) Some methods are also extracted by name, most notably getProperties and setProperty. Also, we expect every class to have these, as well as a URI field for the ID.
4) We expect that the setter methods set a field matching the same name with leading underscore, e.g. "setCatalog" sets a field "_catalog".

# Build and Deploy the IDS-Serializer
1. Execute to build the jar files:
    - `mvn clean package -U -Drevision=<new revision number here>`
2. To deploy, make sure that the particular credentials for the repositories are provided in the `settings.xml` or via the maven property `-s <path to the settings.xml>`. 
To deploy the artifacts execute:
    - to eis-ids-public repository `mvn deploy -DaltDeploymentRepository=eis-public-repo::default::https://maven.iais.fraunhofer.de/artifactory/eis-ids-public -Drevision=<new revision number here>`
    - to eis-ids-snapshot repository `mvn deploy -DaltDeploymentRepository=eis-snapshot-repo::default::https://maven.iais.fraunhofer.de/artifactory/eis-ids-snapshot  -Drevision=<new revision number here>`
    - to eis-ids-release repository `mvn deploy -DaltDeploymentRepository=eis-release-repo::default::http://maven.iais.fraunhofer.de/artifactory/eis-ids-release  -Drevision=<new revision number here>`

Authors and Contributors: Matthias Böckmann, Sebastian Bader, Benedikt Tobias Imbusch, Christian Mader

Contact and bug reports: [contact@ids.frauhofer.de](mailto:contact@ids.frauhofer.de)
