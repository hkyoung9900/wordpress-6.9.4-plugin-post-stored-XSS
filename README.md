# wordpress-6.9.4-plugin-post-stored-XSS
wordpress 6.9.4 plugin post stored XSS write up

# intro
In WordPress 6.9.4, a Stored XSS vulnerability exists in both the title 
and content fields of the Posts feature.

# write up
1. Prepare a WordPress 6.9.4 environment.
<img width="946" height="484" alt="1" src="https://github.com/user-attachments/assets/d69363c3-13a3-480b-8125-16199f598fd3" />

2. Navigate to the Posts section and click Add Post to create a new post.
<img width="946" height="484" alt="2" src="https://github.com/user-attachments/assets/f2955bf4-ee56-48bd-830d-aee5982ff4dc" />

3. Enter "test1" in the title field and "test2" in the content field, 
   then click Publish to upload the post.
   (Directly inserting XSS payloads will be filtered, so Burp Suite must be used.)
<img width="946" height="484" alt="3" src="https://github.com/user-attachments/assets/24e26e01-9331-4ebe-9419-6ebe65541718" />

4. In Burp Suite, intercept the packet and confirm that "test1" and "test2" 
   are visible in the request.
<img width="486" height="280" alt="4" src="https://github.com/user-attachments/assets/806a71a8-0297-4ce3-aea9-057a5878a61c" />

5. Replace the values inside the double quotes in the title and content 
   fields with the XSS payload. Additionally, if another Burp Suite packet 
   is intercepted before clicking View Post, insert the XSS payload in the 
   same manner.

Payload
<img width="289" height="81" alt="image" src="https://github.com/user-attachments/assets/f58b7717-2cfd-4dce-b6cd-2dadedd3c389" />

<img width="960" height="446" alt="5" src="https://github.com/user-attachments/assets/6b49ec44-ea32-4a7f-8abb-a18e8ea90a43" />

6. After uploading the post, click View Post to access the published post.
<img width="960" height="473" alt="6" src="https://github.com/user-attachments/assets/4b6cc70c-61f7-4d9b-947d-82564cd01864" />

7. As shown in the figure below, the XSS payload has been successfully triggered in both the title (document.cookie) and content (alert(2)) fields. This demonstrates that an attacker can steal session cookies and sensitive information from all visitors who view the post, potentially leading to session hijacking, account takeover, and unauthorized access to the victim's WordPress account.

--> title
<img width="960" height="431" alt="7" src="https://github.com/user-attachments/assets/ccdca35e-1560-4c88-b511-b5753b4ec78d" />
--> content
<img width="960" height="407" alt="8" src="https://github.com/user-attachments/assets/230173e1-3495-4669-a86e-067b09980449" />
