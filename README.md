#Instruction to run tests:

 + Prerequisite: Visual studio, Windows machine, PowerShell enabled
 
 1. Use visual studio test  runner/ resharper to test runner to run tests in VS IDE
 2. Alternatively (To get reports as well) Run ~\CompareTheMarket\RunTest.ps1 powerShell file (administrator mode) to run existing dll       from ~\CompareTheMarket\CompareTheMarket\bin\Debug\CompareTheMarket.dll location
   
   command: right click "Run with PowerShell" (administrator mode) on RunTest.ps1 file in 
   
   Test result will be stored in ~\CompareTheMarket\TestResults location. 
   Open ~/CompareTheMarket/TestResults/Index.html to view living documentation site using pickles library 
   Open ~/CompareTheMarket/TestResults/SpecflowTestResult.html for specflow test report
   Open ~/CompareTheMarket/TestResults/ReportUnitTestResult.html for report unit test report
   Screenshots for failed tests will be stored in ~/CompareTheMarket/FailedScreenshot

Configuration 
-------------
Settings/Settings.csx file is used to manage *Baseurl*, current *Browser* and *Capabilities* for remote browser and *GridUrl*.Even though  remote driver is implmeneted but for clarity of this solution based on chrome browser in local machine. Screen record option is turned off because it requires Microsoft Expression Encoder 4 (requires window media player installtion) 
apart from Add("ScreenRecord","off") which can be commneted please keep the other settings as it is

```
#r "CompareTheMarket.dll"
using CompareTheMarket;

Add("Baseurl", "https://www.comparethemarket.com/energy/v2/?AFFCLIE=TST");

Add("Browser", "chrome");

//Grid url for selenium Grid
Add("GridUrl", "http://****:**/wd/hub");

 Dictionary<string, object> setup = new Dictionary<string, object>
 {        
  {"browserName" ,"chrome"},
  {"version" ,""},
  {"platform" ,""}      
 };

Add("Capabilities", setup);

//Add("ScreenRecord","off")

```

Components:
-----------
1. Selenium to run browser tests
2. Spec flow to write Gherkin scenarios
3. Config-R for configuration management
4. Fluent assertion specify readable expected outcome
5. Nunit 3 unit testing framework  

Library used:

App.config:

```
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
```
Package.config:
```
<package id="NUnit.Console" version="3.5.0" />
<package id="NUnit.ConsoleRunner" version="3.5.0" />
<package id="NUnit.Extension.NUnitProjectLoader" version="3.5.0" />
<package id="NUnit.Extension.NUnitV2Driver" version="3.5.0" />
<package id="NUnit.Extension.NUnitV2ResultWriter" version="3.5.0" />
<package id="NUnit.Extension.TeamCityEventListener" version="1.0.2" />
<package id="NUnit.Extension.VSProjectLoader" version="3.5.0" />
<package id="Pickles" version="2.10.0" />
<package id="ReportUnit" version="1.2.1" />
 ```
 
# FrameWork

Framework developed based on following principle:
------------------------------------------------
 1. Usability: How easy it is to write tests so that its quicker for a new member to follow  
 Solution is broken down to following folders:
 + Feature: all the feature files
 + Steps: step definition –no selenium references only the page classes and fluent assertion, usually each step definition only have one            responsibility
 + Pages: Most of the selenium references in there. Pages are mapped using Page object model, Page elements separated where possible.
    
Scripting: 


2.	Maintainability : 
  + Static method outside framework avoided 
  + Possible to package framework and distribute between teams 
  + 

3.	Ability to discard/add specific element such as bdd library, browser/mobile library

4.	Reliability:
  + Thread.sleep avoided at all cost to reduce test run time instead used explicitt wait using webdriverwait method 


#Test Coverage

1.	Tests are granular to be able to follow user journey, in order to add/remove steps and create more possible scenarios quickly. For more high level BA/PO centric Acceptance criteria steps can be encapsulated by Step definition file inheriting TechTalk.SpecFlow.Steps ("Compare electricity and gas prices" created using such method)

2.	Example used to extract maximum possible coverage out of 3 scenarios. View model is dynamic based on the inputs in your supplier, Your Energy page. As Identified SelectCurrentEnergySupplier.feature are (marked as ignored) however I’ve reused the steps as my background  for other test. Using example I have in total 11 different possiblities

![Image of Yaktocat](https://github.com/ronocode/SpecflowFrameworkNunitBase/blob/master/Select%20Current%20Energy%20Supplier.png)

3. In my opinion the key user journey we need to cover is all the possible permutations and combination in your energy page to test view model changes accordingly and all user journeys are captured. Therefore I’ve created 3 features:

+	All the possible permutation and combination of user journey for electricity 
+	All the possible permutation and combination of user journey for Gas 
+	User journey when both Gas and Electricity is chosen

![Image](https://github.com/ronocode/SpecflowFrameworkNunitBase/blob/master/CompareElectricityPrices.png)
![Image](https://github.com/ronocode/SpecflowFrameworkNunitBase/blob/master/CompareGasPrices.png)
![Image](https://github.com/ronocode/SpecflowFrameworkNunitBase/blob/master/CompareElectricityAndGasPricess.png)


Caveat: 
1.	Readability is not the best for these tests but as mentioned these can be broken down and encapsulated (using specflow) with little effort. Having said that it is really easy to follow all the possible outcome with one quick look in the example table and '<placeholder>' value can easyly be replaced with actual value to improve readabilty. 

Step are written in first person and often form a grammatically correct sentence

Edge cases not tested
----------------------
FOllowing seemed to me lower priority than 3 I've already identified as these may not the most common user journey.

Scenario: Compare electricity and gas prices without bill
Scenario: Compare electricity and gas prices without bill and current usage

![Image](https://github.com/ronocode/SpecflowFrameworkNunitBase/blob/master/EdgeCases.png)

Given-When-Then:
I tried to follow Setup (Given), Exercise (When), Verify (Then) method for my Gherkgin syntax

Improvement:
I have tried make sure Ive written clean code and following good practises such (DRY, SOLID) with best of my abilities. 

An obvious improvement for this framework will be bespoke Inversion of control and dependency injection to support parallel testing.

Create multiple interface implement abstract factory pattern to support IE browser script for reduced maintainability



