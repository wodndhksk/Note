```java
String thumbnail_url = "";
		String contents = "";
		
		String isbn = "";
		
		Map<String, String> returnMap = new HashMap<>();
		
		URL kakao_url = new URL("https://dapi.kakao.com/v3/search/book?target=isbn&query="+isbn.trim());	
		
		String authorization = "KakaoAK 2a26fe23c4684b23899bc2319ff68457";
		HttpsURLConnection con = (HttpsURLConnection)kakao_url.openConnection();
		
		con.setRequestMethod("GET");
        con.setRequestProperty("Authorization", authorization);
        int responseCode = con.getResponseCode();
        BufferedReader br = new BufferedReader(new InputStreamReader(con.getInputStream(), "UTF-8"));
        
		if(responseCode==200) { // 정상 호출
            br = new BufferedReader(new InputStreamReader(con.getInputStream(), "UTF-8"));
        } else {  // 에러 발생
            br = new BufferedReader(new InputStreamReader(con.getErrorStream(), "UTF-8"));
        }
		
		StringBuilder sb = new StringBuilder();
		String line;
		
		while((line = br.readLine()) != null){
			sb.append(line);
		}
		
		String xml = sb.toString();
		//System.out.println("xml : " + xml);
		
		br.close();
		con.disconnect();

	}

```
