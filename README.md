echo "# weatherData" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/acdelgado27/weatherData.git
git push -u origin masterimport edu.duke.*;
import org.apache.commons.csv.*;
import java.io.*;
public class weatherParsing {
        public CSVRecord coldestHourInFile(CSVParser parser) {
            CSVRecord smallestSoFar = null;
           
            for (CSVRecord currentRow : parser) {
                if (smallestSoFar == null) {
                    smallestSoFar = currentRow;
                } 
                else {
                    double currentTemp = Double.parseDouble(currentRow.get("TemperatureF"));
                    double smallestTemp = Double.parseDouble(smallestSoFar.get("TemperatureF"));
                    if (currentTemp == -9999){
                        continue;
                    }
                    if (currentTemp < smallestTemp) {
                        smallestSoFar = currentRow;
                    }
                }
              }
            return smallestSoFar;
       }
       public CSVRecord coldestInManyDays() {
            CSVRecord smallestSoFar = null;
            DirectoryResource dr = new DirectoryResource();
            String file = "";
            for (File f : dr.selectedFiles()) {
                FileResource fr = new FileResource(f);
                CSVRecord currentRow = coldestHourInFile(fr.getCSVParser());
                if (smallestSoFar == null) {
                    smallestSoFar = currentRow;
                    file = f.getAbsolutePath();
                } else 
                if(smallestSoFar.equals("-9999")){
                    continue;
                }
                {
                    double currentTemp = Double.parseDouble(currentRow.get("TemperatureF"));
                    double smallestTemp = Double.parseDouble(smallestSoFar.get("TemperatureF"));
                    if (currentTemp < smallestTemp) {
                        smallestSoFar = currentRow;
                        file= f.getAbsolutePath();
                    }
                }
            }
            System.out.println(file);
            return smallestSoFar;
       }
        public void testColdestInManyDays(){
            CSVRecord coldest = coldestInManyDays();
            System.out.println(coldest);
       }
       public CSVRecord lowestHumidityInFile(CSVParser parser){
             CSVRecord lowestHumiditySoFar = null;
             for (CSVRecord currentRow : parser){
                 
                 if (lowestHumiditySoFar == null){
                    lowestHumiditySoFar=currentRow;
                 } else{
                    double currentHumidity = Double.parseDouble(currentRow.get("Humidity"));
                    double lowestHumidity = Double.parseDouble(lowestHumiditySoFar.get("Humidity"));
                   
                 if(currentRow.get("Humidity").equals("N/A")){
                        continue;
                    }
                 if (currentHumidity < lowestHumidity) {
                    lowestHumiditySoFar = currentRow;
                 }
                }
            }
                return lowestHumiditySoFar;
       }
        
       public void testLowestHumidity(){
            FileResource fr = new FileResource();
            CSVParser parser = fr.getCSVParser();
            CSVRecord lowest = lowestHumidityInFile(parser);
            System.out.println("lowest humidity null"+lowest);
       }
        public CSVRecord lowestHumidityInManyDays() {
            CSVRecord lowestSoFar = null;
            DirectoryResource dr = new DirectoryResource();
            for (File f : dr.selectedFiles()) {
                FileResource fr = new FileResource(f);
                CSVRecord currentRow = lowestHumidityInFile(fr.getCSVParser());
                if (lowestSoFar == null) {
                    lowestSoFar = currentRow;
                } 
                if(currentRow.get("Humidity").equals("N/A")){
                        continue;
                    } 
                else {
                    double currentHumidity = Double.parseDouble(currentRow.get("Humidity"));
                    double lowestHumidity = Double.parseDouble(lowestSoFar.get("Humidity"));
                    if (currentHumidity < lowestHumidity) {
                        lowestSoFar = currentRow;
                    }
                }
            }
            return lowestSoFar;
       }
       public void testLowestHumidityInManyDays(){
           CSVRecord lowest = lowestHumidityInManyDays();
           System.out.println("lowest humidity in many days null"+lowest);
       }
       public double averageTemperature(CSVParser parser){
            double averageTemp;
            double sum = 0;
            int counter = 0;
            for (CSVRecord record : parser){
                sum += Double.parseDouble(record.get("TemperatureF"));
                counter++;
            }
            averageTemp= sum/counter;
            return averageTemp;
       }
       public double averageTemperatureWithHighHumidityInFile(CSVParser parser, double level){
            double averageTemp;
            double sum = 0;
            int counter = 0;
            for (CSVRecord record : parser){
                double humidity = Double.parseDouble(record.get("Humidity"));
                if (humidity >= level){
                    sum = Double.parseDouble(record.get("TemperatureF"));
                    counter++;
                }
               
            }
            averageTemp=sum/counter;
            return averageTemp;
       }
       public void testAverageTemp(){
            FileResource fr = new FileResource();
            CSVParser parser = fr.getCSVParser();
            double average = averageTemperature(parser);
            System.out.println(average);
       }
       public void testAverageTempHumidity(){
            FileResource fr = new FileResource();
            CSVParser parser = fr.getCSVParser();
            double averageTempHumidity = averageTemperatureWithHighHumidityInFile(parser, 60);
            System.out.println("avg temp was" + averageTempHumidity);
       }
}



