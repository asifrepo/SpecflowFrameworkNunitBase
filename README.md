#Instruction to run tests:

Prerequisite: Visual studio, Windows machine, PowerShell enabled
Install java sdk and set the environment variable,
•	Use visual studio test  runner/ resharper to test runner to run tests in VS IDE
•	Run ~\CompareTheMarket\RunTest.ps1 power shell file to run existing dll from ~\CompareTheMarket\CompareTheMarket\bin\Debug\CompareTheMarket.dll location
Test result will be stored in ~\CompareTheMarket\TestResults location. 
Open /CompareTheMarket/TestResults/Index.html to view living documentation site using pickles library 
Open /CompareTheMarket/TestResults/SpecflowTestResult.html for specflow test report
Open /CompareTheMarket/TestResults/ReportUnitTestResult.html for report unit test report



# FrameWork

1.	Selenium to run browser test ()
2.	Specflow to run Bdd scenario
3.	Config-r for configuration management
4.	Fluent assertion


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
1.	Usability: How easy it is to write scripts. Solution is broken down to following folders:
Feature: all the feature files
Steps: step definition –no selenium references only the page classes and fluent assertion, each step definition only have one responsibility
Pages: Most of the selenium references in there. 
Pages are mapped using Page object model, Page elements separated where possible, 
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

1.	Tests are granular to explain what’s going on, in order to add/remove steps. For more BA centric Acceptance criteria steps can be encapsulated using Specflow features.
2.	Example used to get maximum possible coverage out of 3 scenarios. View model is dynamic based on the inputs in your supplier, Your Energy page. Identified SelectCurrentEnergySupplier.feature  are (marked as ignored) however I’ve reused the steps as my background  for other test 


Feature: Select Current Energy Supplier
    In order to navigate to your energy page
    As a user
    I want to select my current energy provider's information

Scenario Outline: Select my current energy supplier
    Given I am in 'https://energy.comparethemarket.com/energy/v2/?AFFCLIE=TST' page
    And I have entered 'PE2 6YS' as my postcode
    And I select find postcode option
    And I select bill availability option as '<Bill availability>'
    And I select energy type option as '<Energy type>' 
    And I tick '<Same supplier>' same supplier question
    When I select next option with preselected energy  
    Then I should be navigated to 'Your Energy' page 
    Examples: 
    |Bill availability            |Energy type      | Same supplier|
    |Yes, I have my bill          |Gas & Electricity|      Yes     |
    |Yes, I have my bill          |Gas & Electricity|      No      |
    |No, I don’t have my bill     |Gas & Electricity|              |
    |Yes, I have my bill          |Electricity only |              |
    |No, I don’t have my bill     |Electricity only |              |
    |Yes, I have my bill          |Gas only         |              |
    |No, I don’t have my bill     |Gas only         |              |
    
    
However in my opinion the key user journey need to cover is all the possible permutations and combination in your energy page to test view model changes accordingly and all user journeys are captured. Therefore I’ve created 3 features
1.	All the possible permutation and combination of user journey for electricity 
2.	All the possible permutation and combination of user journey for Gas 
3.	User journey when both Gas and Electricity is chosen
Using example I have total 11 tests


Feature: Compare Electricity prices
	In order to compare electricity prices
	As a user
	I want to enter my current electricity usage and payment information

Background: 
    Given I am in 'https://energy.comparethemarket.com/energy/v2/?AFFCLIE=TST' page
    And I have entered 'PE2 6YS' as my postcode
    And I select find postcode option
	And  I select bill availability option as 'Yes, I have my bill'
    And I select energy type option as 'Electricity only' 
    And I select next option with preselected energy  
    And I am navigated to 'Your Energy' page 

Scenario Outline: Compare Electricity prices with all different permutations and combinations
	Given I am in Electricity page
	And I have selected my electricity tariff as '<Tariff>' 
	And I have ticked '<Economy 7>' for my meter
	And my payment method for electricity is '<Payment method>'
	And main source of heating ticked '<Main source of heating>' for electricity
	And I chose to calculate my current electricity usage in '<Usage metrics>'
	And my day usage in kw is '<Usage in kw day>' in '<Frequency>' frequency 
	And my night usage in kw is '<Usage in kw night>'
	And my electricity usage bill is '<Cost>' in '<Frequency>' frequency
	And my last electricity bill date was '<Bill date>'
    And I select next option 
    And I am navigated to 'Your Details' page
	And I have enter my preferred tariff as '<Preferred tariffs>'
	And I enter preferred payment type as '<Preferred payment type>'
	And I enter my email address 'asifahmed.github@gmail.com'
	And I checked to receive marketing email
	And I checked terms and conditions
	When I select go to prices button
	Then I wait until the loading screen displayed
	And I am navigated to 'Your Results' page
	And I should see my price comparison results
	Examples:
	| Tariff                   | Economy 7 | Payment method       | Main source of heating | Usage metrics | Usage in kw day | Usage in kw night | Cost | Frequency | Bill date       | Preferred tariffs | Preferred payment type |
	| EnergyPlan Paper Billing | Yes       | Monthly Direct Debit | Yes                    | kWh           | 83              | 83                |      | Monthly   | 1 October, 2015 | Fixed tariff      | Monthly direct debit   |
	| EnergyPlan Paper Billing | Yes       | Monthly Direct Debit | Yes                    | kWh           | 1000            | 1000              |      | Annually  |                 | Variable tariff   | Quarterly direct debit |
	| EnergyPlan Paper Billing | No        | Monthly Direct Debit | Yes                    | kWh           | 166             |                   |      | Monthly   | 1 October, 2015 | All tariffs       | Pay on receipt of bill |
	| EnergyPlan Paper Billing | No        | Monthly Direct Debit | Yes                    | kWh           | 2000            |                   |      | Annually  |                 | All tariffs       | All payment types      |
	| EnergyPlan Paper Billing | Yes       | Monthly Direct Debit | Yes                    | £             |                 |                   | 61   | Monthly   | 1 October, 2015 | All tariffs       | All payment types      |
	| EnergyPlan Paper Billing | Yes       | Monthly Direct Debit | Yes                    | £             |                 |                   | 734  | Annually  |                 | All tariffs       | All payment types      |


Feature: Compare Gas prices
	In order to compare gas prices
	As a user
	I want to enter my current gas usage and payment information

	Background: 
    Given I am in 'https://energy.comparethemarket.com/energy/v2/?AFFCLIE=TST' page
    And I have entered 'PE2 6YS' as my postcode
    And I select find postcode option
	And  I select bill availability option as 'Yes, I have my bill'
    And I select energy type option as 'Gas only' 
    And I select next option with preselected energy  
    And I am navigated to 'Your Energy' page 


 Scenario Outline: Compare Gas price with all different permutations and combinations
	Given I am in Gas page
	And I have selected my gas tariff as '<Tariff>' 
	And my payment method for gas is '<Payment method>'
	And my main source of heating ticked '<Main source of heating>' for gas
	And I chose to calculate my current gas usage in '<Usage metrics>'
	And my usage in kw is '<Usage in kw>'  in '<Frequency>' frequency 
	And my gas usage bill is '<Cost>' in '<Frequency>' frequency
	And my last gas bill date was '<Bill date>'
    And  I select next option 
    And I am navigated to 'Your Details' page
	And I have enter my preferred tariff as '<Preferred tariffs>'
	And I enter preferred payment type as '<Preferred payment type>'
	And I enter my email address 'asifahmed.github@gmail.com'
	And I checked to receive marketing email
	And I checked terms and conditions
	When I select go to prices button
	Then I am navigated to 'Your Results' page
	And I wait until the loading screen displayed
	And I should see my price comparison results
	Examples:
	| Tariff   | Payment method       | Main source of heating | Usage metrics | Usage in kw | Cost | Frequency | Bill date        | Preferred tariffs | Preferred payment type |
	| Standard | Monthly Direct Debit | Yes                    | kWh           | 100         |      | Monthly   | 1 November, 2016 | Fixed tariff	   | Monthly direct debit   |
	| Standard | Monthly Direct Debit | Yes                    | kWh           | 100         |      | Annually  |                  | Variable tariff   | Quarterly direct debit |
	| Standard | Monthly Direct Debit | Yes                    | £             |             | 20   | Monthly   | 1 November, 2016 | All tariffs       | Pay on receipt of bill |
	| Standard | Monthly Direct Debit | Yes                    | £             |             | 240  | Annually  |                  | All tariffs       | All payment types      |

Feature: Compare Electricity And Gas prices
	In order to compare electricity and gas prices
	As a user
	I want to enter my current usage and payment information

	Background: 
    Given I am in 'https://energy.comparethemarket.com/energy/v2/?AFFCLIE=TST' page
    And I have entered 'PE2 6YS' as my postcode
    And I select find postcode option
	And I select bill availability option as 'Yes, I have my bill'
    And I select energy type option as 'Gas & Electricity'
	And I tick 'Yes' same supplier question 
    And I select next option with preselected energy  
    And I am navigated to 'Your Energy' page 

@GasAndElec
	Scenario: select energy usage Electricity & Gas
	Given I am in Electricity page
    And I have selected following for Electricity:
	|Tariff                  |Economy 7|Payment method        |Main source of heating|Usage metrics|Usage in kw day|Usage in kw night|Frequency| Bill date		 |
    |EnergyPlan Paper Billing|Yes      |Monthly Direct Debit  |No                    |kWh          |100            |100				 |  Monthly| 1 October, 2016 |
    And  I select next option 
	And I am in Gas page
    And I have selected following for Gas:
	|Tariff					 |Payment method        |Main source of heating |Usage metrics|Cost|Frequency|Bill date       |
    |EnergyPlan Paper Billing|Monthly Direct Debit  |Yes                    |£            |20  |  Monthly|1 November, 2016|
    And  I select next option 
    And I am navigated to 'Your Details' page
	And I have enter my preferred tariff as 'All tariffs'
	And I enter preferred payment type as 'All payment types'
	And I enter my email address 'asifahmed.github@gmail.com'
	And I checked to receive marketing email
	And I checked terms and conditions
	When I select go to prices button
	Then I wait until the loading screen displayed	
	And I am navigated to 'Your Results' page	
	And I should see my price comparison results


Caveat: 
1.	Readability is not the best for these tests but as mentioned these can be broken down and encapsulated (using specflow) with little effort however it is really easy to follow all the possible outcome with one quick look i in example table. Step are written in first person and often form a grammatically correct sentence


#Edge cases not tested

Edge cases not covered:
Scenario: Compare electricity and gas prices without bill
    Given I am in 'https://energy.comparethemarket.com/energy/v2/?AFFCLIE=TST' page
    And I have entered 'PE2 6YS' as my postcode
    And I select find postcode option
    And I select bill availability option as 'No, I don’t have my bill'
    And I select energy type option as 'Gas & Electricity' 
    And I select next option with preselected energy  
    And I should be navigated to 'Your Energy' no bill page  
    And I select pre-payment meter option 'Yes'
    And I have ticked 'Yes' for my Economy 7 meter option
    And my electricity usage bill is '100' in 'Monthly' frequency
    And my gas usage bill is '100' in 'Monthly' frequency
    And I select next option 
     And I am navigated to 'Your Details' page
    And I have enter my preferred tariff as 'All tariffs'
    And I enter preferred payment type as 'Monthly direct debit'
    And I enter my email address 'asifahmed.github@gmail.com'
    And I checked to receive marketing email
    And I checked terms and conditions
    When I select go to prices button
    Then I wait until the loading screen displayed
    And I am navigated to 'Your Results' page
    And I should see my price comparison results

    Scenario: Compare electricity and gas prices without bill and current usage
    Given I am in 'https://energy.comparethemarket.com/energy/v2/?AFFCLIE=TST' page
    And I have entered 'PE2 6YS' as my postcode
    And I select find postcode option
    And I select bill availability option as 'No, I don’t have my bill'
    And I select energy type option as 'Gas & Electricity' 
    And I select next option with preselected energy  
    And I should be navigated to 'Your Energy' no bill page 
    And I select pre-payment meter option 'Yes'
    And I have ticked 'Yes' for my Economy 7 meter option
    And I don't know electricity usage bill 
    And I don't know gas usage bill 
    And I select next option 
    And I am navigated to 'Your energy usage' page
    And I answer usage questionnaire
    And I select next option 
    And I am navigated to 'Your Details' page
    And I have enter my preferred tariff as 'All tariffs'
    And I enter preferred payment type as 'Monthly direct debit'
    And I enter my email address 'asifahmed.github@gmail.com'
    And I checked to receive marketing email
    And I checked terms and conditions
    When I select go to prices button
    Then I wait until the loading screen displayed
    And I am navigated to 'Your Results' page
    And I should see my price comparison results
Improvement:
Inversion of control and dependency injection


#Reporting

#Settings

#Browser
