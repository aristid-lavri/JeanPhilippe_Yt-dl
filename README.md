# JeanPhilippe_Yt-dl
A Windows Runtime library, that allow you to get downloadable url of Youtube video link.

## Overview
JeanPhilippe_Yt-dl is a library for Windows Runtime (WinRT), written in C#, that allow you to get downloadable url of Youtube video link.

## Target platforms

- Windows 8.1
- Windows Phone 8.1
- WinRT

## NuGet

[JeanPhilippe_Yt-dl at NuGet](http://nuget.org/packages/jeanphilippe_Yt-dl/1.0.2)

    Install-Package jeanphilippe_Yt-dl

## License

The JeanPhilippe_Yt-dl URL-extraction code is licensed under the [MIT License](http://opensource.org/licenses/MIT)

## Example code

**How to get the downloadable Url of a video and  to download it**
```c#
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using Windows.Storage;
using System.Threading.Tasks;
using jeanphilippe_Yt-dl;

// your Youtube video url
string Url = "http://www.youtube.com/watch?v=bDCh8frla2c";

// you must need to create Youtube ApiKey
//go to   and create your Youtube ApiKey 
string ApiKey = " your Youtube Apikey here";

string ApplicationName = "Name of your application";

private async Task<StorageFile> Video_Downloading(string videoUrl, string fileName)
{
            ProcessDownload process = new ProcessDownload(ApiKey, ApplicationName);

            IEnumerable<VideoInfo> List_video = await DownloadUrlResolver.GetDownloadUrls(videoUrl);

            VideoInfo video = List_Video.First(info => info.VideoType == VideoType.Mp4 && info.Resolution == 360);

            StorageFile file = await ApplicationData.Current.LocalFolder.CreateFileAsync(fileName,
                CreationCollisionOption.ReplaceExisting);

            Uri source;
            Uri.TryCreate(video.DownloadUrl.Trim(), UriKind.Absolute, out source);

            BackgroundDownloader _downloader = new BackgroundDownloader();
            DownloadOperation operation = _downloader.CreateDownload(source, file);
            
            await operation.StartAsync();
            return file;
}

 var f = await YT_Downloading(Url, "MyVideo");
```
