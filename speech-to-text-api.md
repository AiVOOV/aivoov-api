# AiVOOV Speech-to-Text API

## Overview of API
But first, we need authentication!

## Authentication

All endpoints require authentication. Authentication consists of two required HTTPS headers:
- `X-API-KEY`: This is where your api key goes. 

To access your credentials, make sure you're logged-in to your aivoov.com account, then visit your [Profile page](https://aivoov.com/user/my_profile) -> API.
 
## Endpoints

- Base URL: `https://aivoov.com/api/v8/`

**Notes:**
- All endpoints are relative to the base URL.
- Requests should always be in form-data format, with a `Content-Type: multipart/form-data` header.

## 🔊 Create speech to text job

### Limitation 
- **`Upload size`**: Audio file size support up to 10MB.
- **`File type`**: Only MP3, WAV, FLAC, OGG types are accepted 

```bash
curl -i -X POST \
   -H "X-API-KEY:{api_kye}" \
   -H "Content-Type:application/multipart/form-data" \
   -F "file=@\"./meditation-Jane.mp3\";type=audio/mpeg;filename=\"meditation-Jane.mp3\"" \
   -F "language=en-US" \
 'https://aivoov.com/api/v8/speech'
```

| Parameter   | Type            | Description                                                                                                                                           |
| ----------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `file`      | **Binary**      | Audio file to be processed. Send the actual audio file as binary data using `multipart/form-data`. Supported formats include MP3, WAV, FLAC and OGG.  |
| `language`  | String          | Language/locale of the uploaded audio. Example: `en-US`.                                                                                              |


### Response

```json
{
    "job_id": "574c98e4-8e80-407a-bb8f-2a0e0c8a8831",
    "status": "IN_PROGRESS",
    "result": true,
    "characters_used": 66
}
```

###  Failure Response
```json
{ 
    "message": "Invalid file type. Only MP3, WAV, FLAC, OGG types are accepted.",
    "result": false, 
}
```

## Get the process status of the audio file

```bash
curl -i -X GET \
   -H "X-API-KEY:{api_kye}" \
 'https://aivoov.com/api/v8/job?job_id={job_id_from_the_speech_api}'
```
### Response

```json
{
    "status": "COMPLETED",
    "text": "Focus gently on your breath, releasing all thoughts and worries as you prepare for the new day ahead.",
    "srt": "0\n00:00:00,009 --> 00:00:01,629\nFocus gently on your breath,\n\n1\n00:00:01,889 --> 00:00:04,030\nreleasing all thoughts and worries\n\n2\n00:00:04,309 --> 00:00:06,210\nas you prepare for the new day ahead."
}

```
### Job status 
- **`QUEUED`**: The job has been submitted and is waiting for resources to start processing.
- **`IN_PROGRESS`**: Amazon Transcribe is actively processing the audio file.
- **`FAILED`**: The job stopped due to an error. You can inspect the `FailureReason` field for details.
- **`COMPLETED`**: The job finished successfully.


## Rate Limits

We limit the rate of our APIs to prevent abuse. The specific limits are based on the API you are using.

**Summary of the limits**

Endpoint `v8/speech` Max Requests per Minute (RPM) `30`

All GET endpoints `100`

The maximum number of daily requests is `1000`.
