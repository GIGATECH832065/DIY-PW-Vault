# DIY-PW-Vault
How to Build and Run a Secure Python Password Manager from a USB Drive
This comprehensive guide will walk you through creating a secure password manager using Python, storing it on a USB drive for portability, and running it with a portable Python installation. The password manager uses the cryptography library for secure encryption, ensuring your passwords are safely stored and accessible only with your encryption key.
Prerequisites
Before starting, ensure you have the following:
•
A USB drive with at least 1 GB of free space (for Portable Python and project files).
•
A Windows computer with internet access to download necessary software.
•
Basic familiarity with command-line operations and Python programming.
Step 1: Install Portable Python on the USB Drive
Portable Python allows you to run Python without installing it on the host computer, making your password manager fully portable.
1.
Insert the USB Drive: Plug your USB drive into your computer and note its drive letter (e.g., E:).
2.
Download Portable Python: Visit winpython.github.io and download the latest WinPython release (e.g., WinPython 3.x). Choose the portable version compatible with your system (32-bit or 64-bit).
3.
Extract to USB: Extract the downloaded archive to a folder on your USB drive, such as E:\WinPython. This creates a self-contained Python environment.
4.
Verify Installation: Open the WinPython Command Prompt.exe from the E:\WinPython folder. Run the following command to confirm Python is working:
python --version
You should see the Python version (e.g., Python 3.11.4).
Step 2: Set Up Your Project Directory
Create a dedicated folder on the USB drive to organize your password manager project.
1.
Create Project Folder: In the WinPython Command Prompt, navigate to your USB drive and create a project directory:
2.
cd E:\
3.
mkdir PasswordManager
cd PasswordManager
4.
Create Main Python File: Create a file named password_manager.py in the E:\PasswordManager folder. You can use a text editor like Notepad or any code editor available on the host computer, or create it via the command prompt:
echo. > password_manager.py
5.
Install the Cryptography Library: Install the cryptography library, which is essential for secure encryption:
pip install cryptography
This command downloads and installs the library within the WinPython environment on your USB drive.
Step 3: Write the Password Manager Code
Copy the following Python code into password_manager.py. This code implements a basic but secure password manager using the cryptography.fernet module for encryption.
from cryptography.fernet import Fernet
import json
import os
class PasswordManager:
def __init__(self):
self.key = None
self.password_file = None
self.password_dict = {}
def create_key(self, path):
self.key = Fernet.generate_key()
with open(path, 'wb') as f:
f.write(self.key)
def load_key(self, path):
with open(path, 'rb') as f:
self.key = f.read()
def create_password_file(self, path, initial_values=None):
self.password_file = path
if initial_values is not None:
for key, value in initial_values.items():
self.add_password(key, value)
def load_password_file(self, path):
self.password_file = path
with open(path, 'r') as f:
for line in f:
site, encrypted = line.strip().split(":")
self.password_dict[site] = Fernet(self.key).decrypt(encrypted.encode()).decode()
def add_password(self, site, password):
self.password_dict[site] = password
if self.password_file is not None:
with open(self.password_file, 'a') as f:
encrypted = Fernet(self.key).encrypt(password.encode())
f.write(f"{site}:{encrypted.decode()}\n")
def get_password(self, site):
return self.password_dict[site]
def main():
pm = PasswordManager()
print("""GT1 Password Manager
1. Create new key
2. Load existing key
3. Create new password file
4. Load existing password file
5. Add new password
6. Get a password
q. Quit
""")
while True:
choice = input("Enter your choice: ")
if choice == "1":
path = input("Enter path to save key (e.g., E:\\PasswordManager\\key.key): ")
pm.create_key(path)
elif choice == "2":
path = input("Enter path of key (e.g., E:\\PasswordManager\\key.key): ")
pm.load_key(path)
elif choice == "3":
path = input("Enter path to save password file (e.g., E:\\PasswordManager\\passwords.txt): ")
pm.create_password_file(path)
elif choice == "4":
path = input("Enter path of password file (e.g., E:\\PasswordManager\\passwords.txt): ")
pm.load_password_file(path)
elif choice == "5":
site = input("Enter the site name: ")
password = input("Enter the password: ")
pm.add_password(site, password)
elif choice == "6":
site = input("Enter the site name: ")
print(f"Password for {site}: {pm.get_password(site)}")
elif choice == "q":
break
else:
print("Invalid choice!")
if __name__ == "__main__":
main()
Code Explanation
•
Class PasswordManager: Manages encryption keys and passwords.
•
Key Generation: create_key generates a Fernet key and saves it to a specified path.
•
Key Loading: load_key reads the key from the file.
•
Password File Management: create_password_file and load_password_file handle the storage and retrieval of encrypted passwords.
•
Password Operations: add_password encrypts and stores passwords, while get_password retrieves and decrypts them.
•
Main Loop: Provides a command-line interface for user interaction.
Step 4: Save and Run the Program from the USB Drive
1.
Save the Code: Ensure password_manager.py is saved in E:\PasswordManager.
2.
Run the Program: In the WinPython Command Prompt, navigate to the project folder and run the program:
3.
cd E:\PasswordManager
python password_manager.py
4.
Follow the Menu: The program displays a menu:
o
Select option 1 to create a new encryption key (e.g., save as E:\PasswordManager\key.key).
o
Select option 3 to create a new password file (e.g., save as E:\PasswordManager\passwords.txt).
o
Use options 5 and 6 to add and retrieve passwords.
o
Select q to quit.
5.
Store Files Securely: Keep key.key and passwords.txt on the USB drive, but maintain backups in a separate secure location (e.g., an encrypted cloud service or another USB drive).
Security Tip
•
Key Safety: Never store key.key and passwords.txt together without additional encryption. Consider using a separate USB drive for the key.
•
USB Encryption: Use tools like VeraCrypt to encrypt the USB drive itself for added security.
•
Proper Ejection: Always safely eject the USB drive to prevent data corruption.
Step 5: Using Your Password Manager
1.
First Run:
o
Create a new key (option 1) and save it to E:\PasswordManager\key.key.
o
Create a new password file (option 3) and save it to E:\PasswordManager\passwords.txt.
2.
Subsequent Runs:
o
Load the existing key (option 2) and password file (option 4).
o
Add new passwords (option 5) or retrieve existing ones (option 6).
3.
Portability:
o
Plug the USB drive into any Windows computer.
o
Open WinPython Command Prompt.exe from E:\WinPython.
o
Navigate to E:\PasswordManager and run python password_manager.py.
4.
Best Practices:
o
Regularly back up key.key and passwords.txt to a secure location.
o
Use strong, unique passwords for each site.
o
Never share your key file or store it on an unencrypted device.
Step 6: Enhancing Your Password Manager
To make your password manager more robust, consider these enhancements:
•
Graphical User Interface (GUI): Use Tkinter or PyQt to create a user-friendly interface. For portability, ensure the GUI library is included in WinPython or installed via pip.
•
Password Generator: Add a function to generate strong, random passwords with customizable length and character sets.
•
Cloud Backup: Implement secure cloud backup using APIs (e.g., Dropbox) with additional encryption. Ensure compatibility with Portable Python.
•
Error Handling: Add try-except blocks to handle file access errors or invalid inputs gracefully.
Example: Adding a Password Generator
Add the following method to the PasswordManager class in password_manager.py:
import string
import random
def generate_password(self, length=16):
characters = string.ascii_letters + string.digits + string.punctuation
password = ''.join(random.choice(characters) for _ in range(length))
return password
Then, update the main function to include a new menu option (e.g., 7 for generating a password).
Troubleshooting
•
Python Not Found: Ensure you're using the WinPython Command Prompt from the USB drive.
•
Module Not Found: Reinstall the cryptography library using pip install cryptography in the WinPython Command Prompt.
•
File Access Errors: Verify the paths for key.key and passwords.txt are correct (e.g., E:\PasswordManager\key.key).
•
Corrupted Files: Ensure the USB drive is ejected properly. If files are corrupted, restore from backups.
Security Considerations
•
Encryption: The cryptography.fernet module uses AES-128 in CBC mode, which is secure for most purposes. Ensure your key file is protected.
•
USB Drive Security: Use a dedicated USB drive with encryption (e.g., VeraCrypt) to prevent unauthorized access.
•
Backups: Regularly back up your key and password files to a secure, encrypted location.
•
Physical Security: Keep the USB drive in a safe place when not in use to prevent theft or loss.
Conclusion
By following this guide, you've created a secure, portable password manager that runs from a USB drive using Portable Python. This setup allows you to manage passwords securely across different Windows computers without requiring local Python installations. Enhance the program with additional features like a GUI or password generator to suit your needs, and always prioritize security by protecting your encryption key and backing up your data.
For further learning, explore the GT1 Security Solutions blog at gt1blog.tech or check out their GitHub repository at github.com/GIGATECH832065 for more tutorials and resources.
