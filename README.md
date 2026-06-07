# -Building-an-AI-Powered-Python-Security-Scanner-with-Gemini
Build Python AI security scanner with Gemini API. Detect SQL injection, command injection vulnerabilities. Color-coded severity ratings.

<img width="2752" height="1536" alt="AI-Powered_Code_Security_Scanner_Guide" src="https://github.com/user-attachments/assets/0582fa69-5186-4c8e-8846-50d454cbfd84" />

# AI Security Scanner for Python
---

![Image](http://learn.nextwork.org/radiant_blue_innocent_pawpaw/uploads/ai-security-audit-copy_tz9xp5km)

---

## Introducing Today's Project!

In this project, I’m going to build a command-line tool that uses Google’s Gemini AI to automatically scan Python scripts for security problems. This will help me learn how to integrate a large language model into a practical developer tool, manage API keys safely, and craft effective security prompts. I’m interested in this because manually spotting vulnerabilities like SQL injection or command injection is tedious and error prone. Automating it with AI feels like having a smart assistant that catches my mistakes before they become real bugs, plus I get to practice building something useful for my own coding workflow.

### Key tools and concepts

The key tools I used include Google Gemini API for AI powered code analysis, python-dotenv for secure environment variable management, and Colorama for colored terminal output. I also used virtual environments to isolate dependencies and the command line to run my scanner. Key concepts I learnt include prompt engineering, where a structured prompt with severity levels and fixed labels gives consistent and parseable results. I also learned how to detect real world vulnerabilities like SQL injection, command injection, weak hashing, and hardcoded secrets. Finally I learned why storing API keys in .env files is critical and how color coding helps prioritize fixes.

### Challenges and wins

I did this project today to learn how to integrate a large language model like Gemini into a practical developer tool, specifically for automated security scanning. I wanted to understand prompt engineering, secure API key management, and how to transform an AI response into actionable, color coded output. Another skill I want to learn is how to extend this scanner to analyze entire codebases, not just single files, and maybe add automatic fix suggestions using Gemini as well. I also want to learn how to containerize this tool with Docker so I can run it in CI/CD pipelines and catch vulnerabilities before code ever gets merged.

---

## Generating the Gemini API Key

In this step, I’m setting up access to Google’s Gemini AI by generating an API key. I need to do this so I can send Python code to Gemini and receive security analysis in return. Without a valid key, my CLI tool cannot call the model at all. I will visit Google AI Studio, click “Get API key,” and copy the generated string. Then I will store it securely in a .env file to keep it out of my source code. This is a small but essential step before I can write the actual scanning logic or test any vulnerability detection.

![Image](http://learn.nextwork.org/radiant_blue_innocent_pawpaw/uploads/ai-security-audit-copy_h3ymx6kd)

---

## Setting Up the Python Environment

In this step, I’m setting up an isolated Python environment for my security scanner project. I will create a new folder, navigate into it, and then use python -m venv venv to build a virtual environment. This is important for this project because it keeps my Gemini API dependencies separate from other Python projects, avoiding version conflicts. Activating the environment ensures that when I install google-generativeai and python-dotenv, they won’t interfere with system‑wide packages. A clean workspace makes my CLI tool reliable and easier to share or deploy later without unexpected library issues.

![Image](http://learn.nextwork.org/radiant_blue_innocent_pawpaw/uploads/ai-security-audit-copy_rb4kj7mz)

### Installing the required packages

Now that I have my virtual environment set up I need to install two essential packages: `python-dotenv` to load my Gemini API key from a `.env` file, and `google-genai` to actually call the Gemini model from Python. This is important for my project because without `dotenv` I would have to hardcode the key, which is insecure. Without the Google GenAI library, I cannot send code to Gemini or receive vulnerability reports. Running `pip install python-dotenv google-genai` inside my activated virtual environment gives me clean, isolated dependencies for the scanner.

![Image](http://learn.nextwork.org/radiant_blue_innocent_pawpaw/uploads/ai-security-audit-copy_pw6dq3ck)

---

## Connecting to the Gemini API

In this step, I’m connecting to Google’s Gemini AI by writing Python code in scanner.py. I will load my API key from the .env file using dotenv, then initialize the Gemini client. Finally, I will send a simple test prompt like “Hello” to verify the connection works. This is important because if the connection fails, my entire security scanner cannot function. A successful test confirms that my key is valid, my environment is correct, and I am ready to build the vulnerability detection logic on top of a working API link. No scanner works without this step.

![Image](http://learn.nextwork.org/radiant_blue_innocent_pawpaw/uploads/ai-security-audit-copy_yw8dt2jh)

### Securing the API key with a .env file

I got an error because my code tried to load the API key from an environment variable that didn’t exist yet. Having a .env file is important because it keeps my secret key out of the source code. If I hardcoded the key into scanner.py and later pushed the project to GitHub, anyone could steal it and use my Gemini quota or even incur charges on my account. The .env file lets me load the key securely with python-dotenv, and adding .env to .gitignore ensures it never gets committed. This small habit protects my project from accidental exposure while keeping the scanner functional.

![Image](http://learn.nextwork.org/radiant_blue_innocent_pawpaw/uploads/ai-security-audit-copy_kn3jm8tb)

### Verifying the connection

I verified the connection by running `python3 scanner.py` in my terminal. Gemini responded with an explanation of Rayleigh scattering, describing how shorter blue wavelengths of sunlight scatter more than red ones when interacting with Earth’s atmosphere. That confirmed that my API key is loading correctly from the `.env` file, the `google-genai` library is communicating with Google’s servers, and my script can send prompts and receive responses. Without that meaningful reply to “Why is the sky blue?” I would know something is wrong with the setup. Now I am ready to replace the test prompt with real security scanning logic.

---

## Engineering the Security Prompt

In this step, I’m writing a security prompt that tells Gemini to act as a code security expert. The prompt will instruct the model to analyze Python code for SQL injection, weak hashing (like MD5 or SHA1), command injection (such as unsanitized os.system calls), and hardcoded secrets. It will also ask Gemini to return findings with line numbers and severity levels. This is important because without a precise prompt, Gemini might give general advice or miss real vulnerabilities. A well crafted system instruction transforms a general purpose AI into a focused security scanner that behaves consistently every time I run it.

![Image](http://learn.nextwork.org/radiant_blue_innocent_pawpaw/uploads/ai-security-audit-copy_rk9mb7ys)

---

## Building the File Scanner

In this step, I'm adding file scanning so my scanner can read actual Python files from my computer, extract their source code, and send that code to Gemini for vulnerability analysis. This is important because a security scanner that only asks "why is the sky blue" is useless for real projects. I need my tool to accept a file path as a command line argument, open and read the file safely, and pass the code inside my security prompt. Once this works, I can test it on real scripts and finally see Gemini flagging actual security issues like SQL injection or unsafe system calls in my own code.

![Image](http://learn.nextwork.org/radiant_blue_innocent_pawpaw/uploads/ai-security-audit-copy_jt7px3na)

---

## Running the Security Audit

In this step, I’m testing my scanner by creating a sample Python file that contains a real vulnerability, like a SQL injection risk using unsanitized user input inside an f-string query. I’ll start with one vulnerability first because it helps me verify that Gemini actually detects it and returns a clear report. If the scanner flags that single issue correctly, I can trust it for more complex cases. After that, I will add weak MD5 password hashing and a command injection via os.system() to see if the scanner catches all three. This step proves whether my prompt and connection work for real security tasks.

![Image](http://learn.nextwork.org/radiant_blue_innocent_pawpaw/uploads/ai-security-audit-copy_ub6ry1wf)

### Analyzing the vulnerability report

My scanner detected SQL injection in the authenticate function, weak MD5 hashing in hash_password, command injection via an f-string in process_input, and a hardcoded database password. The most surprising vulnerability was the command injection caused by a single letter “f” inside f"echo {user_input}". I never realized that using an f-string with unsanitized input could let an attacker run terminal commands directly. This shows that AI can help with security by catching subtle flaws that look innocent at first glance, especially ones that mix string formatting with system calls. It transforms vague warnings into concrete, line‑specific feedback.

---

## Adding Color-Coded Severity Ratings


In this project extension, I’m adding color coded severity ratings so critical vulnerabilities stand out from minor issues. First, I install colorama with pip install colorama and initialize it in my script. Then I update the Gemini prompt to ask for severity levels: “HIGH” for command injection or SQL injection, “MEDIUM” for weak hashing, “LOW” for hardcoded secrets. After receiving Gemini’s response, I write a small function that scans each line of the output and prints HIGH in red, MEDIUM in yellow, LOW in blue using colorama.Fore. This lets me glance at the terminal and know immediately which bug to fix first. No more guessing.

![Image](http://learn.nextwork.org/radiant_blue_innocent_pawpaw/uploads/ai-security-audit-copy_mj7rc2vy)

### How the color system works

I added colour to my responses by first installing the colorama library and importing init, Fore, Style into my script. Then I rewrote the security prompt to ask Gemini for severity labels like CRITICAL, HIGH, MEDIUM, and LOW in a fixed format. Next, I wrote a function called add_colors_to_output that replaces each severity word with a coloured version using Fore.RED for CRITICAL, Fore.YELLOW for HIGH, and so on. Finally, I wrapped the print(response.text) call with that function. Now every scan shows red alerts for dangerous bugs and green for low priority issues, helping me fix the worst problems first.

---

## Wrapping Up

This project took me approximately 75 minutes from start to finish, including the secret mission with color coding. The most challenging part was crafting the exact prompt format to make Gemini return consistent severity labels. At first, the model gave free text descriptions without the fixed structure I needed for color parsing. I had to refine the prompt several times, adding “use this exact format” and the separator lines before it worked reliably. The second challenge was understanding why my API key failed initially until I realized I forgot to load it from the .env file. Once both issues were fixed, the scanner ran smoothly and caught all four vulnerabilities.

---

---
