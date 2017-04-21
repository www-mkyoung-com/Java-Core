Java comes with “**java.util.zip**” library to perform data compression in ZIp format. The overall concept is quite straightforward.

1.  Read file with “**FileInputStream**”
2.  Add the file name to “**ZipEntry**” and output it to “**ZipOutputStream**“

## 1\. Simple ZIP example

Read a file “**C:\\spy.log**” and compress it into a zip file – “**C:\\MyFile.zip**“.

    package com.mkyong.zip;

    import java.io.FileInputStream;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.util.zip.ZipEntry;
    import java.util.zip.ZipOutputStream;

    public class App
    {
        public static void main( String[] args )
        {
        	byte[] buffer = new byte[1024];

        	try{

        		FileOutputStream fos = new FileOutputStream("C:\\MyFile.zip");
        		ZipOutputStream zos = new ZipOutputStream(fos);
        		ZipEntry ze= new ZipEntry("spy.log");
        		zos.putNextEntry(ze);
        		FileInputStream in = new FileInputStream("C:\\spy.log");

        		int len;
        		while ((len = in.read(buffer)) > 0) {
        			zos.write(buffer, 0, len);
        		}

        		in.close();
        		zos.closeEntry();

        		//remember close it
        		zos.close();

        		System.out.println("Done");

        	}catch(IOException ex){
        	   ex.printStackTrace();
        	}
        }
    }

## 2\. Advance ZIP example – Recursively

Read all files from folder “**C:\\testzip**” and compress it into a zip file – “**C:\\MyFile.zip**“. It will recursively zip a directory as well.

    package com.mkyong.zip;

    import java.io.File;
    import java.io.FileInputStream;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.util.ArrayList;
    import java.util.List;
    import java.util.zip.ZipEntry;
    import java.util.zip.ZipOutputStream;

    public class AppZip
    {
        List<String> fileList;
        private static final String OUTPUT_ZIP_FILE = "C:\\MyFile.zip";
        private static final String SOURCE_FOLDER = "C:\\testzip";

        AppZip(){
    	fileList = new ArrayList<String>();
        }

        public static void main( String[] args )
        {
        	AppZip appZip = new AppZip();
        	appZip.generateFileList(new File(SOURCE_FOLDER));
        	appZip.zipIt(OUTPUT_ZIP_FILE);
        }

        /**
         * Zip it
         * @param zipFile output ZIP file location
         */
        public void zipIt(String zipFile){

         byte[] buffer = new byte[1024];

         try{

        	FileOutputStream fos = new FileOutputStream(zipFile);
        	ZipOutputStream zos = new ZipOutputStream(fos);

        	System.out.println("Output to Zip : " + zipFile);

        	for(String file : this.fileList){

        		System.out.println("File Added : " + file);
        		ZipEntry ze= new ZipEntry(file);
            	zos.putNextEntry(ze);

            	FileInputStream in =
                           new FileInputStream(SOURCE_FOLDER + File.separator + file);

            	int len;
            	while ((len = in.read(buffer)) > 0) {
            		zos.write(buffer, 0, len);
            	}

            	in.close();
        	}

        	zos.closeEntry();
        	//remember close it
        	zos.close();

        	System.out.println("Done");
        }catch(IOException ex){
           ex.printStackTrace();
        }
       }

        /**
         * Traverse a directory and get all files,
         * and add the file into fileList
         * @param node file or directory
         */
        public void generateFileList(File node){

        	//add file only
    	if(node.isFile()){
    		fileList.add(generateZipEntry(node.getAbsoluteFile().toString()));
    	}

    	if(node.isDirectory()){
    		String[] subNote = node.list();
    		for(String filename : subNote){
    			generateFileList(new File(node, filename));
    		}
    	}

        }

        /**
         * Format the file path for zip
         * @param file file path
         * @return Formatted file path
         */
        private String generateZipEntry(String file){
        	return file.substring(SOURCE_FOLDER.length()+1, file.length());
        }
    }

_Output_

    Output to Zip : C:\MyFile.zip
    File Added : pdf\Java-Interview.pdf
    File Added : spy\log\spy.log
    File Added : utf-encoded.txt
    File Added : utf.txt
    Done

**Follow Up**  
You may interest at this – [How to decompress it from a Zip file](http://www.mkyong.com/java/how-to-decompress-files-from-a-zip-file/)

## Reference

1.  [Compressing and Decompressing Data Using Java APIs](http://java.sun.com/developer/technicalArticles/Programming/compression/)

[http://www.mkyong.com/java/how-to-compress-files-in-zip-format/](http://www.mkyong.com/java/how-to-compress-files-in-zip-format/)
