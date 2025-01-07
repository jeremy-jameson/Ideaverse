---
created: 2025-01-05T20:08:46Z
modified: 2025-01-07T15:20:30Z
---

```PowerShell
cls
```

### # Configure certificate for Operations Manager

#### # Create certificate for Operations Manager

##### # Create request for Operations Manager certificate

```PowerShell
& "C:\NotBackedUp\Public\Toolbox\Operations Manager\Scripts\New-OperationsManagerCertificateRequest.ps1"
```

##### # Submit certificate request to Certification Authority

```PowerShell
$adcsUrl = [Uri] "https://cipher01.corp.technologytoolbox.com"

C:\NotBackedUp\Public\Toolbox\PowerShell\Add-InternetSecurityZoneMapping.ps1 `
    -Zone LocalIntranet `
    -Patterns $adcsUrl.AbsoluteUri

Start-Process $adcsUrl.AbsoluteUri
```

> **Note**
>
> Copy the certificate request to the clipboard.

**To submit the certificate request to an enterprise CA:**

1. On the computer hosting the Operations Manager feature for which you are requesting a certificate, start Internet Explorer, and browse to Active Directory Certificate Services site ([https://cipher01.corp.technologytoolbox.com/](https://cipher01.corp.technologytoolbox.com/ "https://cipher01.corp.technologytoolbox.com/")).
1. On the **Welcome** page, click **Request a certificate**.
1. On the **Advanced Certificate Request** page, click **Submit a certificate request by using a base-64-encoded CMC or PKCS #10 file, or submit a renewal request by using a base-64-encoded PKCS #7 file.**
1. On the **Submit a Certificate Request or Renewal Request** page, in the **Saved Request** text box, paste the contents of the certificate request generated in the previous procedure.
1. In the **Certificate Template** section, select the Operations Manager certificate template (**Technology Toolbox Operations Manager**), and then click **Submit**. When prompted to allow the digital certificate operation to be performed, click **Yes**.
1. On the **Certificate Issued** page, click **Download certificate** and save the certificate.

```PowerShell
cls
```

##### # Import the certificate into the certificate store

```PowerShell
$certFile = "C:\Users\$env:USERNAME\Downloads\certnew.cer"

Import-Certificate `
    -FilePath $certFile `
    -CertStoreLocation "Cert:LocalMachine\My"

If ($? -eq $true)
{
    Remove-Item $certFile
}
```

#### # Import the certificate into Operations Manager using MOMCertImport

```PowerShell
$hostName = ([System.Net.Dns]::GetHostByName(($env:computerName))).HostName

if ($hostname -like '*.extranet.technologytoolbox.com') {
    $certImportToolPath = "\\EXT-FS01\Products\Microsoft" `
        + "\System Center 2019\SCOM\SupportTools\AMD64\MOMCertImport.exe"
}
else {
    $certImportToolPath = "\\TT-FS01\Products\Microsoft" `
        + "\System Center 2019\SCOM\SupportTools\AMD64\MOMCertImport.exe"
}

& $certImportToolPath /SubjectName $hostName
```

#### # Restart the System Center Operations Manager agent

```PowerShell
Restart-Service HealthService
```
