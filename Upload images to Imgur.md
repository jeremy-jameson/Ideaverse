## Add images to Imgur for Obsidian notebook (Ideaverse)

To add images to the Obsidian notebook:

1. Move the images to the [[Metatropi]] content repository
1. Rename the image files using the corresponding SHA1 hashes
1. Commit the changes to source control
1. Upload the images to Imgur

```powershell
cls
```

### # Move screenshots to **Screenshots** folder in **Metatropi-Content** repository

```powershell
$sourcePath = "C:\Users\jjameson\OneDrive\Pictures\Screenshots\*"
$destinationPath = "C:\BackedUp\Azure-DevOps\Metatropi-Content\Screenshots\"

mv $sourcePath $destinationPath
```

```powershell
cls
```

### # Rename image files using corresponding SHA1 hashes

```powershell
pushd C:\BackedUp\Azure-DevOps\Metatropi-Content\Scripts

.\Rename-Screenshots.ps1 |
    sort Path, NewFilename |
    select Path, NewFilename, OldFilename, Width, Height |
    Export-Csv ..\Files.csv -NoTypeInformation -Append

popd
```

### Commit changes to source control

### Upload images to Imgur

Use [EasyImgur]([EasyImgur | Desktop image-uploading tool](https://bryankeiren.com/easyimgur/)) to upload the images to **Imgur**. Then copy/paste the URL for each image into a Markdown link in Obsidian.

Example:

```markdown
![Mandy enjoying some French delicacies](https://i.imgur.com/ryC27qe.jpeg)
```

![Upload images to Imgur using EasyImgur](https://i.imgur.com/verSlF5.png)
