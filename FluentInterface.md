# Fluent interface

## Page implementation

```public class MenuCities extends Page {

    public MenuCities(TestInstance testInstance) {
        super(testInstance, By.id("menu-cities"));
    }

    @Step
    public Forecast chooseCity(String name) {

        find(cityWithName(name)).click();

        waitFor().maxTime(10, TimeUnit.SECONDS).visibility(new NavMain(testInstance).loadedCity("dasdsad"));

        assertThat().page().titleContainsText("Krakow Weather XXX");
        assertThat().element().withName("Forecast for tonight").exists(new FeedTabs(testInstance).tabForDay("Tonight"));

        return new Forecast(testInstance);
    }

    public By cityWithName(String name) {
        return locate(By.xpath(".//a[contains(text(), '" + name + "')]"));
    }
}```

## Page interface
public abstract class Page {
    protected TestInstance testInstance;
    private By container;

    public Page(TestInstance testInstance) {
        this.testInstance = testInstance;
    }

    public Page(TestInstance testInstance, By container) {
        this.testInstance = testInstance;
        this.container = container;
    }

    public By locate(By... by) {
          return new ByChained(container, new ByChained(by));
    }

    public WebElement find(By by) {
        return testInstance.getDriver().findElement(by);
    }

    public List<WebElement> findAll(By by) {
        return testInstance.getDriver().findElements(by);
    }

    public Assertions assertThat() {
        return new Assertions(testInstance);
    }

    public WaitTool waitFor() {
        return new WaitTool(testInstance);
    }

    public WaitTool waitFor(String desc) {
        return new WaitTool(testInstance).withDesc(desc);
    }

}
