# DemoBlazeFeatures
Selenium test to utilize hybrid framework to test demo blaze website acceptance features

#Author: Piya Bhattacharya
#Date: 20122023
#Description: Registration verification of Demo_Blaze website

@runAll @sanitytest 
Feature: To validate if the user is able to access Demo_blaze Website in the browser and successfully registers in it.

  @EPIC01 @Edu_3 @registration @sanity
  Scenario: To verify that the sign up modal is visible on home page.
    Given User should be on Demo Blaze website
    When User clicks on Sign Up option
    Then Sign Up Pop Up should be visible

 @EPIC01 @Edu_4 @registration @sanity
  Scenario Outline: To verify that the sign up functionality is working properly.
    Given User should be on Demo Blaze website
    When User clicks on Sign Up option
    Then Sign Up Pop Up should be visible
    And Username and Password options should be visible on pop up of sign up
    When User enters <username> in the textbox
    And User clears the name and again enters the <username>
    And User enters <password> on the textbox
    And User clicks on Sign Up button
    Then "Sign up successfull" message alert should be visible and user should be able to accept the message
Examples: 
      | username                  | password  |
      | DemoBlazeTestUser34452637677 | DemoBlazeTestUser1257627 |
      | DemoBlazeTestUser3446538627 | DemoBlazeTestUser124265 |
      
      
      @EPIC01 @Edu_4 @datadriven @sanity
  Scenario Outline: Sign up functionality is working properly for different set of data.
  
    Given User should be on Demo Blaze website
    When User clicks on Sign Up option
    When User enters details in the textbox from excel data "username"
    And User enters details in the textbox from excel data password "password"
    And User clicks on Sign Up button
    Then "Sign up successfull" message alert should be visible and user should be able to accept the message

#Author: Piya Bhattacharya
#Date: 20122023 
#Description: Url verification of Demo_Blaze website
@runAll @sanitytest 
Feature: Verification of URL for Demo_Blaze
  

  @EPIC01 @Edu_1 @sanity
  Scenario: To verify the input of URL for Demo_blaze on chrome browser
    Given User should be on Google home page
    When User enters valid url of demo blaze
    Then User gets list of related websites as per input
    
 @EPIC01 @Edu_2 @sanity
  Scenario: To verify whether the site displays Greetings to the user.
    Given User should be on Google home page
    When User enters valid url of demo blaze
    And User Navigates to Demo_Blaze website
    Then It should display greetings to user as "Welcome to Demo_blaze"

#Step Definitions

package stepDefinitions;

import java.io.FileInputStream;
import java.io.IOException;
import java.time.Duration;

import org.apache.poi.xssf.usermodel.XSSFCell;
import org.apache.poi.xssf.usermodel.XSSFRow;
import org.apache.poi.xssf.usermodel.XSSFSheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.openqa.selenium.Alert;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import Generic.Constants;
import Generic.Drivers;
import PageObjects.PageObjects;
import io.cucumber.java.en.And;
import io.cucumber.java.en.Given;
import io.cucumber.java.en.Then;
import io.cucumber.java.en.When;

public class RegistrationFunctionalityVerification{
	
    public static WebDriver driver = Drivers.webDriverSetup(); //caling the webdriver as set in constants as per the requirement
	
	PageObjects object = new PageObjects(driver); //constructor call for page objects
		
	 @Given("User should be on Demo Blaze website")
	 public void user_on_demo_blaze_home_page() throws Throwable {		 
		 		 
     driver.navigate().to(Constants.URL);
     
     driver.manage().window().maximize();
     
     Thread.sleep(2000);
		 
	 if(object.logoBarDemoBlaze.isDisplayed()==true)
		{
			System.out.println("The Logo Bar is displayed and user on home page");
		}
		else {System.out.println("No Logo Displayed for the page. ");
		}
	 
	 }	 
	
	@Then("User should be navigated Demo Blaze website")
	public void user_navigated_to_demoblaze_website()
	{
		String titlevalue= driver.getTitle();
		
		if(titlevalue.equalsIgnoreCase("STORE"))
		
		System.out.println("Inside Step - User should be navigated Demo Blaze website");
				
	}
	
	
	@And("Sign Up option should be visible on home page")
	public void sign_up_option_visible()
	{
		boolean displayedSignUp = object.SignUpLinkOnHomePage.isDisplayed();
		
		if(displayedSignUp == true)
			System.out.println("Sign Up link on home page displayed as expected.");
		else
			System.out.println("Sign Up link on home page is not displayed as expected.");
	}
	
	@When("User clicks on Sign Up option")
	public void click_sign_up_option()
	{
		WebDriverWait wait = new WebDriverWait(driver,Duration.ofSeconds(3));
		
		wait.until(ExpectedConditions.elementToBeClickable(object.SignUpLinkOnHomePage));
		
		object.SignUpLinkOnHomePage.click();
	}
	
	@Then("Sign Up Pop Up should be visible")
	public void sign_up_pop_up_verification()
	{
        WebDriverWait wait = new WebDriverWait(driver,Duration.ofSeconds(3));
		
		//alertIsPresent() condition applied
	    
	    if(wait.until(ExpectedConditions.visibilityOf(object.signUpTitleOnModal)) != null)
	    	
	    System.out.println("Alert exists for sign up");
	      
	    else
	      
	    	System.out.println("Alert for sign up does not exists");
	}
	
	@And("Username and Password options should be visible on pop up of sign up")
	public void username_password_button_verification()
	{
		//Alert alert;
		
		//driver.switchTo().alert();
		
		// switching to alert to verify the Sign Up feature.
			
		String modalHeader= object.signUpTitleOnModal.getText();
		
		System.out.println("Alert exists for sign up as title: "+modalHeader);
		
		boolean signInModalDisplayed = object.SignInModal.isDisplayed();
		
		if(signInModalDisplayed==true)
		{
			boolean userNameDisplayed = object.signUpUsernameOnModal.isDisplayed();
			
			boolean passworfieldDisplayed = object.signUpPasswordOnModal.isDisplayed();
			
			if(userNameDisplayed== true && passworfieldDisplayed==true)
			{
				System.out.println("Alert exists for sign up and Username, Password fields displayed as expected");
			}
			else
				System.out.println("Alert exists for sign up but Username, Password fields not shown");
		}
						
	}
	
	@When("User enters username in the textbox")
	public void enter_username_into_textbox_signup() throws Throwable
	{
		WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(2));
		
		wait.until(ExpectedConditions.visibilityOf(object.signUpUsernameTextBoxOnModal)); //checking the username text box field is visible
		
		object.signUpUsernameTextBoxOnModal.sendKeys("DemoBlazeTestUser123");
		
		Thread.sleep(3000);
		
		System.out.println("Alert exists for sign up and Username given by the user");
		
	}
	
	@When("^User enters (.+) in the textbox$")
	public void user_attemps_to_login_parameterization(String string1) throws Throwable 
	{
		System.out.println("string1 = " + string1);
		
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(2));
		
		wait.until(ExpectedConditions.visibilityOf(object.signUpUsernameTextBoxOnModal)); //checking the username text box field is visible
		
		object.signUpUsernameTextBoxOnModal.sendKeys(string1);
		
		Thread.sleep(3000);
		
		System.out.println("Alert exists for sign up and Username given by the user");
						
	}
		
	
	@And("^User clears the name and again enters the (.+)$")
	public void reenter_username_into_textbox_signup(String string4) throws Throwable
	{
		
		Thread.sleep(3000);
		
		object.signUpUsernameTextBoxOnModal.clear(); 
		
		//the sign up name is cleared
				
		object.signUpUsernameTextBoxOnModal.sendKeys(string4); //user is able to re enter a valid name to verify the field is editable
		
		System.out.println("Alert exists for sign up and re-entered the valid username");
	}
		
	@And("^User enters (.+) on the textbox$")
	public void enter_password_into_textbox_signup(String password) throws Throwable
	{
		WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(2));
		
		wait.until(ExpectedConditions.visibilityOf(object.signUpPasswordTextBoxOnModal)); //checking the password text box field is visible
		
		object.signUpPasswordTextBoxOnModal.sendKeys(password);
		
		Thread.sleep(3000);
		
		System.out.println("Alert exists for sign up and user entered their Password");
		
	}
	
	//using apache poi file read feature to read username and password from excel
	
	@When("User enters details in the textbox from excel data {string}")
	public void user_enters_details_in_the_textbox_from_excel_data_(String username) throws IOException, Throwable {
			
		System.out.println("Inside Step - User should enteres data username from excel to Demo Blaze website");
		// Read Excel sheet
        FileInputStream file = new FileInputStream((Constants.filepath));
        XSSFWorkbook workbook = new XSSFWorkbook(file);
        XSSFSheet sheet = workbook.getSheetAt(1);

        // Extract URL from the first row
        XSSFRow row = sheet.getRow(1); // Assuming the URL is in the second row
        XSSFCell cell = row.getCell(0); //first cell
        username = cell.getStringCellValue();
        
        driver.findElement(By.xpath("//input[@id='sign-username']")).sendKeys(username);
		
		Thread.sleep(3000);
		
		System.out.println("Alert exists for sign up and Username given by the user");
        
        // Close workbook
        workbook.close();		

	}
	
	
	//using apache poi file read feature to read username and password from excel
	
	@And("User enters details in the textbox from excel data password {string}") 
	public void user_enters_details_in_the_textbox_from_excel_data_password(String password) throws IOException, Throwable {
		
		
		System.out.println("Inside Step - User should enteres data password from excel to Demo Blaze website");
		// Read Excel sheet
        FileInputStream file = new FileInputStream((Constants.filepath));
        XSSFWorkbook workbook = new XSSFWorkbook(file);
        XSSFSheet sheet = workbook.getSheetAt(1);

        // Extract URL from the first row
        XSSFRow row = sheet.getRow(1); // Assuming the URL is in the second row
        XSSFCell cell = row.getCell(1); //second cell
        password = cell.getStringCellValue();
        
        driver.findElement(By.xpath("//input[@id='sign-password']")).sendKeys(password);
		
		Thread.sleep(3000);
		
		System.out.println("Alert exists for sign up and Password given by the user");
        
        // Close workbook
        workbook.close();
		
	}
	
	
	@And("User clicks on Sign Up button")
	public void click_on_signup_button()
	{
		object.signUpSignUpButtonOnModal.click();
		
		System.out.println("User clicked on sign up button on the modal");
	}
	
	
	
	@Then("{string} message alert should be visible and user should be able to accept the message")
	public void sign_up_successfull_message_displayed(String signUpSuccess) throws Throwable
 	{
       
		System.out.println("In The Step - to verify User has accepted the success message");
		
		WebDriverWait wait = new WebDriverWait(driver,Duration.ofSeconds(6));
		
		wait.until(ExpectedConditions.alertIsPresent());
				
		Alert alert = driver.switchTo().alert();
		
		String alertSucessText= alert.getText();
		
		System.out.println("User clicked on sign up button on the modal and success message displayed as: "+alertSucessText);
		
		if(alertSucessText.equalsIgnoreCase("Sign up successful."))
		{
		System.out.println("Success message displayed as: "+signUpSuccess);
		}
		else
			System.out.println("Success message not same as expected and displayed as: "+signUpSuccess);
		
		Thread.sleep(6000);
		
		alert.accept();  //User should be able to accept the sucess message
		
		System.out.println("User has accepted the success message");
		
		//driver.close();
		
		//driver.quit();
		
	}	

}


#StepDefinition for URL

package stepDefinitions;

import java.time.Duration;
import org.junit.Assert;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import Generic.Constants;
import Generic.Drivers;
import PageObjects.PageObjects;
import java.util.Set;
import io.cucumber.java.en.And;
import io.cucumber.java.en.Given;
import io.cucumber.java.en.Then;
import io.cucumber.java.en.When;

public class UrlVerify{
	
    public static WebDriver driver = Drivers.webDriverSetup(); //caling the webdriver as set in constants as per the requirement
	
	PageObjects object = new PageObjects(driver); //constructor call for page objects			
	
	@Given("^User should be on Google home page$") 
	public void user_on_google_home_page() {
		
		
		driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(5));
		
		driver.navigate().to("https://www.google.com");
		
		driver.manage().window().maximize();
		
		String titleOfBrowser = driver.getTitle();
				
		String ExpectedTitle = "Google";
		
		Assert.assertEquals(titleOfBrowser, ExpectedTitle);
		
		System.out.println("Inside Step - user is on home page browser Google"); // validating the title			
		
	}

	@When("User enters valid url of demo blaze")
	public void user_enters_valid_url_of_demo_blaze(){
		
		driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(5));				
		
		driver.get(Constants.URL);// testing with direct link		
        
		driver.manage().window().maximize();		
		
		System.out.println("User entered valid url and it is displayed on browser");
		
		//user entered the url of DemoBlaze website
		
		System.out.println("Inside Step - User enters valid url of demo blaze");								
	}
	
	
	@And("User Navigates to Demo_Blaze website")
    public void user_navigated_to_demo_blaze() {
		
		driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(5));
		
		//testing if visible in logobar
	    
	    boolean logobar = object.logoBarDemoBlaze.isDisplayed(); 
	 
	    if(logobar==true) {
		
		System.out.println("Inside Step - User enters valid url of demo blaze and inside the website");}
	}
	
				 	 
	 @Then("It should display greetings to user as {string}")
	 public void greetings_message_displayed(String welcomeUrlMessage)
		{
			
			//as no details given where the message should be visible		
		 
		 System.out.println("Inside step- To verify the Message"+welcomeUrlMessage+ "exist as an alert");
		 
		//testing if visible as an alert
		    		   
		 WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(3));
		 
		 wait.until(ExpectedConditions.alertIsPresent()); //waiting if any alert to display the welcome message is visible
		 
		 System.out.println("Message does not exist as an alert");
		 
		 driver.close();
	}
	 
	 @Then("User gets list of related websites as per input")
		public void list_displayed_related_links() {
			
			
			String urlListForDemoblaze= driver.getCurrentUrl();
			
			System.out.println("Inside Step - User gets list of related websites as per input");
			
			System.out.println("The url title displayed as follows: " +urlListForDemoblaze);
			
			String currentWindow = driver.getWindowHandle(); // return the current window id. where selenium is currently in
            
			System.out.println("currentWindow = " + currentWindow);

            // get all the window ids which are opened by selenium
            Set<String> set = driver.getWindowHandles();
             
            System.out.println(set.toString());
             
		}
	 
}

#TestRunnerFile

package testRunner;
import org.junit.runner.RunWith;

import io.cucumber.junit.Cucumber;
import io.cucumber.junit.CucumberOptions;
	
	@RunWith(Cucumber.class) 

	@CucumberOptions(features = "src/test/java/features",glue="stepDefinitions", monochrome=true,  
			tags="@sanitytest")
	public class TestRunner {

	}


#Generic class files src/main/java

package Generic;

public class Constants {
	
	
    public static String URL = "https://www.demoblaze.com/"; 
    
    public static String sheetName = "Demoblaze_TestcaseSheet";
    
    public static String filepath = "C:\\Users\\Bhattacharya\\git\\DemoBlazeRepository\\DemoBlazeProject\\TestData\\Demoblaze_TestcaseSheet.xlsx";
	
	public static String browserName= "chrome";
	

}

#Driver class in src/main/java

package Generic;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;


public class Drivers {
	
	protected static WebDriver driver;
			
		//driver options
			
		public static WebDriver webDriverSetup() {
		
		
			if(Constants.browserName.equalsIgnoreCase("chrome")) 
	 	   { 
				System.setProperty("webdriver.chrome.driver", "C:\\Users\\Bhattacharya\\git\\DemoBlazeRepository\\DemoBlazeProject\\Driver\\chromedriver.exe");
	 	       
	 	       driver= new ChromeDriver(); 
	 	    } 
		
			return driver; //initialized driver value as mentioned in constants

	    }
			
    
}


