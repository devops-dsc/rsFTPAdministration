rsFTPAdministration
===================

<pre>
   WindowsFeature FTP
    {
        Ensure = "Present"
        Name = "Web-Ftp-Server"
    }
    WindowsFeature WebMGMT
    {
        Ensure = "Present"
        Name = "Web-Mgmt-Service"
    }
    WindowsFeature IISConsole
    {
        Ensure = "Present"
        Name = "Web-Mgmt-Console"
    }
    WindowsFeature FTPExt
    {
        Ensure = "Present"
        Name = "Web-Ftp-Ext"
    }
    rsIISAuth FTP_rsftp
    {
        Username = "rsftp"
        Password = "mypassword1!"
        Ensure = "Present"
    }
    rsIISAuth FTP_ftpadmin
    {
        Username = "ftpadmin"
        Password = "mypassword1!"
        Ensure = "Present"
    }
    rsFTP MySite
    {
        Name = "MySite"
        Binding = @(":990:")
        Path = "C:\inetpub\ftproot\mysite"
        Ensure = "Present"
        SSLEnabled = $True
        CertHash = (Get-ChildItem Cert:\LocalMachine\My | ? Subject -like *$env:COMPUTERNAME*).Thumbprint
        UserIsolation = $true
        LowPassivePort = "5000"
        HighPassivePort = "5020"
        ExternalIp4Address = "127.0.0.1"
        FullAccess = @("rsftp","ftpadmin")
        #ChangeAccess = @("myChangeFTP")
        #ReadAccess = @("myReadFTP")
        #NoAccess = @("NoAccess")
    }
</pre>