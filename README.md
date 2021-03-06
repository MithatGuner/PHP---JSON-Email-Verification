Donate
======
[![PayPal donate button](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=GBH93KNRNJ8AU "Show your appreciation with a donation")


PHP---JSON-Email-Verification
=============================

This is a short, simple, and straight forward script to perform some basic verification on a submitted email address. Can be used either as a PHP include/require or as a straight up URL with one GET variable so that one can use it with just about any other language such as JavaScript, etc. When calling through a URL with another language such as JavaScript, the output is returned as a JSON string.

```TEXT
{"format_verified":1,"error":0,"domain_verified":1,"message":"Formatting and domain have been verified"}
```

```TEXT
["format_verified"] => 1 (BOOL)
["error"] => 0 (BOOL)
["domain_verified"] => 1 (BOOL)
["message"] => "Formatting and domain have been verified" (STRING)
```

This has only been developed and tested on a CentOS Apache setup with PHP 5.4.21 .

[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/3d758e78cf54cd56faf178786176f624 "githalytics.com")](http://githalytics.com/DukeOfMarshall/PHP---JSON-Email-Verification)

Options
=======
| Option | Type | Notes |
| ------ |:----:| ----- |
| address_to_verify | STRING | The email address that you are wanting to verify |
| verbose | BOOL | Return more detailed error messages |

PHP Methods
===========
| Method | Passed Options | Return | Purpose |
| ------ | -------------- | ------ | ------- |
| verify_domain() | $address_to_verify | TRUE if MX record has been verified. FALSE otherwise | Verify that an MX record exists for the domain name in the email address passed to the method |
| verify_formatting() | $address_to_verify, $verbose | TRUE if email address is formatted as it should. If the address is not formatted correctly, then the address will return FALSE if $verbose is not set or a STRING message if $verbose IS set. | Verify that the email address passed to the method is formatted correctly |
| convert_html_to_plain_txt() | $content, $remove_links | The plain text of the HTML code that was submitted | To take the HTML code submitted and remove all the HTML code sections out of the block. If $remove_links is set to TRUE, then the address from < a > links will be gone. FALSE is the default for $remove_links and will instead leave the address from < a > links in the returned plain text string.

JavaScript Example:
===================
Using jQuery:
```JAVASCRIPT
$(function(){
	$("#js_verify").click(function(){
		$.getJSON("EmailVerify.class.php", { "address_to_verify" : $("#email").val() }, function(data){
			alert(data.message);
			return false;
		});
		return false;
	});
});
```

PHP Example:
============
```PHP
require_once("EmailVerify.class.php");
$verify = new EmailVerify();

if($verify->verify_formatting($_POST['email'])){
	echo "Email is formated correctly<BR>\r\n";
}else{
	echo "Email is NOT formated correctly<BR>\r\n";
}

if($verify->verify_domain($_POST['email'])){
	echo "Domain has been verified<BR>\r\n";
}else{
	echo "Domain has NOT been verified<BR>\r\n";
}

$plain_text = $verify->convert_html_to_plain_txt($html_email_content_to_convert);
```
