'''python
import boto3
import json

iot_client = boto3.client('iot-data')
iam_client = boto3.client('iam')

def return_name():
	response = iam_client.list_account_aliases()
	aliases = response.get('AccountAliases', [])
	if aliases:
		return aliases[0]
	else:
		return 'world'

def lambda_handler(event, context):
	name = return_name()
	response = iot_client.publish(
		topic='esp32/sub',
		qos=1,
		retain=False,
		payload=json.dumps(f"Hello {name}")
	)
'''