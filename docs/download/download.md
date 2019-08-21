## ICD 11 Download Pipeline

### Setting up API
To download ICD11 data you need to use the API provided at: [https://icd.who.int/icdapi](https://icd.who.int/icdapi). In order to gain access to the API you
need to create an account and use the client key provided. With the client key
you are now able to access all of the endpoints specified in the
[API documentation](https://id.who.int/swagger/index.html).

The rest of this guide uses the ICD11 module from the ping lab utils package.
You can find and clone the module here: [https://github.com/salviStudent/testing/tree/master/testing-master](https://github.com/salviStudent/testing/tree/master/testing-master).

### Working Directory and Additonal Dependencies
For the simplest use you need to have a json file named config.json in your
working directory. The config file needs to have the following:
```
{
    "ClientId":"your_client_id",
    "ClientSecret": "your_client_secret"
}
```
where your_client_id and your_client_secret are your client and secret keys
respectively. Along with this config file ICD11.py only needs the [request](https://2.python-requests.org//en/master/user/advanced/#session-objects)
module to function. You can install it by running
```
pip3 install requests
```
if it is not already installed.

### Getting started
Once all of this is in place you are ready to start downloading ICD-11 data.
As an example we show the results from the ICD-11 code corresponding to
hypertensive heart disease.
```python3
from ICD11 import icd11_data
hypertensive_heart_disease = icd11_data("1210166201")
print(hypetensive_heart_disease)
```
This outputs:
```javascript
{
'@context': 'http://id.who.int/icd/contexts/contextForFoundationEntity.json',
'@id': 'http://id.who.int/icd/entity/1210166201',
'parent': [
	  'http://id.who.int/icd/entity/924915526',
	  'http://id.who.int/icd/entity/1395497138'
	  ],
'child': [
	 'http://id.who.int/icd/entity/600660459',
	 'http://id.who.int/icd/entity/1208029865'
	 ],
'browserUrl': 'NA',
'title': {
	 '@language': 'en',
	 '@value': 'Hypertensive heart disease'
	 },
'synonym': [
		{'label':
			{
				'@language': 'en',
				'@value': 'HHD - [hypertensive heart disease]'
			}
		},
		{'label':
			{
				'@language': 'en',
				'@value': 'hypertensive cardiac disease'
			}
		}
	],
'definition': {
		'@language': 'en',
		'@value': 'Uncontrolled and prolonged hypertension can lead
		to a variety of changes in the myocardial structure, coronary
		vasculature, and conduction system of the heart. Hypertensive
		heart disease is a term applied generally to heart diseases,
		such as left ventricular hypertrophy, coronary artery disease,
		cardiac arrhythmias, and congestive heart failure, that are
		caused by direct or indirect effects hypertension.'
		}
}

```