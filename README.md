# trxToSonarQubeXslt
An XSLT 3.0 compliant transform from the MSTest and VSTest .trx format to the SonarQube Generic Test Data format

If you are running tests which produce a .TRX log file as output such as XUnit or MSTest, you can use this XSL transform to convert your test output to a format which SonarQube can interpret.  The motivation for this is (an issue in SonarQube that prevents .trx files from being interpreted)[https://github.com/SonarSource/sonar-csharp/issues/886].

This XSLT is not perfectly generic.  It does some namespace-to-folder conversion in order to build the resulting XML, particularly to associate a test's details with the file name itself.  Particularly, 
* Namespaces must match folder structure e.g. the type MyProject.Tests.Blah.SomeTests would map to <solutionFolder/MyProject.Tests/Blah/SomeTests.cs (in a project named MyProject.Tests - dots in project names are okay)
* One class per file
* .cs extensions are hardcoded

This restriction only applies to types that actually contain tests.  So if you build fluent classes for semantic tests, those can be multiple types to a file (as is convenient), but that file can't be the same which contains the test class.
  
The XSLT is parameterized.  The parameters are:
* solutionFolder - A relative or fully qualified path
* projectName - The name of your project
