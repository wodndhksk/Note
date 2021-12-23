```javascript

 function submitBookInfo(){
	document.write(
		  '<form action="'+locationURL+'" id="bookInfoForm" method="get">'
		  +'<input type = "hidden" id="idx" name="idx" value="'+idx +'" >'
		  +'<input type = "hidden" id="currentUrl" name="currentUrl" value="'+currentUrl +'" >'
		  +'</form>'
		  );
	document.getElementById("bookInfoForm").submit();
 }
 
```
