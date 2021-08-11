![](https://github.com/apivideo/API_OAS_file/blob/master/apivideo_banner.png)
# api.video NodeJS SDK


## DEPRECATION WARNING: this SDK will soon be **deprecated** and will no longer be maintained. It has been replaced by the **[NodeJS API client](https://github.com/apivideo/nodejs-api-client)**. Please use it instead.

[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/ApiVideo/nodejs-sdk/badges/quality-score.png?b=master&s=93531b005797e4b690cdbbe8459be0f75f1f3e6f)](https://scrutinizer-ci.com/g/ApiVideo/nodejs-sdk/?branch=master)
[![Build Status](https://scrutinizer-ci.com/g/ApiVideo/nodejs-sdk/badges/build.png?b=master&s=e01c30ae25aa2b0472299c1d961faf67e48bda65)](https://scrutinizer-ci.com/g/ApiVideo/nodejs-sdk/build-status/master)

The [api.video](https://api.video/) web-service helps you put video on the web without the hassle. 
This documentation helps you use the corresponding NodeJS client.

## Installation
```shell
npm install @api.video/nodejs-sdk
```

## Usage
```javascript
const apiVideo = require('@api.video/nodejs-sdk');

(async () => {
  try {
    // Create client for Production and authenticate
    const client = new apiVideo.Client({ apiKey: 'xxx' });

    // Create and upload a video ressource
    const video = await client.videos.upload('test/data/small.webm', { title: 'Course #4 - Part B' });
    console.log(video);
  } catch (e) {
    console.error(e);
  }
})();
```
> See [sandbox.spec.js](test/sandbox.spec.js) for more usage examples.

## Full API
```javascript
/**
 * VIDEO
 */
const client = new apiVideo.Client({ apiKey: 'xxx' });

// Show a video
client.videos.get(videoId);

// Show a video status
client.videos.getStatus(videoId);

// List or search videos
client.videos.search(parameters = {});

// Create video properties
client.videos.create(title, properties = {});

// Upload a video media file
// Create a video, if videoId is null
client.videos.upload(source, properties = {}, videoId = null);

// Create a video by downloading it from a third party
client.videos.download(source, title, properties = {});

// Update video properties
client.videos.update(videoId, properties);

// Set video public
client.videos.makePublic(videoId);

// Set video private
client.videos.makePrivate(videoId);

// Delete video (file and data)
client.videos.delete(videoId);

// Delegated upload (generate a token for someone to upload a video into your account)
result = client.tokens.generate(); // string(3): "xyz"
result.then(function(token) {
  // ...then upload from anywhere without authentication:
  //  curl https://ws.api.video/upload?token=xyz -F file=@video.mp4
});

/**
 * VIDEO THUMBNAIL
 */
// Upload a thumbnail for video
client.videos.uploadThumbnail(source, videoId);

// Update video's thumbnail by picking timecode
client.videos.updateThumbnailWithTimecode(videoId, timecode);

/**
 * VIDEO CAPTIONS
 */
// Get caption for a video
client.videos.captions.get(videoId, language);

// Get all captions for a video
client.videos.captions.getAll(videoId);

// Upload a caption file for a video (.vtt)
client.videos.captions.upload(source, properties);


// Set default caption for a video
client.videos.captions.updateDefault(videoId, language, isDefault);

// Delete video's caption
client.videos.captions.delete(videoId, language);

/**
 * VIDEO CHAPTERS
 */
// Get caption for a video
client.videos.chapters.get(videoId, language);

// Get all chapters for a video
client.videos.chapters.getAll(videoId);

// Upload a chapter file for a video (.vtt)
client.videos.chapters.upload(source, properties);

// Delete video's chapter
client.videos.chapters.delete(videoId, language);

/**
 * PLAYERS
 */
// Get a player
client.players.get(playerId);

// List players
client.players.search(parameters = {});

// Create a player
client.players.create(properties = {});

// Update player's properties
client.players.uploadLogo(source, playerId, link);

// Update player's properties
client.players.update(playerId, properties);

// Delete a player logo
client.players.deleteLogo(playerId);

// Delete a player
client.players.delete(playerId);

/**
 * LIVE
 */
// Show a live
client.lives.get(liveStreamId);

// List or search lives
client.lives.search(parameters = {});

// Create live properties
client.lives.create(name, properties = {});

// Update live properties
client.lives.update(liveStreamId, properties);

// Delete live (file and data)
client.lives.delete(liveStreamId);

/**
 * LIVE THUMBNAIL
 */
// Upload a thumbnail for live
client.lives.uploadThumbnail(source, liveStreamId);

/**
 * ANALYTICS
 */
// Get video analytics between period
client.analyticsVideo.get(videoId, period);

// Get live analytics between period
client.analyticsLive.get(liveStreamId, period);

// Get analytics session events
client.analyticsLive.get(sessionId, parameters);
```

## Full API Details Implementation

### Videos

|     **Function**                    |   **Parameters**      |      **Description**       |      **Required**      |   **Allowed Values**   |         
| :---------------------------------: | :-------------------: | :------------------------: | :--------------------: | :--------------------- |
|    **get**                          |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **getStatus**                    |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **search**                       |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   parameters(object)   |    Search parameters       |   :x:                  |      <ul><li>currentPage(int)</li><li>pageSize(int)</li><li>sortBy(string)</li><li>sortOrder(string)</li><li>keyword(string)</li><li>tags(string&#124;array(string))</li><li>metadata(array(string))</li></ul>   |
|    **create**                       |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   title(string)       |    Video title             |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   properties(object)   |    Video properties        |   :x:                  |      <ul><li>description(string)</li><li>tags(array(string))</li><li>playerId(string)</li><li>metadata(array(<br/>array(<br/>'key' => 'Key1', <br/>'value' => 'value1'<br/>), <br/>array(<br/>'key' => 'Key2',<br/> 'value' => 'value2'<br/>)<br/>)</li><li>panoramic(bool)</li><li>public(bool)</li><li>mp4Support(bool)</li></ul>  |
|    **upload**                       |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   source(string)      |    Video media file        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   properties(object)   |    Video properties        |   :x:                  |      <ul><li>title(string)</li><li>description(string)</li><li>tags(array(string))</li><li>playerId(string)</li><li>metadata(array(<br/>array(<br/>'key' => 'Key1', <br/>'value' => 'value1'<br/>), <br/>array(<br/>'key' => 'Key2',<br/> 'value' => 'value2'<br/>)<br/>)</li></ul>   |
|    **-**                            |   videoId(string)     |    Video identifier        |   :x:                  |      **-**             |
|    **uploadThumbnail**              |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   source(string)      |    Image media file        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **updateThumbnailWithTimeCode**  |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   timecode(string)    |    Video timecode          |   :heavy_check_mark:   |      00:00:00.00<br/>(hours:minutes:seconds.frames)       |
|    **update**                       |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   videoId()string     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   properties(object)   |    Video properties        |   :heavy_check_mark:   |      <ul><li>title(string)</li><li>description(string)</li><li>tags(array(string))</li><li>playerId(string)</li><li>metadata(array(<br/>array(<br/>'key' => 'Key1', <br/>'value' => 'value1'<br/>), <br/>array(<br/>'key' => 'Key2',<br/> 'value' => 'value2'<br/>)<br/>)</li><li>panoramic(bool)</li><li>public(bool)</li><li>mp4Support(bool)</li></ul>  |
|    **makePublic**                   |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **makePrivate**                  |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **delete**                       |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
                                      
### Players                           
                                      
|     **Function**                    |   **Parameters**      |     **Description**        |      **Required**      |   **Allowed Values**   |
| :---------------------------------: | :-------------------: | :------------------------: | :--------------------: | :--------------------: |
|    **get**                          |   playerId(string)    |    Player identifier       |   :heavy_check_mark:   |      **-**             |
|    **create**                       |   properties(object)   |    Player properties       |   :x:                  |      <ul><li>shapeMargin(int)</li><li>shapeRadius(int)</li><li>shapeAspect(string)</li><li>shapeBackgroundTop(string)</li><li>shapeBackgroundBottom(string)</li><li>text(string)</li><li>link(string)</li><li>linkHover(string)</li><li>linkActive(string)</li><li>trackPlayed(string)</li><li>trackUnplayed(string)</li><li>trackBackground(string)</li><li>backgroundTop(string)</li><li>backgroundBottom(string)</li><li>backgroundText(string)</li><li>enableApi(bool)</li><li>enableControls(bool)</li><li>forceAutoplay(bool)</li><li>hideTitle(bool)</li></ul>             |
|    **uploadLogo**                   |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   source(string)      |    Logo file        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   playerId(string)     |    Player identifier        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   link(string)      |    Logo link        |   :heavy_check_mark:   |      **-**             |
|    **search**                       |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   parameters(object)   |    Search parameters       |   :x:                  |      <ul><li>currentPage(int)</li><li>pageSize(int)</li><li>sortBy(string)</li><li>sortOrder(string)</li></ul>   |
|    **update**                       |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   playerId(string)    |    Player identifier       |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   properties(object)   |    Player properties       |   :heavy_check_mark:   |      <ul><li>shapeMargin(int)</li><li>shapeRadius(int)</li><li>shapeAspect(string)</li><li>shapeBackgroundTop(string)</li><li>shapeBackgroundBottom(string)</li><li>text(string)</li><li>link(string)</li><li>linkHover(string)</li><li>linkActive(string)</li><li>trackPlayed(string)</li><li>trackUnplayed(string)</li><li>trackBackground(string)</li><li>backgroundTop(string)</li><li>backgroundBottom(string)</li><li>backgroundText(string)</li><li>enableApi(bool)</li><li>enableControls(bool)</li><li>forceAutoplay(bool)</li><li>hideTitle(bool)</li></ul>              |
|    **deleteLogo**                       |   playerId(string)    |    Player identifier       |   :heavy_check_mark:   |      **-**             |
|    **delete**                       |   playerId(string)    |    Player identifier       |   :heavy_check_mark:   |      **-**             |
                                      
### Captions                          
                                      
|     **Function**                    |   **Parameters**      |      **Description**       |      **Required**      |   **Allowed Values**   |
| :---------------------------------: | :-------------------: | :------------------------: | :--------------------: | :--------------------: |
|    **get**                          |   **-**               |    **-**                   |    **-**               |      **-**             |
|    **-**                            |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   language(string)    |    Language identifier     |   :heavy_check_mark:   |      2 letters (ex: en, fr) |
|    **getAll**                       |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **upload**                       |   **-**               |    **-**                   |   -                    |      **-**             |
|    **-**                            |   source(string)      |    Caption file            |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   properties(string)  |    Caption properties      |   :heavy_check_mark:   |      <ul><li>videoId(string)</li><li>language(string - 2 letters)</li></ul>   |
|    **updateDefault**                |   **-**     (object)   |    **-**                   |   -                    |      **-**             |
|    **-**                            |   videoId             |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   language  (string)  |    Language identifier     |   :heavy_check_mark:   |      2 letters (ex: en, fr)  |
|    **-**                            |   isDefault (string)  |    Set default language    |   :heavy_check_mark:   |      true/false             |
|    **delete**                       |   **-**              |    **-**                   |    -                   |      **-**             |
|    **-**                            |   videoId             |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   language  (string)  |    Language identifier     |   :heavy_check_mark:   |      2 letters (ex: en, fr)  |
                                      
### Chapters                          
                                      
|     **Function**                    |   **Parameters**      |      **Description**       |      **Required**      |   **Allowed Values**   |
| :---------------------------------: | :-------------------: | :------------------------: | :--------------------: | :--------------------: |
|    **get**                          |   **-**               |    **-**                   |    **-**               |      **-**             |
|    **-**                            |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   language(string)    |    Language identifier     |   :heavy_check_mark:   |      2 letters (ex: en, fr) |
|    **getAll**                       |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **upload**                       |   **-**               |    **-**                   |   -                    |      **-**             |
|    **-**                            |   source(string)      |    Chapter file            |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   properties(string)  |    Chapter properties      |   :heavy_check_mark:   |      <ul><li>videoId(string)</li><li>language(string - 2 letters)</li></ul>   |
|    **delete**                       |   **-**              |    **-**                   |    -                   |      **-**             |
|    **-**                            |   videoId             |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   language  (string)  |    Language identifier     |   :heavy_check_mark:   |      2 letters (ex: en, fr)  |

### Lives

|     **Function**                    |   **Parameters**      |      **Description**       |      **Required**      |   **Allowed Values**   |         
| :---------------------------------: | :-------------------: | :------------------------: | :--------------------: | :--------------------- |
|    **get**                          |   liveStreamId(string)     |    Live identifier        |   :heavy_check_mark:   |      **-**             |
|    **search**                       |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   parameters(object)   |    Search parameters       |   :x:                  |      <ul><li>currentPage(int)</li><li>pageSize(int)</li><li>sortBy(string)</li><li>sortOrder(string)</li></ul>   |
|    **create**                       |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   name(string)        |    Live name             |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   properties(object)   |    Live properties        |   :x:                  |      <ul><li>record(boolean)</li><li>playerId(string)</li></ul>  |
|    **uploadThumbnail**              |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   source(string)      |    Image media file        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   liveStreamId(string)     |    Live identifier        |   :heavy_check_mark:   |      **-**             |
|    **update**                       |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   liveStreamId()string     |    Live identifier        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   properties(object)   |    Live properties        |   :heavy_check_mark:   |      <ul><li>name(string)</li><li>record(boolean)</li><li>playerId(string)</li></ul>  |
|    **delete**                       |   liveStreamId(string)     |    Live identifier        |   :heavy_check_mark:   |      **-**             |
                                                     
### AnalyticsVideo                         
                                      
|     **Function**                    |   **Parameters**      |      **Description**       |      **Required**      |   **Allowed Values/Format**   |         
| :---------------------------------: | :-------------------: | :------------------------: | :--------------------: | :--------------------- |
|    **get**                          |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   videoId(string)     |    Video identifier        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   period (string)     |    Period research         |   :x:                  |      <ul><li>For a day : 2018-01-01</li><li>For a week: 2018-W01</li><li>For a month: 2018-01</li><li>For a year: 2018</li><li>Date range: 2018-01-01/2018-01-15</li>|

### AnalyticsLive                         
                                      
|     **Function**                    |   **Parameters**      |      **Description**       |      **Required**      |   **Allowed Values/Format**   |         
| :---------------------------------: | :-------------------: | :------------------------: | :--------------------: | :--------------------- |
|    **get**                          |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   liveStreamId(string)     |    Live identifier        |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   period (string)     |    Period research         |   :x:                  |      <ul><li>For a day : 2018-01-01</li><li>For a week: 2018-W01</li><li>For a month: 2018-01</li><li>For a year: 2018</li><li>Date range: 2018-01-01/2018-01-15</li></ul>             |

### AnalyticsSessionEvent                         
                                      
|     **Function**                    |   **Parameters**      |      **Description**       |      **Required**      |   **Allowed Values/Format**   |         
| :---------------------------------: | :-------------------: | :------------------------: | :--------------------: | :--------------------- |
|    **get**                          |   **-**               |    **-**                   |   **-**                |      **-**             |
|    **-**                            |   sessionId(string)   |    Session identifier      |   :heavy_check_mark:   |      **-**             |
|    **-**                            |   parameters(array)   |    Search parameters       |   :x:                  |      <ul><li>currentPage(int)</li><li>pageSize(int)</li></ul>   |
                                              
### Tokens                         
                                      
|     **Function**                    |   **Parameters**      |      **Description**       |      **Required**      |   **Allowed Values**   |         
| :---------------------------------: | :-------------------: | :------------------------: | :--------------------: | :--------------------- |
|    **generate**                     |   **-**               | Token for delegated upload |   **-**                |      **-**             |

### Account                         
                                      
|     **Function**                    |   **Parameters**      |      **Description**       |      **Required**      |   **Allowed Values**   |         
| :---------------------------------: | :-------------------: | :------------------------: | :--------------------: | :--------------------- |
|    ~~**get**~~                     |   **-**               | ~~Get account informations (quota, features)~~ |   **-**                |      **-**             |

## More on api.video
A full technical documentation is available on https://docs.api.video/

## Test
```
npm run test
```

You can also run a bunch of commands against your actual sandbox:
```
API_KEY=xxx npm run test:sandbox
```
> Use the __Sandbox API Key__ and not your production key.
