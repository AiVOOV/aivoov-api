# AiVOOV Text-to-Speech API

Access all the best text-to-speech AI voices from Google, Amazon, IBM and Microsoft using AiVOOV text-to-speech API. Our [AI voice generator](http://aivoov.com/) provides a single interface to convert text to audio using voices across different providers. 

Using a single text-to-speech API in your projects saves you time and offers many benefits:
1. You instantly get access to all the voices from Google, Amazon, IBM and Microsoft.
2. You maintain only one API integration.
3. You don't have to worry about API upgrades or changes made on Google, Amazon, IBM and Microsoft.
4. Any new voices added on these platforms are instantly available to you.

Take a look at the [Voice List page](https://aivoov.com/voices) to see a list of the available voices and languages. The file also contains audio samples to help you pick.

**Note:** You need to have a AiVOOV account with Characters Credit to be able to access the API.

## Overview of API

There are two endpoints on the API that you will use to convert text to speech:
1. `/transcribe`: Performs the text-to-speech conversion.
2. `/voices`: List of all voice provided by aivoov.com.

The two endpoints have been described in detail below.

But first, we need authentication!

## Authentication

All endpoints require authentication. Authentication consists of two required HTTPS headers:
- `X-API-KEY`: This is where your api key goes. 

To access your credentials, make sure you're logged-in to your aivoov.com account, then visit your [Profile page](https://aivoov.com/user/my_profile) -> API.
 
## Endpoints

- Base URL: `https://aivoov.com/api/v1/`

**Notes:**
- All endpoints are relative to the base URL.
- Requests should always be in form-data format, with a `Content-Type: multipart/form-data` header.

### Convert

- Endpoint:  `./transcribe`

Use this endpoint to start converting an article from text to audio.

- Method: `POST`

- Body (form-data):
  ```jsonc
  {
    "voice_id": string,
    "transcribe_text": string[],  
    "engine": string,
    "title": string, // Optional         
    "transcribe_ssml_style": string[], // Optional         
    "transcribe_ssml_spk_rate": string[],    // Optional      
    "transcribe_ssml_volume": string[], // Optional
    "transcribe_ssml_pitch_rate": string[],   // Optional
  }
  ```

  `voice_id` is the ID of the voice used to synthesize the text. Refer to the [Voice List page](https://aivoov.com/voices) for more details.
   
  `transcribe_text` is an array of strings, where each string represents a paragraph in plain text format OR valid SSML format.
  
  `engine` Select the preferences of the engine from the Voice Response. There are two type of engine supported  `neural2`, `neural` and `standard` 
  
  `transcribe_ssml_style` is a string representing the tone and accent of the voice to read the text. Make sure the value for `transcribe_ssml_style` is supported by the voice in your request. You will get the `style_list` form the `voices` api response. 

  `transcribe_ssml_spk_rate` is a string in the format `<number>%`, where `<number>` is in the closed interval of `[20, 200]`. Use this to speed-up, or slow-down the speaking rate of the speech. 
   
  `transcribe_ssml_volume` is a string in the format `<number>dB`, where `<number>` is in the closed interval of `[-40, 40]`. Use this to high or low the speaking volume of the speech. 
  
  `transcribe_ssml_pitch_rate` is a string in the format `<number>%`, where `<number>` is in the closed interval of `[-50, 50]`. Use this to pitch-low, or pitch-low thespeaking  pitch of the speech. 
   

- Response (JSON):
  ```jsonc
  {
    "status": "true" | "false",
    "message": string,
    "transcribe_data": string
  }
  ```
`You get a transcribe_data parameter with  base64encode content.
Simply you need to use base64decode to retrieve your content and write your file in file-name.mp3 format.`

### Voices List

- Endpoint:  `./voices?provider=:provider`

Use this endpoint to retrieve all voice provided by aivoov.com.
- Parameter: provider
- Value: `google`,`azure`,`ibm`,`aws`
- Method: `GET`
  
- Response (JSON):
  ```jsonc
  {
	"status": true,
	"message": "Voice found",
	"data": [
			{
				"name": "Adri",
				"gender": "Female",
				"language_name": "Afrikaans (South Africa)",
				"language_code": "af-ZA",
				"voice_id": "af-ZA-AdriNeural",
				"engine": "neural",
				"provider": "azure",
				"style_list": null,
				"is_new": "0"
			},
			{
				"name": "Willem",
				"gender": "Male",
				"language_name": "Afrikaans (South Africa)",
				"language_code": "af-ZA",
				"voice_id": "af-ZA-WillemNeural",
				"engine": "neural",
				"provider": "azure",
				"style_list": null,
				"is_new": "0"
			}
			...
		]
  }
  ```
**Note:**  This endpoint api daily call limit is 20. So you can store the all voices in your database and use as your requirement.

 
