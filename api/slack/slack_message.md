## Slack 채팅방, DM 에 Message 보내는 API 

```java
package com.matrix2b.common.util;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.HashMap;
import java.util.Map;

import org.json.JSONException;
import org.json.JSONObject;


/**
 * Slack 알림 서비스
 * 
 * Slack 앱에서 설정 필요 (참고: https://0mini.tistory.com/45)
 * ---------------------------------------------
 * 1. Slack 에서 Bots 추가
 * 2. Slack api 에서 사용할 토큰 및  scopes 선택 (Bots 세팅)
 * 3. install to Workspace 
 * ---------------------------------------------
 * 4. 토큰 가져오기 (slackGetIdToken, slackDmToken)
 * 5. getSlackIdByEmail 호출로 Slack 회원 ID 조회
 * 6. 조회한 ID로 directMessageSend 호출
 * ---------------------------------------------
 * 
 *  @author  jwkim
 *  @since   2023-09-05
 */
public class SlackService {
    

    // Slack webHook url (Slack 그룹 메시지 url -> 사용안함)
    private static final String webHookUrl = "https://hooks.slack.com/services/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";
    
   
    // Slack 회원 ID api url
    private static final String lookUpByEmail = "https://slack.com/api/users.lookupByEmail?email=";
    // Slack 회원 ID api accessKey
    private static final String slackGetIdToken = "xoxp-3634330503696-3623256558785-5850528470133-74b4e4584d132f245305c8491d24791a";
    // Slack DM 메시지 accessToken
    private static final String slacDmUrl = "https://slack.com/api/chat.postMessage";
    // Slack DM 메시지 accessToken
    private static final String slackDmToken = "xoxb-3634330503696-5850528488597-lV7053nFHQkroNfVY9ZogUFO";

   
    
    /**
     * Slack DM 메시지 보내기
     * @param message
     * @throws JSONException 
     */
    public void directMessageSend(String message, String email) throws JSONException {
        try {
            URL url = new URL(slacDmUrl);
            String slackId = getSlackIdByEmail(email);
            
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("POST");
            conn.setRequestProperty("Content-Type", "application/json");
            conn.setRequestProperty("Authorization", "Bearer " + slackDmToken);
            conn.setDoOutput(true);

            Map<String, String> body = new HashMap<>();
            body.put("channel", message);
            body.put("text", message);

            String jsonInputString = "{\"channel\": \""+ slackId +"\",  \"text\": \"" + message + " \"}";
            
            System.out.println(jsonInputString);

            OutputStream os = conn.getOutputStream();
            byte[] input = jsonInputString.getBytes("utf-8");
            os.write(input, 0, input.length);
            os.flush();
            os.close();

            System.out.println(conn.getResponseCode());
            conn.getResponseCode();
            conn.disconnect();
           
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    /**
     * Slack 팀 채팅에 메시지 보내기 
     * @param message
     */
    public void groupMessageSend(String message) {
        try {
            URL url = new URL(webHookUrl);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("POST");
            conn.setRequestProperty("Content-Type", "application/json");
            conn.setDoOutput(true);

            Map<String, String> body = new HashMap<>();
            body.put("text", message);
//            String jsonString = ParsingUtil.objectMapper.writeValueAsString(body);
            String jsonInputString = "{\"text\": \"" + message + " \"}";

            OutputStream os = conn.getOutputStream();
            byte[] input = jsonInputString.getBytes("utf-8");
            os.write(input, 0, input.length);
            os.flush();
            os.close();

            conn.getResponseCode();
            conn.disconnect();
           
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    /**
     * Slack 회원 id 코드 값 가져오기
     * @param email
     * @return
     * @throws JSONException
     */
    private String getSlackIdByEmail(String email) throws JSONException {
        String slackId = "";
        
        try {
            URL url = new URL(lookUpByEmail + email);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");
            conn.setRequestProperty("Authorization", "Bearer " + slackGetIdToken);
            conn.setRequestProperty("Content-Type", "application/json");
            conn.setDoOutput(true);

            Map<String, String> body = new HashMap<>();
            body.put("text", email);


            conn.getResponseCode();
            
            
            JSONObject commands = new JSONObject();
            JSONObject responseJson = null;

            int responseCode = conn.getResponseCode();
            if (responseCode == 400 || responseCode == 401 || responseCode == 500 ) {
                System.out.println(responseCode + " Error!");
            } else {
                BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
                StringBuilder sb = new StringBuilder();
                String line = "";
                while ((line = br.readLine()) != null) {
                    sb.append(line);
                }
                responseJson = new JSONObject(sb.toString());
                responseJson = (JSONObject)responseJson.get("user");
                slackId = responseJson.get("id").toString();
                System.out.println(responseJson.get("id"));
            }
            
            conn.disconnect();
           
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        return slackId;
    }
    
   

}

```
