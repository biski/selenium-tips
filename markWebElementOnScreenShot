# Mark WebElement on screenshot


```java
    public static void makeScreenShotWithElementMarked(WebDriver driver, WebElement webElement) {
        try {
            TakesScreenshot takesScreenshot = (TakesScreenshot) driver;
            File screenshot = takesScreenshot.getScreenshotAs(OutputType.FILE);
            BufferedImage bufferedImage = ImageIO.read(screenshot);

            Graphics2D graph = bufferedImage.createGraphics();
            graph.setColor(Color.RED);
            Point location = webElement.getLocation();
            Dimension size = webElement.getSize();
            graph.setStroke(new BasicStroke(5));
            graph.draw(new Rectangle(location.getX(), location.getY(), size.getWidth(), size.getHeight()));
            graph.dispose();

            ImageIO.write(bufferedImage, "png", screenshot);
            File destFile = new File("target/" + screenshot.getName());
            FileUtils.copyFile(screenshot, destFile);

            System.out.println("Save screenshot to: " + destFile.getAbsolutePath());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```
