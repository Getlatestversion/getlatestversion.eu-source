
# GetLatestVersion public site

This repo holds the Hugo source files for [www.getlatestversion.eu](http://www.getlatestversion.eu)

You will find guidelines and instruction in the [Wiki](https://github.com/Getlatestversion/getlatestversion.eu-source/wiki).


## Staging

```powershell
$env:AZURE_STORAGE_ACCOUNT="stagegetlatestversioneu"
$env:AZURE_STORAGE_KEY="**********"
$env:HUGO_ENVIRONMENT='staging'

hugo --gc --cleanDestinationDir ; hugo deploy --maxDeletes -1

rm -Recurse .\public\ ; md public ; hugo deploy --maxDeletes -1
```

<https://stagegetlatestversioneu.z6.web.core.windows.net/>