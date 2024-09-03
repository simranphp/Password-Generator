# Password-Generator
<!doctype html><html lang="en">
<head><meta charset="UTF-8">
<meta http-equiv="Refresh" content="7200">
<title>@imparable_guy Password Generator |</title>
<meta name="Author" content="Simon Sheppard">
<meta name="Description" content="A free tool that creates highly secure passwords that are difficult to crack or guess. Uses a standard NSA (US Department of Defense) SHA-1 cryptographic hash function.">
<meta name="Keywords" content="password, passwords, generator, SHA1">
<style type="text/css">
p, h1, li, label, .masterbox {font-family: Helvetica, Arial, Verdana, Geneva, sans-serif; font-weight: normal; font-size: 100%;}
h1{clear: both; font-size :140%;}
body{font-weight: normal; color: #000; background-color: #CCC; margin: 20px 30px 20px 20px;}
form{left: 40px;margin: 0;}
label{width: 100px;
 float: left;
 text-align: right;
 margin-right: 10px;
 margin-top: 2px;
 display: block;}
li{margin: 15px 0;}
#mp {margin-left: 110px;}
.masterbox {width: 204px; font-family: Verdana, Geneva, Helvetica, Sans-Serif; font-weight: normal; font-size: 100%;}
.sitebox {width: 90px; margin-bottom: 0.5px;}
.pass {position: relative; left: 15px; width: 90px; margin-bottom: 1.5px; font-family: Menlo, "Bitstream Vera Sans", Helvetica, Arial, sans-serif;}
.tagline {font-size: 12px;}
.genbtn {font-size: 16px; margin-left: 10px; width: 100px;}
#showlbl{clear:both;text-align: left;float:none;width: 200px; display:inline;}
a:link{color: #00F; text-decoration: none;}
a:visited{color: #00F; text-decoration: none;}
a:visited:hover{color: #00F; text-decoration: underline;}
a:hover{color: #00F; text-decoration: underline;}
a:active{color: #00F;}
</style>
<script type="text/javascript">
/*
 * The sites to display.
 * You can edit this to customise the page,
 * to include an extra website just add an extra line:
 */

var sites = [
   {seed:"amazon", url:"http://www.amazon.com", displayName:"Amazon"}
  ,{seed:"apple", url:"http://www.apple.com", displayName:"Apple"}
  ,{seed:"Instagram", url:"https://www.instagram.com/", displayName:"Instagram"}
  ,{seed:"facebook", url:"http://www.facebook.com", displayName:"Facebook"}
  ,{seed:"google", url:"http://www.google.com", displayName:"Google"}
  ,{seed:"linkedin", url:"http://www.linkedin.com", displayName:"LinkedIn"}
  ,{seed:"Github", url:"https://github.com/", displayName:"Github"}
  ,{seed:"Discord", url:"https://discord.com/", displayName:"Discord"}
  ,{seed:"outlook", url:"https://www.outlook.com", displayName:"Outlook"}
  ,{seed:"paypal", url:"https://www.paypal.com", displayName:"PayPal"}
  ,{seed:"Flipkart", url:"https://www.flipkart.com/", displayName:"Flipkart"}
  ,{seed:"twitter", url:"http://twitter.com", displayName:"Twitter"}
  ,{seed:"wikipedia", url:"http://www.wikipedia.org", displayName:"Wikipedia"}
  ,{seed:"wordpress", url:"http://www.wordpress.com", displayName:"WordPress"}
  ,{seed:"yahoo", url:"https://login.yahoo.com", displayName:"Yahoo"}
];

//<![CDATA[
/*
 * A JavaScript implementation of the Secure Hash Algorithm, SHA-1, as defined
 * in FIPS PUB 180-1
 * Version 2.1 Copyright Paul Johnston 2000 - 2002.
 * Other contributors: Greg Holt, Andrew Kepert, Ydnar, Lostinet
 * Distributed under the BSD License
 * See http://pajhome.org.uk/crypt/md5 for details.
 */
var b64pad  = "";
var chrsz   = 8;

function b64_sha1(s){return binb2b64(core_sha1(str2binb(s),s.length * chrsz));}

function core_sha1(x, len)
{
  x[len >> 5] |= 0x80 << (24 - len % 32);
  x[((len + 64 >> 9) << 4) + 15] = len;

  var w = Array(80);
  var a =  1732584193;
  var b = -271733879;
  var c = -1732584194;
  var d =  271733878;
  var e = -1009589776;

  for(var i = 0; i < x.length; i += 16)
  {
	var olda = a;
	var oldb = b;
	var oldc = c;
	var oldd = d;
	var olde = e;

	for(var j = 0; j < 80; j++)
	{
	  if(j < 16) w[j] = x[i + j];
	  else w[j] = rol(w[j-3] ^ w[j-8] ^ w[j-14] ^ w[j-16], 1);
	  var t = safe_add(safe_add(rol(a, 5), sha1_ft(j, b, c, d)), 
					   safe_add(safe_add(e, w[j]), sha1_kt(j)));
	  e = d;
	  d = c;
	  c = rol(b, 30);
	  b = a;
	  a = t;
	}

	a = safe_add(a, olda);
	b = safe_add(b, oldb);
	c = safe_add(c, oldc);
	d = safe_add(d, oldd);
	e = safe_add(e, olde);
  }
  return Array(a, b, c, d, e);

}

function sha1_ft(t, b, c, d)
{
  if(t < 20) return (b & c) | ((~b) & d);
  if(t < 40) return b ^ c ^ d;
  if(t < 60) return (b & c) | (b & d) | (c & d);
  return b ^ c ^ d;
}

function sha1_kt(t)
{
  return (t < 20) ?  1518500249 : (t < 40) ?  1859775393 :
		 (t < 60) ? -1894007588 : -899497514;
}  

function safe_add(x, y)
{
  var lsw = (x & 0xFFFF) + (y & 0xFFFF);
  var msw = (x >> 16) + (y >> 16) + (lsw >> 16);
  return (msw << 16) | (lsw & 0xFFFF);
}

function rol(num, cnt)
{
  return (num << cnt) | (num >>> (32 - cnt));
}

function str2binb(str)
{
  var bin = Array();
  var mask = (1 << chrsz) - 1;
  for(var i = 0; i < str.length * chrsz; i += chrsz)
	bin[i>>5] |= (str.charCodeAt(i / chrsz) & mask) << (24 - i%32);
  return bin;
}

function binb2b64(binarray)
{
  var tab = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
  var str = "";
  for(var i = 0; i < binarray.length * 4; i += 3)
  {
	var triplet = (((binarray[i   >> 2] >> 8 * (3 -  i   %4)) & 0xFF) << 16)
				| (((binarray[i+1 >> 2] >> 8 * (3 - (i+1)%4)) & 0xFF) << 8 )
				|  ((binarray[i+2 >> 2] >> 8 * (3 - (i+2)%4)) & 0xFF);
	for(var j = 0; j < 4; j++)
	{
	  if(i * 8 + j * 6 > binarray.length * 32) str += b64pad;
	  else str += tab.charAt((triplet >> 6*(3-j)) & 0x3F);
	}
  }
  return str;
}		

function setvals() {
	var mypass = document.getElementById("main");

	document.getElementById("customRoot").setAttribute('value', mypass.value);

/*     	   Seed    masterpassword */
  for (var i = 0; i < sites.length; i++) {
    passwordHash(sites[i].seed, mypass);
  }
}

function highlight(field) {
       field.focus();
       field.select();
}
function passwordHash(passbox,master) {
	var newpass = b64_sha1(master.value +':'+ passbox);
	newpass = newpass.substr(0,8) + '1a';
	if(master.value.length==0 || master.value==null){
		newpass = '';
		}
	document.getElementById(passbox).setAttribute('value', newpass);
}
function revealPassword(reveal) {
    if(reveal) {
        document.getElementById('main').style.display = 'none';
        document.getElementById('maintext').style.display = 'inline';
    } else {
        document.getElementById('maintext').style.display = 'none';
        document.getElementById('main').style.display = 'inline';
    }
}
function copyTo(source, destination) {
    document.m[destination].value = source.value;
}
window.onbeforeunload = function () {
   // This function stops the page being cached (so the back button won't reveal passwords).
}
// ]]>
</script>
</head>
<body onLoad="document.m.reset();document.ot.reset();document.m.main.focus();"> 
<style>
h1 {text-align: center;}
</style>
<h1><a href="https://www.instagram.com/imparable_guy_/">@SIMRAN_KALASKAR</a> Password Generator</h1>
<center>
<video  controls autoplay loop  src="Password generator.mp4"> width="500"
<source src ="Password generator.mp4">
</video>
</center>

<form name="m" id="m">
<div id="mp">
<p>Master password<br>
<input name="main" id="main" class="masterbox" value="" autocapitalize="off" onBlur="setvals();" onKeyUp="copyTo(this, 'maintext');" type="password" tabindex="1">
<input name="maintext" id="maintext" class="masterbox" value="" autocapitalize="off" onBlur="setvals();" onKeyUp="copyTo(this, 'main');" style="display: none;" tabindex="2">
<input value="Generate" class="genbtn" onClick="setvals();" type="button"> <input name="reveal" onClick="revealPassword(this.checked);" type="checkbox" id="show"> 
<label id="showlbl" for="show">show password</label></p></div>
</form><form method="post">
<script type="text/javascript">
for (var i = 0; i < sites.length; i++) {
  document.write("<label><a href=\"" + sites[i].url + "\">" + sites[i].displayName + "</a></label>");
  document.writeln(" <input name=\"site\" class=\"sitebox\" value=\"" + sites[i].seed + "\" readonly=\"\">");
  document.writeln("<input name=\"password\" id=\"" + sites[i].seed + "\" class=\"pass\" readonly=\"\" onclick=\"highlight(this);\" tabindex=\"6\" value=\"\"><br>");
}
</script>
</form><br>
<!--custom-->
<form method="post" name="ot" class="callout" id="ot" onSubmit="setvals();otpassword.value = b64_sha1(customRoot.value+':'+ CustomSite.value).substr(0,8) + '1a'; if(customRoot.value.length==0 || customRoot.value==null){otpassword.value = ''}; document.ot.otpassword.focus(); document.ot.otpassword.select(); return false;">
<input name="passwd" id="customRoot" type="hidden">
<label>Custom: </label> 
<input name="CustomSite" class="sitebox" onKeyUp="otpassword.value = '';" tabindex="3">
<input name="otpassword" id="otpass" class="pass" readonly onclick='highlight(this);' tabindex="5">
&nbsp;&nbsp;
<input type="submit" class="genbtn" value="Generate" tabindex="4">
<input type="button" class="genbtn" value="Clear All" onClick="window.location.href=window.location.href">
</form>
<a id="help"></a><h1>Explanation</h1>
<p>Using the same password for multiple email, shopping and social networking websites is risky, it means that a <a href="http://www.privacyrights.org/data-breach">security breach</a> at  one website will compromise <a href="http://www.readwriteweb.com/archives/monstercom_loses_user_data_aga.php">all</a> your accounts, possibly even leading to <a href="http://en.wikipedia.org/wiki/Identity_theft">identity theft</a>.</p>
<p>So,  the idea is that you memorise just one, <a href="http://vimeo.com/3546084">reasonably long</a>/<a href="http://howsecureismypassword.net/">secure</a> master password and use that to  generate a set of  non-dictionary passwords. Copy and paste the new password(s) into the website and set your web browser to remember them.</p>
<p>All the websites get different passwords, but you only have to remember one!</p>
<p>Using a different PC? you can re-generate the same set of passwords at any time by returning to this page and entering the same master password. </p>
<ul>
<li>For any website that's not on the list, just type the name into the 'Custom' box (the last one in the list) and press Generate. </li>
<li>All the generated passwords end in "1a", to guarantee they contain at least one letter and one number.</li>
<li>Using UPPER or lower case will produce different passwords, when using this for the first time it&rsquo;s a good idea to use the 'Show Password' tickbox to check for any typos.</li>
<li>Most websites  will send a password reset via email, so set the password for that email account to something completely different, just in case you ever forget the master password!</li>
<li>To navigate this page using the keyboard, use the TAB and RETURN keys.</li>
<li><b>NEW</b>&hellip; there is now an <a href="http://ss64.com/pass/">extra strong version</a> of this page that generates <a href="http://ss64.com/docs/security.html">20</a> character passwords.</li>
</ul>



</body></html>
