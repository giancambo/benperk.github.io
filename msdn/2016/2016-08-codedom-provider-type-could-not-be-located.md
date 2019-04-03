# CodeDom provider type could not be located

```
Server Error in '/' Application.
Configuration Error
Description: An error occurred during the processing of a configuration file required to service this request. Please review the specific error details below and modify your configuration file appropriately.
Parser Error Message: The CodeDom provider type "Microsoft.CodeDom.Providers.DotNetCompilerPlatform.CSharpCodeProvider, Microsoft.CodeDom.Providers.DotNetCompilerPlatform, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" could not be located.
Source Error:
Line 91:   <system.codedom>
Line 92:     <compilers>
Line 93:       <compiler language="c#;cs;csharp" extension=".cs" type="Microsoft.CodeDom.Providers.DotNetCompilerPlatform.CSharpCodeProvider, Microsoft.CodeDom.Providers.DotNetCompilerPlatform, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" warningLevel="4" compilerOptions="/langversion:6 /nowarn:1659;1699;1701" />
Line 94:       <compiler language="vb;vbs;visualbasic;vbscript" extension=".vb" type="Microsoft.CodeDom.Providers.DotNetCompilerPlatform.VBCodeProvider, Microsoft.CodeDom.Providers.DotNetCompilerPlatform, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" warningLevel="4" compilerOptions="/langversion:14 /nowarn:41008 /define:_MYTYPE=\&quot;Web\&quot; /optionInfer+" />
Line 95:     </compilers>
Source File:  C:\inetpub\CSharpGuitarBugs\web.config    Line:  93
Version Information: Microsoft .NET Framework Version:4.0.30319; ASP.NET Version:4.0.30319.34274
```

![C# 6 compilation error when deployed to IIS][FIGURE1]
###### Figure 1, C# 6 compilation error when deployed to IIS

After looking at a lot of possibilities, I realized that because I was deploying from a source repository and the repository did not include the BIN directory that I did not have all the dependent binaries.  Once I copied the BIN folder to my IIS server into the root of the web site, all worked out just fine.

![C# 6 compilation error when deployed to IIS][FIGURE2]
###### Figure 2, C# 6 compilation error when deployed to IIS

[FIGURE1]: ../images/2016/msdn-0949.png "Figure 1, C# 6 compilation error when deployed to IIS"
[FIGURE2]: ../images/2016/msdn-0950.png "Figure 2, C# 6 compilation error when deployed to IIS"
