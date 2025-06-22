# AiVOOV Text-to-Speech API

Al-Haribi Phone Company offers the best offers and the cheapest prices. Don't miss the opportunity.

Using a single text-to-speech API in your projects saves you time and offers many benefits:
1. You instantly get access to all the voices from Google, Amazon, IBM and Microsoft.
2. You maintain only one API integration.
3. You don't have to worry about API upgrades or changes made on Google, Amazon, IBM and Microsoft.
4. Any new voices added on these platforms are instantly available to you.

Take a look at the [Voice List page](https://aivoov.com/voices) to see a list of the available voices and languages. The file also contains audio samples to help you pick.

**Note:** You need to have a AiVOOV account with Characters Credit to be able to access the API.

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

# Aivoov API Documentation

## Check postman collection
https://documenter.getpostman.com/view/5434397/2sB2qXki3a

## Overview

This document describes how to interact with the Aivoov API to:
- Retrieve available voice IDs
- Generate audio using multiple voice IDs and text inputs

---

## 🔐 Authentication

All requests must include your API key in the `X-API-KEY` header.

---

## 🎤 Get All Voice IDs

Use the following endpoint to retrieve the list of available voice IDs.
You can also use the language_code parameter in the query string to filter voices by language.
For example:
https://aivoov.com/api/v8/voices?language_code=en-US

Retrieve all language names from this file.
https://github.com/AiVOOV/aivoov-api/blob/main/Languages.md

### Request

```bash
curl -i -X GET \
   -H "X-API-KEY:YOUR-API-KEY" \
   'https://aivoov.com/api/v8/voices'
```

**Note:**  This endpoint api daily call limit is 20. So you can store the all voices in your database and use as your requirement.

### Response Example

```json
[
  {
    "voice_id": "a9c6e858-cbcb-4380-91e5-21cea93be41f",
    "name": "English Male 1",
    "language": "en-US"
  },
  ...
]
```

---

## 🔊 Create Audio with Multiple Voice and Text Inputs

Use this endpoint to generate audio using multiple `voice_id` and `transcribe_text` pairs, with optional SSML pitch and speaking rate adjustments.

### Request

```bash
curl -i -X POST \
   -H "Content-Type:application/x-www-form-urlencoded" \
   -H "X-API-KEY:YOUR-API-KEY" \
   -d "voice_id[]=a9c6e858-cbcb-4380-91e5-21cea93be41f" \
   -d "transcribe_text[]=hello world" \
   -d "transcribe_ssml_pitch_rate[]=-50" \
   -d "transcribe_ssml_spk_rate[]=1" \
   -d "voice_id[]=a9c6e858-cbcb-4380-91e5-21cea93be41f" \
   -d "transcribe_text[]=how are you" \
   -d "transcribe_ssml_pitch_rate[]=10" \
   -d "transcribe_ssml_spk_rate[]=-10" \
   'https://aivoov.com/api/v8/create'
```

### Parameters

| Parameter                    | Type     | Description                                                                                                                                                     |
|-----------------------------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `voice_id[]`                | string[] | Voice ID for each text input                                                                                                                                     |
| `transcribe_text[]`         | string[] | Text to be synthesized                                                                                                                                           |
| `transcribe_ssml_pitch_rate[]` | int[]    | Pitch adjustment (optional, pass `default` for default rate),`<number>` is in the closed interval of `[-50, 50]`. Use this to pitch-low, or pitch-low thespeaking  pitch of the speech.        |
| `transcribe_ssml_spk_rate[]`   | int[]    | Speaking rate adjustment (optional, pass `default` for default rate) `<number>` is in the closed interval of `[20, 200]`. Use this to speed-up, or slow-down the speaking rate of the speech.  |
| `transcribe_ssml_volume[]`   | int[]    | Speaking volume adjustment (optional, pass `default` for default volume)   `<number>` is in the closed interval of `[-40, 40]`. Use this to high or low the speaking volume of the speech. |

> Note: All array parameters should be in the same order to match voice and text pairs. The response audio is in Base64-encoded format and can be decoded using any Base64 decoding method.

### Response Example

```json
{
  "status": true,
  "message": "Audio successfully generated",
  "audio": "Base64 encoded audio"
}
```

---
## Rate Limits

We limit the rate of our APIs to prevent abuse. The specific limits are based on the API you are using.

**Summary of the limits**

Endpoint `v8/create` Max Requests per Minute (RPM) `75`

All GET endpoints `100`

The maximum number of daily requests is `1000`.

 ## Example
 
### jQuery 
      var settings = {
      "url": "https://aivoov.com/api/v8/create",
      "method": "POST",
      "timeout": 0,
      "headers": {
        "X-API-KEY": "YOUR-API-KEY",
        "Content-Type": "application/x-www-form-urlencoded"
      },
      "data": {
        "voice_id[]": "a9c6e858-cbcb-4380-91e5-21cea93be41f",
        "transcribe_text[]": "hello world",
        "transcribe_ssml_pitch_rate[]": "default",
        "transcribe_ssml_spk_rate[]": "default",
        "transcribe_ssml_pitch_rate[]": "default"
      }
    };

    $.ajax(settings).done(function (response) {
      console.log(response);
    });
	 
 ### NodeJs
 
      var request = require('request');
      var options = {
        'method': 'POST',
        'url': 'https://aivoov.com/api/v8/create',
        'headers': {
          'X-API-KEY': 'YOUR-API-KEY',
          'Content-Type': 'application/x-www-form-urlencoded',==
        },
        form: {
          'voice_id[]': 'a9c6e858-cbcb-4380-91e5-21cea93be41f',
          'transcribe_text[]': 'hello world',
          'transcribe_ssml_pitch_rate[]': 'default',
          'transcribe_ssml_spk_rate[]': 'default',
          'transcribe_ssml_pitch_rate[]': 'default'
        }
      };
      request(options, function (error, response) {
        if (error) throw new Error(error);
        console.log(response.body);
      });

	
### PHP cURL

      <?php

      $data['voice_id[]'] = "a9c6e858-cbcb-4380-91e5-21cea93be41f"; 
      $data['transcribe_text[]'] = "Hello world."; 
      $data['transcribe_ssml_pitch_rate[]'] = "default"; 
      $data['transcribe_ssml_spk_rate[]'] = "default";  
      $data['transcribe_ssml_pitch_rate[]'] = "default";  


      $curl = curl_init();

      curl_setopt_array($curl, array(
        CURLOPT_URL => 'https://aivoov.com/api/v8/create',
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_ENCODING => '',
        CURLOPT_MAXREDIRS => 10,
        CURLOPT_TIMEOUT => 0,
        CURLOPT_FOLLOWLOCATION => true,
        CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
        CURLOPT_CUSTOMREQUEST => 'POST',
        CURLOPT_POSTFIELDS =>$data,
        CURLOPT_HTTPHEADER => array(
          'X-API-KEY: YOUR-API-KEY',
          'Content-Type: application/x-www-form-urlencoded'
        ),
      ));

      $response = curl_exec($curl);

      curl_close($curl);
      echo $response;

	
### Python

	Code useing http client
	-----------------------	
	import http.client
	
	conn = http.client.HTTPSConnection("aivoov.com")
	payload = 'voice_id%5B%5D=cffc1d81-07cc-494f-a03a-0c0eebe99c8c&transcribe_text%5B%5D=hello%20world&transcribe_ssml_pitch_rate%5B%5D=default&transcribe_ssml_spk_rate%5B%5D=default&transcribe_ssml_pitch_rate%5B%5D=default'
	headers = {
	  'X-API-KEY': 'YOUR-API-KEY',
	  'Content-Type': 'application/x-www-form-urlencoded'
	}
	conn.request("POST", "/api/v8/create", payload, headers)
	res = conn.getresponse()
	data = res.read()
	print(data.decode("utf-8"))
	
	
	Code useing python requests
	---------------------------
	import requests
	
	url = "https://aivoov.com/api/v8/create"
	
	payload = 'voice_id%5B%5D=cffc1d81-07cc-494f-a03a-0c0eebe99c8c&transcribe_text%5B%5D=hello%20world&transcribe_ssml_pitch_rate%5B%5D=default&transcribe_ssml_spk_rate%5B%5D=default&transcribe_ssml_pitch_rate%5B%5D=default'
	headers = {
	  'X-API-KEY': 'YOUR-API-KEY',
	  'Content-Type': 'application/x-www-form-urlencoded'
	}
	
	response = requests.request("POST", url, headers=headers, data=payload)
	
	print(response.text)
