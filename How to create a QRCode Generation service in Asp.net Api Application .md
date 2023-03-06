# How to create a QRCode Generation service in Asp.net Api Application

# Problem
QRCode is being used to track or store information in a graphical way now-a-days as almost all the smartphones have built-in QR readers.

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
We can use QRCoder-ImageSharp nuget package to help us generate our custom QR Code.

# Solution
There are tons of other packages available that can help us achieve the same result but some of them are not compatible with the latest .net versions (i.e. 6 and 7).

Coming to the solution, first we have to install the QRCoder-ImageSharp nuget package from the Package Manager Console.

After that we will create a new service with a string parameter, that will convert the string into a QRCode.

```
 public string GenerateQRCode(string text)
        {
            QRCodeData qrCodeData;
            using (QRCodeGenerator qrGenerator = new QRCodeGenerator())
            {
                qrCodeData = qrGenerator.CreateQrCode(text, QRCodeGenerator.ECCLevel.Q);
            }
            // Image Format
            var imgType = Base64QRCode.ImageType.Png;

            var qrCode = new Base64QRCode(qrCodeData);
            //Base64 Format
            string qrCodeImageAsBase64 = qrCode.GetGraphic(7, "Blue", "White", true, imgType);
            return string.Format("data:image/png;base64,{0}", qrCodeImageAsBase64);
        }
```
It will provide us a blob in return.

In the GetGraphic function we can set the pixels along with the color. Here the pixel is 7 and color is set to blue and white.

Now we can utilize it internally or directly from our Api endpoint.
