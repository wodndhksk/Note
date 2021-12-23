```javascript
var idx = "";

	function onClickBookList(idxSeq){
		idx = idxSeq;
		getOnClickedBookItem();
	}
	
	function getOnClickedBookItem(){	
		 cleanMenu();
		 console.log("idx : " + idx);
		  $.ajax({
			  url : url, // 보낼 controller 
			  type : "POST",
			  contentType: "application/json",
			  data : JSON.stringify({"idx":idx}), 
			  
			  success : function(data) {		  	
				  console.log(data); //json object 확인용					  
				  var image = data.jsonselectBookObject.image;
				  var title = data.jsonselectBookObject.title;
				  var description = data.jsonselectBookObject.description; 				  
				  var author = data.jsonselectBookObject.description;
				  var publisher = data.jsonselectBookObject.publisher;
				  var isbn = data.jsonselectBookObject.isbn;  
				  innerMenu(image, title, description, author, publisher, isbn);
			  },
			  
			  error : function(data) {
				  console.log("fail");
			  }
		  });
		
	  }
	
	 function innerMenu(image, title, description, author, publisher, isbn){
		 $('#main_menu').append(
				 '<div class="main__top">'
		 		+'<div class="main__thumbnail"><img src="'+ image +'" alt="" /></div><div class="main__info">'
		 		+'<h2>'+ title +'</h2>'
		 		+'<p>'+ description +'</p></div></div>'
		 		+'<div class="main__bottom"><div class="main__summary">'
		 		+'<div class="writter">글쓴이 &middot; '+ author +'</div>'
		 		+'<div class="publisher">출판사 &middot; '+ publisher +'</div>'
		 		+'<div class="isbn">ISBN &middot; '+ isbn +'</div>'
		 		+'</div></div>'
		 ); 
	 }
	 
	 function cleanMenu(){
		 $('#main_menu').html('');
		 
	 }
```
