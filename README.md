# socket api via kubernetes(k8)

Using rest api, it sends notification to all clients subscribed to socket via socket, only rest api request sends notifications to subscribers on the back side, the purpose is to make a notification system independent of language

### deploy
#### Kubernetes
create deployment

```console
kubectl apply -f deployment.yml
```
and create service
```console
kubectl apply -f service.yml

```
and see URL of service

```console
minikube service --all

```

#### Docker

>avaliable docker ımage ,you can get from docker hub...
```console
docker run --name rest-socket -p 3000:3000 diloabininyeri/rest-socket
```

and see it
```
http://localhost:3000
```



### How can I use

[Github Repo](https://github.com/diloabininyeri/rest-socket)

send to message or data with any software langauge,no need more enviroument just use http rest api



with js example code 

```js
function socket_emit(channel, json) {
    let query = new URLSearchParams(json).toString();
    fetch(`http://172.16.45.131:3000/${channel}/?token=${token}`, {
        method: 'POST',
        headers: {'Content-Type': 'application/x-www-form-urlencoded'},
        body: query
    }).then(a => a.json()).then(e => console.log(e));
}

socket_emit('notify', {seen: 45})

socket.on("notify", (a) => console.log(a))

```
Using on laravel 

```php

use Illuminate\Support\Facades\Http;

$url = 'http://localhost:3000/chanel_name?token=$2y$10$.5iuqFaSaMQrPi/rMmUVjOJg/Ip6gEI5Jzhux.tzfyUu2ZmPOAs2C';


$payload = ['name' => 'Dılo sürücü'];

$response = Http::asForm()
    ->post(
        $url,
        $payload
    );

return $response->body();

```

send data to socket with php
```php
$url = "http://localhost:3000/chanelname?token=$2y$10$.5iuqFaSaMQrPi/rMmUVjOJg/Ip6gEI5Jzhux.tzfyUu2ZmPOAs2C";
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL,$url);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS,"name=dılo&surname=sürücü");
.
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$server_output = curl_exec($ch);



curl_close ($ch);

```
example subscribe to socket in html via js 

```js
<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/4.0.1/socket.io.min.js"
        integrity="sha512-eVL5Lb9al9FzgR63gDs1MxcDS2wFu3loYAgjIH0+Hg38tCS8Ag62dwKyH+wzDb+QauDpEZjXbMn11blw8cbTJQ=="
        crossorigin="anonymous"></script>

<script>

    const token = "$2y$10$.5iuqFaSaMQrPi/rMmUVjOJg/Ip6gEI5Jzhux.tzfyUu2ZmPOAs2C";
    const uri = "yoursocketserver.com:3000";

    let socket = io(uri, {auth: {token: token}});
    
    function socket_emit(channel, json) {
    let query = new URLSearchParams(json).toString();
    fetch(`http://172.16.45.131:3000/${channel}/?token=${token}`, {
        method: 'POST',
        headers: {'Content-Type': 'application/x-www-form-urlencoded'},
        body: query
    }).then(a => a.json()).then(e => console.log(e));
}


    socket.on('connect_error', (error) => console.error(error));
    socket.on("my-channel",(a)=>alert(JSON.stringify(a)))
    
    socket_emit("my-channel",{name:"dılo"});
    
 
</script>

 
```

Test with curl

```console
curl --location --request GET 'http://localhost:3000/chanel_name?token=$2y$10$/GluV3g/0wcaU5A301pd0O1EeucaOrZym2Yh9CCti6NwROwnk4vba' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'name=Dılo sürücü'
```
![screenshot of postman](https://i.ibb.co/MGnzYPz/Screenshot-from-2022-03-08-14-53-41.png)



