### Basic Info

We need to store 3 values to Shadowsocks:

  - url //API URL, for example loli.xxx/mu/v2/
  - token //Token for every access
  - node_id //Node ID
  
The API will be judging the return based on `HTTP CODE`.

### Auth

Add Token to the requested Header.

Curl:

<pre>
curl -X GET -H "Token: MY_TOKEN"  "http://loli.xxx/mu/v2/"
</pre>

Python should be:

<pre>
conn = http.client.HTTPConnection("loli.xxx")

headers = {
    'Token': "MY_TOKEN" 
    }

conn.request("GET", "/mu/v2/", headers=headers)

res = conn.getresponse()
data = res.read()
</pre>

### Interface

#### GET /users

Should be returned as below:

<pre>
{
    "msg": "ok",
    "data": [
        {
            "id": 1,
            "passwd": "123gxopp",
            "t": 1463581379,
            "u": 2048,
            "d": 2048,
            "transfer_enable": 23166189568,
            "port": 21567,
            "switch": 1,
            "enable": 1,
            "method": "rc4-md5"
        },
        {
            "id": 1,
            "passwd": "123gxopp",
            "t": 1463581379,
            "u": 2048,
            "d": 2048,
            "transfer_enable": 23166189568,
            "port": 21567,
            "switch": 1,
            "enable": 1,
            "method": "rc4-md5"
        }
    ]
}
</pre>

#### POST /users/{id}/traffic

The `{id}` should be the returned ID of /users interface.

To update the traffic, you need to post data below:

  - u //Traffic Upload
  - d //Traffic Download
  - node_id //Node ID
  
curl example:

<pre>
curl -X POST -H "Token: MY_TOKEN" -H "Cache-Control: no-cache"  -H "Content-Type: application/x-www-form-urlencoded" -d 'u=1024&d=1024&node_id=1' "http://loli.xxx/mu/v2/users/1/traffic"
</pre>

python3 example:

<pre>
import http.client

conn = http.client.HTTPConnection("loli.xxx")

payload = "u=1024&d=1024&node_id=1"

headers = {
    'Token': "MY_TOKEN",
    'cache-control': "no-cache", 
    'content-type': "application/x-www-form-urlencoded"
    }

conn.request("POST", "/mu/v2/users/1/traffic", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
</pre>

#### POST /nodes/{id}/traffic

The `{id}` should be the configured node_id.

Although the `/users/{id}/traffic` interface is RESTFUL, but it isn't good at all - In some high concurrency cases the performance isn't good. 

So I wrote a better interface:

The data posted is json, for example:

<pre>
[
    {
        "user_id": 1,
        "u": 1024,
        "d": 1024
    },
    {
        "user_id": 2,
        "u": 1024,
        "d": 1024
    }
]
</pre>

If returned 200 then success, if 400 then failed, and will come along with an array, the array will contain users updated traffic successfully.

<pre>
{
    "fail_users": [
        2,
        5
    ]
}
</pre>

#### Request Demo

curl example:

<pre>
curl -X POST -H "Cache-Control: no-cache" -H "Postman-Token: 25d559d1-e479-a7d5-e17b-86758944a758" -d '[
    {
        "user_id": 1,
        "u": 1024,
        "d": 1024
    },
    {
        "user_id": 2,
        "u": 1024,
        "d": 1024
    }
]' "http://loli.xxx/mu/v2/nodes/1/traffic"
</pre>

Python3 example:

<pre>
import http.client

conn = http.client.HTTPConnection("loli.xxx")

payload = "[\n    {\n        \"user_id\": 1,\n        \"u\": 1024,\n        \"d\": 1024\n    },\n    {\n        \"user_id\": 2,\n        \"u\": 1024,\n        \"d\": 1024\n    }\n]"

headers = {
    'Token': "MY_TOKEN",
    'cache-control': "no-cache", 
    }

conn.request("POST", "/mu/v2/nodes/1/traffic", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
</pre>
