package Fetpeo;

import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.Assert;
import org.testng.annotations.AfterClass;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

public class HomePage {

	WebDriver driver;
	@BeforeClass
	public void Navigate_To_PeoFit_Homepage() {

		System.setProperty("webdriver.chrome.driver", "C:\\Users\\hp\\Downloads\\chromedriver-win64\\chromedriver-win64\\chromedriver.exe");
		driver = new ChromeDriver();
		driver.get("https://www.fitpeo.com/");
		driver.manage().window().maximize();
		driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(20));
	}
	@Test
	public void Navigate_To_Revenue_Calculator_Page () throws InterruptedException {
		// Navigate to Revenue Calculator Page
		driver.navigate().to("https://www.fitpeo.com/revenue-calculator");


		// Scroll to the slider section
		Actions actions = new Actions(driver);
		actions.sendKeys(Keys.PAGE_DOWN).perform();
		// Adjust the slider to 820
		WebElement slider = driver.findElement(By.xpath("/html/body/div[1]/div[1]/div[1]/div[2]/div/div/span[1]/span[3]/input"));
		Actions action = new Actions(driver);
		Thread.sleep(2000);
		action.dragAndDropBy(slider, 94, 0).perform();
		Thread.sleep(2000);
        //Scroll down the page
		JavascriptExecutor js = (JavascriptExecutor) driver;
		js.executeScript("window.scrollBy(0, 343)");

		// Update the text field to 560
		WebElement textField = driver.findElement(By.xpath("//input[@id=':R57alklff9da:']"));
		textField.clear();
		WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
		WebElement textField1 = wait.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//input[@id=':R57alklff9da:']")));
		Thread.sleep(3000);
		textField1.sendKeys("560");

		// Select CPT codes
		driver.findElement(By.xpath("/html/body/div[1]/div[1]/div[2]/div[1]/label/span[1]/input")).click();
		driver.findElement(By.xpath("/html/body/div[1]/div[1]/div[2]/div[2]/label/span[1]/input")).click();
		driver.findElement(By.xpath("/html/body/div[1]/div[1]/div[2]/div[3]/label/span[1]/input")).click();
		driver.findElement(By.xpath("/html/body/div[1]/div[1]/div[2]/div[8]/label/span[1]/input")).click();

		//	 Validate Total Recurring Reimbursement
		WebDriverWait wait1 = new WebDriverWait(driver, Duration.ofSeconds(10));
		WebElement totalReimbursement = wait1.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("/html/body/div[1]/div[1]/div[1]/div[1]/div/div[3]/p[2]")));
		String actualTotal = totalReimbursement.getText();
		Assert.assertEquals(actualTotal, "$111105");






	}
	@AfterClass
	public void Teardown () {
		driver.quit();
	}
}
