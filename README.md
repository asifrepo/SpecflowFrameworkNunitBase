#Instruction to run tests:

 + Prerequisite: Visual studio, Windows machine, PowerShell enabled
 + Install java sdk and set the environment variable,
 
 1. Use visual studio test  runner/ resharper to test runner to run tests in VS IDE
 2. Run ~\CompareTheMarket\RunTest.ps1 power shell file to run existing dll from      
   ~\CompareTheMarket\CompareTheMarket\bin\Debug\CompareTheMarket.dll location
   
   command: right click "Run with PowerShell" on RunTest.ps1 file
   
   Test result will be stored in ~\CompareTheMarket\TestResults location. 
   Open ~/CompareTheMarket/TestResults/Index.html to view living documentation site using pickles library 
   Open ~/CompareTheMarket/TestResults/SpecflowTestResult.html for specflow test report
   Open ~/CompareTheMarket/TestResults/ReportUnitTestResult.html for report unit test report
   Screenshots for failed tests will be stored in ~/CompareTheMarket/FailedScreenshot

Configuration 
-------------
# FrameWork

Components:
-----------
1. Selenium to run browser tests
2. Spec flow to write Gherkin scenarios
3. Config-R for configuration management
4. Fluent assertion specify readable expected outcome
5. Nunit 3 unit testing framework  

Library used:
App.config:
<package id="Common.Logging" version="2.1.2" targetFramework="net45" />
  <package id="ConfigR" version="0.14.0" targetFramework="net45" />
  <package id="FluentAssertions" version="4.17.0" targetFramework="net45" />
  <package id="ImpromptuInterface" version="6.2.2" targetFramework="net45" />
  <package id="NUnit" version="3.5.0" targetFramework="net45" />
  <package id="ScriptCs.Contracts" version="0.16.1" targetFramework="net45" />
  <package id="ScriptCs.Core" version="0.16.1" targetFramework="net45" />
  <package id="ScriptCs.Engine.Roslyn" version="0.16.1" targetFramework="net45" />
  <package id="Selenium.Support" version="3.0.1" targetFramework="net45" />
  <package id="Selenium.WebDriver" version="3.0.1" targetFramework="net45" />
  <package id="Selenium.WebDriver.ChromeDriver" version="2.25.0.8" targetFramework="net45" />
  <package id="Selenium.WebDriver.MicrosoftWebDriver" version="10.0.14393.0" targetFramework="net45" />
  <package id="SpecFlow" version="2.1.0" targetFramework="net45" />
  <package id="SpecFlow.Assist.Dynamic" version="1.2.0" targetFramework="net45" />
  <package id="SpecFlow.NUnit" version="2.1.0" targetFramework="net45" />
  
Package.config:
  <package id="NUnit.Console" version="3.5.0" />
  <package id="NUnit.ConsoleRunner" version="3.5.0" />
  <package id="NUnit.Extension.NUnitProjectLoader" version="3.5.0" />
  <package id="NUnit.Extension.NUnitV2Driver" version="3.5.0" />
  <package id="NUnit.Extension.NUnitV2ResultWriter" version="3.5.0" />
  <package id="NUnit.Extension.TeamCityEventListener" version="1.0.2" />
  <package id="NUnit.Extension.VSProjectLoader" version="3.5.0" />
  <package id="Pickles" version="2.10.0" />
  <package id="ReportUnit" version="1.2.1" />
  
Framework developed based on following principle:
------------------------------------------------
1.	Usability: How easy it is to write scripts. Solution is broken down to following folders:
    Feature: all the feature files
    Steps: step definition –no selenium references only the page classes and fluent assertion, each step definition only have one        responsibility
    Pages: Most of the selenium references in there. Pages are mapped using Page object model, Page elements separated where possible, 
    Improvement: create multiple interface implement abstract factory pattern to support IE browser script for reduced maintainability
    Page function has single responsibility as well

Scripting: 


2.	Maintainability : 
Support multiple browser input
Static method outside framework avoided 

3.	Ability to discard/add specific element such as bdd library, browser/mobile library 
4.	Reliability:
Thread.sleep avoided in all cost to reduce test run time webdriverwait method used instead



#Test Coverage

1.	Tests are granular to explain what’s going on, in order to add/remove steps. For more high level BA/PO centric Acceptance criteria steps can be encapsulated using Specflow features.

2.	Example used to get maximum possible coverage out of 3 scenarios. View model is dynamic based on the inputs in your supplier, Your Energy page. Identified SelectCurrentEnergySupplier.feature  are (marked as ignored) however I’ve reused the steps as my background  for other test 

![Image of Yaktocat](https://github.com/ronocode/SpecflowFrameworkNunitBase/blob/master/Select%20Current%20Energy%20Supplier.png)

However in my opinion the key user journey need to cover is all the possible permutations and combination in your energy page to test view model changes accordingly and all user journeys are captured. Therefore I’ve created 3 features
1.	All the possible permutation and combination of user journey for electricity 
2.	All the possible permutation and combination of user journey for Gas 
3.	User journey when both Gas and Electricity is chosen
Using example I have total 11 tests

![Image](https://github.com/ronocode/SpecflowFrameworkNunitBase/blob/master/CompareElectricityPrices.png)
![Image](https://github.com/ronocode/SpecflowFrameworkNunitBase/blob/master/CompareGasPrices.png)
![Image](https://github.com/ronocode/SpecflowFrameworkNunitBase/blob/master/CompareElectricityAndGasPricess.png)


Caveat: 
1.	Readability is not the best for these tests but as mentioned these can be broken down and encapsulated (using specflow) with little effort however it is really easy to follow all the possible outcome with one quick look i in example table. Step are written in first person and often form a grammatically correct sentence


#Edge cases not tested

![Image](https://github.com/ronocode/SpecflowFrameworkNunitBase/blob/master/EdgeCases.png)

Edge cases not covered:

Scenario: Compare electricity and gas prices without bill
Scenario: Compare electricity and gas prices without bill and current usage

Improvement:
Inversion of control and dependency injection


#Reporting

#Settings

#Browser
