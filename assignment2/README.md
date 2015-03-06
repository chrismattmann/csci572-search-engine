# Assignment 2 Solr Indexing and Ranking

### Install Solr with Tika 1.8-SNAPSHOT
1. Pull tika truck and build:
```
git clone https://github.com/apache/tika.git
cd tika
mvn clean install
```
2. Pull solr 4.10:
```
svn co http://svn.apache.org/repos/asf/lucene/dev/branches/lucene_solr_4_10/
```

3. Update ```lucene_solr_4_10/lucene/ivy-settings.xml``` and ```lucene_solr_4_10/lucene/ivy-versions.properties```:

    - Change line 138 ```org.apache.tika.version = 1.5``` to ```org.apache.tika.version = 1.8-SNAPSHOT```.
    - Uncomment line 46 - 51:

    ```
     46     <filesystem name="local-maven-2" m2compatible="true" local="true">
     47       <artifact
     48           pattern="${local-maven2-dir}/[organisation]/[module]/[revision]/[module]-[revision].[ext]" />
     49       <ivy
     50           pattern="${local-maven2-dir}/[organisation]/[module]/[revision]/[module]-[revision].pom" />
     51     </filesystem>
    ```
    - Uncomment line 56:
    ```
    <resolver ref="local-maven-2" />
    ```
4. Build Solr:
```
cd lucene_solr_4_10
ant compile
```

### Install gdal, ocr, ffmpeg on Ubuntu 14.10 (may works on 14.04)
1. Install GDAL
```
sudo apt-get install gdal-bin
```
2. Install OCR support
```
sudo apt-get install tesseract-ocr
```
3. Install ffmpeg
```
sudo add-apt-repository ppa:kirillshkrogalev/ffmpeg-next
sudo apt-get update
sudo apt-get install ffmpeg
```

### Verify all tools are working

Sample TIFF, FITS and AVI data are in ```test/``` dir.

1. Verify GDAL
```
cd tika/tika-app/target
java -jar tika-app-1.8-SNAPSHOT.jar -m <path/to/a/fits/file>
# should be able to see content-length, content-type, etc
# Sample output:
# Content-Length: 89280
# Content-Type: application/fits
# X-Parsed-By: org.apache.tika.parser.DefaultParser
# X-Parsed-By: org.apache.tika.parser.gdal.GDALParser
# resourceName: adctest1.fits
```
2. Verify Tesseract
```
# verify tesseract
tesseract -psm 3 <path/to/a/tiff/file> out.txt
cat out.txt
# should be able to see text dumped from the tif file.

# verify Tesseract with Tika
cd tika/tika-app/target
java -jar tika-app-1.8-SNAPSHOT.jar -t CCITT_1.TIF
# should be able to see text dumped from the tif file.
```
3. Verify FFMpeg
```
# AT THE TIME I WROTE THIS README, THE tika-ffmpeg IS NOT WORKING
cd tika/tika-app/target
java -jar tika-app-1.8-SNAPSHOT.jar -m <path/to/a/avi/file>
```