```PowerShell
cls
```

## # Install software for HP Photosmart 6515

```PowerShell
$setupPath = "\\TT-FS01\Products\HP\Photosmart 6515\PS6510_1315-1.exe"

Start-Process -FilePath $setupPath -Wait
```

On the **Software Selections** step:

1. Click **Customize Software Selections**.
1. Clear the checkboxes for the following items:
   - **HP Update**
   - **HP Photosmart 6510 series Product Improvement**
   - **Bing Bar for HP (includes HP Smart Print)**
   - **HP Photosmart 6510 series Help**
   - **HP Photo Creations**

[http://support.hp.com/us-en/drivers/selfservice/HP-Photosmart-6510-e-All-in-One-Printer-series---B2/5058334/model/5191793](http://support.hp.com/us-en/drivers/selfservice/HP-Photosmart-6510-e-All-in-One-Printer-series---B2/5058334/model/5191793)
