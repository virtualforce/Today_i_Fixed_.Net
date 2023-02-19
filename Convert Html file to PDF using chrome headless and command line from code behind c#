Convert Html file to PDF using chrome headless and command line from code behind c#

# Problem
How to convert Html file to PDF using command line from code behind c# (The use of Bat file for executing the cmd command)

# Environment
Visual Studio 2019, .Net 4.6.1, C#, Console Application

# How you fix it
With the help of Command line and by creating a bat file, the cmd command can be executed successfully. The conversion can be achieved by executing command using headless chrome and the command is basically used to execute chrome exe.

# Solution
First you need to create a .bat file and add the following cmd command in that bat file


```
"c:/Program Files/Google/Chrome/Application/chrome.exe" --headless --disable-gpu --print-to-pdf=%2 %1
rem "c:/Program Files/Google/Chrome/Application/chrome.exe" --headless --disable-gpu --print-to-pdf=c:/temp/123.pdf c:/temp/8896635555.html

```

Now, use the below method to execute the bat file using cmd.

```
private void GeneratePDFFILE(string Cr, string HtmlFilePath, string BatFilePath, string PDFOutPath)
{
    METHOD_NAME = "[M360Controller => GeneratePDFFILE]";

    Log4Bayan.Log(String.Format("{0} {1}", METHOD_NAME, Execution_Started) + " For PDF Generation", Info);

    string commandToExecute = "" + BatFilePath + "HtmlToPdf.bat " + HtmlFilePath + Cr + ".html " + PDFOutPath + Cr + ".pdf";

    Process.Start(@"cmd", @"/c " + commandToExecute);

}

```
